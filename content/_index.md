---
title: "Startseite"
layout: hextra-home
# Header-Bild der Startseite (content/header.webp), zugleich Vorschaubild (og:image)
images: ["header.webp"]
---

<div class="home-hero">
<div class="home-hero-text">

<div class="hx:mt-6 hx:mb-6">
{{< hextra/hero-headline >}}
  Projekte, Code &amp; Vorträge
{{< /hextra/hero-headline >}}
</div>

<div class="hx:mb-12">
{{< hextra/hero-subtitle >}}
  Eine Sammlung von Projekten,&nbsp;<br class="hx:sm:block hx:hidden" />Präsentationen und Notizen zu Technologie
  <br class="hx:sm:block hx:hidden" />
  von Dr. Hans Hubbertz
{{< /hextra/hero-subtitle >}}
</div>

<div class="hx:mb-6">
{{< hextra/hero-button text="Alle Posts" link="posts/" >}}
</div>

</div>

{{< home-header alt="Nahaufnahme eines Mikrocontroller-Boards mit Chips und Stiftleisten" >}}

</div>

<div class="hx:mt-6"></div>

{{< hextra/feature-grid cols="3" >}}
  {{< hextra/feature-card
    title="Projekte"
    subtitle="Von der Idee bis zur Umsetzung."
    icon="cube"
    link="kategorien/projekte/"
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
