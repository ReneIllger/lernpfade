---
title: "Modul 02 — Dependencies verwalten"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Gradle
modul-nr: 2
dauer: 45 min
voraussetzungen:
  - "[[01-projektstruktur|Modul 01: Was ist Gradle & build.gradle]]"
---

# Modul 02 — Dependencies verwalten
_„Der Markt der tausend Bibliotheken"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | [[01-projektstruktur\|Modul 01]] abgeschlossen · Gradle-Projekt `expedition-tools` vorhanden |
| **Lernziel** | Du kannst Bibliotheken als Dependencies in der `build.gradle` eintragen, ihre Koordinaten nachschlagen, Gradle-Configurations korrekt einsetzen und den Unterschied zwischen `implementation` und `testImplementation` erklären. |

---

## Worum geht es?

Kairo, ein belebter Marktplatz. Old Finn führt Hartwood durch enge Gassen, vorbei an Händlern mit Gewürzen, Büchern und Werkzeug. „Hier gibt es alles, was du brauchst," sagt er. „JSON-Parser. HTTP-Clients. Datenbankverbindungen. Alles schon fertig, von anderen geschrieben." Er bleibt vor einem Stand stehen. „Die Frage ist nicht: Baue ich es selbst? Die Frage ist: Kenne ich die Koordinaten des richtigen Lieferanten?"

Lila zieht ein kleines Notizbuch heraus. „Jede Bibliothek hat drei Angaben: Wer hat sie gebaut. Wie heißt sie. Welche Version." Sie schreibt auf: `group:name:version`. „Das reicht. Gradle holt den Rest."

Hartwood nickt. „Und das Beste: Gradle weiß auch, wofür wir die Bibliothek brauchen. Nur zum Testen? Nur für den Produktionscode? Nur zur Laufzeit?" Er klopft auf das Notizbuch. „Das nennt sich Configuration."

**Deine Mission:** Lerne, wie du externe Bibliotheken in Gradle einbindest — und was hinter dem `dependencies`-Block steckt.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Bibliothek suchen und Koordinaten finden | 🟢 | ~7 Min. |
| 2 | Erste Dependency einbinden und nutzen | 🟢 | ~8 Min. |
| 3 | Configurations verstehen | 🟡 | ~10 Min. |
| 4 | Test-Dependency und erster Test | 🟡 | ~11 Min. |
| 5 | Dependency-Report analysieren | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Bibliothek suchen und Koordinaten finden 🟢

> Lila klappt ihr Notizbuch auf. „Jeder Händler hat eine Adresse. Für Java-Bibliotheken heißt die Adresse Maven Central."

**Deine Aufgabe:**
Öffne Maven Central, suche nach einer Bibliothek und finde ihre Gradle-Koordinaten.

**Was du dafür brauchst:**

Maven Central ist das weltweit größte Repository für Java-Bibliotheken — Gradle nutzt es genauso wie andere Build-Tools.

1. Öffne [search.maven.org](https://search.maven.org) im Browser
2. Suche nach `gson` (eine JSON-Bibliothek von Google)
3. Klicke auf das erste Ergebnis (`com.google.code.gson / gson`)
4. Wähle die neueste stabile Version
5. Klicke auf den Reiter **Gradle (Groovy)** oder das Gradle-Symbol

Gradle zeigt dir die fertige Zeile:
```groovy
implementation 'com.google.code.gson:gson:2.10.1'
```

Das Format ist: `'group:name:version'`

| Teil | Bedeutung | Beispiel |
|------|-----------|---------|
| `group` | Organisation oder Paketpräfix | `com.google.code.gson` |
| `name` | Name der Bibliothek | `gson` |
| `version` | Versionsnummer | `2.10.1` |

> [!tip] Tipp
> Maven Central hat für Gradle einen fertigen Code-Schnipsel — das kleine Kopier-Symbol daneben kopiert ihn direkt in die Zwischenablage.

<details>
<summary>📜 Lösungsvorschlag</summary>

Auf [search.maven.org](https://search.maven.org) → Suche „gson" → `com.google.code.gson:gson`

Gradle-Koordinaten (Stand dieser Einheit):
```groovy
implementation 'com.google.code.gson:gson:2.10.1'
```

Prüfe selbst, ob eine neuere Version verfügbar ist — die Versionsnummer kann sich geändert haben.

</details>

**Zum Nachdenken:** Wer stellt die Bibliotheken auf Maven Central bereit? Und wer prüft, ob sie sicher und vertrauenswürdig sind?

---

## Aufgabe 2 — Erste Dependency einbinden und nutzen 🟢

> Old Finn legt die Koordinaten auf den Tisch. „Eintragen. Gradle holt sie. Fertig."

**Deine Aufgabe:**
Binde `gson` in die `build.gradle` ein und nutze sie in einer kurzen Java-Klasse.

**Was du dafür brauchst:**

Öffne `build.gradle` und trage die Dependency in den `dependencies`-Block ein:

```groovy
dependencies {
    implementation 'com.google.code.gson:gson:2.10.1'

    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'
}
```

Speichere die Datei. In IntelliJ erscheint oben rechts ein Elefant-Symbol oder ein „Load Gradle Changes"-Hinweis — klicke darauf. Gradle lädt Gson jetzt herunter.

Dann: Lege eine Klasse `JsonBeispiel.java` unter `src/main/java/de/hartwood/` an:

```java
package de.hartwood;

import com.google.gson.Gson;

public class JsonBeispiel {
    public static void main(String[] args) {
        Gson gson = new Gson();
        String json = gson.toJson("Expedition bereit");
        System.out.println(json);
    }
}
```

> [!tip] Tipp 1
> Wenn IntelliJ `Gson` rot unterstreicht, obwohl du die Dependency eingetragen hast: Klicke auf „Load Gradle Changes" (das Elefanten-Symbol oben rechts in der build.gradle).

> [!tip] Tipp 2
> Heruntergeladene Bibliotheken werden im **Gradle-Cache** gespeichert: `~/.gradle/caches/`. Einmal heruntergeladen, wird eine Bibliothek von dort wiederverwendet — auch in anderen Projekten.

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe beim Ausführen von `JsonBeispiel.main()`:
```
"Expedition bereit"
```

Gson hat den String als JSON serialisiert — daher die Anführungszeichen. JSON-Strings werden immer mit `"` umschlossen.

</details>

**Zum Nachdenken:** Gson wurde automatisch heruntergeladen. Was passiert, wenn du offline arbeitest und die Bibliothek noch nicht im Gradle-Cache ist?

---

## Aufgabe 3 — Configurations verstehen 🟡

> Hartwood breitet drei Kisten auf dem Tisch aus. „Diese hier kommt mit ins Feld — immer. Diese nur zum Testen — die lassen wir hier. Und diese brauchen wir erst, wenn wir ankommen." Er sieht auf. „Gradle nennt das Configurations."

**Deine Aufgabe:**
Lies die Tabelle der wichtigsten Gradle Configurations und beantworte die Fragen.

**Was du dafür brauchst:**

Gradle unterscheidet, wofür eine Dependency gebraucht wird. Das nennt sich **Configuration**:

| Configuration | Wann verfügbar | Im fertigen JAR | Typischer Einsatz |
|---------------|:--------------:|:---------------:|-------------------|
| `implementation` | Kompilieren & Laufzeit | ✅ | Bibliotheken, die dein Code direkt nutzt |
| `testImplementation` | Nur beim Testen | ❌ | Test-Frameworks wie JUnit |
| `compileOnly` | Nur beim Kompilieren | ❌ | Annotationen, Servlet-API (der Server liefert sie zur Laufzeit) |
| `runtimeOnly` | Nur zur Laufzeit | ✅ | Datenbanktreiber (kein direkter Import im Code) |
| `api` | Kompilieren & Laufzeit | ✅ | Nur mit `java-library`-Plugin: Typ wird nach außen sichtbar gemacht |

**Merkhilfe:**
- `implementation` → Die Bibliothek gehört ins Programm, du nutzt sie direkt
- `testImplementation` → Nur fürs Testen, kommt nicht ins fertige Programm
- `runtimeOnly` → Wird gebraucht, aber nicht direkt importiert (z.B. JDBC-Treiber)

> [!tip] Tipp
> `testImplementation` ist die häufigste Wahl für Test-Frameworks. Ohne diesen Scope würde JUnit im fertigen Programm landen — das wäre verschwendeter Speicher.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Welche Configuration wäre richtig?**

| Bibliothek | Configuration |
|---|---|
| Gson (JSON, direkt im Code genutzt) | `implementation` |
| JUnit 5 (Test-Framework) | `testImplementation` |
| PostgreSQL JDBC-Treiber | `runtimeOnly` |
| Lombok (Annotation-Prozessor) | `compileOnly` |

</details>

**Zum Nachdenken:** Was wäre das Problem, wenn du JUnit mit `implementation` statt `testImplementation` einbindest? Was würde das für das fertige Programm bedeuten?

---

## Aufgabe 4 — Test-Dependency und erster Test 🟡

> Lila stellt die Testkiste auf den Tisch. „Diese bleibt im Labor. Aber ohne sie wissen wir nicht, ob unser Gerät funktioniert."

**Deine Aufgabe:**
Stelle sicher, dass JUnit 5 korrekt eingebunden ist, und schreibe einen ersten Unit-Test.

**Was du dafür brauchst:**

JUnit 5 sollte bereits in deiner `build.gradle` stehen (aus Modul 01). Prüfe, ob dieser Block vorhanden ist:

```groovy
dependencies {
    implementation 'com.google.code.gson:gson:2.10.1'

    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'
}

test {
    useJUnitPlatform()
}
```

> [!info] Was ist `platform(...)`?
> `platform('org.junit:junit-bom:5.10.0')` ist eine **Bill of Materials (BOM)** — ein spezielles Paket, das die Versionen aller JUnit-Teilbibliotheken festlegt. So musst du für `junit-jupiter-api`, `junit-jupiter-engine` usw. keine Versionen einzeln angeben.

Lege nun unter `src/test/java/de/hartwood/` eine Testklasse an:

```java
package de.hartwood;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class JsonBeispielTest {

    @Test
    void toJsonSollStringMitAnfuehrungszeichenLiefern() {
        var gson = new com.google.gson.Gson();
        String result = gson.toJson("test");
        assertEquals("\"test\"", result);
    }
}
```

Starte den Test mit dem grünen Pfeil in IntelliJ oder im Terminal:
```
gradle test
```

> [!tip] Tipp
> `useJUnitPlatform()` im `test`-Block ist wichtig — ohne diesen Eintrag findet Gradle JUnit 5 nicht und führt keine Tests aus.

<details>
<summary>📜 Lösungsvorschlag</summary>

Der Test sollte grün werden. Gson gibt `"test"` (mit Anführungszeichen) zurück, weil JSON-Strings immer gequotet sind.

Testbericht nach `gradle test`:
```
JsonBeispielTest > toJsonSollStringMitAnfuehrungszeichenLiefern() PASSED
```

Der HTML-Bericht liegt unter `build/reports/tests/test/index.html` — IntelliJ kann ihn direkt im Browser öffnen.

</details>

**Zum Nachdenken:** `testImplementation` sorgt dafür, dass JUnit nicht im fertigen Programm landet. Was würde passieren, wenn du trotzdem `implementation` verwendest — was wäre die Konsequenz?

---

## Aufgabe 5 — Dependency-Report analysieren 🔴

> [!example] Expedition ins Unbekannte
> Old Finn schlägt ein dickes Register auf. „Jede Bibliothek hat selbst wieder Abhängigkeiten. Und die haben Abhängigkeiten. Gradle kann dir zeigen, was wirklich alles mitreist."

**Deine Aufgabe:**
Lass dir den vollständigen Dependency-Report anzeigen und interpretiere die Ausgabe.

**Was du dafür brauchst:**

Führe im Terminal aus:
```
gradle dependencies
```

Gradle zeigt alle Configurations mit ihren direkten und transitiven Dependencies. Für die `runtimeClasspath`-Configuration sieht es ungefähr so aus:

```
runtimeClasspath - Runtime classpath of source set 'main'.
\--- com.google.code.gson:gson:2.10.1

testRuntimeClasspath - Runtime classpath of source set 'test'.
+--- com.google.code.gson:gson:2.10.1
\--- org.junit:junit-bom:5.10.0
     +--- org.junit.jupiter:junit-jupiter-api:5.10.0
     +--- org.junit.jupiter:junit-jupiter-engine:5.10.0
     \--- ...
```

**Gezielter Report für eine Configuration:**
```
gradle dependencies --configuration runtimeClasspath
```

> [!info] Weiterführende Quelle
> **Gradle Dependency Management** — Die vollständige Referenz zu Configurations, Resolutions und Conflicts.
> 🔗 [docs.gradle.org/current/userguide/dependency_management.html](https://docs.gradle.org/current/userguide/dependency_management.html)

> [!tip] Tipp
> `gradle dependencies` zeigt **alle** Configurations — das kann sehr lang werden. Mit `--configuration runtimeClasspath` oder `--configuration testRuntimeClasspath` fokussierst du die Ausgabe.

<details>
<summary>📜 Lösungsvorschlag</summary>

In der Ausgabe siehst du:
- **Direkte Dependencies**: Die, die du selbst in `build.gradle` eingetragen hast
- **Transitive Dependencies**: Die, die deine Dependencies selbst brauchen (eingerückt, mit `---`)

Gson hat keine transitiven Dependencies — es ist vollständig eigenständig.
JUnit bringt durch die BOM mehrere Teilbibliotheken mit.

</details>

**Zum Nachdenken:** Gson bringt keine transitiven Dependencies mit. Was bedeutet das verglichen mit einer Bibliothek, die selbst zehn weitere Dependencies mitbringt? Was könnte das für dein Projekt bedeuten?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Koordinaten auf Maven Central gefunden, Gson eingebunden, Gradle-Sync durchgeführt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — Configurations erklärt, JUnit als `testImplementation` eingebunden, Test geschrieben und bestanden _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Dependency-Report gelesen, transitive Dependencies erklärt _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum ist es sinnvoller, Bibliotheken per `build.gradle` zu verwalten, anstatt JAR-Dateien manuell herunterzuladen und ins Projekt zu kopieren?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Team hat Bibliotheken. Im nächsten Modul lernst du Tasks — das Herzstück von Gradle. Was sind Tasks, wie rufst du sie auf, wie hängen sie zusammen und wie schreibst du eigene?

→ Weiter zu: [[03-tasks|Modul 03: Tasks & Build Lifecycle]]

---

> _„Halt den Hut fest und die Koordinaten parat."_
> — Old Finn McGraw
