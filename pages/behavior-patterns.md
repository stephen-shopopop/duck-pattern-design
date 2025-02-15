---
layout: center
---

# Design patterns - Behavior

Les behavioral patterns ou patrons de comportement. Il en existe 11 et ils permettent de définir comment organiser nos objets pour que ceux-ci collaborent.

---
layout: center
---

# Table des matières

- Chain of responsibility
- Command
- Iterator
- Mediator
- Memento
- Observer
- State
- Strategy
- Template
- Visitor

---
layout: two-cols
---

# Chain of responsibility

Ce pattern permet de séparer les objets émetteurs de requêtes et les objets chargés de recevoir et traiter les requêtes.

On va donc éliminer le couplage entre les éléments en donnant une chance à plusieurs récepteurs de gérer une requête. C’est très pratique lorsque la chaine est composée dynamiquement et que les récepteurs sont déterminés à l’exécution (~ runtime).

Ce pattern implique 2 participants:

- **Handler:** interface que les handlers “concrets” devront implémenter. C’est le garant de la logique de chainage.
- **Handler “concret”:** traite les requêtes dont il est responsable. Il connait et peut accéder à son successeur dans la chaine.

::right::

```ts
interface DocumentHandler {
  setNext(handler: DocumentHandler): DocumentHandler;
  handle(documentType: string): string;
}
class PDFHandler implements DocumentHandler {
  private nextHandler: DocumentHandler;
  setNext(handler: DocumentHandler): DocumentHandler { this.nextHandler = handler; return handler; }
  handle(documentType: string): string {
    if (documentType === 'pdf') { return 'Handling PDF Document'; }
    if (this.nextHandler) { return this.nextHandler.handle(documentType); }
    return `Cannot Handle Document Type: ${documentType}`; 
  }
}
class TextHandler implements DocumentHandler {
  private nextHandler: DocumentHandler;
  setNext(handler: DocumentHandler): DocumentHandler { this.nextHandler = handler; return handler; }
  handle(documentType: string): string {
    if (documentType === 'text') { return 'Handling Text Document'; } 
    if (this.nextHandler) { return this.nextHandler.handle(documentType); } 
    return `Cannot Handle Document Type: ${documentType}`;
  }
}
const pdfHandler = new PDFHandler();
const textHandler = new TextHandler();
pdfHandler.setNext(textHandler);
console.log(pdfHandler.handle('pdf')); // Output: Handling PDF Document
console.log(pdfHandler.handle('text')); // Output: Handling Text Document
console.log(pdfHandler.handle('excel')); // Cannot Handle Document Type: excel
```

---
layout: two-cols
---

# Command

Ce pattern consiste à encapsuler dans un objet l’ensemble des informations nécessaires pour effectuer une action, immédiatement ou à une date ultérieure. Il nécessite l’implication de 5 participants:

- Un exécutant chargé de gérer l’exécution des actions.
- Une commande qui représente l’abstraction d’une action.
- Une commande “concrète” qui est l’implémentation d’une commande.
- Un récepteur qui est l’objet final sur lequel sera appliquée l’action portée par la commande.
- Un client qui va se charger d’instancier les commandes “concrètes” et de les passer à l’exécutant.

:: right ::

```ts
interface Command {
  execute(): void;
}
class Receiver {
  public turnOn() { console.log("Light is on"); }
  public turnOff() { console.log("Light is off"); }
}
class LightOnCommand implements Command {  
  constructor(private light: Receiver) {/** */ }
  execute() { this.light.turnOn(); }
}
class LightOffCommand implements Command {  
  constructor(private light: Receiver) { /** */ }
  execute() { this.light.turnOff(); }
}
class Invoker {  
  constructor(private command: Command) { /** */}
  press() { this.command.execute(); }
}

const light = new Receiver();
const lightOnCommand = new LightOnCommand(light);
const lightOffCommand = new LightOffCommand(light);
const switchOn = new Invoker(lightOnCommand);
const switchOff = new Invoker(lightOffCommand);
switchOn.press(); // Light is on
switchOff.press(); // Light is off
```

---
layout: two-cols
---

# Iterator

Les collections font partie des types de données les plus utilisées en développement. Le pattern Iterator propose un moyen standard de parcourir les objets d’une liste sans avoir besoin d’exposer la structure interne de cette liste.

Le modèle de l'itérateur est utilisé pour parcourir une collection d'éléments. La plupart des langages de programmation fournissent des abstractions pour l'itération, comme la boucle for. Cependant, vous pouvez créer vos propres itérateurs en JavaScript en utilisant la propriété Symbol.iterator. Le code ci-dessous crée une fonction d'intervalle personnalisée qui peut être utilisée dans une boucle for ordinaire.

:: right ::

```ts
function range(start: number, end: number, step = 1) {
  let started = start

  return {
    [Symbol.iterator]() {
      return this
    },
    next() {
      if (started < end) {
        started = started + step

        return { value: started, done: false }
      }
      return { done: true, value: end }
    },
  }
}

for (const n of range(0, 20, 5)) {
  console.log(n) // 5, 10, 15, 20
}
```

---
layout: two-cols
---

# Mediator

Le médiator est un élément dont la vocation est la gestion et le contrôle des interactions dans un ensemble d’objets sans que ceux-ci aient besoin de se connaitre mutuellement.

Ce pattern est similaire au pattern Facade, à la différence que ce dernier fonctionne uniquement avec des échanges unidirectionnels entre les composants du système (de l’extérieur vers l’intérieur).

:: right ::

```ts
interface Receiver {
  receive(message: string): void;
}
interface Sender {
  recipients: Programmer[];
  send(message: string): void;
}
class Programmer implements Receiver {
  constructor(public name: string) {/** */}
  receive(message: string) { console.log(`${this.name} received: ${message}`); }
}
class MessageMediator implements Sender {
  recipients: Programmer[] = [];
  add(recipient: Programmer) { this.recipients.push(recipient); }
  send(message: string): void { this.recipients.map(recipient => recipient.receive(message)); }
}

function spamMonster(message: string, worker: MessageMediator) { worker.send(message); }

const messageMediator = new MessageMediator();
const user1 = new Programmer("Linus Torvalds");
const user2 = new Programmer("Tim Cook");
messageMediator.add(user1);
messageMediator.add(user2);
spamMonster("I'd Like to Add you to My Professional Network", messageMediator);
// Linus Torvalds received: I'd Like to Add you to My Professional Network
// Tim Cook received: I'd Like to Add you to My Professional Network
```

---
layout: two-cols
---

# Memento

Ce pattern permet de “photographier” et de stocker l’état exact d’un objet (autrement dit, faire un “snapshot”) dans le but de pouvoir le restaurer à l’identique ultérieurement (sans violer le principe d’encapsulation).

Ce pattern va donc avoir 3 acteurs:

- **Originator:** classe chargée de maintenir les données principales.
- **Memento:** classe utilisée pour stocker les données de la classe Originator (le “snapshot”)
- **CareTaker:** classe qui va contenir les instances du(des) Memento(s)

:: right ::

```ts
interface Memento {
  getState(): string;
}
class ConcreteMemento implements Memento {
  constructor(private state: string) { /** */ }
  getState(): string { return this.state; }
}
class Originator {
  constructor(private state: string) { console.log(`Originator: My initial state is: ${state}`); }
  doSomething(value: string): void { this.state = value; }
  save(): Memento { return new ConcreteMemento(this.state); }
  restore(memento: Memento) { this.state = memento.getState(); }
}
class Caretaker {
  #mementos: Memento[] = [];
  constructor(private originator: Originator) { /** */}
  backup() { this.#mementos.push(this.originator.save()); }
  undo(): void { if (!this.#mementos.length) return; this.originator.restore(this.#mementos.pop()); }
  showHistory() { for (const memento of this.#mementos) { console.log(memento.getState()); } }
}

const originator = new Originator('Hello the world!'); // Originator: My initial state is: Hello the world!
const caretaker = new Caretaker(originator);
caretaker.backup();
originator.doSomething('How are you?');
caretaker.backup();
caretaker.showHistory(); // Hello the world! \n How are you?
caretaker.undo(); // State is "Hello the world!"
```

---
layout: two-cols
---

# Observer

C’est un pattern très souvent utilisé lors du développement d’une application. Il offre le moyen à un objet (le sujet) d’avertir (~ notifier) d’autres objets d’un évènement.

Cela permet quand un objet change d’état, que tous les objets en relation soient notifiés et puissent adapter leur comportement (cas d’une interface graphique par exemple)

:: right ::

```ts
type Observer<T> = (value: T) => void

class Observable<T = string> {
  #observers: Array<Observer<T>> = []

  attach(func: Observer<T>) {
    if (typeof func !== 'function' || this.#observers.includes(func)) { return }
    this.#observers.push(func)
  }
  detach(func: Observer<T>) {
    this.#observers = this.#observers.filter((observer) => observer !== func)
  }
  notify(data: T) {
    for (const observer of this.#observers) { observer(data) }
  }
}

const observable = new Observable()

function logger(data: string) {
  console.log(`${Date.now()} ${data}`)
}

observable.attach(logger)
observable.notify('hello') // console.log('hello')
observable.detach(logger)
observable.notify('Hello the world') // nothing send to logger
```

---
layout: two-cols
---

# State

Ce pattern repose sur le principe de la machine à état et est très utile pour modéliser des workflows.

Le pattern State permet de gérer cela en évitant l’utilisation abusive des instructions conditionnelles (if… else…) et en assouplissant l’ajout d’un nouvel état. Chaque état sera modélisé par une classe dédiée (héritant d’une interface commune) contenant les comportements spécifiques à cet état. Une classe dite “de contexte” permet de garder une référence vers l’état courant. Le principal inconvénient de ce pattern est que la logique du workflow est diluée dans plusieurs classes.

:: right ::

```ts
class MusicPlayer {
  constructor(private state: State) { this.transitionTo(state); }
  transitionTo(state: State) {
    console.log(`MusicPlayer: Transition to ${(state.constructor).name}.`);
    this.state = state;
  }
  request() { this.state.handle(this); }
}
interface State {
  handle(context: MusicPlayer): void;
}
class PlayingState implements State {
  handle(context: MusicPlayer) { console.log("Player is in playing state", '-', context); }
}
class PausedState implements State {
  handle(context: MusicPlayer) { console.log("Player is in paused state", '-', context); }
}
class StoppedState implements State {
  handle(context: MusicPlayer) { console.log("Player is in stopped state", '-', context); }
}

const musicPlayer = new MusicPlayer(new StoppedState());
musicPlayer.request(); // Player is in stopped state - MusicPlayer { state: StoppedState {} }
musicPlayer.transitionTo(new PlayingState());
musicPlayer.request(); // Player is in playing state - MusicPlayer { state: PlayingState {} }
musicPlayer.transitionTo(new PausedState());
musicPlayer.request(); // Player is in paused state - MusicPlayer { state: PausedState {} }
```

---
layout: two-cols
---

# Strategy

Le pattern Strategy permet de mettre en place plusieurs algorithmes pour une même tâche et de pouvoir en changer de manière simple et transparente.

Dans le cas où il y a peu d’algorithmes différents en jeu, l’utilisation de ce pattern n’est pas forcément pertinente. Cela va alourdir le code sans réel intérêt.

:: right ::

```ts
interface Strategy {
  doAlgorithm(data: string[]): string[]
}
class ConcreteStrategyA implements Strategy {
  doAlgorithm(data: string[]): string[] { return data.sort() }
}
class ConcreteStrategyB implements Strategy {
  doAlgorithm(data: string[]): string[] { return data.reverse() }
}
class Context {
  constructor(private strategy: Strategy) {/** */}
  setStrategy(strategy: Strategy) {
    this.strategy = strategy
  }
  doSomeBusinessLogic(): void {
    const result = this.strategy.doAlgorithm(['a', 'b', 'c', 'd', 'e'])
    console.log(result.join(','))
  }
}

const context = new Context(new ConcreteStrategyA())
console.log('Client: Strategy is set to normal sorting.')
context.doSomeBusinessLogic() // a,b,c,d,e
console.log('Client: Strategy is set to reverse sorting.')
context.setStrategy(new ConcreteStrategyB())
context.doSomeBusinessLogic() // e,d,c,b,a
```

---
layout: two-cols
---

# Template

Il permet de mettre en place la trame générale d’un algorithme (c’est-à-dire les étapes) et offre la possibilité de déléguer la réalisation d’une ou plusieurs étapes à d’autres objets.

e pattern Template ressemble au pattern Strategy à la différence que ce dernier n’impose pas de structure (les étapes) et qu’il permet donc de faire varier la totalité de l’algorithme. L’autre différence fondamentale est que Strategy utilise la délégation alors que Template se base sur l’héritage.

:: right ::

```ts
class BaseClass {
  templateMethod() {
    this.actionA();
    this.actionB();
  }
  actionA(): void {
    throw new Error('should not be invoker by BaseClass');
  };

  actionB(): void {
    throw new Error('should not be invoker by BaseClass');
  }
}
class ConcreteAClass extends BaseClass {
  actionA() { console.log('A take actionA')
  }
  actionB() { console.log('A take actionB') }
}
class ConcreteBClass extends BaseClass {
  actionA() { console.log('B take actionA') }
  actionB() { console.log('B take actionB') }
}

const a = new ConcreteAClass();
const b = new ConcreteBClass();
a.templateMethod(); // A take actionA, A take actionB
b.templateMethod(); // B take actionA, B take actionB
```

---
layout: two-cols
---

# Visitor

Ce pattern permet une séparation entre les données et les traitements.

Il permet d’ajouter de nouvelles fonctionnalités à une famille de classes sans modifier ses classes directement. C’est très pratique lorsque l’on a un ensemble de classes fermées (fourni par un tiers par exemple) et que l’on souhaite ajouter un nouveau traitement sur ces classes ou que l’on possède une liste d’objets hétérogènes auxquels on souhaite appliquer des comportements.

:: right ::

```ts
interface ExportVisitor {
  exportText(textElement: TextElement): void;
}
interface DocumentElement {
  accept(visitor: ExportVisitor): void;
}

class TextElement implements DocumentElement {
  constructor(private content: string) {/** */}
  accept(visitor: ExportVisitor) { visitor.exportText(this); }
  getContent(): string { return this.content; }
}
class PDFExporter implements ExportVisitor {
  exportText(textElement: TextElement) { console.log(`Exporting text to PDF: ${textElement.getContent()}`); }
}
class HTMLExporter implements ExportVisitor {
  exportText(textElement: TextElement): void { console.log(`Exporting text to HTML: ${textElement.getContent()}`); }
}

const textElement = new TextElement("This is a text element.");
const pdfExporter = new PDFExporter();
textElement.accept(pdfExporter); // Exporting text to PDF: This is a text element.
const htmlExporter = new HTMLExporter();
textElement.accept(htmlExporter); // Exporting text to HTML: This is a text element.
```
