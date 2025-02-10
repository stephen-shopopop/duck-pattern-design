---
layout: center
---

# Principe SOLID

La programmation orientée objet c’est bien sûr le polymorphisme, l’héritage et l’encapsulation. 

Mais en plus de ces notions de base relativement théoriques, et juste avant de parcourir les différents patterns, je souhaite faire un tour sur 5 principes importants de la POO regroupés sous l’acronyme SOLID (Robert C. Martin). Tout développeur, en particulier en entreprise, se doit de les connaitre. Ils permettent de garantir une évolution simple, une maintenance efficace et une bonne robustesse des applications que l’on développe.

---
layout: center
---

# SRP : Single Responsability Principle

<br>

## Principe de responsabilité unique

<br>

**Une classe doit avoir une et une seule raison de changer. Si une classe a plus d’une responsabilité, alors ces responsabilités deviennent couplées et des modifications apportées à l’une peuvent porter atteinte à l’autre.**

---
layout: center
---

# OCP : Open/Close Principle

<br>

## Principe d’ouvert/fermé

<br>

**Les entités logicielles doivent être ouvertes pour l’extension mais fermées à la modification.**

Autrement dit, une classe doit pouvoir être étendue mais sans modification de son code. On peut répondre à ce principe avec différents moyens, comme l’abstraction, la généricité ou certains design patterns (Factory par exemple). Cela permet de limiter fortement les risques de régression. Elle reste bien sûr ouverte à la modification en cas de défaut (bug).

---
layout: center
---

# LSP : Liskov Substitution Principle

<br>

## Principe de substitution de Liskov

<br>

**Tout type de base doit pouvoir être remplacé par l’un de ses sous-types.**

L’exemple le plus connu est celui du rectangle et du carré et je vous invite à aller lire l’article de [Wikipédia](https://en.wikipedia.org/wiki/Liskov_substitution_principle) à ce sujet, il est très bien fait. Généralement, la bonne solution pour respecter ce principe est de manipuler des interfaces ou d’utiliser l’héritage.

---
layout: center
---

# ISP : Interface Segregation Principle

<br>

## Principe de ségrégation des interfaces

<br>

**Un client ne doit jamais être forcé de dépendre d’une interface qu’il n’utilise pas.**

L’idée est donc de découper au mieux nos interfaces par besoin. Cela facilite le décomposition et la compréhension. Attention tout de même à ne pas abuser de ce principe, car on peut très vite se retrouver avec une démultiplication des interfaces qui deviendra contre-productive.

---
layout: center
---

# DIP : Dependency Inversion Principle

<br>

## Principe de l’inversion des dépendances

<br>

**Il ne faut pas dépendre (directement) de l’implémentation bas niveau que vous allez utiliser.**

Les modules haut niveau regroupent les fonctionnalités métier et ceux de bas niveau les briques techniques (communication, log, persistance…). L’idée est que les points de contact entre les modules soient matérialisés par des abstractions pour réduire le couplage et faciliter le remplacement d’un module de bas niveau (changement de base de données par exemple).

---
layout: center
---

# Références

Pour comprendre de manière un peu plus fun ces 5 principes, je vous invite à aller lire [cet article](https://www.arolla.fr/les-principes-solid-dans-la-vie-de-tous-les-jours/) qui les illustre par des situations de la vie courante.
