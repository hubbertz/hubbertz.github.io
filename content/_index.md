---
title: "Startseite"
layout: hextra-home
---

<div class="hx:mt-6 hx:mb-6">
{{< hextra/hero-headline >}}
  Projekte, Code &amp; Vorträge
{{< /hextra/hero-headline >}}
</div>

<div class="hx:mb-12">
{{< hextra/hero-subtitle >}}
  Eine Sammlung eigener Projekte, Open-Source-Arbeit,&nbsp;<br class="hx:sm:block hx:hidden" />Präsentationen und Notizen zu Technologie.
{{< /hextra/hero-subtitle >}}
</div>

<div class="hx:mb-6">
{{< hextra/hero-button text="Alle Posts" link="posts/" >}}
</div>

<div class="hx:mt-6"></div>

{{< hextra/feature-grid cols="2" >}}
  {{< hextra/feature-card
    title="Projekte"
    subtitle="Eigene Projekte – von der Idee bis zur Umsetzung."
    icon="cube"
    link="kategorien/projekte/"
  >}}
  {{< hextra/feature-card
    title="Open Source"
    subtitle="Beiträge zu Open-Source-Projekten und veröffentlichter Code."
    icon="code"
    link="kategorien/open-source/"
  >}}
  {{< hextra/feature-card
    title="Präsentationen"
    subtitle="Vorträge, Folien und Workshops."
    icon="presentation-chart-bar"
    link="kategorien/praesentationen/"
  >}}
  {{< hextra/feature-card
    title="Technologie"
    subtitle="Notizen und Artikel zu Werkzeugen, Sprachen und Konzepten."
    icon="chip"
    link="kategorien/technologie/"
  >}}
{{< /hextra/feature-grid >}}
