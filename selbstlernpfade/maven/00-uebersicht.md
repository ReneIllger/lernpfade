---
title: Modulreihe — Maven
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
  - ausbildung/tools
lernpfad: Maven
dauer-gesamt: "4 × 45 Min."
zielgruppe: "FIAE & FISI (gemischt)"
voraussetzungen:
  - Java-Grundkenntnisse (Klassen, Methoden)
  - IDE installiert (IntelliJ oder Eclipse)
  - Terminal-Grundkenntnisse (Verzeichnisse, Befehle)
---

# Modulreihe: Maven

## Worum geht es?

In dieser Modulreihe lernst du Maven — das meistgenutzte Build-Tool der Java-Welt. Du verstehst, wie Maven Projekte strukturiert, externe Bibliotheken verwaltet und aus Quellcode fertige Programme baut.

Am Ende der Reihe kannst du selbstständig Maven-Projekte anlegen, Dependencies einbinden, den Build-Lifecycle verstehen und eigene Builds konfigurieren.

---

## Module im Überblick

| Nr. | Titel | Inhalt | Typ |
|-----|-------|--------|-----|
| [[01-projektstruktur\|01]] | Was ist Maven & die pom.xml | Warum Maven, Standardstruktur, GroupId/ArtifactId/Version | Pflicht |
| [[02-dependencies\|02]] | Dependencies verwalten | Maven Central, G:A:V-Koordinaten, Scopes | Pflicht |
| [[03-build-lifecycle\|03]] | Build Lifecycle & Phasen | compile, test, package, install — was wann passiert | Pflicht |
| [[04-plugins\|04]] | Plugins & Konfiguration | Compiler-Plugin, ausführbares JAR, Plugin-Konfiguration | Vertiefung |

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
| [Apache Maven](https://maven.apache.org) | Build-Tool & Dependency-Management |
| [Maven Central](https://search.maven.org) | Repository für Java-Bibliotheken |
| IntelliJ IDEA | IDE mit integrierter Maven-Unterstützung |
