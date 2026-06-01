---
title: "Modul 04 — Plugins & Gradle Wrapper"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Gradle
modul-nr: 4
dauer: 45 min
voraussetzungen:
  - "[[03-tasks|Modul 03: Tasks & Build Lifecycle]]"
---

# Modul 04 — Plugins & Gradle Wrapper
_„Die Ausrüstung der Profis"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[03-tasks\|Modul 03]] abgeschlossen · Projekt mit Tests und eigenem Task vorhanden |
| **Lernziel** | Du kannst erklären, was Gradle Plugins sind und wie sie den Build erweitern, das Application-Plugin für ausführbare Programme einsetzen und den Gradle Wrapper korrekt nutzen und erklären. |

---

## Worum geht es?

Istanbul, ein Spezialausrüster für Expeditionen. Hartwood betritt das Lager und sieht Regale bis zur Decke. „Ich dachte, wir haben alles," sagt er. Lila schüttelt den Kopf. „Das ist das Basispaket. Für echte Expeditionen braucht man mehr." Sie zieht ein Katalogbuch hervor. „Plugins. Erweiterungen für das Basiswerkzeug."

Old Finn greift nach einem kleinen Gerät im Regal. „Das hier ist das Application-Plugin. Damit wird aus deinem Code ein echtes Programm — mit Start-Skript, fertig zum Ausführen, ohne dass jemand wissen muss, wie Java funktioniert." Er legt es auf den Tisch. „Und dann ist da noch der Wrapper." Er zieht einen dünnen Umschlag hervor. „Der sorgt dafür, dass jeder im Team dieselbe Gradle-Version nutzt. Automatisch. Ohne Installation."

Lila nickt. „Plugins sind der Grund, warum Gradle so flexibel ist. Der Kern kann fast nichts — aber mit den richtigen Plugins kann er alles."

**Deine Mission:** Lerne, wie Plugins den Build erweitern, baue ein ausführbares Programm mit dem Application-Plugin und richte den Gradle Wrapper ein.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Plugins verstehen — was sie sind und wie sie funktionieren | 🟢 | ~7 Min. |
| 2 | Application-Plugin einrichten | 🟢 | ~8 Min. |
| 3 | Distribution bauen und ausführen | 🟡 | ~10 Min. |
| 4 | Gradle Wrapper einrichten und nutzen | 🟡 | ~11 Min. |
| 5 | Plugin aus dem Gradle Plugin Portal einbinden | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Plugins verstehen 🟢

> Lila blättert durch den Katalog. „Jedes Plugin fügt neue Tasks hinzu. Und neue Konfigurationsmöglichkeiten. Du sagst, was du brauchst — das Plugin weiß, wie es geht."

**Deine Aufgabe:**
Lies die Erklärung zu Gradle Plugins und ordne die Beispiele den richtigen Kategorien zu.

**Was du dafür brauchst:**

Gradle selbst kann zunächst wenig — es ist ein Framework. Plugins fügen die eigentliche Funktionalität hinzu.

**Arten von Plugins:**

| Typ | Herkunft | Beispiel |
|-----|----------|---------|
| Core Plugins | Gradle mitgeliefert | `java`, `application`, `war` |
| Community Plugins | Gradle Plugin Portal | `com.github.johnrengelman.shadow` |
| Lokale Plugins | Eigenes Projekt | Eigene Build-Logik |

**Core Plugins** werden nur mit ihrer ID eingebunden:
```groovy
plugins {
    id 'java'
    id 'application'
}
```

**Community Plugins** brauchen zusätzlich eine Version:
```groovy
plugins {
    id 'com.github.johnrengelman.shadow' version '8.1.1'
}
```

**Was ein Plugin mitbringt:**
- Neue **Tasks** (z.B. `jar`, `run`, `installDist`)
- Neue **Configurations** (z.B. `implementation`, `runtimeOnly`)
- Neue **Konfigurationsblöcke** in `build.gradle` (z.B. `application { }`, `java { }`)

> [!tip] Tipp
> Alle Community-Plugins findest du auf dem Gradle Plugin Portal: [plugins.gradle.org](https://plugins.gradle.org)

<details>
<summary>📜 Lösungsvorschlag</summary>

Das `java`-Plugin bringt u.a. mit:
- Tasks: `compileJava`, `test`, `jar`, `build`
- Configurations: `implementation`, `testImplementation`, `runtimeOnly`
- Konfigurationsblöcke: `java { }`, `test { }`

Das `application`-Plugin erweitert `java` um:
- Tasks: `run`, `installDist`, `distZip`, `distTar`
- Konfigurationsblock: `application { }`

</details>

**Zum Nachdenken:** Das `java`-Plugin war von Anfang an in deiner `build.gradle`. Was hätte ohne es nicht funktioniert?

---

## Aufgabe 2 — Application-Plugin einrichten 🟢

> Old Finn hält das Application-Plugin hoch. „Das macht aus Code ein Programm. Echtes Programm. Mit Start-Skript. Jeder kann es ausführen — auch ohne Gradle, ohne IntelliJ, ohne irgendetwas."

**Deine Aufgabe:**
Ergänze das `application`-Plugin in deiner `build.gradle` und konfiguriere die Hauptklasse.

**Was du dafür brauchst:**

Ändere den `plugins`-Block:
```groovy
plugins {
    id 'java'
    id 'application'
}
```

Füge dann den `application`-Konfigurationsblock hinzu:
```groovy
application {
    mainClass = 'de.hartwood.Main'
}
```

Stelle sicher, dass `Main.java` unter `src/main/java/de/hartwood/` existiert und eine `main()`-Methode hat:

```java
package de.hartwood;

public class Main {
    public static void main(String[] args) {
        System.out.println("Expedition bereit.");
    }
}
```

Dann: Starte das Programm direkt mit Gradle:
```
gradle run
```

> [!tip] Tipp
> `gradle run` kompiliert automatisch, falls nötig — du musst nicht separat `compileJava` aufrufen.

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe von `gradle run`:
```
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :run
Expedition bereit.

BUILD SUCCESSFUL in 1s
2 actionable tasks: 1 executed, 1 up-to-date
```

Das Programm läuft direkt aus dem Build heraus.

</details>

**Zum Nachdenken:** `gradle run` startet das Programm direkt aus dem Build — nicht aus einem JAR. Was ist der Unterschied? Wann wäre `gradle run` nützlich, wann bräuchte man das JAR?

---

## Aufgabe 3 — Distribution bauen und ausführen 🟡

> Lila rollt die fertige Karte auf. „Das Programm läuft auf deinem Rechner. Aber die Crews in Luxor und am Sinai haben kein Gradle. Wir brauchen ein Paket, das überall läuft."

**Deine Aufgabe:**
Baue eine fertige Distribution — ein Paket mit Start-Skript und allen Bibliotheken.

**Was du dafür brauchst:**

Das `application`-Plugin fügt den Task `installDist` hinzu — er baut eine vollständige Distribution im `build/install/`-Verzeichnis:

```
gradle installDist
```

Danach liegt in `build/install/expedition-tools/` folgende Struktur:
```
expedition-tools/
├── bin/
│   ├── expedition-tools        ← Start-Skript für Linux/Mac
│   └── expedition-tools.bat    ← Start-Skript für Windows
└── lib/
    ├── expedition-tools-1.0-SNAPSHOT.jar
    └── gson-2.10.1.jar         ← alle Dependencies dabei
```

Starte das Programm über das Skript:
```
./build/install/expedition-tools/bin/expedition-tools
```

**Alternativen:**
- `gradle distZip` — packt die Distribution als ZIP
- `gradle distTar` — packt die Distribution als TAR

> [!tip] Tipp 1
> Die Distribution ist vollständig — sie enthält dein JAR und alle Dependencies. Wer sie auf einem anderen Rechner ausführen will, braucht nur ein JRE (Java Runtime), nicht Gradle.

> [!tip] Tipp 2
> Unter Windows: Das `.bat`-Skript nutzen, unter Linux/Mac: Das Skript ohne Endung. Du musst es ggf. erst ausführbar machen: `chmod +x build/install/expedition-tools/bin/expedition-tools`

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe beim Ausführen des Start-Skripts:
```
Expedition bereit.
```

Die Distribution enthält alle nötigen JARs — sie ist ohne Gradle ausführbar.

</details>

**Zum Nachdenken:** Die Distribution enthält `gson-2.10.1.jar` separat — kein Fat JAR. Was sind Vor- und Nachteile dieses Ansatzes verglichen mit einem einzigen JAR, das alles enthält?

---

## Aufgabe 4 — Gradle Wrapper einrichten und nutzen 🟡

> Old Finn legt den Umschlag auf den Tisch. „Der Wrapper. Damit läuft Gradle auf jedem Rechner in exakt derselben Version — ohne dass jemand Gradle installieren muss." Er tippt auf die Schachtel. „Das ist die eigentliche Professionalisierung."

**Deine Aufgabe:**
Verstehe, was der Gradle Wrapper ist, und nutze ihn statt des global installierten Gradle.

**Was du dafür brauchst:**

Der **Gradle Wrapper** ist ein kleines Skript (`gradlew` / `gradlew.bat`), das Gradle in einer festen Version herunterlädt und ausführt — unabhängig davon, was (oder ob überhaupt etwas) global installiert ist.

IntelliJ hat den Wrapper schon angelegt. Sieh dir `gradle/wrapper/gradle-wrapper.properties` an:

```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.x-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

`distributionUrl` legt die exakte Gradle-Version fest.

**Ab jetzt: Wrapper statt `gradle`:**
```
# statt: gradle build
./gradlew build       # Linux/Mac
gradlew.bat build     # Windows
```

Beim ersten Aufruf lädt `./gradlew` die angegebene Gradle-Version automatisch herunter.

**Wrapper-Version aktualisieren:**
```
gradle wrapper --gradle-version 8.7
```

> [!info] Warum Wrapper in Git?
> `gradlew`, `gradlew.bat` und `gradle/wrapper/` gehören ins Git-Repository. So hat jeder, der das Projekt auscheckt, sofort die richtige Gradle-Version — ohne manuelle Installation.

> [!tip] Tipp
> In CI/CD-Pipelines (GitHub Actions, Jenkins) nutzt man immer `./gradlew` — nicht `gradle`. So ist garantiert, dass die vom Projekt festgelegte Version verwendet wird.

<details>
<summary>📜 Lösungsvorschlag</summary>

Beim ersten `./gradlew build` siehst du:
```
Downloading https://services.gradle.org/distributions/gradle-8.x-bin.zip
...
BUILD SUCCESSFUL
```

Danach wird die heruntergeladene Version gecacht und wiederverwendet.

</details>

**Zum Nachdenken:** Ein Team-Mitglied hat Gradle 7.6 global installiert, du hast Gradle 8.4. Was passiert, wenn ihr beide `gradle build` ausführt — und was passiert, wenn ihr beide `./gradlew build` ausführt?

---

## Aufgabe 5 — Plugin aus dem Gradle Plugin Portal einbinden 🔴

> [!example] Expedition ins Unbekannte
> Hartwood schlägt das Katalogbuch auf. „Was das eingebaute Werkzeug nicht kann, liefert das Plugin Portal. Tausende Plugins, von der Community gebaut." Er blättert. „Zum Beispiel: Ein Fat JAR bauen, das alle Dependencies enthält. Dafür gibt es das Shadow Plugin."

**Deine Aufgabe:**
Binde das Shadow Plugin aus dem Gradle Plugin Portal ein und baue damit ein ausführbares Fat JAR.

**Was du dafür brauchst:**

Das Shadow Plugin erstellt ein JAR, das alle Dependencies enthält — nützlich, wenn eine einfache, portable Executable gebraucht wird.

Füge das Plugin im `plugins`-Block hinzu:
```groovy
plugins {
    id 'java'
    id 'application'
    id 'com.github.johnrengelman.shadow' version '8.1.1'
}
```

Danach steht der neue Task `shadowJar` zur Verfügung:
```
./gradlew shadowJar
```

Das Fat JAR landet in `build/libs/expedition-tools-1.0-SNAPSHOT-all.jar`.

Starte es direkt:
```
java -jar build/libs/expedition-tools-1.0-SNAPSHOT-all.jar
```

> [!info] Weiterführende Quelle
> **Shadow Plugin** — Vollständige Dokumentation mit Konfigurationsmöglichkeiten.
> 🔗 [imperceptiblethoughts.com/shadow](https://imperceptiblethoughts.com/shadow/)

> [!tip] Tipp
> Das Fat JAR enthält alle Dependencies direkt — kein separates `lib/`-Verzeichnis nötig. Ideal für Microservices, CLI-Tools oder einfaches Deployment.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach `./gradlew shadowJar`:
```
$ java -jar build/libs/expedition-tools-1.0-SNAPSHOT-all.jar
Expedition bereit.
```

Vergleich der JAR-Größen:
- `expedition-tools-1.0-SNAPSHOT.jar` — wenige KB (nur dein Code)
- `expedition-tools-1.0-SNAPSHOT-all.jar` — einige hundert KB (dein Code + Gson + JUnit-Runtime)

</details>

**Zum Nachdenken:** Shadow (Fat JAR) und `installDist` (Distribution mit separaten JARs) lösen dasselbe Problem auf unterschiedliche Weise. Welchen Ansatz würdest du wählen für: (a) ein kleines CLI-Tool, (b) einen Server mit vielen Microservices?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Plugin-Konzept erklärt, Application-Plugin eingerichtet, `gradle run` erfolgreich _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — Distribution gebaut und ausgeführt, Gradle Wrapper erklärt und genutzt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Community-Plugin eingebunden, Fat JAR gebaut, Unterschied zu Distribution erklärt _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum gehört der Gradle Wrapper ins Git-Repository — und was wäre das Problem, wenn jedes Team-Mitglied stattdessen seine eigene Gradle-Version nutzt?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Modulreihe abgeschlossen

Das Expeditionsteam ist ausgerüstet. Du kannst:

- ✅ Gradle-Projekte anlegen und die `build.gradle` lesen und anpassen
- ✅ Dependencies aus Maven Central einbinden und Configurations korrekt einsetzen
- ✅ Tasks erklären, aufrufen, analysieren und eigene schreiben
- ✅ Plugins einsetzen — vom Application-Plugin bis zu Community-Plugins
- ✅ Den Gradle Wrapper nutzen und erklären

**Mögliche nächste Schritte (auf eigene Faust):**
- **Kotlin DSL:** `build.gradle.kts` statt `build.gradle` — typsicher und IDE-freundlicher
- **Multi-Project Builds:** Mehrere Subprojekte in einem gemeinsamen Build
- **Build Scans:** `./gradlew build --scan` erzeugt einen interaktiven Online-Build-Report
- **Gradle in CI/CD:** Wie `./gradlew clean build` in GitHub Actions läuft

→ Zurück zur [[00-uebersicht|Modulreihe: Gradle]]

---

> _„Halt den Hut fest und den Wrapper im Repository."_
> — Old Finn McGraw
