---
title: "Modul 03 — Slash Commands"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
modul-nr: 3
dauer: 45 min
voraussetzungen:
  - "[[02-nachrichten|Modul 02]] abgeschlossen — Listener funktioniert, Prefix-Commands laufen"
---

# Modul 03 — Slash Commands
_„Das offizielle Protokoll"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[02-nachrichten\|Modul 02]] abgeschlossen · IntelliJ IDEA mit Edu-Lizenz · OpenJDK 25 · JDA 5.x eingebunden · Bot läuft in Testserver |
| **Lernziel** | Du kannst Slash Commands bei Discord registrieren, mit einem eigenen Listener verarbeiten und Commands mit optionalen Parametern ausstatten. |

---

## Worum geht es?

Drei Tage nach der ersten erfolgreichen Verbindung klopft Old Finn mit einer Neuigkeit an Hartwoods Tür. „Die Netzbetreiber haben das Protokoll geändert." Er legt ein zerknittertes Merkblatt auf den Tisch. „Ausrufezeichen-Codes sind veraltet. Wer jetzt Befehle schickt, nutzt Schrägstriche. Das neue System zeigt jedem Crewmitglied automatisch an, welche Befehle verfügbar sind — und wie sie zu benutzen sind. Kein Raten mehr."

Lila schaut kurz rein, Fliegerkappe schief auf dem Kopf. „Ich will `/status` eintippen und meinen Standort mitschicken können. Dann weiß Hartwood immer, wo die Vega gerade ist." Sie wirft einen Zettel auf den Tisch und verschwindet wieder.

Hartwood nickt. „Dann upgraden wir den Agenten."

**Das lernst du in diesem Modul:** Slash Commands funktionieren anders als Prefix-Commands — sie müssen erst bei Discord *registriert* werden, bevor sie auftauchen. Erst dann kann dein Listener auf sie reagieren.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Slash Commands verstehen — das neue Protokoll | 🟢 | ~7 Min. |
| 2 | `/ping` als Slash Command registrieren | 🟢 | ~8 Min. |
| 3 | `SlashCommandInteractionEvent` verarbeiten | 🟡 | ~10 Min. |
| 4 | Command mit Parameter: `/status [ort]` | 🟡 | ~10 Min. |
| 5 | Ephemeral Replies — Antworten nur für den Absender | 🔴 | ~10 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Slash Commands verstehen — das neue Protokoll 🟢

> Old Finn faltet das Merkblatt auseinander. „Der Unterschied zum alten System ist größer als er aussieht. Früher hat der Agent jede Nachricht gelesen und selbst entschieden, ob sie ein Befehl war. Jetzt meldet er Discord vorab, welche Befehle er kennt. Discord zeigt sie an. Der Nutzer wählt. Erst dann kommt der Ruf beim Agenten an."

**Deine Aufgabe:**
Lies die Erklärung durch und beantworte die Frage am Ende in eigenen Worten.

**Was du dafür brauchst:**

Bei Slash Commands läuft der Ablauf in **zwei getrennten Schritten**:

```
Schritt 1 — Registrierung (einmalig beim Start):
  dein Code  →  meldet Discord: "Ich kenne /ping und /status"
  Discord    →  speichert das und zeigt die Commands im UI an

Schritt 2 — Ausführung (bei jeder Nutzung):
  Nutzer tippt /ping
  Discord    →  schickt ein Event an deinen Bot
  dein Code  →  verarbeitet das SlashCommandInteractionEvent
```

Daraus folgen zwei wichtige Unterschiede zu Prefix-Commands:

| | Prefix-Command (`!ping`) | Slash Command (`/ping`) |
|---|---|---|
| Registrierung nötig? | Nein | **Ja** |
| Sichtbar im UI? | Nein | **Ja, mit Autovervollständigung** |
| Parameter strukturiert? | Nein (manuelles Parsen) | **Ja, typisiert** |
| Event-Klasse | `MessageReceivedEvent` | **`SlashCommandInteractionEvent`** |
| Antwort-Methode | `channel.sendMessage()` | **`event.reply()`** |

> [!warning] Wichtig: Guild- vs. Global Commands
> Du kannst Commands **global** (für alle Server) oder **pro Server (Guild)** registrieren.
> - **Guild Commands**: Sofort aktiv — ideal zum Entwickeln und Testen
> - **Global Commands**: Bis zu **1 Stunde Verzögerung** bis sie erscheinen
>
> **Für dieses Modul: immer Guild Commands verwenden.**

> [!tip] Tipp
> `guild.updateCommands()` überschreibt die gesamte Command-Liste des Servers jedes Mal neu. Das ist beim Entwickeln praktisch — im Produktivbetrieb würde man gezielter vorgehen.

<details>
<summary>📜 Lösungsvorschlag</summary>

Keine Code-Aufgabe — eine gute Antwort könnte so lauten:

_„Bei Slash Commands muss ich Discord zuerst mitteilen, welche Befehle mein Bot kennt. Discord zeigt diese dann im Interface an. Erst wenn ein Nutzer einen Befehl auswählt, bekommt mein Bot ein Event. Bei Prefix-Commands liest der Bot jede Nachricht selbst — bei Slash Commands übernimmt Discord die Eingabe."_

</details>

**Zum Nachdenken:** Welchen Vorteil hat es für den Nutzer, dass Slash Commands im Discord-UI angezeigt werden? Und was bedeutet das für die Dokumentation des Bots?

---

## Aufgabe 2 — `/ping` als Slash Command registrieren 🟢

> Hartwood öffnet sein Notizbuch und schreibt den ersten Befehl auf: `/ping`. „Fangen wir damit an. Wenn er das akzeptiert, bauen wir den Rest." Er schiebt das Buch rüber. „Zeig Discord, was der Agent kann."

**Deine Aufgabe:**
Registriere einen Slash Command `/ping` beim Testserver. Dazu erweiterst du die `Main`-Klasse: Nach `jda.awaitReady()` holst du dir die Guild (deinen Testserver) und registrierst dort den Command.

**Was du dafür brauchst:**

Die **Guild-ID** deines Testservers: In Discord → Rechtsklick auf den Server → „Server-ID kopieren" (Entwicklermodus muss aktiviert sein unter Einstellungen → Erweitert).

Commands werden mit `Commands.slash()` definiert:

```java
import net.dv8tion.jda.api.interactions.commands.build.Commands;

guild.updateCommands().addCommands(
    Commands.slash("ping", "Sendet ein Ping an den Agenten")
).queue();
```

> [!tip] Tipp 1
> Den Entwicklermodus aktivierst du in Discord unter: Nutzereinstellungen → Erweitert → Entwicklermodus.

> [!tip] Tipp 2
> `jda.getGuildById("DEINE_GUILD_ID")` kann `null` zurückgeben, wenn der Bot nicht auf dem Server ist. Prüfe das mit einer `if`-Abfrage, bevor du weitermachst.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
package de.hartwood;

import net.dv8tion.jda.api.JDA;
import net.dv8tion.jda.api.JDABuilder;
import net.dv8tion.jda.api.entities.Guild;
import net.dv8tion.jda.api.interactions.commands.build.Commands;

public class Main {

    public static void main(String[] args) throws InterruptedException {
        JDA jda = JDABuilder.createDefault("DEIN_TOKEN_HIER")
                .addEventListeners(new NachrichtenListener())
                .enableIntents(GatewayIntent.MESSAGE_CONTENT)
                .build();

        jda.awaitReady();
        System.out.println("Agent online.");

        Guild guild = jda.getGuildById("DEINE_GUILD_ID_HIER");
        if (guild == null) {
            System.out.println("Testserver nicht gefunden!");
            return;
        }

        guild.updateCommands().addCommands(
            Commands.slash("ping", "Sendet ein Ping an den Agenten")
        ).queue();

        System.out.println("Slash Commands registriert.");
    }
}
```

Nach dem Start: Tippe in deinem Testserver `/` ein — `/ping` sollte jetzt in der Liste erscheinen.

</details>

**Zum Nachdenken:** `updateCommands()` ersetzt die gesamte Command-Liste. Was würde passieren, wenn du den Bot neu startest, aber `updateCommands()` vergisst aufzurufen — und vorher andere Commands registriert waren?

---

## Aufgabe 3 — `SlashCommandInteractionEvent` verarbeiten 🟡

> Der Command erscheint in der Liste — aber wenn Lila `/ping` auswählt, dreht sich Discord im Kreis und zeigt dann: „Der Agent hat nicht geantwortet." Hartwood runzelt die Stirn. „Registrieren reicht nicht. Er muss auch antworten."

**Deine Aufgabe:**
Erstelle eine neue Listener-Klasse `SlashCommandListener`, die `ListenerAdapter` erweitert. Überschreibe `onSlashCommandInteraction` und antworte auf `/ping` mit `Pong!`. Registriere den neuen Listener in `Main`.

**Was du dafür brauchst:**

```java
@Override
public void onSlashCommandInteraction(SlashCommandInteractionEvent event) {
    // Name des aufgerufenen Commands
    String command = event.getName();  // z.B. "ping"

    // Antwort senden
    event.reply("Pong!").queue();
}
```

> [!warning] Jeder Slash Command MUSS beantwortet werden
> Discord erwartet innerhalb von **3 Sekunden** eine Antwort. Antwortet dein Bot nicht, zeigt Discord dem Nutzer eine Fehlermeldung. Stelle sicher, dass jeder `case` in deinem Handler ein `event.reply()` auslöst.

> [!tip] Tipp 1
> Nutze `event.getName()` mit einem `switch`, um übersichtlich auf verschiedene Commands zu reagieren — genau wie in Modul 02.

> [!tip] Tipp 2
> Den neuen Listener beim `JDABuilder` in `Main` mit `.addEventListeners()` hinzufügen — du kannst ihn mit Komma neben den bestehenden `NachrichtenListener` setzen.

<details>
<summary>📜 Lösungsvorschlag</summary>

**SlashCommandListener.java:**
```java
package de.hartwood;

import net.dv8tion.jda.api.events.interaction.command.SlashCommandInteractionEvent;
import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class SlashCommandListener extends ListenerAdapter {

    @Override
    public void onSlashCommandInteraction(SlashCommandInteractionEvent event) {
        switch (event.getName()) {
            case "ping" -> event.reply("Pong!").queue();
            default     -> event.reply("Unbekannter Befehl.").queue();
        }
    }
}
```

**Main.java** (aktualisiert):
```java
JDABuilder.createDefault("DEIN_TOKEN_HIER")
        .addEventListeners(new NachrichtenListener(), new SlashCommandListener())
        .build();
```

</details>

**Zum Nachdenken:** Du hast jetzt zwei Listener: einen für Nachrichten, einen für Slash Commands. Wäre es besser, alles in einer Klasse zu bündeln — oder ist die Trennung sinnvoll? Begründe deine Meinung.

---

## Aufgabe 4 — Command mit Parameter: `/status [ort]` 🟡

> Lilas Zettel liegt noch auf dem Tisch. „`/status` mit Standort", steht drauf. Hartwood hebt ihn auf. „Der Agent soll den Ort, den Lila mitschickt, in der Antwort wiederholen. Damit alle im Kanal sehen, wo die Vega ist." Er schreibt darunter: _Optional. Wenn kein Ort angegeben, dann 'Standort unbekannt'._

**Deine Aufgabe:**
Registriere einen zweiten Slash Command `/status` mit einem **optionalen** String-Parameter `ort`. Der Bot antwortet mit: `Vega-Status: Standort [ort]` — oder `Vega-Status: Standort unbekannt`, wenn kein Ort übergeben wurde.

**Was du dafür brauchst:**

Parameter werden beim Registrieren mit `.addOption()` definiert:

```java
import net.dv8tion.jda.api.interactions.commands.OptionType;

Commands.slash("status", "Statusmeldung der Vega")
    .addOption(OptionType.STRING, "ort", "Aktueller Standort", false)
    //                                                          ^^^^
    //                                            false = optional
```

Im Handler holst du den Wert mit `event.getOption()`. Diese Methode gibt `null` zurück, wenn der Parameter nicht übergeben wurde:

```java
OptionMapping ortOption = event.getOption("ort");
String ort = (ortOption != null) ? ortOption.getAsString() : "unbekannt";
```

> [!tip] Tipp 1
> Vergiss nicht, `/status` auch in `guild.updateCommands()` einzutragen — zusammen mit `/ping`. `updateCommands()` ersetzt die komplette Liste, also müssen immer alle Commands übergeben werden.

> [!tip] Tipp 2
> `OptionType.STRING` ist für freien Text. Es gibt auch `OptionType.INTEGER`, `OptionType.BOOLEAN`, `OptionType.USER` und weitere — je nachdem, was du vom Nutzer erwartest.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Main.java** — aktualisierte Command-Registrierung:
```java
guild.updateCommands().addCommands(
    Commands.slash("ping", "Sendet ein Ping an den Agenten"),
    Commands.slash("status", "Statusmeldung der Vega")
            .addOption(OptionType.STRING, "ort", "Aktueller Standort", false)
).queue();
```

**SlashCommandListener.java** — aktualisierter Handler:
```java
@Override
public void onSlashCommandInteraction(SlashCommandInteractionEvent event) {
    switch (event.getName()) {
        case "ping" -> event.reply("Pong!").queue();
        case "status" -> {
            OptionMapping ortOption = event.getOption("ort");
            String ort = (ortOption != null) ? ortOption.getAsString() : "unbekannt";
            event.reply("Vega-Status: Standort " + ort).queue();
        }
        default -> event.reply("Unbekannter Befehl.").queue();
    }
}
```

</details>

**Zum Nachdenken:** Was wäre der Unterschied, wenn du den Parameter auf `true` (required/pflicht) setzt? Wann wäre das sinnvoll — und wann nicht?

---

## Aufgabe 5 — Ephemeral Replies — Antworten nur für den Absender 🔴

> [!example] Expedition ins Unbekannte
> Hartwood hält inne. „Es gibt Informationen, die nicht für alle bestimmt sind." Er denkt an den Maharadscha-Tresor, an die Koordinaten der zweiten Grabungsstätte, die niemand außerhalb der Crew kennen soll. „Wenn Lila `/status` schickt, muss das nicht jeder im Kanal lesen. Nur sie soll die Bestätigung sehen."

**Deine Aufgabe:**
Füge dem `/status`-Command eine **Ephemeral Reply** hinzu: Die Antwort soll nur für die Person sichtbar sein, die den Command ausgeführt hat — nicht für alle anderen im Channel.

Ergänze außerdem einen neuen Command `/expedition` mit zwei Pflicht-Parametern: `ziel` (String) und `dauer` (Integer, Anzahl Tage). Der Bot antwortet mit einer strukturierten Meldung — und auch diese Antwort soll ephemeral sein.

**Was du dafür brauchst:**

Ephemeral Replies werden mit `.setEphemeral(true)` markiert:

```java
event.reply("Nur du siehst das.").setEphemeral(true).queue();
```

Pflicht-Parameter in der Registrierung:
```java
.addOption(OptionType.STRING,  "ziel",  "Ziel der Expedition",   true)  // true = Pflicht
.addOption(OptionType.INTEGER, "dauer", "Dauer in Tagen",        true)
```

> [!info] Weiterführende Quelle
> **JDA Wiki — Slash Commands** — vollständige Dokumentation zu Commands, Optionen und Interaction-Replies.
> 🔗 [jda.wiki/using-jda/interactions/application-commands/slash-commands](https://jda.wiki/using-jda/interactions/application-commands/slash-commands/)

> [!tip] Tipp
> Pflicht-Parameter liefern nie `null` — du kannst `event.getOption("ziel").getAsString()` direkt aufrufen, ohne `null`-Prüfung.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Registrierung (Main.java):**
```java
guild.updateCommands().addCommands(
    Commands.slash("ping", "Sendet ein Ping an den Agenten"),
    Commands.slash("status", "Statusmeldung der Vega")
            .addOption(OptionType.STRING, "ort", "Aktueller Standort", false),
    Commands.slash("expedition", "Neue Expedition melden")
            .addOption(OptionType.STRING,  "ziel",  "Ziel der Expedition", true)
            .addOption(OptionType.INTEGER, "dauer", "Dauer in Tagen",      true)
).queue();
```

**Handler (SlashCommandListener.java):**
```java
@Override
public void onSlashCommandInteraction(SlashCommandInteractionEvent event) {
    switch (event.getName()) {
        case "ping" -> event.reply("Pong!").queue();

        case "status" -> {
            OptionMapping ortOption = event.getOption("ort");
            String ort = (ortOption != null) ? ortOption.getAsString() : "unbekannt";
            event.reply("Vega-Status: Standort " + ort)
                 .setEphemeral(true)
                 .queue();
        }

        case "expedition" -> {
            String ziel  = event.getOption("ziel").getAsString();
            long   dauer = event.getOption("dauer").getAsLong();
            event.reply("Expedition geplant nach **" + ziel + "** — Dauer: " + dauer + " Tage.")
                 .setEphemeral(true)
                 .queue();
        }

        default -> event.reply("Unbekannter Befehl.").queue();
    }
}
```

</details>

**Zum Nachdenken:** Ephemeral Replies verschwinden, sobald der Nutzer den Client neu lädt. Für welche Arten von Bot-Antworten wären sie geeignet — und für welche eher nicht?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — `/ping` ist registriert und wird im Discord-UI angezeigt, der Bot antwortet darauf _(Grundprinzip verstanden)_
- [ ] **Gut-Ziel** — `/status` mit optionalem `ort`-Parameter funktioniert, Handler ist sauber strukturiert _(sicherer Umgang mit Slash Commands)_
- [ ] **Profi-Ziel** — Ephemeral Replies und Pflicht-Parameter sind umgesetzt, `/expedition` funktioniert vollständig _(produktionsnaher Code)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Du hast jetzt zwei Wege, Befehle zu verarbeiten: Prefix-Commands und Slash Commands. Wann würdest du welchen Weg wählen — und gibt es überhaupt noch einen sinnvollen Einsatzfall für Prefix-Commands?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Der Agent antwortet — aber nur mit nacktem Text. Discord kann viel mehr: formatierte Nachrichten mit Farben, Feldern und Struktur, sogenannte **Embeds**. Im nächsten Modul lernt Hartwoods Crew, wie Expeditionsberichte als übersichtliche Karten im Chat erscheinen — statt als langer Textblock.

→ Weiter zu: [[04-embeds|Modul 04: Embeds & formatierte Ausgaben]]

---

> _„Halt den Hut fest und die Parameter niemals ohne Prüfung."_
> — Old Finn McGraw
