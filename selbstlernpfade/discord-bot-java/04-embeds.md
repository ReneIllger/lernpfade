---
title: "Modul 04 — Embeds & formatierte Ausgaben"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
modul-nr: 4
dauer: 45 min
voraussetzungen:
  - "[[03-slash-commands|Modul 03]] abgeschlossen — Slash Commands laufen, `/expedition`-Command ist vorhanden"
---

# Modul 04 — Embeds & formatierte Ausgaben
_„Der Expeditionsbericht"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟨 Fortgeschritten |
| **Voraussetzungen** | [[03-slash-commands\|Modul 03]] abgeschlossen · IntelliJ IDEA mit Edu-Lizenz · OpenJDK 25 · JDA 5.x · `/expedition`-Command aus Modul 03 vorhanden |
| **Lernziel** | Du kannst mit dem `EmbedBuilder` strukturierte, formatierte Discord-Nachrichten erstellen und diese in Slash Commands einbinden. |

---

## Worum geht es?

Old Finn legt Hartwoods letzten Bericht auf den Tisch — drei Zeilen unformatierter Text: „Expedition nach Luxor. 7 Tage. 3 Funde. 840 Liretta." Er schüttelt den Kopf. „Das ist kein Bericht. Das ist ein Zettel vom Gemüsehändler."

Lila schaut von ihrem Schraubenschlüssel auf. „Die Crews in Kairo und Kathmandu können damit nichts anfangen. Wer hat welche Funde gemacht? Was sind die Koordinaten? Wann wurde gesendet?"

Hartwood schlägt sein Notizbuch auf — vollgeschrieben mit sauberen Tabellen, Fundlisten, Datumsangaben und Skizzen. „Genau so soll der Agent berichten. Strukturiert. Lesbar. Mit allem, was man braucht."

**Das lernst du in diesem Modul:** Discord unterstützt sogenannte **Embeds** — formatierte Nachrichtenblöcke mit Titel, Farbe, Feldern und mehr. Du ersetzt die schlichten Textantworten des Agenten durch echte Expeditionsberichte.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Was sind Embeds? | 🟢 | ~6 Min. |
| 2 | Ersten Embed erstellen — Titel, Beschreibung, Farbe | 🟢 | ~9 Min. |
| 3 | Felder hinzufügen — inline und block | 🟡 | ~10 Min. |
| 4 | Embed in den `/expedition`-Command einbauen | 🟡 | ~10 Min. |
| 5 | Vollständiger Expeditionsbericht — Footer, Timestamp & Farbe nach Status | 🔴 | ~10 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Was sind Embeds? 🟢

> Old Finn zieht ein gefaltetes Dokument aus seinem Duster-Mantel. Es sieht aus wie ein offizieller Postboten-Bericht: Kopfzeile, Farbstreifen am Rand, sauber gegliederte Felder. „So kommuniziert jemand, dem seine Informationen etwas wert sind." Er tippt auf das Dokument. „Discord nennt das Embed. Du wirst es bauen."

**Deine Aufgabe:**
Lies die Erklärung durch. Du brauchst noch keinen Code — aber merke dir die Struktur, denn du wirst sie in den nächsten Aufgaben Schritt für Schritt aufbauen.

**Was du dafür brauchst:**

Ein **Embed** in Discord ist eine strukturierte Nachricht mit visuellen Elementen. Es wird von Discord als formatierter Block dargestellt — mit einem farbigen Rand links und klar getrennten Bereichen:

```
┌─────────────────────────────────────────┐
│  [Autor-Icon]  Autor-Name               │
│                                         │
│  Titel                                  │
│  Beschreibung (längerer Fließtext)      │
│                                         │
│  Feld 1 (inline) │ Feld 2 (inline)      │
│  Feld 3 (volle Breite)                  │
│                                         │
│  [Thumbnail]         [Bild unten]       │
│  Footer-Text               Timestamp    │
└─────────────────────────────────────────┘
  ▲
  Farbstreifen (du wählst die Farbe)
```

In JDA baust du Embeds mit dem `EmbedBuilder`:

```java
import net.dv8tion.jda.api.EmbedBuilder;
import java.awt.Color;

EmbedBuilder embed = new EmbedBuilder();
embed.setTitle("Titel hier");
embed.setDescription("Beschreibung hier");
embed.setColor(Color.ORANGE);

// Fertiggestelltes Embed-Objekt:
MessageEmbed fertig = embed.build();
```

> [!tip] Tipp
> `EmbedBuilder` folgt dem **Builder-Pattern**: Du rufst Methoden auf demselben Objekt auf und baust es Schritt für Schritt auf. Am Ende schließt `build()` den Bau ab und gibt ein unveränderliches `MessageEmbed`-Objekt zurück.

<details>
<summary>📜 Lösungsvorschlag</summary>

Keine Code-Aufgabe. Eine gute Beschreibung des Embed-Konzepts:

_„Ein Embed ist eine formatierte Nachricht in Discord, die optisch hervorgehoben wird. Im Gegensatz zu nacktem Text hat sie eine Struktur mit Titel, Beschreibung, Feldern und einem farbigen Rand. In JDA baue ich Embeds mit dem `EmbedBuilder`, der nach dem Builder-Pattern arbeitet."_

</details>

**Zum Nachdenken:** Wann würdest du einen Embed verwenden — und wann reicht eine einfache Textnachricht? Nenne je ein konkretes Beispiel aus dem Expeditionsalltag der Crew.

---

## Aufgabe 2 — Ersten Embed erstellen — Titel, Beschreibung, Farbe 🟢

> Hartwood legt einen frischen Bogen Papier hin. „Fangen wir mit dem Rahmen an. Titel. Kurze Beschreibung. Und die Farbe der Expedition: Goldocker — Wüstensand." Er deutet auf IntelliJ. „Schreib eine Methode, die diesen Rahmen baut. Noch keine Felder, noch kein Inhalt. Erst der Rahmen."

**Deine Aufgabe:**
Schreibe eine private Methode `erstelleTestEmbed()` in deinem `SlashCommandListener`, die einen `EmbedBuilder` aufbaut und als `MessageEmbed` zurückgibt. Titel: `Hartwood Expeditions`, Beschreibung: `Agent bereit. Warte auf Befehle.`, Farbe: Gold (`0xFFD700`). Antworte vorübergehend auf `/ping` mit diesem Embed statt mit `Pong!`.

**Was du dafür brauchst:**

Farben kannst du auf drei Wegen übergeben:
```java
embed.setColor(Color.ORANGE);                // benannte Java-Farbe
embed.setColor(new Color(255, 215, 0));      // RGB-Werte
embed.setColor(0xFFD700);                    // Hex-Wert (direkt als int)
```

Antworte mit einem Embed statt mit Text:
```java
event.replyEmbeds(embed.build()).queue();
```

> [!tip] Tipp
> `replyEmbeds()` für Slash Commands — `channel.sendMessageEmbeds()` für normale Nachrichten. Beide akzeptieren ein oder mehrere `MessageEmbed`-Objekte.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
private MessageEmbed erstelleTestEmbed() {
    EmbedBuilder embed = new EmbedBuilder();
    embed.setTitle("Hartwood Expeditions");
    embed.setDescription("Agent bereit. Warte auf Befehle.");
    embed.setColor(0xFFD700);
    return embed.build();
}
```

Im Handler:
```java
case "ping" -> event.replyEmbeds(erstelleTestEmbed()).queue();
```

</details>

**Zum Nachdenken:** `embed.build()` gibt ein `MessageEmbed` zurück — ein unveränderliches Objekt. Warum macht es Sinn, das Bauen vom Verwenden zu trennen? Was würde passieren, wenn du am `EmbedBuilder` nach `build()` noch Änderungen vornimmst?

---

## Aufgabe 3 — Felder hinzufügen — inline und block 🟡

> Lila lehnt in der Türe. „Titel und Farbe — schön. Aber wo stehen Ort, Dauer, Funde?" Sie zählt an den Fingern ab. „Ich brauche Informationen, die ich auf einen Blick lesen kann. Nebeneinander, wenn sie zusammengehören. Untereinander, wenn es viel Text ist."

**Deine Aufgabe:**
Erweitere `erstelleTestEmbed()` um vier Felder:
- `Standort` und `Dauer` sollen **nebeneinander** erscheinen (inline)
- `Funde` soll die **volle Breite** einnehmen (nicht inline)
- Füge einen `Footer`-Text hinzu: `Hartwood Expeditions — Kairo 1938`

**Was du dafür brauchst:**

```java
// Inline-Feld (erscheint nebeneinander, max. 3 pro Zeile)
embed.addField("Standort", "Luxor", true);
embed.addField("Dauer",    "7 Tage", true);

// Block-Feld (nimmt volle Breite ein)
embed.addField("Funde", "3 Artefakte, darunter ein goldener Skarabäus", false);

// Footer
embed.setFooter("Hartwood Expeditions — Kairo 1938");
```

> [!warning] Limits beachten
> Discord setzt klare Grenzen für Embeds:
> - Maximal **25 Felder** pro Embed
> - Feldname: max. **256 Zeichen**
> - Feldwert: max. **1024 Zeichen**
> - Gesamtlänge des Embeds: max. **6000 Zeichen**
>
> Wer die Limits überschreitet, bekommt von JDA eine Exception.

> [!tip] Tipp 1
> Drei `true`-Felder hintereinander ergeben eine saubere dreispaltige Zeile. Zwei `true`-Felder ergeben eine zweispaltige. Danach bricht Discord automatisch um.

> [!tip] Tipp 2
> Ein leeres Feld (`addField("", "", true)`) kann als unsichtbarer Platzhalter dienen, um das Layout zu steuern — z.B. um eine zweispaltige Zeile links auszurichten.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
private MessageEmbed erstelleTestEmbed() {
    EmbedBuilder embed = new EmbedBuilder();
    embed.setTitle("Hartwood Expeditions");
    embed.setDescription("Agent bereit. Warte auf Befehle.");
    embed.setColor(0xFFD700);

    embed.addField("Standort", "Luxor",   true);
    embed.addField("Dauer",    "7 Tage",  true);
    embed.addField("Funde", "3 Artefakte, darunter ein goldener Skarabäus", false);

    embed.setFooter("Hartwood Expeditions — Kairo 1938");
    return embed.build();
}
```

</details>

**Zum Nachdenken:** Discord zeigt maximal drei Inline-Felder nebeneinander. Was passiert, wenn du vier `true`-Felder hintereinander setzt? Teste es — und erkläre das Ergebnis.

---

## Aufgabe 4 — Embed in den `/expedition`-Command einbauen 🟡

> Hartwood klappt sein Notizbuch zu. „Genug mit dem Testrahmen. Jetzt die echte Arbeit." Er zeigt auf den `/expedition`-Command aus dem letzten Modul. „Die Parameter, die die Crew eingibt — Ziel und Dauer — sollen im Bericht auftauchen. Dynamisch. Nicht fest verdrahtet."

**Deine Aufgabe:**
Ersetze die einfache Textantwort des `/expedition`-Commands durch einen Embed. Die Werte für `ziel` und `dauer` aus den Command-Optionen sollen in den Feldern des Embeds erscheinen. Der Embed-Titel lautet `Expeditionsbericht`, die Farbe ist `0x8B4513` (Sattelbraun — Lederfarbe).

Schreibe dafür eine separate Methode `erstelleExpeditionsBericht(String ziel, long dauer)`, die einen `MessageEmbed` zurückgibt.

**Was du dafür brauchst:**

Du kennst bereits alles, was du brauchst:
- `EmbedBuilder` mit Titel, Farbe, Feldern und Footer
- Parameterwerte holst du aus dem Event: `event.getOption("ziel").getAsString()`
- Antwort per `event.replyEmbeds(...).queue()`

Denke an eine sinnvolle Feldstruktur: Was gehört inline, was braucht die volle Breite?

> [!tip] Tipp
> Lagere die Embed-Erstellung in eine eigene Methode aus — so bleibt der `switch`-Block in `onSlashCommandInteraction` übersichtlich und du kannst den Embed-Builder später leicht erweitern.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Methode:**
```java
private MessageEmbed erstelleExpeditionsBericht(String ziel, long dauer) {
    EmbedBuilder embed = new EmbedBuilder();
    embed.setTitle("Expeditionsbericht");
    embed.setDescription("Neue Expedition wurde gemeldet und bestätigt.");
    embed.setColor(0x8B4513);

    embed.addField("Ziel",  ziel,          true);
    embed.addField("Dauer", dauer + " Tage", true);
    embed.addField("Status", "Geplant", true);

    embed.setFooter("Hartwood Expeditions — Kairo 1938");
    return embed.build();
}
```

**Im Handler:**
```java
case "expedition" -> {
    String ziel  = event.getOption("ziel").getAsString();
    long   dauer = event.getOption("dauer").getAsLong();
    event.replyEmbeds(erstelleExpeditionsBericht(ziel, dauer))
         .setEphemeral(true)
         .queue();
}
```

</details>

**Zum Nachdenken:** Du hast jetzt eine Methode, die einen `MessageEmbed` baut und zurückgibt. Welchen Vorteil hätte es, wenn du diese Methode in eine eigene Klasse `EmbedFactory` auslagern würdest — besonders wenn der Bot später viele verschiedene Embeds produziert?

---

## Aufgabe 5 — Vollständiger Expeditionsbericht — Footer, Timestamp & Farbe nach Status 🔴

> [!example] Expedition ins Unbekannte
> Old Finn zieht einen Brief aus dem Mantel. Absender: das Kairoer Büro. „Hartwood braucht drei verschiedene Berichte: einen für laufende Expeditionen, einen für abgeschlossene, einen für abgebrochene. Die Crews sollen auf einen Blick sehen, was der Status ist — allein an der Farbe." Er legt drei Farbproben auf den Tisch: Gold, Grün, Rot.

**Deine Aufgabe:**
Erweitere den `/expedition`-Command um einen dritten **Pflicht-Parameter** `status` vom Typ `STRING`. Er soll nur die Werte `laufend`, `abgeschlossen` oder `abgebrochen` akzeptieren — nutze dafür **Choices** bei der Command-Registrierung. Die Embed-Farbe richtet sich nach dem Status:

| Status | Farbe |
|---|---|
| `laufend` | `0xFFD700` Gold |
| `abgeschlossen` | `0x2ECC71` Grün |
| `abgebrochen` | `0xE74C3C` Rot |

Füge außerdem einen **Timestamp** (aktueller Zeitpunkt) und eine **Autorenzeile** (`Dr. V. Hartwood`) zum Embed hinzu.

**Was du dafür brauchst:**

**Choices** schränken die erlaubten Werte eines Parameters ein — Discord zeigt sie als Dropdown-Menü an:

```java
import net.dv8tion.jda.api.interactions.commands.Command;

.addOption(OptionType.STRING, "status", "Expeditionsstatus", true,
    // true am Ende = Choices erzwingen
    false)  // nicht autocomplete
// Choices werden separat hinzugefügt:
```

In JDA 5 werden Choices direkt an die Option gehängt:

```java
Commands.slash("expedition", "Neue Expedition melden")
    .addOption(OptionType.STRING, "ziel",   "Ziel",   true)
    .addOption(OptionType.INTEGER,"dauer",  "Dauer",  true)
    .addOptions(new OptionData(OptionType.STRING, "status", "Status", true)
        .addChoices(
            new Command.Choice("Laufend",      "laufend"),
            new Command.Choice("Abgeschlossen","abgeschlossen"),
            new Command.Choice("Abgebrochen",  "abgebrochen")
        ))
```

**Timestamp und Autor:**
```java
embed.setTimestamp(java.time.Instant.now());
embed.setAuthor("Dr. V. Hartwood");
```

> [!info] Weiterführende Quelle
> **JDA Dokumentation — EmbedBuilder** — vollständige Liste aller verfügbaren Methoden mit Beschreibung.
> 🔗 [docs.jda.wiki/net/dv8tion/jda/api/EmbedBuilder.html](https://docs.jda.wiki/net/dv8tion/jda/api/EmbedBuilder.html)

> [!tip] Tipp
> Nutze einen `switch`-Ausdruck, der die Farbe als `int` zurückgibt — dann hältst du die `if-else`-Kette für die Farbwahl kurz und übergibst das Ergebnis direkt an `embed.setColor()`.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Registrierung (Main.java):**
```java
import net.dv8tion.jda.api.interactions.commands.build.OptionData;
import net.dv8tion.jda.api.interactions.commands.Command;

guild.updateCommands().addCommands(
    Commands.slash("ping", "Sendet ein Ping an den Agenten"),
    Commands.slash("status", "Statusmeldung der Vega")
            .addOption(OptionType.STRING, "ort", "Aktueller Standort", false),
    Commands.slash("expedition", "Neue Expedition melden")
            .addOption(OptionType.STRING,  "ziel",  "Ziel der Expedition", true)
            .addOption(OptionType.INTEGER, "dauer", "Dauer in Tagen",      true)
            .addOptions(new OptionData(OptionType.STRING, "status", "Status", true)
                .addChoices(
                    new Command.Choice("Laufend",       "laufend"),
                    new Command.Choice("Abgeschlossen", "abgeschlossen"),
                    new Command.Choice("Abgebrochen",   "abgebrochen")
                ))
).queue();
```

**Methode (SlashCommandListener.java):**
```java
private MessageEmbed erstelleExpeditionsBericht(String ziel, long dauer, String status) {
    int farbe = switch (status) {
        case "abgeschlossen" -> 0x2ECC71;
        case "abgebrochen"   -> 0xE74C3C;
        default              -> 0xFFD700;  // laufend
    };

    EmbedBuilder embed = new EmbedBuilder();
    embed.setAuthor("Dr. V. Hartwood");
    embed.setTitle("Expeditionsbericht");
    embed.setColor(farbe);

    embed.addField("Ziel",    ziel,              true);
    embed.addField("Dauer",   dauer + " Tage",   true);
    embed.addField("Status",  status,             true);

    embed.setFooter("Hartwood Expeditions — Kairo 1938");
    embed.setTimestamp(java.time.Instant.now());
    return embed.build();
}
```

**Im Handler:**
```java
case "expedition" -> {
    String ziel   = event.getOption("ziel").getAsString();
    long   dauer  = event.getOption("dauer").getAsLong();
    String status = event.getOption("status").getAsString();
    event.replyEmbeds(erstelleExpeditionsBericht(ziel, dauer, status))
         .setEphemeral(true)
         .queue();
}
```

</details>

**Zum Nachdenken:** Choices schränken die Eingabe auf der Discord-Seite ein — aber was würde passieren, wenn jemand den Command über die API direkt aufruft und einen ungültigen Wert schickt? Wie könntest du deinen Code gegen diesen Fall absichern?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — `/ping` antwortet mit einem einfachen Embed mit Titel, Beschreibung und Farbe _(Grundprinzip verstanden)_
- [ ] **Gut-Ziel** — `/expedition` gibt einen strukturierten Embed mit Feldern, Footer und den Command-Parametern zurück _(sicherer Umgang mit EmbedBuilder)_
- [ ] **Profi-Ziel** — Status-Choices sind eingebunden, Embed-Farbe reagiert dynamisch auf den Status, Timestamp und Autor sind gesetzt _(produktionsnaher Code)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Du hast die Embed-Erstellung in eine eigene Methode ausgelagert. Welche Vor- und Nachteile hätte es, wenn du stattdessen eine eigene Klasse `EmbedFactory` mit statischen Methoden anlegen würdest — und ab wann würde das Sinn ergeben?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Der Agent berichtet, antwortet und formatiert. Aber er ist noch in sich geschlossen — er weiß nur, was ihm direkt gegeben wird. Im letzten Modul lernt die Crew, wie der Agent die Welt da draußen anzapft: **externe APIs**. Lila braucht aktuelle Wetterdaten für ihre Flugrouten — und Hartwood will wissen, was der aktuelle Kurs einer bestimmten Antiquität auf dem Kairoer Markt ist.

→ Weiter zu: [[05-externe-api|Modul 05: Externe API anbinden]]

---

> _„Halt den Hut fest und die Feldwerte unter tausend Zeichen."_
> — Old Finn McGraw
