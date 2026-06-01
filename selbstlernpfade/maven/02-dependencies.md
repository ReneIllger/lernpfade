---
title: "Modul 02 — Dependencies verwalten"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Maven
modul-nr: 2
dauer: 45 min
voraussetzungen:
  - "[[01-projektstruktur|Modul 01: Was ist Maven & die pom.xml]]"
---

# Modul 02 — Dependencies verwalten
_„Der Schwarzmarkt der Bibliotheken"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | [[01-projektstruktur\|Modul 01]] abgeschlossen · Maven-Projekt `expedition-tools` vorhanden |
| **Lernziel** | Du kannst Bibliotheken als Dependencies in der pom.xml eintragen, ihre Koordinaten auf Maven Central nachschlagen und Dependency-Scopes korrekt einsetzen. |

---

## Worum geht es?

Alexandrien, ein Wochenmarkt. Lila steht vor einem Händler, der alte Landkarten verkauft — keine selbst gezeichneten Kopien, sondern Originale aus dem 17. Jahrhundert. „Warum alles selbst zeichnen," sagt sie mit einem Lächeln, „wenn andere die Arbeit längst getan haben?"

Dr. Hartwood nickt. „In Java ist das nicht anders. JSON parsen, HTTP-Requests senden, Datumsformate umrechnen — das hat alles schon jemand geschrieben. Wir müssen das Rad nicht neu erfinden." Er klopft auf die pom.xml auf seinem Schreibblock. „Maven weiß, wo diese fertigen Räder lagern. Man muss nur die richtigen Koordinaten kennen."

Old Finn zieht einen Zettel aus der Jackentasche. Darauf stehen drei Wörter: `groupId. artifactId. version.` „Das reicht. Maven holt sich den Rest selbst."

**Deine Mission:** Lerne, wie du externe Bibliotheken in dein Maven-Projekt einbindest — und was hinter dem Mechanismus steckt.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Maven Central erkunden | 🟢 | ~7 Min. |
| 2 | Erste Dependency einbinden | 🟢 | ~8 Min. |
| 3 | Dependency-Scopes verstehen | 🟡 | ~10 Min. |
| 4 | Test-Dependency hinzufügen | 🟡 | ~11 Min. |
| 5 | Dependency-Baum analysieren | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Maven Central erkunden 🟢

> Old Finn faltet den Zettel auseinander. „Alle Bibliotheken der Java-Welt, an einem Ort. Gratis zugänglich. Maven Central nennen die das. Ich war skeptisch — aber es stimmt."

**Deine Aufgabe:**
Öffne Maven Central und suche nach einer bekannten Bibliothek. Finde dort die Maven-Koordinaten.

**Was du dafür brauchst:**

Maven Central ist das zentrale Repository für Java-Bibliotheken: [search.maven.org](https://search.maven.org)

1. Öffne Maven Central im Browser
2. Suche nach `gson` (eine Bibliothek von Google zum JSON-Parsen)
3. Klicke auf das erste Ergebnis (`com.google.code.gson / gson`)
4. Wähle die neueste stabile Version
5. Klicke auf das kleine **XML-Icon** (oder den Tab „pom.xml") — Maven zeigt dir den fertigen `<dependency>`-Block

Das Ergebnis sieht so aus:
```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.x.x</version>
</dependency>
```

> [!tip] Tipp
> Das kleine Kopier-Symbol neben dem Dependency-Block auf Maven Central kopiert den XML-Ausschnitt direkt in die Zwischenablage — du musst nicht abtippen.

<details>
<summary>📜 Lösungsvorschlag</summary>

Auf [search.maven.org](https://search.maven.org) → Suche nach „gson" → erstes Ergebnis: `com.google.code.gson:gson`

Dort findest du den fertigen XML-Block für die aktuelle Version. Zum Zeitpunkt dieser Einheit war `2.10.1` die stabile Version — prüfe selbst, ob eine neuere verfügbar ist.

</details>

**Zum Nachdenken:** Wer stellt die Bibliotheken auf Maven Central bereit — und wer prüft deren Qualität? Würdest du jeder Bibliothek blind vertrauen?

---

## Aufgabe 2 — Erste Dependency einbinden 🟢

> Lila legt den Dependency-Block auf den Tisch. „Der Händler hat die Koordinaten. Jetzt trägst du sie in die Akte ein, und Maven holt die Ware selbst ab."

**Deine Aufgabe:**
Binde `gson` als Dependency in dein Projekt ein und nutze sie in einer kurzen Java-Klasse.

**Was du dafür brauchst:**

Füge in der `pom.xml` nach dem `<properties>`-Block ein `<dependencies>`-Element ein:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.10.1</version>
    </dependency>
</dependencies>
```

Speichere die Datei. In IntelliJ erscheint oben rechts ein kleines Elefanten-Symbol (Maven-Reload) — klicke darauf. Maven lädt `gson` jetzt herunter.

Dann: Lege eine neue Klasse `JsonBeispiel.java` unter `src/main/java/de/hartwood/` an und nutze Gson:

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
> Wenn IntelliJ `Gson` rot unterstreicht, obwohl du die Dependency eingetragen hast: Klicke auf das Elefant-Symbol (Maven Reload). IntelliJ synchronisiert sich dann mit der pom.xml.

> [!tip] Tipp 2
> Wo werden heruntergeladene Bibliotheken gespeichert? Im **lokalen Maven-Cache**: `~/.m2/repository/`. Einmal heruntergeladen, wird eine Bibliothek von dort wiederverwendet — auch in anderen Projekten.

<details>
<summary>📜 Lösungsvorschlag</summary>

Ausgabe beim Ausführen von `JsonBeispiel`:
```
"Expedition bereit"
```

Gson hat den String als JSON serialisiert (daher die Anführungszeichen).

</details>

**Zum Nachdenken:** Die Bibliothek wurde automatisch heruntergeladen — aber von wo genau? Und was passiert, wenn du offline arbeitest und die Bibliothek noch nicht im lokalen Cache ist?

---

## Aufgabe 3 — Dependency-Scopes verstehen 🟡

> Hartwood schreibt in sein Notizbuch: „Nicht alle Werkzeuge reisen mit ins Feld. Manche brauchen wir nur im Labor — zum Testen, zum Prüfen. Die packen wir nicht in das fertige Paket." Er sieht auf. „Das nennt sich Scope."

**Deine Aufgabe:**
Lese die Tabelle der Dependency-Scopes und beantworte die Fragen darunter.

**Was du dafür brauchst:**

Maven kennt verschiedene **Scopes** — sie legen fest, wann eine Dependency verfügbar ist:

| Scope | Verfügbar beim Kompilieren | Verfügbar beim Testen | Im fertigen Paket (JAR) |
|-------|:---:|:---:|:---:|
| `compile` (Standard) | ✅ | ✅ | ✅ |
| `test` | ❌ | ✅ | ❌ |
| `provided` | ✅ | ✅ | ❌ |
| `runtime` | ❌ | ✅ | ✅ |

**Beispiel-Fälle:**
- **JUnit** (Test-Framework) → `test`: Wird nur für Tests gebraucht, kommt nicht ins fertige Programm
- **Servlet-API** → `provided`: Der Server (Tomcat) stellt sie zur Laufzeit bereit, du brauchst sie nur zum Kompilieren
- **JDBC-Treiber** → `runtime`: Wird nicht direkt im Code importiert, aber zur Laufzeit gebraucht

Wenn kein Scope angegeben wird, gilt `compile` — die Bibliothek ist überall verfügbar und landet im fertigen Paket.

> [!tip] Tipp
> Du kannst den Scope in der pom.xml so angeben:
> ```xml
> <dependency>
>     <groupId>org.junit.jupiter</groupId>
>     <artifactId>junit-jupiter</artifactId>
>     <version>5.10.0</version>
>     <scope>test</scope>
> </dependency>
> ```

<details>
<summary>📜 Lösungsvorschlag</summary>

**Welchen Scope würdest du wählen?**

| Bibliothek | Scope |
|---|---|
| Eine JSON-Bibliothek, die du in deinem Code nutzt | `compile` (Standard) |
| JUnit für deine Tests | `test` |
| Der JDBC-Treiber für die Datenbankverbindung | `runtime` |
| Die Servlet-API beim Entwickeln für Tomcat | `provided` |

</details>

**Zum Nachdenken:** Was wäre das Problem, wenn du JUnit mit dem Scope `compile` einbindest statt `test`? Was würde das für das fertige Programm bedeuten?

---

## Aufgabe 4 — Test-Dependency hinzufügen 🟡

> Lila legt eine neue Kiste auf den Tisch. „Testausrüstung. Bleibt im Labor." Sie tippt auf die Aufschrift: **JUnit 5**. „Die packen wir mit Scope test ein."

**Deine Aufgabe:**
Füge JUnit 5 als Test-Dependency hinzu und schreibe einen ersten Unit-Test für eine einfache Methode.

**Was du dafür brauchst:**

Füge in der `pom.xml` folgende Dependency hinzu:

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

Lege dann unter `src/test/java/de/hartwood/` eine Testklasse an:

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

Starte den Test mit dem grünen Pfeil neben der Methode in IntelliJ.

> [!tip] Tipp
> IntelliJ zeigt Testklassen im Projektbaum unter `src/test/java` an — dort landen sie automatisch, wenn du sie im richtigen Verzeichnis anlegst.

<details>
<summary>📜 Lösungsvorschlag</summary>

Der Test sollte grün werden — Gson gibt `"test"` (mit Anführungszeichen) zurück, weil JSON-Strings immer in Anführungszeichen stehen.

Falls IntelliJ den Test nicht findet: Prüfe, ob die Testklasse im richtigen Paket unter `src/test/java/de/hartwood/` liegt.

</details>

**Zum Nachdenken:** Warum landet JUnit nicht im fertigen Programm, obwohl du es als Dependency eingetragen hast? Was passiert mit `scope=test` beim Build?

---

## Aufgabe 5 — Dependency-Baum analysieren 🔴

> [!example] Expedition ins Unbekannte
> Old Finn blättert in einem dicken Wälzer. „Jede Bibliothek hat selbst wieder Abhängigkeiten. Und die haben Abhängigkeiten. Und so weiter." Er klappt das Buch zu. „Maven weiß, was von alldem wirklich ins Gepäck muss. Aber du kannst es ihm auch direkt fragen."

**Deine Aufgabe:**
Lass dir den vollständigen Dependency-Baum deines Projekts im Terminal anzeigen und interpretiere die Ausgabe.

**Was du dafür brauchst:**

Führe im Projektverzeichnis aus:
```
mvn dependency:tree
```

Maven zeigt dir alle Dependencies — direkte und transitive (also Dependencies deiner Dependencies). Die Ausgabe sieht in etwa so aus:

```
[INFO] de.hartwood:expedition-tools:jar:1.0-SNAPSHOT
[INFO] +- com.google.code.gson:gson:jar:2.10.1:compile
[INFO] \- org.junit.jupiter:junit-jupiter:jar:5.10.0:test
[INFO]    +- org.junit.jupiter:junit-jupiter-api:jar:5.10.0:test
[INFO]    +- org.junit.jupiter:junit-jupiter-params:jar:5.10.0:test
[INFO]    \- org.junit.jupiter:junit-jupiter-engine:jar:5.10.0:test
```

Beachte: Du hast nur `junit-jupiter` eingetragen — Maven hat automatisch die nötigen Untermodule (API, Params, Engine) hinzugezogen.

> [!info] Weiterführende Quelle
> **Maven Dependency Plugin** — Alle Möglichkeiten zur Dependency-Analyse.
> 🔗 [maven.apache.org/plugins/maven-dependency-plugin](https://maven.apache.org/plugins/maven-dependency-plugin/)

> [!tip] Tipp
> `mvn dependency:tree -Dverbose` zeigt auch Konflikte und ausgeschlossene Dependencies.

<details>
<summary>📜 Lösungsvorschlag</summary>

Die Ausgabe zeigt zwei Ebenen:
- **Direkte Dependencies**: Die, die du selbst in der pom.xml eingetragen hast
- **Transitive Dependencies**: Die, die deine Dependencies brauchen

Das `+--` und `\--` ist die Baumstruktur. Rechts steht der Scope (`:compile` oder `:test`).

</details>

**Zum Nachdenken:** Gson hat keine transitiven Dependencies — es ist vollständig eigenständig. Was könnte ein Vorteil davon sein? Was würde es bedeuten, wenn eine Bibliothek sehr viele transitive Dependencies mitbringt?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Dependency auf Maven Central gefunden, in pom.xml eingebunden, Maven-Reload durchgeführt _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — Scopes erklärt, JUnit als Test-Dependency eingebunden, Test geschrieben und bestanden _(sicherer Umgang)_
- [ ] **Profi-Ziel** — Dependency-Baum ausgelesen und transitive Dependencies erklärt _(Transfer geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum ist es sinnvoll, Bibliotheken per pom.xml zu verwalten, anstatt JAR-Dateien manuell herunterzuladen und ins Projekt zu kopieren?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Das Team hat Bibliotheken. Aber wie werden daraus fertige Programme? Im nächsten Modul lernst du den Maven Build Lifecycle — was passiert hinter den Kulissen, wenn du `mvn package` tippst, und warum Tests dabei automatisch laufen.

→ Weiter zu: [[03-build-lifecycle|Modul 03: Build Lifecycle & Phasen]]

---

> _„Halt den Hut fest und die Koordinaten parat."_
> — Old Finn McGraw
