---
title: Build-Tools — Einführung & Vergleich (Maven & Gradle)
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
zielgruppe: "FIAE & FISI (gemischt)"
voraussetzungen:
  - Java-Grundkenntnisse (Klassen, Methoden)
  - Terminal-Grundkenntnisse
---

# Build-Tools — Einführung & Vergleich
_„Werkzeuge, die den Unterschied machen"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java-Tools |
| **Typ** | Einstiegsseite & Überblick |
| **Zielgruppe** | FIAE & FISI |
| **Lernziel** | Du kannst erklären, wozu Build-Tools dienen, was Maven und Gradle gemeinsam haben und worin sie sich unterscheiden. |

---

## Die Geschichte dahinter

Kairo, Frühjahr 1938. Dr. Vincent Hartwood läuft mit einem Stapel Papiere durch den Lesesaal. Dreißig Java-Quelldateien, vier externe Bibliotheken, zwei Entwickler — und kein Mensch weiß, in welcher Reihenfolge das alles zusammengebaut werden soll. „Wir haben vier externe Bibliotheken", murmelt er. „Manuell heruntergeladen. In einem Ordner namens `libs/`. Und die Hälfte davon stimmt nicht mehr mit der Version überein, die Ahmad in Kairo verwendet."

Lila McGovern stellt eine Tasse Tee auf den Stapel. „Das ist ein bekanntes Problem. Und es gibt eine bekannte Lösung." Sie dreht einen Zettel um. Darauf: **Build-Tools**.

Old Finn McGraw tippt an seinen Hut. „Jedes professionelle Java-Projekt, das ich kenne, nutzt eines. Entweder **Maven** oder **Gradle**. Wer keines nutzt, verbringt mehr Zeit mit dem Zusammenbauen als mit dem eigentlichen Programmieren."

---

## Was ist ein Build-Tool?

Ein **Build-Tool** ist ein Programm, das den Prozess des Kompilierens, Testens und Paketierens von Software automatisiert.

Ohne Build-Tool muss man manuell:
1. Alle `.java`-Dateien in der richtigen Reihenfolge mit `javac` kompilieren
2. Externe Bibliotheken herunterladen, speichern und dem Classpath hinzufügen
3. Tests ausführen — mit der richtigen Classpath-Konfiguration
4. Alles zu einer `.jar`-Datei zusammenpacken
5. Das bei jeder Änderung wiederholen

Bei kleinen Projekten mit 2–3 Dateien ist das lästig. Bei einem Projekt mit 200 Dateien und 30 externen Bibliotheken ist es **schlicht nicht mehr machbar**.

> [!example] Das Handwerker-Analogon
> Ein Tischler könnte theoretisch jeden Nagel mit einem Stein einschlagen. Aber er nimmt einen Hammer — nicht weil er den Stein nicht könnte, sondern weil der Hammer schneller, präziser und wiederholbar ist. Build-Tools sind der Hammer der Softwareentwicklung.

---

## Was leisten Build-Tools?

| Aufgabe | Ohne Build-Tool | Mit Build-Tool |
|---------|----------------|----------------|
| **Kompilieren** | `javac` manuell auf jede Datei | `mvn compile` / `gradle compileJava` |
| **Abhängigkeiten** | Manuell herunterladen, versionieren | Automatisch aus dem Internet laden |
| **Tests ausführen** | Classpath manuell zusammenstellen | `mvn test` / `gradle test` |
| **Paketieren** | `jar`-Befehle manuell ausführen | `mvn package` / `gradle jar` |
| **Versionskonsistenz** | „Bei mir läuft's" | Alle nutzen exakt dieselben Versionen |
| **CI/CD-Integration** | Aufwändige Skripte | Direkte Unterstützung |

> [!info] Dependency Management
> Das wichtigste Feature von Build-Tools ist oft das **Dependency Management**: Statt Bibliotheken manuell herunterzuladen, trägt man sie einfach in eine Konfigurationsdatei ein — das Tool lädt sie automatisch in der richtigen Version aus dem Internet. Nie wieder: „Welche Version hatten wir nochmal?"

---

## Die zwei großen Werkzeuge: Maven und Gradle

In der Java-Welt haben sich zwei Build-Tools durchgesetzt. Beide lösen dieselben Probleme — mit unterschiedlichen Ansätzen.

### Maven — Konvention über Konfiguration

Maven (2004, Apache) folgt dem Prinzip **Convention over Configuration**: Es gibt eine festgelegte Standardstruktur, einen festgelegten Ablauf (den Build Lifecycle) und eine deklarative Konfigurationsdatei (`pom.xml` in XML).

Wer die Konventionen einhält, muss kaum konfigurieren. Wer davon abweicht, kämpft gegen das Tool.

```xml
<!-- pom.xml — deklarativ, immer gleich aufgebaut -->
<dependencies>
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.12.0</version>
    </dependency>
</dependencies>
```

### Gradle — Flexibilität und Geschwindigkeit

Gradle (2007) geht einen anderen Weg: Die Konfigurationsdatei (`build.gradle`) ist kein XML, sondern **echter Code** (Groovy oder Kotlin). Das macht sie flexibler — und erfordert etwas mehr Verständnis.

Gradle ist außerdem deutlich schneller als Maven, weil es Ergebnisse zwischenspeichert (Incremental Builds) und Tasks parallel ausführen kann.

```groovy
// build.gradle — code-basiert, flexibel
dependencies {
    implementation 'org.apache.commons:commons-lang3:3.12.0'
}
```

---

## Maven vs. Gradle — direkter Vergleich

| Merkmal | Maven | Gradle |
|---------|-------|--------|
| **Konfigurationssprache** | XML (`pom.xml`) | Groovy/Kotlin (`build.gradle`) |
| **Prinzip** | Konvention über Konfiguration | Flexibel, code-basiert |
| **Lernkurve** | Flach — klare Struktur | Etwas steiler — mehr Konzepte |
| **Geschwindigkeit** | Solide | Schneller (Incremental Builds, Cache) |
| **Lesbarkeit** | Ausführlich (XML) | Kompakter, code-ähnlich |
| **Verbreitung** | Sehr weit verbreitet (Enterprise) | Standard bei Android, wächst stark |
| **Erweiterbarkeit** | Über Plugins | Über Plugins + eigene Tasks in Code |
| **Build-Ausgabe** | `target/` | `build/` |
| **Zentrales Repository** | Maven Central | Maven Central (kompatibel) |
| **Wrapper** | Maven Wrapper (mvnw) | Gradle Wrapper (gradlew) |

> [!tip] Was du dir merken sollst
> Beide Tools sind **kompatibel mit Maven Central** — dem weltgrößten Repository für Java-Bibliotheken. Du kannst dieselben Bibliotheken in beiden Tools verwenden, nur die Syntax unterscheidet sich.

---

## Wann welches Tool?

> [!question] Maven oder Gradle?
> In der Praxis entscheidest nicht du — das entscheidet das Projekt, in das du einsteigst. Beide zu kennen ist deshalb wichtiger als eine Präferenz.

**Maven** ist oft die erste Wahl wenn:
- das Projekt schon besteht und Maven nutzt (weitermachen)
- das Team Maven kennt und keine Migration möchte
- einfache, standardisierte Projekte gebaut werden

**Gradle** ist oft die erste Wahl wenn:
- Android-Entwicklung (Gradle ist der Standard)
- das Projekt komplexe Build-Logik braucht
- Build-Geschwindigkeit wichtig ist (CI/CD, große Projekte)
- das Team modernes Tooling bevorzugt

> [!example] In der Ausbildung
> Ihr lernt beide Tools kennen — nicht um eins zu wählen, sondern um in beiden Projektwelten arbeitsfähig zu sein. In vielen Betrieben begegnet euch Maven in gewachsenen Enterprise-Projekten und Gradle in neueren oder Android-Projekten.

---

## Die Lernpfade

Wenn du die Konzepte verstanden hast, geht es in die Praxis:

| Lernpfad | Schwerpunkt | Module |
|----------|-------------|--------|
| [[maven/00-uebersicht\|Modulreihe Maven]] | Deklarativ, XML, Build Lifecycle | 4 Module · 4 × 45 Min. |
| [[gradle/00-uebersicht\|Modulreihe Gradle]] | Code-basiert, Tasks, Wrapper | 4 Module · 4 × 45 Min. |

> [!info] Reihenfolge
> Du kannst mit beiden Lernpfaden unabhängig voneinander starten — Gradle-Modul 01 erklärt Build-Konzepte von Grund auf. Wenn du Maven schon kennst, wirst du bei Gradle vieles wiedererkennen und kannst die Unterschiede aktiv vergleichen.

---

## Zum Nachdenken

> **Warum reicht es nicht, einfach alle `.java`-Dateien in einem Ordner zu sammeln und `javac *.java` aufzurufen?**

Überlege: Was passiert bei 50 Dateien in 20 Unterordnern? Was passiert, wenn eine externe Bibliothek aktualisiert wird? Was passiert, wenn ein neues Teammitglied das Projekt zum ersten Mal aufmacht?

---

> _„Das Werkzeug macht nicht den Programmierer. Aber ein Programmierer ohne Werkzeug ist ein Archäologe ohne Pinsel."_
> — Old Finn McGraw
