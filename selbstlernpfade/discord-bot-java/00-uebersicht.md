---
title: Modulreihe — Discord Bot mit Java
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
dauer-gesamt: "5 × 45 Min."
zielgruppe: "FIAE & FISI (gemischt)"
voraussetzungen:
  - Java-Grundkenntnisse (Variablen, Methoden, Klassen)
  - IDE installiert (IntelliJ oder Eclipse)
  - Maven-Grundkenntnisse von Vorteil, aber nicht zwingend
---

# Modulreihe: Discord Bot mit Java

## Worum geht es?

In dieser Modulreihe baust du Schritt für Schritt einen funktionierenden Discord-Bot mit Java. Du lernst dabei, wie externe Bibliotheken eingebunden werden, wie ereignisgesteuerte Programmierung funktioniert und wie du eine echte Anwendung mit einer API verbindest.

Am Ende der Reihe hast du einen Bot, der auf Slash Commands reagiert und formatierte Nachrichten sendet.

---

## Module im Überblick

| Nr. | Titel | Inhalt | Typ |
|-----|-------|--------|-----|
| [[01-projektsetup\|01]] | Projektsetup & Bot registrieren | Discord Developer Portal, JDA-Dependency einbinden, Bot starten | Pflicht |
| [[02-nachrichten\|02]] | Nachrichten empfangen & antworten | Event-System, MessageReceivedEvent, erster Ping-Bot | Pflicht |
| [[03-slash-commands\|03]] | Slash Commands | Commands registrieren, Interaktion verarbeiten, Antworten senden | Pflicht |
| [[04-embeds\|04]] | Embeds & formatierte Ausgaben | MessageEmbed, Felder, Farben, wann Embeds sinnvoll sind | Pflicht |
| [[05-externe-api\|05]] | Externe API anbinden | HTTP-Request aus Java, JSON parsen, API-Daten im Bot nutzen | Vertiefung |

> [!info] Hinweis zur Reihenfolge
> Module 01–04 bauen aufeinander auf und sollten der Reihe nach bearbeitet werden.
> Modul 05 ist eine optionale Vertiefung und setzt Modul 03 voraus.

---

## Differenzierung

Jedes Modul enthält:
- **Basis-Aufgabe** — Pflicht, mit dem Kerninhalt direkt lösbar
- **Vertiefungs-Aufgabe** — Optional, erfordert Transfer oder Eigeninitiative
- **Hinweise** — Ausklappbare Hilfestellungen, nicht sofort sichtbar

Schwächere Azubis bearbeiten Basis + Reflexion.
Stärkere Azubis bearbeiten zusätzlich die Vertiefung.

---

## Verwendete Technologien

| Technologie | Zweck |
|---|---|
| OpenJDK 25 | Programmiersprache |
| [JDA (Java Discord API)](https://github.com/discord-jda/JDA) | Discord-Bibliothek |
| Maven | Build-Tool & Dependency-Management |
| Discord Developer Portal | Bot-Registrierung & Token |
