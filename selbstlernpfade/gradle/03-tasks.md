---
title: "Modul 03 — Tasks & Build Lifecycle"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Gradle
modul-nr: 3
dauer: 45 min
voraussetzungen:
  - "[[02-dependencies|Modul 02: Dependencies verwalten]]"
---

# Modul 03 — Tasks & Build Lifecycle
_„Die Routen der Expedition"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[02-dependencies\|Modul 02]] abgeschlossen · Projekt mit Gson und JUnit vorhanden |
| **Lernziel** | Du kannst erklären, was Gradle Tasks sind, eingebaute Tasks aufrufen und interpretieren, Task-Abhängigkeiten verstehen und einen einfachen eigenen Task schreiben. |

---

## Worum geht es?

Das Lager, kurz vor Aufbruch. Hartwood liest die Checkliste vor: „Ausrüstung zusammenpacken. Karten kopieren. Proviant verladen. Team briefen. Aufbrechen." Er sieht auf. „Jeder Schritt hängt vom vorherigen ab. Du kannst nicht aufbrechen, wenn das Team noch nicht gebrieft ist. Du kannst nicht briefen, wenn die Karten noch nicht kopiert sind."

Lila nickt. „Bei Gradle heißt jeder solche Schritt ein **Task**. Kompilieren ist ein Task. Testen ist ein Task. Ein JAR bauen ist ein Task." Sie öffnet die `build.gradle`. „Und der Clou: Tasks können voneinander abhängen. Wenn du sagst ,führe diesen Task aus', führt Gradle automatisch alle Tasks davor aus, die gebraucht werden."

Old Finn dreht seinen Hut in der Hand. „Und du kannst eigene Tasks schreiben. Für alles, was das Standard-Set nicht abdeckt." Er lächelt. „Das ist der Teil, den ich mag."

**Deine Mission:** Lerne, wie Gradle Tasks funktionieren — und schreibe deinen ersten eigenen.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Eingebaute Tasks entdecken | 🟢 | ~7 Min. |
| 2 | Build-Tasks ausführen und verstehen | 🟢 | ~8 Min. |
| 3 | Task-Abhängigkeiten verstehen | 🟡 | ~10 Min. |
| 4 | Eigenen Task schreiben | 🟡 | ~11 Min. |
| 5 | Incremental Build & clean | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Eingebaute Tasks entdecken 🟢

> Lila schlägt das Handbuch auf. „Bevor wir eigene Routen planen, schauen wir, welche bereits eingezeichnet sind."

**Deine Aufgabe:**
Lass dir alle verfügbaren Tasks deines Projekts anzeigen und erkunde die Ausgabe.

**Was du dafür brauchst:**

Führe im Terminal aus:
```
gradle tasks
```

Gradle listet alle verfügbaren Tasks gruppiert auf. Die wichtigsten Gruppen:

```
Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
clean - Deletes the build directory.

Verification tasks
------------------
check - Runs all checks.
test - Runs the test suite.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Help tasks
----------
dependencies - Displays all dependencies.
tasks - Displays the tasks runnable from root project.
```

> [!tip] Tipp
> Mit `gradle tasks --all` siehst du auch versteckte Tasks — z.B. `compileJava`, `processResources`, `compileTestJava`. Diese Tasks werden normalerweise automatisch als Teil von `build` oder `test` ausgeführt.

<details>
<summary>📜 Lösungsvorschlag</summary>

Die wichtigsten Tasks des Java-Plugins:

| Task | Was er tut |
|------|-----------|
| `compileJava` | Kompiliert `src/main/java/` → `build/classes/` |
| `compileTestJava` | Kompiliert `src/test/java/` → `build/test-classes/` |
| `test` | Führt alle Tests aus |
| `jar` | Packt den Code in ein JAR |
| `build` | Führt alles aus: compile → test → jar |
| `clean` | Löscht `build/` |

</details>

**Zum Nachdenken:** `build` ruft automatisch `test` auf, und `test` ruft `compileTestJava` auf. Was wäre der Vorteil dieses automatischen Durchlaufens, verglichen damit, alles manuell in der richtigen Reihenfolge aufzurufen?

---

## Aufgabe 2 — Build-Tasks ausführen und verstehen 🟢

> Old Finn schlägt auf den Tisch. „Genug gelesen. Jetzt laufen."

**Deine Aufgabe:**
Führe die wichtigsten Tasks aus und beobachte die Ausgabe genau.

**Was du dafür brauchst:**

Führe im Terminal nacheinander aus und notiere, was passiert:

```
gradle compileJava
```
```
gradle test
```
```
gradle build
```

**Was du in der Ausgabe siehst:**

Bei `gradle test`:
```
> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
> Task :test

JsonBeispielTest > toJsonSollStringMitAnfuehrungszeichenLiefern() PASSED

BUILD SUCCESSFUL in 3s
7 actionable tasks: 7 executed
```

**Wichtige Details:**
- `NO-SOURCE` bedeutet: Dieser Task existiert, hatte aber nichts zu tun (kein `resources`-Verzeichnis mit Dateien)
- Gradle führt `compileJava` automatisch vor `test` aus — auch wenn du nur `test` aufgerufen hast
- Am Ende steht, wie viele Tasks ausgeführt wurden

> [!tip] Tipp
> Der HTML-Testbericht liegt unter `build/reports/tests/test/index.html`. Du kannst ihn im Browser öffnen — er zeigt Testergebnisse übersichtlich mit Laufzeiten und Fehlermeldungen.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach `gradle build` mit `BUILD SUCCESSFUL`:
- `build/classes/java/main/` — kompilierter Produktionscode
- `build/classes/java/test/` — kompilierte Tests
- `build/libs/expedition-tools-1.0-SNAPSHOT.jar` — fertiges JAR
- `build/reports/tests/test/index.html` — Testbericht

</details>

**Zum Nachdenken:** `gradle build` führt auch die Tests aus. Was würde passieren, wenn ein Test fehlschlägt — wird dann trotzdem ein JAR gebaut?

---

## Aufgabe 3 — Task-Abhängigkeiten verstehen 🟡

> Hartwood zeichnet Pfeile zwischen den Etappen auf der Karte. „Dieser Schritt braucht jenen. Jener braucht diesen. Gradle kennt das Netz — und geht immer den kürzesten Weg."

**Deine Aufgabe:**
Lass dir die Task-Abhängigkeiten anzeigen und erkläre, warum `build` so viele Tasks auslöst.

**Was du dafür brauchst:**

Führe aus:
```
gradle build --dry-run
```

`--dry-run` zeigt dir, welche Tasks in welcher Reihenfolge ausgeführt würden — ohne sie tatsächlich auszuführen:

```
:compileJava SKIPPED
:processResources SKIPPED
:classes SKIPPED
:jar SKIPPED
:compileTestJava SKIPPED
:processTestResources SKIPPED
:testClasses SKIPPED
:test SKIPPED
:check SKIPPED
:build SKIPPED
```

Das ist der **Task-Graph** für `build`. Gradle berechnet ihn vor jedem Build automatisch.

**Außerdem:** Sieh dir die Task-Abhängigkeiten für `test` an:
```
gradle test --dry-run
```

> [!tip] Tipp
> Der `--dry-run`-Flag ist nützlich, wenn du verstehen willst, was ein komplexer Build machen würde, bevor du ihn wirklich startest. In großen Projekten kann das den Überblick retten.

<details>
<summary>📜 Lösungsvorschlag</summary>

`gradle test --dry-run` zeigt:
```
:compileJava SKIPPED
:processResources SKIPPED
:classes SKIPPED
:compileTestJava SKIPPED
:processTestResources SKIPPED
:testClasses SKIPPED
:test SKIPPED
```

`test` hängt von `testClasses` ab → `testClasses` hängt von `compileTestJava` ab → `compileTestJava` hängt von `classes` ab → `classes` hängt von `compileJava` ab.

Gradle löst diese Kette automatisch auf.

</details>

**Zum Nachdenken:** Gradle kennt den gesamten Task-Graph und kann Tasks **parallel** ausführen, wenn sie voneinander unabhängig sind. Was könnte das für die Build-Geschwindigkeit bei großen Projekten bedeuten?

---

## Aufgabe 4 — Eigenen Task schreiben 🟡

> Lila legt das Handbuch weg. „Die eingebauten Routen reichen für 90 % der Fälle. Aber manchmal braucht die Expedition eine eigene Abkürzung."

**Deine Aufgabe:**
Schreibe einen einfachen eigenen Task in der `build.gradle` und führe ihn aus.

**Was du dafür brauchst:**

Tasks werden direkt in der `build.gradle` definiert. Die einfachste Form:

```groovy
tasks.register('expeditionBriefing') {
    group = 'expedition'
    description = 'Gibt eine Expeditions-Zusammenfassung aus.'
    doLast {
        println "=== Expedition Briefing ==="
        println "Projekt: ${project.name}"
        println "Version: ${project.version}"
        println "Gruppe:  ${project.group}"
        println "==========================="
    }
}
```

Füge diesen Block ans Ende der `build.gradle` ein und führe aus:
```
gradle expeditionBriefing
```

**Erkläre den Aufbau:**

| Teil | Bedeutung |
|------|-----------|
| `tasks.register('name')` | Registriert einen neuen Task mit diesem Namen |
| `group` | Gruppiert den Task in `gradle tasks` |
| `description` | Kurzbeschreibung, die in `gradle tasks` erscheint |
| `doLast { }` | Der Code, der beim Ausführen läuft — am Ende des Tasks |
| `${project.name}` | Groovy-String-Interpolation — greift auf Projekt-Properties zu |

> [!tip] Tipp 1
> Es gibt auch `doFirst { }` — der Code darin läuft am Anfang des Tasks, vor dem eigentlichen Inhalt. Nützlich, wenn ein Task von einem Plugin stammt und du etwas davor ausführen willst.

> [!tip] Tipp 2
> Mit `gradle tasks --group expedition` siehst du nur die Tasks deiner eigenen Gruppe.

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe von `gradle expeditionBriefing`:
```
> Task :expeditionBriefing
=== Expedition Briefing ===
Projekt: expedition-tools
Version: 1.0-SNAPSHOT
Gruppe:  de.hartwood
===========================

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

</details>

**Zum Nachdenken:** Wozu könnte ein eigener Task in der Praxis nützlich sein? Nenne zwei konkrete Beispiele aus echten Projekten.

---

## Aufgabe 5 — Incremental Build & clean 🔴

> [!example] Expedition ins Unbekannte
> Old Finn beobachtet, wie Gradle beim zweiten Build nichts tut. „Es weiß, dass sich nichts geändert hat. Es arbeitet nicht doppelt." Er klopft auf seinen Kopf. „Das nennt sich Incremental Build. Ist der eigentliche Trick."

**Deine Aufgabe:**
Erkunde Gradles Incremental Build — und lerne, wann `clean` wirklich nötig ist.

**Was du dafür brauchst:**

**Schritt 1 — Ersten Build ausführen:**
```
gradle build
```

**Schritt 2 — Sofort nochmal bauen, ohne Änderungen:**
```
gradle build
```

Die Ausgabe des zweiten Builds sieht so aus:
```
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar UP-TO-DATE
> Task :compileTestJava UP-TO-DATE
> Task :testClasses UP-TO-DATE
> Task :test UP-TO-DATE
> Task :check UP-TO-DATE
> Task :build UP-TO-DATE

BUILD SUCCESSFUL in 0s
9 actionable tasks: 9 up-to-date
```

`UP-TO-DATE` bedeutet: Gradle hat erkannt, dass sich die Eingaben (Quellcode, Dependencies) nicht geändert haben — der Task wird übersprungen. Das macht Gradle deutlich schneller als viele andere Build-Tools.

**Wann ist `gradle clean` nötig?**
```
gradle clean
gradle build
```

`clean` löscht `build/` vollständig. Nötig bei:
- Problemen mit alten `.class`-Dateien nach Umbenennungen
- Unerklärlichem Build-Verhalten
- CI/CD-Pipelines (immer sauberer Build)

> [!info] Weiterführende Quelle
> **Gradle Build Cache** — Wie Gradle Build-Ergebnisse zwischen Projekten und sogar Rechnern teilt.
> 🔗 [docs.gradle.org/current/userguide/build_cache.html](https://docs.gradle.org/current/userguide/build_cache.html)

> [!tip] Tipp
> `gradle clean build` kombiniert beide Befehle — `clean` läuft zuerst, dann `build`. Das ist der Standard-Build in CI/CD-Pipelines.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach `gradle clean`:
- `build/` ist gelöscht

Nach `gradle build`:
- Alle Tasks laufen erneut durch — kein `UP-TO-DATE`

Das ist der Unterschied zum normalen `gradle build` nach einer kleinen Änderung: Nur betroffene Tasks werden neu ausgeführt.

</details>

**Zum Nachdenken:** Incremental Build ist eine der größten Stärken von Gradle. Was müsste Gradle intern wissen und speichern, um zu entscheiden, ob ein Task `UP-TO-DATE` ist?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — `gradle tasks` gelesen, `compileJava`, `test` und `build` ausgeführt, Tasks erklärt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — Task-Abhängigkeiten mit `--dry-run` analysiert, eigenen Task geschrieben und ausgeführt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Incremental Build erklärt, `clean` korrekt eingeordnet, Frage zum Build-Cache beantwortet _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Was ist der Unterschied zwischen einem Task und einer Phase bei anderen Build-Tools — und was macht den Task-Ansatz von Gradle flexibler?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Team baut, testet und paketiert. Im nächsten Modul lernst du Plugins: Wie sie den Task-Graph erweitern, wie du das Application-Plugin für ausführbare Programme nutzt und wie der Gradle Wrapper sicherstellt, dass alle im Team denselben Gradle verwenden.

→ Weiter zu: [[04-plugins|Modul 04: Plugins & Gradle Wrapper]]

---

> _„Halt den Hut fest und die Tasks in Ordnung."_
> — Old Finn McGraw
