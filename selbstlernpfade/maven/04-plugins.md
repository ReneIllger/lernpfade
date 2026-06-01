---
title: "Modul 04 — Plugins & Konfiguration"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Maven
modul-nr: 4
dauer: 45 min
voraussetzungen:
  - "[[03-build-lifecycle|Modul 03: Build Lifecycle & Phasen]]"
---

# Modul 04 — Plugins & Konfiguration
_„Die Werkzeugkiste des Meisters"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[03-build-lifecycle\|Modul 03]] abgeschlossen · Projekt mit JAR-Build und Tests vorhanden |
| **Lernziel** | Du kannst Maven-Plugins benennen und konfigurieren, den Compiler explizit steuern und ein ausführbares JAR mit allen Dependencies (Fat JAR) bauen. |

---

## Worum geht es?

Istanbul, Werkstatt eines alten Mechanikers. Hartwood legt die Einzelteile einer kaputten Stationsausrüstung auf dem Tisch aus. Der Mechaniker schüttelt den Kopf. „Das Grundwerkzeug kann ich reparieren. Aber was ihr da bauen wollt — das braucht Spezialwerkzeug." Er öffnet eine schwere Holzkiste. „Plugins."

Lila nickt. „Maven hat von Haus aus ein paar Werkzeuge eingebaut: Compiler, Tester, Paketierer. Aber wenn du mehr willst — ein ausführbares Programm, Code-Analyse, automatische Releases — dann holst du dir das passende Plugin."

Old Finn hebt ein Werkzeug aus der Kiste. „Und die gute Nachricht: Alle sprechen dieselbe Sprache. Alle stecken in der pom.xml. Und alle laufen im selben Build."

**Deine Mission:** Lerne, wie Maven Plugins funktionieren, konfiguriere den Compiler explizit und bau ein ausführbares JAR, das überall läuft — auch ohne installiertes Gson.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Plugins verstehen — was sie sind und wie sie funktionieren | 🟢 | ~7 Min. |
| 2 | Compiler-Plugin explizit konfigurieren | 🟢 | ~8 Min. |
| 3 | Surefire-Plugin — Tests steuern | 🟡 | ~10 Min. |
| 4 | Fat JAR bauen mit dem Assembly Plugin | 🟡 | ~11 Min. |
| 5 | Eigene Plugin-Ausführung — Goals manuell starten | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Plugins verstehen 🟢

> Der Mechaniker öffnet seine Kiste und legt drei Werkzeuge auf den Tisch. „Jedes hat einen Namen. Jedes hat eine Aufgabe. Und jedes weißt, wann es dran ist."

**Deine Aufgabe:**
Lies die Erklärung zu Maven Plugins und beantworte die Fragen darunter.

**Was du dafür brauchst:**

Maven selbst kann fast nichts — es ist nur ein Framework. Die eigentliche Arbeit machen **Plugins**. Jede Phase des Build Lifecycle ist mit einem oder mehreren Plugin-Goals verknüpft.

Ein **Goal** ist eine konkrete Aufgabe eines Plugins. Die Notation lautet:
```
plugin-name:goal-name
```

Beispiele:
- `compiler:compile` — kompiliert den Quellcode
- `surefire:test` — führt Tests aus
- `jar:jar` — packt das JAR
- `dependency:tree` — zeigt den Dependency-Baum (kennst du aus Modul 02)

**Diese Plugins nutzt Maven standardmäßig — auch ohne Konfiguration:**

| Plugin | Standard-Goal | Gebunden an Phase |
|--------|--------------|-------------------|
| `maven-compiler-plugin` | `compile` | `compile` |
| `maven-surefire-plugin` | `test` | `test` |
| `maven-jar-plugin` | `jar` | `package` |
| `maven-install-plugin` | `install` | `install` |
| `maven-clean-plugin` | `clean` | `clean` |

Du hast diese Plugins schon genutzt — sie liefen im Hintergrund. In der pom.xml tauchen sie erst auf, wenn du sie konfigurieren willst.

> [!tip] Tipp
> Alle offiziellen Maven-Plugins findest du unter [maven.apache.org/plugins](https://maven.apache.org/plugins/index.html) — mit vollständiger Dokumentation.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Welches Plugin wurde bei `mvn test` aktiv?**
→ `maven-surefire-plugin` mit dem Goal `test`

**Welches Goal ist für das JAR zuständig?**
→ `maven-jar-plugin:jar`, gebunden an die Phase `package`

</details>

**Zum Nachdenken:** Maven lädt Plugins genauso herunter wie Dependencies — aus Maven Central. Was könnte das für Sicherheit und Reproduzierbarkeit bedeuten?

---

## Aufgabe 2 — Compiler-Plugin explizit konfigurieren 🟢

> Lila schlägt das Notizbuch auf. „Das Compiler-Werkzeug läuft immer — auch ohne Anweisung. Aber wenn du es anpassen willst, musst du es explizit benennen."

**Deine Aufgabe:**
Konfiguriere das `maven-compiler-plugin` explizit in der pom.xml und setze die Java-Version auf 17.

**Was du dafür brauchst:**

Plugins werden in der pom.xml im `<build>`-Abschnitt konfiguriert:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <release>17</release>
            </configuration>
        </plugin>
    </plugins>
</build>
```

Füge diesen Block in deine `pom.xml` ein — direkt vor dem schließenden `</project>`-Tag (oder nach `<properties>`).

> [!info] `<release>` vs. `<source>` / `<target>`
> `<release>17</release>` ist der moderne Weg (ab Maven Compiler Plugin 3.6+). Es setzt gleichzeitig source, target und das Java-Toolset — präziser als die alten `<source>` und `<target>` Properties.

> [!tip] Tipp
> Auch wenn du `<maven.compiler.release>` schon in `<properties>` gesetzt hast, ist die explizite Plugin-Konfiguration besser lesbar und erlaubt mehr Einstellmöglichkeiten — z.B. zusätzliche Compiler-Argumente.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach dem Hinzufügen: `mvn compile` ausführen — der Build sollte weiterhin mit `BUILD SUCCESS` enden.

Im Build-Output erscheint jetzt die konfigurierte Plugin-Version:
```
[INFO] --- maven-compiler-plugin:3.11.0:compile ---
```

</details>

**Zum Nachdenken:** Du konfigurierst eine spezifische Plugin-Version (`3.11.0`). Was würde passieren, wenn du keine Version angibst — und warum könnte das in Teams ein Problem sein?

---

## Aufgabe 3 — Surefire-Plugin: Tests steuern 🟡

> Hartwood legt den Testbericht auf den Tisch. „Alle Tests grün. Aber manchmal müssen wir einzelne Szenarien gezielt prüfen — oder bestimmte Tests überspringen. Das Surefire-Werkzeug lässt sich dafür konfigurieren."

**Deine Aufgabe:**
Konfiguriere das `maven-surefire-plugin` für JUnit 5 und lerne, wie du einzelne Tests gezielt ausführst.

**Was du dafür brauchst:**

Das Surefire-Plugin führt Tests aus. Für JUnit 5 braucht es eine explizite Konfiguration:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.1.2</version>
</plugin>
```

Ab Surefire 3.x wird JUnit 5 automatisch erkannt — du brauchst keine weitere Konfiguration.

**Tests gezielt ausführen:**
```
# Nur eine bestimmte Testklasse
mvn test -Dtest=JsonBeispielTest

# Nur eine bestimmte Methode
mvn test -Dtest=JsonBeispielTest#toJsonSollStringMitAnfuehrungszeichenLiefern

# Tests überspringen (Notlösung!)
mvn package -DskipTests
```

> [!warning] skipTests mit Bedacht
> `-DskipTests` sollte nur ein Notbehelf sein — z.B. wenn du weißt, dass bestimmte Tests aktuell brechen und das Problem bekannt ist. Im Team-Alltag und CI/CD sollte `skipTests` nie Standard sein.

> [!tip] Tipp
> Die Testberichte im XML-Format liegen nach dem Build unter `target/surefire-reports/`. Das ist nützlich für CI-Systeme wie Jenkins oder GitHub Actions, die diese Reports auswerten können.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach dem Hinzufügen der Surefire-Konfiguration: `mvn test` ausführen.

Output mit Surefire 3.x:
```
[INFO] --- maven-surefire-plugin:3.1.2:test ---
[INFO] Running de.hartwood.JsonBeispielTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
```

</details>

**Zum Nachdenken:** `-DskipTests` überspringt die Ausführung, `-Dmaven.test.skip=true` überspringt sogar die Kompilierung der Tests. Wann würde der zweite Befehl sinnvoll sein?

---

## Aufgabe 4 — Fat JAR bauen mit dem Assembly Plugin 🟡

> Old Finn hält das fertige JAR hoch. „Alles drin. Dependencies, Code, alles. Man muss nichts extra installieren — einfach starten." Er grinst. „Das nenn' ich ein richtiges Reisegepäck."

**Deine Aufgabe:**
Binde das `maven-assembly-plugin` ein und baue ein ausführbares "Fat JAR" — ein JAR, das alle Dependencies enthält.

**Was du dafür brauchst:**

Das Assembly Plugin kann verschiedene Paketformen bauen. Die `jar-with-dependencies` ist die einfachste Form eines Fat JAR:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>3.6.0</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>de.hartwood.Main</mainClass>
            </manifest>
        </archive>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
    </configuration>
    <executions>
        <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

Füge diesen Block in `<build><plugins>` ein und führe dann aus:
```
mvn clean package
```

In `target/` entsteht jetzt eine zweite JAR-Datei:
```
expedition-tools-1.0-SNAPSHOT-jar-with-dependencies.jar
```

Starte sie direkt:
```
java -jar target/expedition-tools-1.0-SNAPSHOT-jar-with-dependencies.jar
```

> [!tip] Tipp 1
> `<mainClass>` setzt den Einstiegspunkt im JAR-Manifest. Ohne diesen Eintrag würde `java -jar` mit einer Fehlermeldung scheitern: „no main manifest attribute".

> [!tip] Tipp 2
> Das `<execution>`-Element bindet das Plugin an eine Phase. Hier: Das Goal `single` wird in der Phase `package` automatisch ausgeführt — immer wenn du `mvn package` rufst.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach `mvn clean package` mit `BUILD SUCCESS`:
```
$ java -jar target/expedition-tools-1.0-SNAPSHOT-jar-with-dependencies.jar
Expedition bereit.
```

Das Fat JAR enthält jetzt Gson — es funktioniert ohne separate Classpath-Angabe.

Größenvergleich: Das normale JAR ist wenige Kilobyte groß. Das Fat JAR kann je nach Dependencies mehrere Megabyte groß sein.

</details>

**Zum Nachdenken:** Ein Fat JAR ist bequem, aber groß. Wenn dein Projekt von drei anderen Teams als Bibliothek genutzt wird und du ein Fat JAR veröffentlichst — was könnte das Problem sein?

---

## Aufgabe 5 — Goals manuell starten 🔴

> [!example] Expedition ins Unbekannte
> Hartwood klappt die Werkzeugkiste zu. „Du weißt jetzt, was jedes Werkzeug tut. Aber manchmal willst du ein einzelnes Werkzeug direkt benutzen — ohne die ganze Expedition neu zu starten."

**Deine Aufgabe:**
Führe Plugin-Goals direkt aus, ohne den vollständigen Build Lifecycle zu starten. Erkunde dabei, welche Goals das Assembly Plugin noch anbietet.

**Was du dafür brauchst:**

Du kannst Plugin-Goals direkt aufrufen, ohne eine Lifecycle-Phase:
```
mvn plugin-name:goal
```

Beispiele:
```
# Zeige Dependency-Baum (kennst du aus Modul 02)
mvn dependency:tree

# Zeige alle Properties und deren Werte
mvn help:effective-pom

# Zeige, welche Plugins aktiv sind und was sie tun
mvn help:describe -Dplugin=compiler
```

**Deine Aufgabe:** Führe `mvn help:effective-pom` aus und erkläre, was du siehst. Warum ist die Ausgabe viel länger als deine eigentliche pom.xml?

> [!info] Weiterführende Quelle
> **Maven Plugin-Suche** — Alle verfügbaren Plugins durchsuchen.
> 🔗 [maven.apache.org/plugins](https://maven.apache.org/plugins/index.html)

> [!tip] Tipp
> `effective-pom` zeigt die **vollständige** pom.xml nach dem Zusammenführen mit dem Super POM (dem globalen Maven-Standard-POM). Dort siehst du auch alle Standard-Plugins mit ihren Default-Konfigurationen — selbst wenn du sie nie explizit in deine pom.xml eingetragen hast.

<details>
<summary>📜 Lösungsvorschlag</summary>

`mvn help:effective-pom` gibt eine sehr lange XML-Ausgabe aus — weil Maven intern eine **Super POM** hat, die alle Standardwerte definiert. Deine pom.xml wird mit dieser Super POM zusammengeführt.

In der Ausgabe siehst du u.a.:
- `maven-compiler-plugin` mit Default-Version
- `maven-surefire-plugin` mit Default-Version
- `maven-install-plugin`, `maven-deploy-plugin`, usw.

Das erklärt, warum dein Projekt ohne Plugin-Konfiguration schon funktioniert.

</details>

**Zum Nachdenken:** Jedes Maven-Projekt erbt von der Super POM. Was bedeutet das für die Reproduzierbarkeit von Builds, wenn du keine expliziten Plugin-Versionen festlegst?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Plugin-Konzept erklärt, Compiler-Plugin konfiguriert, Standard-Plugins benannt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — Surefire für JUnit 5 konfiguriert, Fat JAR gebaut und direkt ausgeführt _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Goals direkt aufgerufen, `effective-pom` interpretiert, Super POM erklärt _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Wann ist ein Fat JAR die richtige Lösung — und wann würde man es lieber vermeiden? Nenne je ein Beispiel.**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Modulreihe abgeschlossen

Das Expeditionsteam hat das Ziel erreicht. Du kannst:

- ✅ Maven-Projekte anlegen und strukturieren
- ✅ Die pom.xml lesen, verstehen und gezielt anpassen
- ✅ Dependencies aus Maven Central einbinden und Scopes einsetzen
- ✅ Den Build Lifecycle erklären und Phasen gezielt aufrufen
- ✅ Plugins konfigurieren und ein ausführbares Fat JAR bauen

**Mögliche nächste Schritte (auf eigene Faust):**
- **Multi-Module-Projekte:** Mehrere Maven-Module in einem Parent-Projekt — nützlich bei größeren Anwendungen
- **Maven Wrapper (`mvnw`):** Sicherstellen, dass alle im Team dieselbe Maven-Version nutzen
- **Profiles:** Verschiedene Build-Konfigurationen für dev, test, prod
- **Maven in CI/CD:** Wie `mvn clean install` in GitHub Actions oder Jenkins läuft

→ Zurück zur [[00-uebersicht|Modulreihe: Maven]]

---

> _„Halt den Hut fest und die pom.xml auf dem neuesten Stand."_
> — Old Finn McGraw
