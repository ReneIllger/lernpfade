---
title: Modulreihe — Gradle
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Gradle
dauer-gesamt: "4 × 45 Min."
zielgruppe: "FIAE & FISI (gemischt)"
voraussetzungen:
  - Java-Grundkenntnisse (Klassen, Methoden)
  - IDE installiert (IntelliJ oder Eclipse)
  - Terminal-Grundkenntnisse (Verzeichnisse, Befehle)
---

# Modulreihe: Gradle

## Worum geht es?

In dieser Modulreihe lernst du Gradle — ein modernes, flexibles Build-Tool für die Java-Welt. Du verstehst, wie Gradle Projekte konfiguriert, externe Bibliotheken verwaltet und aus Quellcode fertige Programme baut.

Am Ende der Reihe kannst du selbstständig Gradle-Projekte anlegen, Dependencies einbinden, den Build-Lifecycle verstehen und eigene Tasks schreiben.

> [!info] Kein Vorwissen nötig
> Diese Modulreihe setzt kein Wissen über Maven oder andere Build-Tools voraus. Build-Konzepte werden von Grund auf erklärt.

---

## Module im Überblick

| Nr. | Titel | Inhalt | Typ |
|-----|-------|--------|-----|
| [[01-projektstruktur\|01]] | Was ist Gradle & build.gradle | Warum Gradle, Standardstruktur, erste Konfiguration | Pflicht |
| [[02-dependencies\|02]] | Dependencies verwalten | Repositories, Koordinaten, Configurations | Pflicht |
| [[03-tasks\|03]] | Tasks & Build Lifecycle | Eingebaute Tasks, eigene Tasks, Abhängigkeiten zwischen Tasks | Pflicht |
| [[04-plugins\|04]] | Plugins & Konfiguration | Java-Plugin, Application-Plugin, Gradle Wrapper | Vertiefung |

> [!info] Hinweis zur Reihenfolge
> Module 01–03 bauen aufeinander auf und sollten der Reihe nach bearbeitet werden.
> Modul 04 ist eine optionale Vertiefung und setzt Modul 03 voraus.

---

## Differenzierung

Jedes Modul enthält:
- **Basis-Aufgaben** — Pflicht, mit dem Kerninhalt direkt lösbar
- **Vertiefungs-Aufgabe** — Optional, erfordert Transfer oder Eigeninitiative
- **Hinweise** — Ausklappbare Hilfestellungen, nicht sofort sichtbar

Schwächere Azubis bearbeiten Basis + Reflexion.
Stärkere Azubis bearbeiten zusätzlich die Vertiefung.

---

## Verwendete Technologien

| Technologie | Zweck |
|---|---|
| OpenJDK 17+ | Programmiersprache |
| [Gradle](https://gradle.org) | Build-Tool & Dependency-Management |
| [Maven Central](https://search.maven.org) | Repository für Java-Bibliotheken |
| Groovy DSL | Konfigurationssprache für `build.gradle` |
| IntelliJ IDEA | IDE mit integrierter Gradle-Unterstützung |

> [!note] Groovy vs. Kotlin DSL
> Gradle-Konfigurationen können in **Groovy** (`build.gradle`) oder **Kotlin** (`build.gradle.kts`) geschrieben werden. Diese Modulreihe nutzt Groovy — es ist lesbarer für Einsteiger und in der Praxis weit verbreitet.
