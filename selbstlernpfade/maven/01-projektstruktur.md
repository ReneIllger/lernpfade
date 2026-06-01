---
title: "Modul 01 — Was ist Maven & die pom.xml"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Maven
modul-nr: 1
dauer: 45 min
voraussetzungen:
  - keine (erstes Modul der Reihe)
---

# Modul 01 — Was ist Maven & die pom.xml
_„Das Archiv des Wüstenwinds"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | Java-Grundkenntnisse · IntelliJ IDEA installiert · Terminal verfügbar |
| **Lernziel** | Du kannst ein Maven-Projekt anlegen, die Standardstruktur erklären und die wichtigsten Felder der pom.xml benennen und befüllen. |

---

## Worum geht es?

Kairo, Frühjahr 1938. Dr. Vincent Hartwood breitet in seinem Büro im British Museum Reading Room einen Stapel Grabungsberichte aus. Dreizehn Teams in fünf Ländern — und jedes Team hat seinen Quellcode anders organisiert. Lila McGovern, seine Assistentin, schiebt ihm schweigend einen Bericht hin: „Jedes Mal, wenn wir den Code eines anderen Teams übernehmen wollen, verbringen wir drei Tage damit, die Struktur zu verstehen. Drei Tage."

Old Finn McGraw hängt mit dem Rücken an der Wand und trimmt sein Schnurrbart. „Es gibt ein Werkzeug", sagt er ohne aufzublicken. „Eines, das die gesamte Welt der Java-Entwickler auf dasselbe Fundament stellt. Maven heißt es. Ich hab's in Amsterdam bei einem niederländischen Archivaren gesehen." Er klappt sein Messer zu. „Alle Projekte sehen gleich aus. Alle Abhängigkeiten stehen in einer einzigen Datei. Und der Build läuft überall gleich — in Kairo wie in Amsterdam."

**Deine Mission:** Lerne, wie Maven Projekte aufbaut, und lege dein erstes Maven-Projekt an. Das wird das Fundament für alles sein, was Hartwoods Expeditionsteam in den nächsten Wochen braucht.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Maven installieren & prüfen | 🟢 | ~6 Min. |
| 2 | Erstes Projekt anlegen — Standardstruktur erkunden | 🟢 | ~8 Min. |
| 3 | Die pom.xml lesen und verstehen | 🟡 | ~10 Min. |
| 4 | pom.xml anpassen — Projekt beschreiben | 🟡 | ~12 Min. |
| 5 | Projekt ohne IDE bauen | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Maven installieren & prüfen 🟢

> Old Finn legt einen verwitterten Zettel auf den Tisch. Darauf: eine kurze Liste. „Erst prüfen, ob das Werkzeug da ist. Dann arbeiten. Immer in dieser Reihenfolge."

**Deine Aufgabe:**
Prüfe, ob Maven auf deinem Rechner installiert ist. Falls nicht, installiere es.

**Was du dafür brauchst:**

Öffne ein Terminal und gib ein:
```
mvn -version
```

Wenn Maven installiert ist, siehst du eine Ausgabe wie:
```
Apache Maven 3.9.x
Maven home: /usr/share/maven
Java version: 17.x.x
```

Falls der Befehl nicht gefunden wird:
- **Windows:** Maven von [maven.apache.org/download.cgi](https://maven.apache.org/download.cgi) herunterladen, entpacken, `bin/`-Verzeichnis zur PATH-Variable hinzufügen
- **macOS (mit Homebrew):** `brew install maven`
- **IntelliJ:** Hat Maven eingebaut — du kannst das integrierte Maven nutzen (Menü: `View → Tool Windows → Maven`)

> [!tip] Tipp
> In IntelliJ findest du ein Terminal direkt im IDE-Fenster unten (Reiter „Terminal"). Dort funktionieren alle Maven-Befehle genauso wie im externen Terminal.

<details>
<summary>📜 Lösungsvorschlag</summary>

Eine erfolgreiche Ausgabe von `mvn -version` bedeutet: Maven ist einsatzbereit.
Falls die Version älter als 3.6 ist — das ist für diese Modulreihe kein Problem.

</details>

**Zum Nachdenken:** Maven ist ein externes Tool, das du separat installierst — aber IntelliJ bringt Maven auch eingebaut mit. Was könnte der Unterschied sein? In welcher Situation wäre das integrierte Maven praktisch, in welcher das installierte besser?

---

## Aufgabe 2 — Erstes Projekt anlegen — Standardstruktur erkunden 🟢

> Lila schlägt ein zerlesenes Notizbuch auf. „Jedes Grabungscamp hat dieselbe Grundstruktur: Koordinatenzelt, Lagerhalle, Dokumentationsraum. Wenn du das einmal weißt, findest du dich in jedem Camp zurecht." Sie zeigt auf die Seite. „Bei Maven ist das nicht anders."

**Deine Aufgabe:**
Lege ein neues Maven-Projekt in IntelliJ an und erkunde die Verzeichnisstruktur.

**Was du dafür brauchst:**

**In IntelliJ:**
1. `File → New → Project`
2. Links: „New Project" — Build-System: **Maven** — Language: **Java**
3. Name: `expedition-tools`, GroupId: `de.hartwood`, ArtifactId: `expedition-tools`
4. JDK: 17 (oder höher) → „Create"

Maven legt automatisch folgende Struktur an:

```
expedition-tools/
├── pom.xml                    ← Konfigurationsdatei des Projekts
└── src/
    ├── main/
    │   └── java/              ← dein Produktionscode
    └── test/
        └── java/              ← deine Tests
```

**Erkunde die Struktur:** Öffne den Projektbaum in IntelliJ und klicke durch die Verzeichnisse. Wo würdest du eine neue Klasse anlegen? Wo kämen Tests hin?

> [!tip] Tipp
> Die Trennung von `main/java` und `test/java` ist kein Zufall — Maven weiß genau, welcher Code zum Produktionscode gehört und welcher nur für Tests gebraucht wird. Testcode landet nie im fertigen Programm.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach dem Anlegen sollte dein Projektbaum in IntelliJ so aussehen:

```
expedition-tools
├── src
│   ├── main
│   │   └── java          ← neue Klassen hier anlegen
│   └── test
│       └── java          ← Testklassen hier anlegen
└── pom.xml
```

Falls IntelliJ noch ein `src/main/resources`-Verzeichnis anlegt: Das ist für Konfigurationsdateien, Bilder usw. — nicht für Java-Klassen.

</details>

**Zum Nachdenken:** Warum legt Maven Test-Code und Produktionscode in verschiedene Verzeichnisse? Was wäre das Problem, wenn alles in einem gemeinsamen `src/`-Ordner wäre?

---

## Aufgabe 3 — Die pom.xml lesen und verstehen 🟡

> Hartwood nimmt die erste Seite des Expeditionsplans und streicht mit dem Finger über die Kopfzeile. „Name. Herkunft. Version. Jedes Dokument beginnt damit. Die Maven-Leute nennen das Koordinaten — wie auf einer Karte."

**Deine Aufgabe:**
Öffne die `pom.xml` deines Projekts und identifiziere die wichtigsten Bereiche.

**Was du dafür brauchst:**

Eine frisch angelegte `pom.xml` sieht ungefähr so aus:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>de.hartwood</groupId>
    <artifactId>expedition-tools</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```

**Die wichtigsten Felder:**

| Feld | Bedeutung | Beispiel |
|------|-----------|---------|
| `groupId` | Organisation oder Paketpräfix — wie ein Nachname | `de.hartwood` |
| `artifactId` | Name des Projekts / der Bibliothek — wie ein Vorname | `expedition-tools` |
| `version` | Aktuelle Version — `SNAPSHOT` = in Entwicklung | `1.0-SNAPSHOT` |
| `properties` | Wiederverwendbare Einstellungen, z.B. Java-Version | `<maven.compiler.source>17` |

Die Kombination `groupId:artifactId:version` nennt man **Maven-Koordinaten** — sie identifizieren ein Projekt weltweit eindeutig.

> [!tip] Tipp 1
> `SNAPSHOT` in der Version bedeutet: Diese Version ist noch in Entwicklung, nicht fertig veröffentlicht. Fertige Releases haben Versionen wie `1.0`, `2.3.1` usw.

> [!tip] Tipp 2
> `groupId` folgt üblicherweise der umgekehrten Domain-Schreibweise: Eine Firma mit Domain `hartwood.de` würde `de.hartwood` als GroupId verwenden.

<details>
<summary>📜 Lösungsvorschlag</summary>

In der `pom.xml` des Projekts `expedition-tools`:
- `groupId` = `de.hartwood` (die Organisation)
- `artifactId` = `expedition-tools` (der Projektname)
- `version` = `1.0-SNAPSHOT` (in Entwicklung)
- `properties` definiert die Java-Version für den Compiler

</details>

**Zum Nachdenken:** Warum gibt es sowohl `groupId` als auch `artifactId`? Könnte ein einziger Name nicht ausreichen?

---

## Aufgabe 4 — pom.xml anpassen — Projekt beschreiben 🟡

> Lila klappt ihr Notizbuch auf. „Eine gute Akte hat mehr als nur einen Namen. Sie hat eine Beschreibung, eine Internetadresse, einen Verwendungszweck." Sie deutet auf die pom.xml. „Maven kann all das aufnehmen."

**Deine Aufgabe:**
Erweitere die `pom.xml` um eine Beschreibung, eine URL und eine Java-Version deiner Wahl. Stelle außerdem sicher, dass der Compiler die richtige Java-Version verwendet.

**Was du dafür brauchst:**

Du kannst die `pom.xml` um optionale, aber nützliche Felder erweitern:

```xml
<name>Expedition Tools</name>
<description>Hilfsprogramme für Hartwoods Expeditionsteam</description>
<url>https://github.com/hartwood/expedition-tools</url>
```

Diese Felder haben keine Auswirkung auf den Build, werden aber z.B. bei der Veröffentlichung einer Bibliothek auf Maven Central verwendet.

**Außerdem:** Stelle sicher, dass `maven.compiler.source` und `maven.compiler.target` auf **17** (oder deine installierte Java-Version) gesetzt sind.

> [!tip] Tipp
> Ab Maven 3.9 und Java 9+ kannst du statt `source` und `target` auch `<maven.compiler.release>17</maven.compiler.release>` verwenden — das ist kürzer und präziser.

<details>
<summary>📜 Lösungsvorschlag</summary>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>de.hartwood</groupId>
    <artifactId>expedition-tools</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>Expedition Tools</name>
    <description>Hilfsprogramme für Hartwoods Expeditionsteam</description>

    <properties>
        <maven.compiler.release>17</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```

</details>

**Zum Nachdenken:** Für ein kleines privates Projekt wirkt `name` und `description` in der pom.xml übertrieben. Wann würde es sinnvoll werden, diese Felder sorgfältig zu befüllen?

---

## Aufgabe 5 — Projekt ohne IDE bauen 🔴

> [!example] Expedition ins Unbekannte
> Old Finn zieht seinen Mantel an und öffnet die Tür. „IntelliJ ist ein schönes Werkzeug. Aber was machst du, wenn kein IntelliJ da ist? Auf dem Schiff nach Alexandria? Im Camp ohne Strom?" Er wirft dir einen Schlüssel zu. „Du nimmst das Terminal. Und Maven."

**Deine Aufgabe:**
Baue dein Projekt ausschließlich über das Terminal, ohne IntelliJ zu benutzen. Lege dazu eine kleine Java-Klasse an und kompiliere das Projekt per Befehl.

**Was du dafür brauchst:**

1. Lege unter `src/main/java/de/hartwood/` eine Datei `Main.java` mit folgendem Inhalt an:
```java
package de.hartwood;

public class Main {
    public static void main(String[] args) {
        System.out.println("Expedition bereit.");
    }
}
```

2. Öffne das Terminal und navigiere in das Projektverzeichnis (wo die `pom.xml` liegt).

3. Führe aus:
```
mvn compile
```

Maven kompiliert jetzt deinen Code. Die `.class`-Dateien landen in `target/classes/`.

> [!info] Weiterführende Quelle
> **Maven in 5 Minuten** — Die offizielle Kurzeinführung, sehr kompakt.
> 🔗 [maven.apache.org/guides/getting-started/maven-in-five-minutes.html](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)

> [!tip] Tipp
> Wenn `mvn compile` fehlschlägt mit „Source option 5 is no longer supported" — dann ist die Java-Version in der pom.xml zu alt. Stelle `maven.compiler.release` auf 17.

<details>
<summary>📜 Lösungsvorschlag</summary>

```
$ cd expedition-tools
$ mvn compile
[INFO] Scanning for projects...
[INFO] Building expedition-tools 1.0-SNAPSHOT
[INFO] --- maven-compiler-plugin:3.x.x:compile ---
[INFO] Compiling 1 source file to /.../ target/classes
[INFO] BUILD SUCCESS
```

Nach erfolgreichem Build liegt die kompilierte Klasse unter:
`target/classes/de/hartwood/Main.class`

</details>

**Zum Nachdenken:** Du siehst, dass Maven einen Ordner `target/` anlegt. Was würdest du vermuten, was dort noch alles landen könnte im Laufe eines vollständigen Builds?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Maven-Projekt angelegt, Standardstruktur erklärt, pom.xml-Felder benannt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — pom.xml angepasst, Compiler-Version gesetzt, Felder sinnvoll befüllt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Projekt per Terminal kompiliert, BUILD SUCCESS gesehen, target/-Struktur erkundet _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Was ist der Vorteil einer einheitlichen Projektstruktur — und was wäre das Problem, wenn jedes Team seine eigene Struktur erfinden dürfte?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Fundament steht. Hartwoods Team hat eine sauber strukturierte Basis — aber alle Bibliotheken, die sie brauchen, müssen noch eingebunden werden. Im nächsten Modul lernst du, wie Maven externe Bibliotheken verwaltet, wo sie herkommen und warum du sie nie manuell herunterladen musst.

→ Weiter zu: [[02-dependencies|Modul 02: Dependencies verwalten]]

---

> _„Halt den Hut fest und die pom.xml sauber."_
> — Old Finn McGraw
