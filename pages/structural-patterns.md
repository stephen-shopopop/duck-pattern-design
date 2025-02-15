---
layout: center
---

# Design patterns - Structural

Après les patterns creationals, nous allons aborder les structural patterns ou patrons de structure. Il en existe 7 et ils permettent de définir comment organiser nos objets.

---
layout: center
---

# Table des matières

- Adapter
- Bridge
- Composite
- Decorator
- Façade
- Flyweight
- Proxy

---
layout: two-cols
---

# Adapter

L’objectif du pattern Adapter est de faire passer quelque chose pour autre chose sans perturber le reste de l’application.

C’est très utile dans le cas où le logiciel évolue et que l’on souhaite conserver au mieux la rétrocompatibilité descendante ou encore si l’on n’a pas accès aux objets mais que l’on souhaite les personnaliser.

Toute la complexité d’adaptation est cachée. En plus de s’adapter à la cible, il est aussi possible de réaliser des opérations de conversion si nécessaire.

::right::

```ts
class Adaptee {
  fetchData() {
    return {
      data: { name: 'john', age: 25, social: { email: 'john@doe.com' } },
    } as const;
  }
}

interface Target {
  email: string;
}

class Adapter implements Target {
  constructor(private adaptee: Adaptee) {/** */}

  get email(): string {
    return this.adaptee.fetchData().data.social.email;
  }
}

function clientCode(data: Target) {
  console.log(`Email: ${data.email}`);
}

const adaptee = new Adaptee();
const adapter = new Adapter(adaptee);
clientCode(adapter); // Email: john@doe.com'
```

---
layout: two-cols
---

# Bridge

Le pattern Bridge permet de découpler l’interface d’une classe de son implémentation de manière à ce qu’elles puissent évoluer séparément.

les patterns Adapter et Bridge paraissent proches, mais il faut tout de même noter quelques différences fondamentales:

- Adapter fait que les choses fonctionnent après qu’elles aient été conçues. Bridge les fait travailler avant d’être. (Bridge est conçu dès le départ alors que Adapter intervient généralement ultérieurement pour faire travailler des classes ensemble alors que ce n’était pas prévu à l’origine).
- Bridge est basé sur le principe **préférer la composition à l’héritage**

::right::

```ts
abstract class UI {
  constructor(protected backend: Backend) {/** */}
  abstract render(): void;
}

abstract class Backend {
  abstract getData(): string;
}

class AndroidUI extends UI {
  render() {
    const data = this.backend.getData();
    console.log("AndroidUI: Rendering data from the backend ->", data);
  }
}

class MobileBackend implements Backend {
  getData() {
    return "MobileBackend: Data from the backend";
  }
}

const mobileBackend = new MobileBackend();
const androidUI = new AndroidUI(mobileBackend);
androidUI.render(); // AndroidUI: Rendering data from the backend -> MobileBackend: Data from the backend
```

---
layout: two-cols
---

# Composite

Le pattern Composite permet de créer des objets complexes par assemblage d’objets simples.

Ce pattern a du sens uniquement si votre modèle peut-être représenté sous la forme d’un arbre. Il va aussi permettre de traiter les objets individuels de la même façon que l’ensemble de l’arbre. Pour cela, les objets regroupés doivent posséder des opérations communes.

:: right ::

```ts

interface Composite {
  getPrice(): number;
}

class Product implements Composite {
  constructor(private price: number) { /** */ }
  getPrice(): number { return this.price; }
}

class DiscountedProduct implements Composite {
  constructor(private price: number, private discount: number) { /** */}
  getPrice(): number { return this.price - this.discount; }
}

class ProductContainer implements Composite {
  #children: Composite[] = [];
  add(component: Composite) { this.#children.push(component); }
  getPrice(): number { return this.#children.reduce((totalPrice, child) => totalPrice + child.getPrice(), 0); }
}

const laptop = new Product(300);
const phone = new DiscountedProduct(200, 50);
const bigBox = new ProductContainer();
bigBox.add(laptop);
bigBox.add(phone);
console.log("Total price of big Box:", bigBox.getPrice()); // Output: Total price of big Box: 450
```

---
layout: two-cols
---

# Decorator

Le pattern Decorator permet d’apporter de nouvelles responsabilités (~ fonctionnalités ou comportements) à un objet existant, tout en gardant plus de souplesse qu’avec l’héritage.

- Utilisation des services “à la carte” (contrairement à l’héritage)
- Introduction des services de manière conditionnelle ET dynamique (le décorateur étant instancié au runtime)

:: right ::

```ts
class SimpleCoffee {
  cost() { return 5; }
  description() { return "Simple Coffee"; }
}

class MilkDecorator {
  constructor(private coffee: SimpleCoffee) {}
  cost() { return this.coffee.cost() + 2; }
  description() { return `${this.coffee.description()}, Milk`; }
}

// Create a simple coffee
const coffee = new SimpleCoffee();
// Add milk to the coffee
const coffeeWithMilk = new MilkDecorator(coffee);
console.log(`${coffeeWithMilk.description()} - cost: ${coffeeWithMilk.cost()}`) // Simple Coffee, Milk - cost: 7
```

---
layout: two-cols
---

# Façade

Le pattern Façade permet de fournir une interface simple pour manipuler un système complexe.

Contrairement à l’Adapter, le pattern Façade a pour objectif de simplifier une interface:

- Présenter volontairement un objet plus adapté par rapport à un client spécifique ou limiter les méthodes et/ou propriétés exposées
- En cas d’utilisation de plusieurs objets, permets d’agréger les appels dans un seul objet (pour réduire le nombre d’appels)

:: right ::

```ts
class PlumbingSystem {
  #pressure = 300
  setPressure(v: number) { this.#pressure = v }
  turnOn() { console.log(`Plumbing turn on with ${this.#pressure}`) }
  turnOff() { console.log(`Plumbing turn off with ${this.#pressure}`) }
}

class ElectricalSystem {
  #voltage = 220
  setVoltage(v: number) { this.#voltage = v }
  turnOn() { console.log(`Eletrical turn on with ${this.#voltage}`) }
  turnOff() { console.log(`Eletrical turn off with ${this.#voltage}`) }
}

class House {
  #plumbing = new PlumbingSystem()
  #electrical = new ElectricalSystem()

  turnOnSystems() {  this.#electrical.setVoltage(120); this.#electrical.turnOn(); this.#plumbing.setPressure(500); this.#plumbing.turnOn() }

  shutDown() { this.#plumbing.turnOff(); this.#electrical.turnOff() }
}

const client = new House()
client.turnOnSystems() // Eletrical turn on with 120; Plumbing turn on with 500
client.shutDown() // Eletrical turn off with 120; Plumbing turn off with 500
```

---
layout: two-cols
---

# Flyweight

Le pattern Flyweight (“poids mouche” en français dans le texte !) est utilisé pour optimiser le nombre d’objets présent en mémoire.

Pour cela, Flyweight va chercher à mutualiser les propriétés communes entre objets. Un objet possède deux types de données:

- des **données intrinsèques:** propre à l’objet (caractère ASCII…) et peut donc être partagé.
- des **données extrinsèques:** propre au contexte (couleur, taille, position du caractère…) et ne peut donc pas être partagé.

:: right ::

```ts
class Book {
  constructor(public title: string, public author: string, public isbn: string) { /** */}
}

class BookBuilder {
  #books = new Map<string, Book>()
  #bookList: Array<Book & { availability: boolean; sales: number }> = []
  #createBook(title: string, author: string, isbn: string) {
    const existingBook = this.#books.get(isbn)
    if (existingBook) { console.log('Book exist!'); return existingBook; }
    const book = new Book(title, author, isbn)
    this.#books.set(isbn, book)
    console.log('Book created!')
    return book
  }

  addBook(title: string, author: string, isbn: string, availability: boolean, sales: number) {
    const book = {...this.#createBook(title, author, isbn), availability, sales }
    this.#bookList.push(book)
    return book
  }
}

const library = new BookBuilder()
library.addBook('Harry Potter', 'JK Rowling', 'AB123', false, 100) // Book created!
library.addBook('Harry Potter', 'JK Rowling', 'AB123', true, 50) // Book exist!
```

---
layout: two-cols
---

# Proxy

Un proxy est une classe utilisée à la place d’une autre classe. Contrairement au pattern Adapter, le pattern Proxy va proposer une classe de substitution lorsque la classe réelle n’est pas directement utilisable. Proxy peut donc affecter le comportement, mais pas l’interface.

Ce pattern est particulièrement utilisé dans les cas suivants:

- Accès à des objets distants (base de données, disque dur réseau…) et donc potentiellement inaccessibles.
- Accès à des objets volumineux ou consommateur en ressources et devant être manipulé avec précaution.
- Accès à des objets nécessitant des droits d’accès spécifiques.

:: right ::

```ts
class Personn {
  constructor(public name) { /** */}
}

const bigBrother = new Proxy(new Personn('jeff'), {
  get(target, key) {
    console.log(`Tracking key: "${String(key)}"`)

    return Reflect.get(target, key)
  },
  set(target, key, value) {
    console.log(`Updating key: "${String(key)}" with value: "${value}"`)

    return Reflect.set(target, key, value)
  },
})

bigBrother.name // Tracking key: "name"
bigBrother.name = 'bob' // Updating key: "name" with value: "bob"
```
