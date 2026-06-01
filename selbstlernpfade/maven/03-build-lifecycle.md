---
title: "Modul 03 — Build Lifecycle & Phasen"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Maven
modul-nr: 3
dauer: 45 min
voraussetzungen:
  - "[[02-dependencies|Modul 02: Dependencies verwalten]]"
---

# Modul 03 — Build Lifecycle & Phasen
_„Die Routen der Expedition"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[02-dependencies\|Modul 02]] abgeschlossen · Maven-Projekt mit Dependencies vorhanden |
| **Lernziel** | Du kannst den Maven Build Lifecycle erklären, die wichtigsten Phasen benennen und gezielt aufrufen — und verstehst, warum Tests beim Build automatisch ausgeführt werden. |

---

## Worum geht es?

Das Expeditionslager, Nacht. Lila legt drei Karten auf den Tisch — von der Küste bis zum Zielort, aufgeteilt in Etappen. „Eine Expedition funktioniert nicht, indem man einfach losläuft. Es gibt eine Route. Etappen. Jede Etappe muss abgeschlossen sein, bevor die nächste beginnt." Sie tippt auf die erste Karte. „Maven arbeitet genauso."

Hartwood nickt. „Wir kompilieren erst den Code. Dann laufen die Tests. Dann packen wir alles in ein Paket. Und erst wenn all das geklappt hat, wird das Paket im lokalen Repository abgelegt." Er lehnt sich zurück. „Das nennt sich Build Lifecycle. Und jede dieser Etappen — jede Phase — hat ihren Namen."

Old Finn zieht an seiner Pfeife. „Und der Trick ist: Wenn du sagst ,Geh bis Etappe vier', läuft Maven alle Etappen davor automatisch mit. Kein manuelles Abhaken."

**Deine Mission:** Verstehe den Maven Build Lifecycle und führe die wichtigsten Phasen gezielt durch.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Die Lifecycle-Phasen kennenlernen | 🟢 | ~7 Min. |
| 2 | mvn compile & test ausführen | 🟢 | ~8 Min. |
| 3 | mvn package — das JAR bauen | 🟡 | ~10 Min. |
| 4 | mvn install — ins lokale Repository | 🟡 | ~11 Min. |
| 5 | mvn clean — Artefakte aufräumen | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Die Lifecycle-Phasen kennenlernen 🟢

> Lila zeigt auf die erste Karte. „Jede Etappe hat einen Namen. Wenn du den Namen kennst, weißt du, wo du stehst."

**Deine Aufgabe:**
Lerne die wichtigsten Phasen des Maven Default Lifecycle kennen und ordne sie der richtigen Reihenfolge zu.

**Was du dafür brauchst:**

Maven hat drei eingebaute Lifecycles. Der wichtigste ist der **Default Lifecycle** — er enthält 23 Phasen, von denen du diese regelmäßig nutzen wirst:

| Phase | Was passiert dort |
|-------|------------------|
| `validate` | Prüft, ob das Projekt gültig ist und alle nötigen Infos vorhanden sind |
| `compile` | Kompiliert den Quellcode (`src/main/java`) → `.class`-Dateien in `target/classes/` |
| `test-compile` | Kompiliert den Testcode (`src/test/java`) |
| `test` | Führt alle Unit-Tests aus — Build schlägt fehl, wenn Tests fehlschlagen |
| `package` | Packt den kompilierten Code in ein JAR (oder WAR, EAR) in `target/` |
| `verify` | Prüft das Paket auf Qualitätskriterien (z.B. Integrationstests) |
| `install` | Kopiert das Paket in das lokale Maven-Repository (`~/.m2/`) |
| `deploy` | Lädt das Paket in ein Remote-Repository hoch (z.B. Nexus, GitHub Packages) |

> [!info] Wichtige Regel
> Wenn du eine Phase aufrufst, führt Maven **alle Phasen davor** automatisch aus.
> `mvn package` läuft: validate → compile → test-compile → test → **package**

> [!tip] Tipp
> Es gibt noch zwei weitere Lifecycles: **clean** (löscht `target/`) und **site** (generiert Projektdokumentation). `clean` kennst du schon vom Aufräumen.

<details>
<summary>📜 Lösungsvorschlag</summary>

Reihenfolge der Kernphasen:
`validate` → `compile` → `test-compile` → `test` → `package` → `verify` → `install` → `deploy`

</details>

**Zum Nachdenken:** Warum läuft `test` automatisch vor `package`? Was wäre das Risiko, wenn man diese Reihenfolge umkehren könnte?

---

## Aufgabe 2 — mvn compile & test ausführen 🟢

> Old Finn verschränkt die Arme. „Reden ist gut. Laufen ist besser. Los — erste Etappe."

**Deine Aufgabe:**
Führe `mvn compile` und danach `mvn test` im Terminal aus. Beobachte die Ausgabe.

**Was du dafür brauchst:**

Öffne das Terminal im Projektverzeichnis (wo die `pom.xml` liegt) und führe aus:

```
mvn compile
```

Dann:
```
mvn test
```

**Was du in der Ausgabe siehst:**

Bei `mvn compile`:
```
[INFO] Compiling X source files to .../target/classes
[INFO] BUILD SUCCESS
```

Bei `mvn test`:
```
[INFO] --- maven-surefire-plugin --- Running de.hartwood.JsonBeispielTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

> [!tip] Tipp
> `mvn test` führt automatisch auch `compile` und `test-compile` aus — du musst nicht separat kompilieren.

<details>
<summary>📜 Lösungsvorschlag</summary>

Wenn beide Befehle mit `BUILD SUCCESS` enden: Alles korrekt.

Falls `mvn test` mit `BUILD FAILURE` endet:
- Schau dir die Fehlerausgabe genau an — Maven gibt dir die betroffene Testklasse und die Fehlermeldung
- Häufige Ursache: Testklasse liegt im falschen Verzeichnis (muss unter `src/test/java/...` sein)

</details>

**Zum Nachdenken:** `mvn test` läuft alle Tests im Projekt automatisch. Was würde das in einem großen Projekt mit tausenden Tests bedeuten — und wie könnte man damit umgehen?

---

## Aufgabe 3 — mvn package — das JAR bauen 🟡

> Lila rollt die zweite Karte auf. „Kompiliert. Getestet. Jetzt wird das Gepäck gepackt — in ein Paket, das wir überall hinschicken können."

**Deine Aufgabe:**
Baue dein Projekt mit `mvn package` und untersuche das entstandene JAR.

**Was du dafür brauchst:**

```
mvn package
```

Nach erfolgreichem Build findet sich in `target/` eine JAR-Datei:
```
target/
├── classes/                          ← kompilierter Produktionscode
├── test-classes/                     ← kompilierte Tests
├── expedition-tools-1.0-SNAPSHOT.jar ← das fertige Paket
└── surefire-reports/                 ← Testberichte (XML & TXT)
```

Du kannst das JAR direkt ausführen — falls deine `Main`-Klasse eine `main()`-Methode hat:
```
java -cp target/expedition-tools-1.0-SNAPSHOT.jar de.hartwood.Main
```

> [!tip] Tipp 1
> `-cp` steht für **Classpath** — damit sagst du Java, wo es die Klassen finden soll.

> [!tip] Tipp 2
> Das JAR enthält standardmäßig nur deinen Code — nicht die Dependencies (wie Gson). Ein "Fat JAR" mit allen Dependencies ist Thema von Modul 04.

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe von `java -cp target/expedition-tools-1.0-SNAPSHOT.jar de.hartwood.Main`:
```
Expedition bereit.
```

Falls eine `ClassNotFoundException` für Gson erscheint: Das ist korrekt — Gson ist nicht im JAR enthalten. Das behebst du in Modul 04 mit dem Maven Assembly Plugin.

</details>

**Zum Nachdenken:** Das JAR enthält deinen Code, aber nicht Gson. Wie würde ein anderes Team dein Programm ausführen können? Was bräuchten sie zusätzlich?

---

## Aufgabe 4 — mvn install — ins lokale Repository 🟡

> Hartwood nimmt das fertige Paket und legt es ins Regal des Archivraums. „Wenn es hier liegt, kann jede andere Expedition es benutzen — ohne es neu zu bauen."

**Deine Aufgabe:**
Installiere dein Projekt im lokalen Maven-Repository und erkläre, was das bedeutet.

**Was du dafür brauchst:**

```
mvn install
```

`install` macht dasselbe wie `package` — und kopiert das JAR zusätzlich in das **lokale Maven-Repository** unter `~/.m2/repository/`.

Danach liegt dein JAR hier:
```
~/.m2/repository/de/hartwood/expedition-tools/1.0-SNAPSHOT/
```

**Warum ist das nützlich?**
Wenn du ein zweites Maven-Projekt hast, kannst du `expedition-tools` dort als Dependency eintragen:

```xml
<dependency>
    <groupId>de.hartwood</groupId>
    <artifactId>expedition-tools</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

Maven findet das JAR dann automatisch im lokalen Repository — ganz ohne es auf Maven Central veröffentlichen zu müssen.

> [!tip] Tipp
> Das ist besonders nützlich für interne Bibliotheken, die man in mehreren Projekten nutzt, aber nicht öffentlich machen will.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach `mvn install` mit `BUILD SUCCESS`:
```
~/.m2/repository/de/hartwood/expedition-tools/1.0-SNAPSHOT/expedition-tools-1.0-SNAPSHOT.jar
```

Um das zu prüfen, kannst du im Terminal eingeben:
```
ls ~/.m2/repository/de/hartwood/expedition-tools/
```

</details>

**Zum Nachdenken:** `mvn install` kopiert ins lokale Repository — `mvn deploy` würde in ein Remote-Repository laden. Wann wäre `deploy` sinnvoll, und wer würde das in einem Team-Umfeld typischerweise ausführen?

---

## Aufgabe 5 — mvn clean — Artefakte aufräumen 🔴

> [!example] Expedition ins Unbekannte
> Old Finn sieht auf den überfüllten `target/`-Ordner. „Altes Gepäck schleppt sich nicht von selbst weg. Manchmal muss man aufräumen — komplett von vorne anfangen." Er greift zum Besen. „In Maven heißt das clean."

**Deine Aufgabe:**
Räume den Build auf und erkläre, wann `mvn clean` wichtig ist. Kombiniere dann `clean` und `package` zu einem einzigen Befehl.

**Was du dafür brauchst:**

```
mvn clean
```

`clean` löscht das gesamte `target/`-Verzeichnis. Alle kompilierten Klassen, JARs und Testberichte sind weg.

**Wann ist das wichtig?**
- Wenn du sicherstellen willst, dass keine alten `.class`-Dateien den Build beeinflussen
- Nach einer Umbenennung von Klassen (alte `.class`-Dateien bleiben sonst liegen)
- Auf CI/CD-Systemen startet man fast immer mit `clean`

**Kombinierter Befehl:**
```
mvn clean package
```

Das löscht zuerst `target/`, führt dann alle Phasen bis `package` aus — ein sauberer Build von Grund auf.

> [!info] Weiterführende Quelle
> **Maven Build Lifecycle** — Die vollständige Referenz aller Phasen und Lifecycles.
> 🔗 [maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

> [!tip] Tipp
> `mvn clean install` ist der "Standard-Build" in vielen Teams und CI/CD-Pipelines — sauberer Build, Tests laufen, Ergebnis landet im lokalen Repository.

<details>
<summary>📜 Lösungsvorschlag</summary>

```
$ mvn clean package
[INFO] --- maven-clean-plugin --- Deleting .../target
[INFO] Compiling 1 source file...
[INFO] Tests run: 1, Failures: 0...
[INFO] Building jar: .../target/expedition-tools-1.0-SNAPSHOT.jar
[INFO] BUILD SUCCESS
```

Das `target/`-Verzeichnis existiert erst nach `clean package` wieder — frisch gebaut, keine alten Artefakte.

</details>

**Zum Nachdenken:** Auf einem CI-Server wird bei jedem Commit `mvn clean install` ausgeführt. Was würde es bedeuten, wenn man stattdessen nur `mvn install` (ohne `clean`) ausführen würde?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Lifecycle-Phasen in richtiger Reihenfolge benannt, `mvn compile` und `mvn test` erfolgreich ausgeführt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — JAR gebaut, im lokalen Repository installiert, Zweck von `install` erklärt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — `mvn clean package` kombiniert ausgeführt und erklärt, wann `clean` unverzichtbar ist _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum laufen Tests standardmäßig als Teil des Builds — und wann könnte man sie mit `mvn package -DskipTests` überspringen? Ist das eine gute Idee?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Team kann bauen, testen und paketieren. Aber das JAR enthält noch nicht alle Abhängigkeiten — und der Compiler lässt sich noch feiner konfigurieren. Im nächsten Modul lernst du Maven Plugins: wie sie funktionieren, wie du sie konfigurierst und wie du ein ausführbares "Fat JAR" baust.

→ Weiter zu: [[04-plugins|Modul 04: Plugins & Konfiguration]]

---

> _„Halt den Hut fest und die Phasen in Reihenfolge."_
> — Old Finn McGraw
