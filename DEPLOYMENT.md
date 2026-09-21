# Deployment

Die Seite läuft unter <https://re-code.de> und wird von GitHub Pages
ausgeliefert. Dieses Dokument beschreibt den Aufbau und vor allem die zwei
Einstellungen, die man **nicht** verändern darf.

## Aufbau

```
Browser ──https──> nginx des Providers ──http──> GitHub Pages
                   91.204.5.97                   Host: hubbertz.github.io
                   TLS-Zertifikat                liefert den Inhalt
                   opensource-consult
```

Der A-Record von `re-code.de` zeigt **nicht** auf GitHub, sondern auf den
Reverse Proxy des Providers. Der terminiert TLS mit einem eigenen Zertifikat
und reicht die Anfrage unverschlüsselt an GitHub Pages weiter — und zwar unter
dem Host-Namen `hubbertz.github.io`, nicht unter `re-code.de`.

Gebaut wird mit Hugo (extended) und dem Theme [Hextra][hextra], das als
Hugo-Modul über `go.mod` eingebunden ist. Der Workflow
[`.github/workflows/pages.yml`](.github/workflows/pages.yml) baut nach
`public/` und lädt das Verzeichnis als Pages-Artefakt hoch.

[hextra]: https://github.com/imfing/hextra

## Einstellungen unter Settings → Pages

| Feld | Wert | |
|---|---|---|
| Source | **GitHub Actions** | nie „Deploy from a branch" |
| Custom domain | **leer lassen** | siehe unten |
| Enforce HTTPS | aus | ohne Custom Domain ohnehin ohne Belang |

### Warum „Custom domain" leer bleiben muss

Sobald dort `re-code.de` eingetragen ist, beantwortet GitHub Pages Anfragen
für den Host `hubbertz.github.io` — also genau die Anfragen des Proxys —
pauschal mit `301 → http://re-code.de/`. Zusammen mit der http→https-Regel des
Proxys entsteht eine Endlosschleife, und der Browser meldet
`ERR_TOO_MANY_REDIRECTS`:

```
Browser  →  https://re-code.de/          nginx
         →  http, Host: hubbertz.github.io  →  GitHub
         ←  301 auf http://re-code.de/
Browser  →  http://re-code.de/           nginx: 301 auf https
         →  ... von vorn
```

Das TLS-Zertifikat für `re-code.de` stellt der Provider aus, nicht GitHub.
GitHub muss die Domain deshalb gar nicht kennen.

Trägt man die Domain trotzdem ein, zeigt GitHub zusätzlich dauerhaft
`NotServedByPagesError` an — GitHub prüft, ob die Domain auf seine eigenen
IPs (185.199.108–111.153) zeigt, was hinter einem Proxy nie zutreffen kann.
Diese Meldung ist ein Symptom des Proxy-Aufbaus, keine Fehlerursache.

### Warum die Source auf „GitHub Actions" stehen muss

Bei „Deploy from a branch" liefert GitHub den Branch-Inhalt direkt aus und
ignoriert den Workflow. Da im Repository kein fertiges HTML liegt — `public/`
ist in [`.gitignore`](.gitignore) — würde Pages ins Leere zeigen und für alle
Adressen 404 liefern.

Beim Umstellen auf einen Branch legt GitHub außerdem automatisch eine
`CNAME`-Datei im Publish-Verzeichnis an, sobald eine Custom Domain gesetzt
ist. Falls so eine Datei wieder auftaucht: löschen.

## Veröffentlichen

```bash
git add -A
git commit -m "..."
git push
```

Der Push startet den Workflow. Bis die Änderung live sichtbar ist, vergehen
rund 5–10 Minuten: Build und Deployment dauern ein bis zwei Minuten, den Rest
braucht die CDN-Verteilung.

## Wenn eine Änderung nicht sichtbar wird

GitHub Pages liegt hinter Fastly. Der Cache unterscheidet nach
`Accept-Encoding`, und einzelne Varianten können veraltete Antworten
festhalten — auch 404er und Weiterleitungen aus einer früheren Fehlkonfiguration.
Ein Reload im Browser hilft dann nicht, der Cache liegt am Edge.

So prüft man, was der Ursprung wirklich liefert:

```bash
# Cache-Zustand sichtbar machen (Age und X-Cache beachten)
curl -sS -o /dev/null -D- https://re-code.de/ | grep -iE 'HTTP/|age:|x-cache:|location:'

# Eine selten genutzte Cache-Variante erzwingt meist einen frischen Abruf
curl -sS -H 'Accept-Encoding: deflate' https://re-code.de/ | head

# GitHub Pages direkt fragen, am Proxy vorbei
curl -sS -I -H 'Host: hubbertz.github.io' http://185.199.108.153/
```

Query-Strings taugen nicht als Cache-Buster — Pages ignoriert sie beim
Cache-Key. Jedes neue Deployment löst einen Purge aus; das ist der
zuverlässigste Weg, festhängende Einträge loszuwerden.
