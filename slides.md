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
  <a href="https://github.com/stephen-shopopop/duck-pattern design" target="_blank" class="slidev-icon-btn">
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
src: ./pages/solid-principe.md
---

---
src: ./pages/creational-patterns.md
---

---
src: ./pages/structural-patterns.md
---
