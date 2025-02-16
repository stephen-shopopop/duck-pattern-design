---
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Welcome to duck pattern design
titleTemplate: '%s - Duck Pattern Design'
info: |
  ## Duck refactoring
  Presentation slides for developers.
author: stephen deletang
keywords: pattern design
presenter: true
browserExporter: build
download: true,
exportFilename: ducker-pattern-design-exported
export:
  format: pdf
  timeout: 30000
  dark: false
  withClicks: false
  withToc: false
twoslash: true
selectable: true
record: dev
contextMenu: true
wakelock: true
overviewSnapshots: false
colorSchema: auto
routerMode: history
aspectRation: 16/9
canvasWidth: 980
themeConfig:
  primary: '#5d8392'
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Duck design patterns

Presentation slides for developers

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/stephen-shopopop/duck-pattern-design" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: center
---

## Définitions

Schéma formant une solution reconnue comme fiable et robuste (aka. bonne pratique) à un problème connu ou récurrent

---

# Les design patterns

A partir de cette définition, on peut donc dire que les design patterns (patrons de conception en français) naissent via l’expérience. En effet, être confronté plusieurs fois à un même problème permet d’améliorer et d’affiner à chaque fois la solution qu’on y apporte. Au bout d’un moment, en fédérant les travaux d’autres personnes ayant été confrontées au même problème, on va finir par aboutir à une solution qui s’avère être la mieux adaptée pour ce problème précis. Il suffit ensuite d’en retirer la moelle pour obtenir un modèle réutilisable permettant de résoudre le problème de manière efficace et performante.

<br>

Un design pattern est donc une capitalisation du travail. On peut y voir 3 points positifs:

- Évite de réinventer la roue !
- Gain de temps
- Permet de s’assurer que l’on applique une “bonne” solution (fiable, robuste et éprouvée)

---

# Design Pattern = Nom + Problème + Solution + Conséquence

Historiquement, les design patterns proviennent des travaux de Christopher Alexander (architecte) dans les années 70 et ont été formalisés dans le livre “Design Patterns – Elements of Reusable Object-Oriented Software” du Gang Of Four en 1995 (ou GoF : groupe de 4 informaticiens, Erich Gamma, Richard Helm, Ralph Johnson et John Vlissides, auteurs de nombreux ouvrages en programmation orientée objet). Les patrons de conception ont été divisés par le GoF en 3 catégories que nous allons parcourir ensemble.

Dans cette présentation nous décrirons ces différents pattern avec le language typescript.

---

# Pré-requis

TypeScript offre un support entier pour le mot-clé class introduit avec ES2015.

Tout comme les autres fonctionnalités JavaScript, TypeScript ajoute les annotations de type et autres éléments de syntaxe pour vous permettre d’exprimer les relations entre les classes et les autres types [typescript les classes](https://www.typescriptlang.org/fr/docs/handbook/2/classes.html).

Livre [TypeScript Notions fondamentales (2e édition)](https://www.editions-eni.fr/livre/typescript-notions-fondamentales-2e-edition-9782409041266)

---
layout: center
---

# Les 3 fondamentaux de la POO

---
layout: center
---

# Encapsulation

L’encapsulation permet de faire voir l’objet à l’extérieur comme une boîte noire ayant certaines propriétés (attributs et fonctions) et ayant un comportement spécifié. La manière dont ces propriétés ont été implémentées est alors cachée aux utilisateurs de la classe. L’implémentation peut être modifiée sans changer le comportement extérieur de l’objet. Cela permet donc de séparer la spécification du comportement d’un objet, de l’implémentation pratique de ces spécifications.

---
layout: center
---

## Héritage

L’héritage, permettant entre autres la réutilisabilité et l’adaptabilité des objets. Ce principe est basé sur des classes dont les "filles" héritent des caractéristiques de leur(s) "mère(s)". Chacune des classes filles peut donc posséder les mêmes caractéristiques que ses classes mères et bénéficier de caractéristiques supplémentaires à celles de ces classes mères. Bien sur, toutes les méthodes de la classe héritée (fille) peuvent être redéfinies. Chaque classe fille peut, si le programmeur n’a pas défini de limitation, devenir à son tour classe mère.

---
layout: center
---

## Polymorphisme

Cette capacité dérive directement du principe d’héritage vu précédemment. En effet, comme on le sait déjà, un objet va hériter des attributs et méthodes de ses ancêtres. Mais un objet garde toujours la capacité de pouvoir redéfinir une méthode. On voit donc apparaître ici ce concept de polymorphisme : choisir en fonction des besoins quelle méthode ancêtre appeler, et ce au cours même de l’exécution.

---
src: ./pages/solid-principe.md
---

---
src: ./pages/creational-patterns.md
---

---
src: ./pages/structural-patterns.md
---

---
src: ./pages/behavior-patterns.md
---

---
layout: end
---
