---
layout: center
---

# Design patterns - Creational

les premiers types de patterns que nous allons aborder sont les creational patterns ou patrons de création. Il en existe 5 et ils permettent de définir la manière de faire l’instanciation et la configuration des classes ou des objets de manière souple tout en minimisant le couplage et en maximisant la réutilisation du code.

---
layout: center
---

# Table des matières

- Singleton
- Factory
- Abstract Factory
- Builder
- Prototype

---
layout: two-cols
---

# Singleton

C’est probablement le design pattern le plus connu et le plus utilisé. Il permet de s’assurer qu’une classe ne peut posséder qu’une seule instance à un moment donné et fournit un accès unique à cette instance.

Ce pattern est très utile lorsque l’on souhaite accéder à un périphérique physique (imprimante, disque dur…) ou que l’on a besoin de garantir l’accès à un interlocuteur unique (gestionnaire de logs, d’exceptions…).

::right::

```ts
class Singleton {
  static #instance: Singleton

  readonly #mode = 'dark'

  // prevent new with private constructor
  private constructor() {
    /** */
  }

  static getInstance(): Singleton {
    if (!Singleton.#instance) {
      Singleton.#instance = new Singleton()
    }

    return Singleton.#instance
  }

  someBusinessLogic() {
    return Singleton.#instance.#mode
  }
}

const singleton = new Singleton() // throws error
const singleton = Singleton.getInstance()

console.log(singleton.someBusinessLogic()) // dark
```

---
layout: two-cols
---

# Singleton autre méthodes

Javascript propose une autre méthode à base de [ObjectFreeze](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) ou encore de [ObjectSeal](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object/seal).

La méthode Object.freeze() permet de geler un objet, c'est-à-dire qu'on empêche d'ajouter de nouvelles propriétés, de supprimer ou d'éditer des propriétés existantes, y compris en ce qui concerne leur caractère énumérable, configurable ou pour l'accès en écriture. L'objet devient ainsi immuable. La méthode renvoie l'objet ainsi « gelé ».

La méthode Object.seal() scelle un objet afin d'empêcher l'ajout de nouvelles propriétés, en marquant les propriétés existantes comme non-configurables. Les valeurs des propriétés courantes peuvent toujours être modifiées si elles sont accessibles en écriture.

::right::

**Object.freeze:**

```ts
const singleton = Object.freeze({
  mode: 'dark',
  someBusinessLogic: function () {
    return this.mode
  },
})

console.log(singleton.someBusinessLogic()) // dark
```

**Object.seal:**

```ts
const singleton = Object.seal({
  mode: 'dark',
  someBusinessLogic: function () {
    return this.mode
  },
})

console.log(singleton.someBusinessLogic()) // dark
```

---
layout: two-cols
---

# Factory

Une factory est une méthode ou une fonction qui crée un objet ou un ensemble d'objets sans exposer la logique de création au client.

Les principes d’une bonne conception objet recommandent l’utilisation d’interfaces plutôt que des implémentations dans le but de découpler au maximum l’appelant et les classes concrètes (ce qui permet de remplacer facilement une implémentation par une autre sans tout modifier). A un moment donné, en général lors de l’exécution, il sera tout de même nécessaire d’instancier des objets concrets: c’est là qu’intervient le pattern Factory. Ce pattern se base justement sur les interfaces pour permettre à l’application de ne manipuler qu’un seul type (l’interface) et de masquer l’origine réelle des objets.

::right::

```ts
class IOSButton {
  readonly display = 'ios'
}

class AndroidButton {
  readonly display = 'android'
}

class ButtonFactory {
  createButton(os: string): IOSButton | AndroidButton {
    if (os === 'ios') {
      return new IOSButton()
    }

    return new AndroidButton()
  }
}

// Factory
const factory = new ButtonFactory()

const btn1 = factory.createButton('ios')
console.log(btn1.display) // 'ios'

const btn2 = factory.createButton('andrdoid')
console.log(btn2.display) // 'android'
```

---

# Abstract factory

Très proche du pattern Factory, l’Abstract Factory est chargée de créer des familles d’objets (liés ou dépendants).

Commençons par quelques rappels. En POO, l’abstraction est un mécanisme permettant de regrouper des comportements communs à un ensemble d’objets.

En fonction d’un choix, une factory “générale” (via une méthode GetFactory(…)) retourne une factory “spécialisée”. C’est cette dernière qui se charge de créer les instances souhaitées. Ces factories vont donc exposer autant de méthodes de création qu’il y a de produits dans une famille. Cela offre de nombreux avantages:

- L’isolation des classes concrètes: comme pour le pattern Factory, le client travaille uniquement avec les interfaces.
- Un changement facilité des familles de produit en remplaçant simplement la fabrique concrète.
- Une cohérence garantie entre les éléments d’une même famille.

Ce pattern nécessite une certaine gymnastique intellectuelle en particulier à cause du grand nombre d’acteurs, de classes et d’interfaces [Wikipedia](https://fr.wikipedia.org/wiki/Fabrique_abstraite).

---
layout: two-cols
---

# Builder

Ce pattern permet de séparer la construction d’un objet de sa représentation. Il est très utile pour créer, à partir d’un même processus, des représentations différentes.

En JavaScript, nous pouvons y parvenir grâce au chaînage de méthodes.

::right::

```ts
class HotDog {
  constructor(
    readonly bread: string,
    ketchup?: boolean,
    mustard?: boolean,
    kraut?: boolean,
  ) {/** */}

  addKetchup() {
    this.ketchup = true
    return this
  }
  addMustard() {
    this.mustard = true
    return this
  }
  addKraut() {
    this.kraut = true
    return this
  }
}

const myLunch = new HotDog('gluten free').addKetchup().addMustard().addKraut()

console.log(myLunch) // { ketchup: true, mustard: true, kraut: true, bread: 'gluten free' }
```

---
layout: two-cols
---

# Prototype

Le prototype permet aux objets d'être des clones d'autres objets, plutôt que d'être étendus par héritage. Ce pattern est surtout pertinent dans le cas où la création d’une nouvelle instance est significativement plus lente que son clonage.

Pour être complet sur le sujet, il est important de bien faire la différence entre les types de copies:

- **Shallow Copy (~ copie superficielle):** tous les membres de type valeur sont copiés. Pour les membres de type référence, seule la référence est copiée mais pas l’objet référencé.
- **Deep Copy:** C’est une duplication complète, donc les objets référencés sont eux aussi dupliqués. L’original et la copie pointent donc vers des instances différentes.

::right::

<br>

```ts
const zombie = Object.seal({
  mode: 'night',
  eatBrains: function () {
    return `yum 🧠 at ${this.mode}`
  },
})

const chad = Object.create(zombie, { name: { value: 'chad' } }) as
  & typeof zombie
  & Readonly<{ name: string }>

chad.mode = 'day'

console.log(chad.eatBrains()) // yum 🧠 at day
console.log(zombie.eatBrains()) // yum 🧠 at nigth

console.log(chad.name) // chad
```

La méthode statique [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) crée un nouvel objet, en utilisant un objet existant comme prototype de l'objet nouvellement créé.
