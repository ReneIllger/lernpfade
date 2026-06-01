---
title: "Modul 01 — Was ist Gradle & build.gradle"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Gradle
modul-nr: 1
dauer: 45 min
voraussetzungen:
  - keine (erstes Modul der Reihe)
---

# Modul 01 — Was ist Gradle & die build.gradle
_„Das lebende Bauwerk"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | Java-Grundkenntnisse · IntelliJ IDEA installiert · Terminal verfügbar |
| **Lernziel** | Du kannst erklären, wozu ein Build-Tool dient, ein Gradle-Projekt anlegen, die Standardstruktur benennen und die wichtigsten Felder der `build.gradle` lesen und befüllen. |

---

## Worum geht es?

Kairo, Frühjahr 1938. Dr. Vincent Hartwood sitzt im überfüllten Lesesaal der Nationalbibliothek und hält einen Stapel loser Java-Dateien in der Hand. Dreißig Quelldateien. Vier externe Bibliotheken. Zwei Entwickler im Team — und keiner kann dem anderen erklären, wie das Programm gebaut wird. „Du nimmst alle .java-Dateien, kompilierst sie mit javac, dann lädst du die Bibliotheken herunter, fügst sie dem Classpath hinzu, und dann—" Er bricht ab. „Das kann so nicht funktionieren."

Lila McGovern stellt eine Tasse Tee auf den Tisch. „Es gibt Werkzeuge, die das alles übernehmen. Die wissen, wie ein Java-Projekt aufgebaut ist. Die holen die Bibliotheken selbst. Die kompilieren, testen, paketieren — auf Knopfdruck." Sie schiebt einen Zettel rüber. Darauf steht: **Gradle**.

Old Finn McGraw lehnt in der Tür und tippt an seinen Hut. „Moderner als die Konkurrenz. Schneller. Und die Konfiguration liest sich fast wie normaler Code." Er zwinkert. „Fast."

**Deine Mission:** Lerne, wozu ein Build-Tool überhaupt gut ist, und lege dein erstes Gradle-Projekt an.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Gradle installieren & prüfen | 🟢 | ~6 Min. |
| 2 | Erstes Projekt anlegen — Struktur erkunden | 🟢 | ~8 Min. |
| 3 | Die build.gradle lesen und verstehen | 🟡 | ~10 Min. |
| 4 | build.gradle anpassen — Projektinfo ergänzen | 🟡 | ~12 Min. |
| 5 | Projekt ohne IDE bauen | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Gradle installieren & prüfen 🟢

> Old Finn legt einen verwitterten Zettel auf den Tisch. „Erst prüfen, ob das Werkzeug da ist. Dann reden."

**Deine Aufgabe:**
Prüfe, ob Gradle auf deinem Rechner installiert ist. Falls nicht, installiere es.

**Was du dafür brauchst:**

Öffne ein Terminal und gib ein:
```
gradle -version
```

Eine erfolgreiche Ausgabe sieht so aus:
```
------------------------------------------------------------
Gradle 8.x
------------------------------------------------------------
Build time:   ...
JVM:          17.x (...)
OS:           ...
```

Falls der Befehl nicht gefunden wird:
- **macOS (mit Homebrew):** `brew install gradle`
- **Windows (mit Scoop):** `scoop install gradle`
- **Manuell:** Auf [gradle.org/releases](https://gradle.org/releases) herunterladen, entpacken, `bin/` zur PATH-Variable hinzufügen
- **IntelliJ:** Gradle ist eingebaut — du kannst das integrierte Gradle nutzen

> [!tip] Tipp
> Für echte Projekte nutzt man meist den **Gradle Wrapper** (`./gradlew`) statt einer global installierten Version — das ist Thema von Modul 04. Für diese Einheit reicht eine globale Installation.

<details>
<summary>📜 Lösungsvorschlag</summary>

Eine erfolgreiche Ausgabe von `gradle -version` bedeutet: Gradle ist einsatzbereit.
Ab Version 7.x funktionieren alle Befehle dieser Modulreihe.

</details>

**Zum Nachdenken:** Gradle kann global installiert oder projekt-spezifisch (via Wrapper) genutzt werden. Was könnte das Problem sein, wenn alle im Team unterschiedliche Gradle-Versionen global installiert haben?

---

## Aufgabe 2 — Erstes Projekt anlegen — Struktur erkunden 🟢

> Lila schlägt ihr Notizbuch auf. „Jedes Lager hat dieselbe Grundstruktur. Zelt, Lager, Archiv. Bei Gradle ist das genauso — eine Struktur, die jeder kennt."

**Deine Aufgabe:**
Lege ein neues Gradle-Projekt in IntelliJ an und erkunde die Verzeichnisstruktur.

**Was du dafür brauchst:**

**In IntelliJ:**
1. `File → New → Project`
2. Links: „New Project" — Build-System: **Gradle** — Language: **Java** — Gradle DSL: **Groovy**
3. Name: `expedition-tools`, GroupId: `de.hartwood`, ArtifactId: `expedition-tools`
4. JDK: 17 (oder höher) → „Create"

Gradle legt automatisch folgende Struktur an:

```
expedition-tools/
├── build.gradle          ← Konfigurationsdatei des Projekts
├── settings.gradle       ← Projektname und Subprojekte
├── gradlew               ← Gradle Wrapper (Linux/Mac)
├── gradlew.bat           ← Gradle Wrapper (Windows)
├── gradle/
│   └── wrapper/
│       └── gradle-wrapper.properties
└── src/
    ├── main/
    │   ├── java/         ← dein Produktionscode
    │   └── resources/    ← Konfigurationsdateien, Bilder, etc.
    └── test/
        ├── java/         ← deine Tests
        └── resources/
```

**Erkunde:** Öffne `settings.gradle` — was steht dort? Öffne dann `build.gradle` und lies den Inhalt.

> [!tip] Tipp
> Die Trennung von `main/java` und `test/java` ist bewusst: Gradle weiß, was Produktionscode ist und was nur für Tests gebraucht wird. Testcode landet nie im fertigen Programm.

<details>
<summary>📜 Lösungsvorschlag</summary>

`settings.gradle` enthält nur eine Zeile:
```groovy
rootProject.name = 'expedition-tools'
```

Das ist der Projektname — wichtig, wenn Gradle mehrere Module verwaltet.

</details>

**Zum Nachdenken:** Gradle legt sowohl `build.gradle` als auch `settings.gradle` an. Warum braucht es zwei Dateien statt einer? Was könnte der Zweck der Trennung sein?

---

## Aufgabe 3 — Die build.gradle lesen und verstehen 🟡

> Hartwood nimmt die `build.gradle` in die Hand und tippt auf die einzelnen Abschnitte. „Drei Blöcke. Jeder hat seinen Zweck. Wer das versteht, versteht Gradle."

**Deine Aufgabe:**
Öffne die `build.gradle` und identifiziere die wichtigsten Bereiche. Erkläre jeden Block in eigenen Worten.

**Was du dafür brauchst:**

Eine frisch angelegte `build.gradle` für ein Java-Projekt sieht ungefähr so aus:

```groovy
plugins {
    id 'java'
}

group = 'de.hartwood'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'
}

test {
    useJUnitPlatform()
}
```

**Die wichtigsten Bereiche:**

| Block | Bedeutung |
|-------|-----------|
| `plugins { }` | Welche Gradle-Plugins aktiv sind — `java` bringt compile, test, jar mit |
| `group` / `version` | Koordinaten des Projekts — wie Name und Version |
| `repositories { }` | Wo Gradle nach Bibliotheken sucht — hier: Maven Central |
| `dependencies { }` | Welche externen Bibliotheken gebraucht werden |
| `test { }` | Konfiguration für den Test-Task — hier: JUnit 5 aktivieren |

> [!tip] Tipp 1
> `group` und `version` sind die Koordinaten, die dein Projekt identifizieren — ähnlich wie ein Name und eine Versionsnummer auf einem Paket.

> [!tip] Tipp 2
> `mavenCentral()` ist das zentrale Repository für Java-Bibliotheken. Gradle sucht dort automatisch nach allen eingetragenen Dependencies.

<details>
<summary>📜 Lösungsvorschlag</summary>

- `plugins { id 'java' }` → aktiviert alle Java-Build-Fähigkeiten
- `group = 'de.hartwood'` → Organisationspräfix (wie ein Nachname)
- `version = '1.0-SNAPSHOT'` → aktuelle Version, `SNAPSHOT` = in Entwicklung
- `repositories { mavenCentral() }` → Gradle sucht Bibliotheken auf Maven Central
- `dependencies { ... }` → JUnit 5 als Test-Bibliothek eingebunden
- `test { useJUnitPlatform() }` → sagt Gradle, dass Tests mit JUnit 5 laufen

</details>

**Zum Nachdenken:** Die `build.gradle` sieht aus wie Code — und ist auch welcher (Groovy). Was könnte das für Möglichkeiten eröffnen, die eine reine XML-Konfiguration nicht hätte?

---

## Aufgabe 4 — build.gradle anpassen 🟡

> Lila schreibt in ihr Notizbuch. „Ein gutes Dokument beschreibt sich selbst. Beschreibung, Zweck, Version — alles sollte aus der Akte hervorgehen."

**Deine Aufgabe:**
Ergänze die `build.gradle` um eine Beschreibung und stelle sicher, dass die Java-Version korrekt gesetzt ist.

**Was du dafür brauchst:**

Füge folgende Zeilen in die `build.gradle` ein — direkt nach `version`:

```groovy
description = 'Hilfsprogramme für Hartwoods Expeditionsteam'
```

Und für die Java-Version — füge diesen Block hinzu:

```groovy
java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}
```

**Oder moderner** (ab Java 9 und Gradle 6+):
```groovy
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}
```

Der Toolchain-Ansatz ist präziser: Gradle sucht automatisch nach einem passenden JDK oder lädt es herunter.

> [!tip] Tipp
> `sourceCompatibility` und `targetCompatibility` sind der ältere Weg. Das `toolchain`-Block ist der moderne Ansatz und funktioniert auch, wenn verschiedene Entwickler unterschiedliche JDK-Versionen installiert haben.

<details>
<summary>📜 Lösungsvorschlag</summary>

```groovy
plugins {
    id 'java'
}

group = 'de.hartwood'
version = '1.0-SNAPSHOT'
description = 'Hilfsprogramme für Hartwoods Expeditionsteam'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'
}

test {
    useJUnitPlatform()
}
```

</details>

**Zum Nachdenken:** Was ist der Vorteil der `toolchain`-Konfiguration gegenüber `sourceCompatibility`? Stell dir vor, dein Team hat Mitglieder mit Java 17, Java 21 und Java 11 — was würde jeweils passieren?

---

## Aufgabe 5 — Projekt ohne IDE bauen 🔴

> [!example] Expedition ins Unbekannte
> Old Finn zieht seinen Mantel an. „IntelliJ ist bequem. Aber der Server in der Wüste hat kein IntelliJ. Nur ein Terminal. Und Gradle."

**Deine Aufgabe:**
Lege eine kleine Java-Klasse an und baue das Projekt ausschließlich über das Terminal.

**Was du dafür brauchst:**

1. Erstelle das Verzeichnis `src/main/java/de/hartwood/` und lege dort `Main.java` an:

```java
package de.hartwood;

public class Main {
    public static void main(String[] args) {
        System.out.println("Expedition bereit.");
    }
}
```

2. Öffne das Terminal im Projektverzeichnis und führe aus:
```
gradle compileJava
```

Die kompilierten `.class`-Dateien landen in `build/classes/java/main/`.

> [!info] `build/` statt `target/`
> Gradle nutzt `build/` als Ausgabeordner — nicht `target/` wie andere Build-Tools. Alle generierten Dateien (kompilierter Code, JARs, Testberichte) landen dort.

> [!tip] Tipp
> Falls du den Gradle Wrapper nutzen willst (empfohlen): Ersetze `gradle` durch `./gradlew` (Linux/Mac) oder `gradlew.bat` (Windows). Der Wrapper stellt sicher, dass die richtige Gradle-Version verwendet wird.

<details>
<summary>📜 Lösungsvorschlag</summary>

```
$ gradle compileJava

> Task :compileJava

BUILD SUCCESSFUL in 2s
1 actionable task: 1 executed
```

Die kompilierte Klasse liegt jetzt unter:
`build/classes/java/main/de/hartwood/Main.class`

</details>

**Zum Nachdenken:** Gradle legt `build/` an — ähnlich wie andere Build-Tools ihren eigenen Ausgabeordner haben. Würdest du diesen Ordner in Git einchecken? Warum (nicht)?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Gradle-Projekt angelegt, Standardstruktur erklärt, `build.gradle`-Blöcke benannt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — `build.gradle` angepasst, Java-Version konfiguriert, Unterschied `sourceCompatibility` vs. Toolchain erklärt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Projekt per Terminal kompiliert, `build/`-Struktur erkundet, Wrapper erklärt _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum braucht ein Java-Projekt überhaupt ein Build-Tool — was würde ohne es passieren, wenn das Projekt wächst?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Fundament steht. Im nächsten Modul lernst du, wie Gradle externe Bibliotheken verwaltet — wo sie herkommen, wie du sie einbindest und was „Configurations" bedeutet.

→ Weiter zu: [[02-dependencies|Modul 02: Dependencies verwalten]]

---

> _„Halt den Hut fest und die build.gradle sauber."_
> — Old Finn McGraw
