---
title: "Modul 02 — Nachrichten empfangen & antworten"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
modul-nr: 2
dauer: 45 min
voraussetzungen:
  - "[[01-projektsetup|Modul 01]] abgeschlossen — Bot ist online, Maven-Projekt läuft, JDA eingebunden"
---

# Modul 02 — Nachrichten empfangen & antworten
_„Der erste Ruf aus Luxor"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | [[01-projektsetup\|Modul 01]] abgeschlossen · IntelliJ IDEA mit Edu-Lizenz · OpenJDK 25 · JDA 5.x eingebunden · Bot-Token vorhanden |
| **Lernziel** | Du kannst einen EventListener in JDA implementieren, auf eingehende Nachrichten reagieren und einfache Prefix-Commands verarbeiten. |

---

## Worum geht es?

Der Agent ist online. Hartwood sieht es an der grünen Lampe auf seinem Gerät. Aber als Lila eine Testnachricht aus dem Kairoer Büro schickt — „Vega startklar. Warte auf Koordinaten." — passiert: nichts. Der Agent empfängt, aber er antwortet nicht. Er hört nicht zu.

„Ein Bote, der nur schweigt, ist kein Bote", sagt Old Finn und klopft die Asche aus seiner Pfeife. „Er muss auf Ereignisse reagieren. Jede eingehende Nachricht ist ein Ereignis. Und auf jedes Ereignis kann eine Antwort folgen."

Er zeichnet mit dem Finger eine Linie durch den Staub auf dem Tisch:

```
Discord-Nachricht  →  Ereignis (Event)  →  dein Code  →  Antwort
```

**Das ist die Grundlage von allem, was dieser Agent tun wird.** In diesem Modul bringst du ihn zum ersten Mal zum Sprechen.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Das Event-System verstehen | 🟢 | ~7 Min. |
| 2 | Ersten Listener registrieren | 🟢 | ~8 Min. |
| 3 | Ping → Pong: auf eine bestimmte Nachricht reagieren | 🟡 | ~10 Min. |
| 4 | Prefix-Commands: mehrere Befehle verarbeiten | 🟡 | ~10 Min. |
| 5 | Den eigenen Bot ignorieren | 🔴 | ~10 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Das Event-System verstehen 🟢

> Old Finn lehnt sich zurück und zieht an seiner Pfeife. „Stell dir vor, du sitzt an einem Telegraphen-Empfänger. Jedes Mal, wenn ein Signal reinkommt, klopft es. Manchmal ist es eine Nachricht. Manchmal ein Verbindungstest. Manchmal tritt jemand dem Kanal bei." Er tippt auf Hartwoods Notizbuch. „JDA macht dasselbe. Es klopft. Du entscheidest, worauf du antwortest."

**Deine Aufgabe:**
Lies die folgende Erklärung aufmerksam durch und beantworte danach die Frage am Ende — schriftlich in deinem eigenen Notizbuch oder in einer Textdatei.

**Was du dafür brauchst:**

JDA arbeitet **ereignisgesteuert** (*event-driven*). Das bedeutet: Dein Code läuft nicht in einer Schleife, die ständig prüft „Kam gerade eine Nachricht?". Stattdessen meldet Discord jedes Ereignis automatisch — und JDA ruft dafür eine Methode in deinem Code auf.

Ein **Event** ist dabei ein Objekt, das alle Informationen zu einem Ereignis enthält. Zum Beispiel:

| Event-Klasse | Wird ausgelöst, wenn… |
|---|---|
| `MessageReceivedEvent` | eine Nachricht in einem Channel gesendet wird |
| `GuildMemberJoinEvent` | jemand dem Server beitritt |
| `ReadyEvent` | der Bot vollständig verbunden ist |

Damit JDA weiß, welche Events dich interessieren, registrierst du einen **EventListener** — eine Klasse, die auf bestimmte Events „hört" und darauf reagiert.

Das Grundprinzip in Java:

```
          Discord
             │
             ▼
         JDA-Kern
             │  (löst Events aus)
             ▼
      dein EventListener
             │  (deine Methoden werden aufgerufen)
             ▼
         dein Code
```

> [!tip] Tipp
> Dieses Muster heißt **Observer-Pattern** — ein wichtiges Entwurfsmuster in der Softwareentwicklung. Wenn du mehr darüber lesen möchtest: Merke dir den Begriff für später.

<details>
<summary>📜 Lösungsvorschlag</summary>

Hier kein Code — das ist eine Verständnisaufgabe. Eine gute Antwort auf die Reflexionsfrage könnte so aussehen:

_„Ereignisgesteuert bedeutet, dass mein Code nicht aktiv nach Änderungen sucht, sondern passiv wartet und nur dann ausgeführt wird, wenn etwas passiert. Das spart Rechenzeit und macht den Code übersichtlicher, weil jede Reaktion an einer eigenen Stelle definiert ist."_

</details>

**Zum Nachdenken:** Welchen Vorteil hat das ereignisgesteuerte Modell gegenüber einer Schleife, die alle 100 Millisekunden prüft „Gibt es neue Nachrichten?" — und welchen Nachteil könnte es haben?

---

## Aufgabe 2 — Ersten Listener registrieren 🟢

> Lila schaut über deine Schulter auf den Code. „Also: Du schreibst eine Klasse, die lauscht. Und du sagst dem Agenten: Diese Klasse ist dein Ohr." Sie tippt auf IntelliJ. „Mach's."

**Deine Aufgabe:**
Erstelle eine neue Klasse `NachrichtenListener`, die von `ListenerAdapter` erbt. Registriere sie beim Bot in deiner `Main`-Klasse. Der Bot soll beim Start in der Konsole ausgeben, dass der Listener aktiv ist.

**Was du dafür brauchst:**

JDA stellt die abstrakte Klasse `ListenerAdapter` bereit. Sie enthält für jedes Event eine leere Methode, die du nach Bedarf überschreibst (*Override*). Du musst nicht alle Methoden implementieren — nur die, die dich interessieren.

```java
public class NachrichtenListener extends ListenerAdapter {
    // hier überschreibst du später einzelne Methoden
}
```

Den Listener registrierst du beim `JDABuilder` in der `Main`-Klasse:

```java
JDABuilder.createDefault("DEIN_TOKEN")
        .addEventListeners(new NachrichtenListener())
        .enableIntents(GatewayIntent.MESSAGE_CONTENT)
        .build();
```

> [!tip] Tipp
> Du kannst mehrere Listener mit `.addEventListeners(listener1, listener2, ...)` auf einmal registrieren — praktisch, wenn dein Bot später viele Funktionen hat.

<details>
<summary>📜 Lösungsvorschlag</summary>

**NachrichtenListener.java:**
```java
package de.hartwood;

import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class NachrichtenListener extends ListenerAdapter {
    // Events folgen in den nächsten Aufgaben
}
```

**Main.java** (aktualisiert):
```java
package de.hartwood;

import net.dv8tion.jda.api.JDA;
import net.dv8tion.jda.api.JDABuilder;
import net.dv8tion.jda.api.requests.GatewayIntent;

public class Main {

    public static void main(String[] args) throws InterruptedException {
        JDA jda = JDABuilder.createDefault("DEIN_TOKEN_HIER")
                .enableIntents(GatewayIntent.MESSAGE_CONTENT)
                .addEventListeners(new NachrichtenListener())
                .build();

        jda.awaitReady();
        System.out.println("Agent online. Listener aktiv.");
    }
}
```

</details>

**Zum Nachdenken:** Warum erbt `NachrichtenListener` von `ListenerAdapter` und implementiert nicht direkt das Interface `EventListener`? Was ist der praktische Unterschied?

---

## Aufgabe 3 — Ping → Pong: auf eine bestimmte Nachricht reagieren 🟡

> Die erste Nachricht aus Luxor trifft ein. Hartwood starrt auf den Bildschirm — der Agent schweigt. „Er empfängt", murmelt er. „Aber er antwortet nicht." Old Finn grinst. „Weil du ihm noch nicht gesagt hast, was er antworten soll. Fang einfach an: Wenn jemand 'Ping' schickt — antwortet er 'Pong'."

**Deine Aufgabe:**
Überschreibe in deinem `NachrichtenListener` die Methode `onMessageReceived`. Wenn die empfangene Nachricht exakt `!ping` lautet, soll der Bot mit `Pong!` antworten.

**Was du dafür brauchst:**

> [!warning] MESSAGE_CONTENT Intent aktivieren
> Damit `getContentRaw()` den Nachrichtentext liefert, muss der Intent in zwei Schritten aktiviert sein:
> 1. Im Discord Developer Portal unter „Bot" → **Message Content Intent** einschalten (hast du in Modul 01 erledigt)
> 2. Im Code beim `JDABuilder` → `.enableIntents(GatewayIntent.MESSAGE_CONTENT)` setzen
>
> Fehlt einer der beiden Schritte, erscheint eine WARN-Meldung im Log und `getContentRaw()` gibt immer einen leeren String zurück.

Die Methode, die bei jeder eingehenden Nachricht aufgerufen wird:

```java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    // event enthält alle Informationen zur Nachricht
}
```

Aus dem `event`-Objekt holst du dir:

| Ausdruck | Liefert |
|---|---|
| `event.getMessage().getContentRaw()` | Den Nachrichtentext (unformatiert) |
| `event.getChannel()` | Den Channel, in dem die Nachricht ankam |
| `event.getChannel().sendMessage("Text")` | Sendet eine Antwort in denselben Channel |
| `.queue()` | Führt das Senden asynchron aus — **immer anhängen!** |

> [!tip] Tipp 1
> `.queue()` am Ende des `sendMessage()`-Aufrufs nicht vergessen. Ohne `.queue()` passiert buchstäblich nichts — JDA führt Aktionen nicht automatisch aus, du musst sie explizit „einreihen".

> [!tip] Tipp 2
> Nutze `equals()` für den Vergleich von Strings — nicht `==`. In Java vergleicht `==` bei Objekten die Speicheradresse, nicht den Inhalt.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
package de.hartwood;

import net.dv8tion.jda.api.events.message.MessageReceivedEvent;
import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class NachrichtenListener extends ListenerAdapter {

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        String nachricht = event.getMessage().getContentRaw();

        if (nachricht.equals("!ping")) {
            event.getChannel().sendMessage("Pong!").queue();
        }
    }
}
```

Starte den Bot, gehe in deinen Testserver und schreibe `!ping` in einen Channel. Der Agent sollte mit `Pong!` antworten.

</details>

**Zum Nachdenken:** Was passiert, wenn du `!PING` (in Großbuchstaben) schreibst? Wie könntest du den Befehl schreib­unempfindlich machen — und wäre das sinnvoll?

---

## Aufgabe 4 — Prefix-Commands: mehrere Befehle verarbeiten 🟡

> Hartwood schreibt in sein Notizbuch: `!ping`, `!status`, `!hilfe`. „Wir brauchen mehr als ein Stichwort. Der Agent muss einen Befehlssatz kennen." Er schaut auf. „Und er muss wissen, dass alles mit einem Ausrufezeichen ein Befehl ist — alles andere ignoriert er."

**Deine Aufgabe:**
Erweitere deinen Listener um zwei weitere Befehle:
- `!status` → Der Bot antwortet mit einer kurzen Statusmeldung (z.B. „Agent aktiv. Koordinaten werden übertragen.")
- `!hilfe` → Der Bot antwortet mit einer Liste der verfügbaren Befehle

Strukturiere den Code so, dass du leicht weitere Befehle ergänzen kannst — ohne die `onMessageReceived`-Methode immer länger werden zu lassen.

**Was du dafür brauchst:**

Eine `if-else-if`-Kette funktioniert für wenige Befehle. Für mehr Übersicht kannst du `switch` verwenden — in modernem Java (14+) auch als **Switch-Expression**:

```java
switch (nachricht) {
    case "!ping"   -> event.getChannel().sendMessage("Pong!").queue();
    case "!status" -> event.getChannel().sendMessage("...").queue();
    default        -> { /* unbekannter Befehl — erstmal ignorieren */ }
}
```

Alternativ: Lagere jeden Befehl in eine eigene private Methode aus.

> [!tip] Tipp
> Prüfe zuerst, ob die Nachricht überhaupt mit `!` beginnt — bevor du den vollständigen Vergleich machst. Das spart unnötige Vergleiche bei normalen Unterhaltungen im Channel.
> ```java
> if (!nachricht.startsWith("!")) return;
> ```

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
package de.hartwood;

import net.dv8tion.jda.api.events.message.MessageReceivedEvent;
import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class NachrichtenListener extends ListenerAdapter {

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        String nachricht = event.getMessage().getContentRaw();

        if (!nachricht.startsWith("!")) return;

        switch (nachricht) {
            case "!ping"   -> event.getChannel().sendMessage("Pong!").queue();
            case "!status" -> event.getChannel().sendMessage(
                    "Agent aktiv. Koordinaten werden übertragen.").queue();
            case "!hilfe"  -> event.getChannel().sendMessage(
                    "Verfügbare Befehle: `!ping`, `!status`, `!hilfe`").queue();
            default        -> { /* unbekannter Befehl */ }
        }
    }
}
```

</details>

**Zum Nachdenken:** Der `switch` wird mit jedem neuen Befehl länger. Welche Alternative aus der objektorientierten Programmierung könnte das lösen, wenn der Bot irgendwann 20 oder 30 Befehle kennt?

---

## Aufgabe 5 — Den eigenen Bot ignorieren 🔴

> [!example] Expedition ins Unbekannte
> Hartwood runzelt die Stirn. „Da ist ein Problem." Er dreht den Bildschirm zu Old Finn. Der Agent hat auf seine eigene `!hilfe`-Antwort reagiert — und dann auf die Antwort der Antwort. Eine Endlosschleife aus Nachrichten, bis der Server die Verbindung trennte. Old Finn pfeift leise durch die Zähne. „Klassisch. Er hört sich selbst zu."

**Deine Aufgabe:**
Ein Bot, der auf seine eigenen Nachrichten reagiert, gerät in eine Endlosschleife. Verhindere das: Der Listener soll Nachrichten vom Bot selbst sowie Nachrichten von anderen Bots grundsätzlich ignorieren.

Erweitere außerdem das Verhalten bei unbekannten Befehlen: Statt zu schweigen, soll der Agent antworten — aber nur, wenn die Nachricht wirklich mit `!` beginnt und kein bekannter Befehl ist.

**Was du dafür brauchst:**

Aus dem `event`-Objekt:

| Ausdruck | Liefert |
|---|---|
| `event.getAuthor()` | Der User, der die Nachricht geschrieben hat |
| `event.getAuthor().isBot()` | `true`, wenn der Autor ein Bot ist |
| `event.getJDA().getSelfUser()` | Das eigene Bot-Konto |

> [!info] Weiterführende Quelle
> **JDA Wiki — Using Events** — Erklärt das Event-System ausführlich mit weiteren Beispielen.
> 🔗 [jda.wiki/using-jda/using-events](https://jda.wiki/using-jda/using-events/)

> [!tip] Tipp
> Die Prüfung auf Bot-Nachrichten gehört ganz an den **Anfang** der `onMessageReceived`-Methode — noch vor jeder anderen Logik. Nutze ein frühes `return`, um die Methode sofort zu verlassen.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
package de.hartwood;

import net.dv8tion.jda.api.events.message.MessageReceivedEvent;
import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class NachrichtenListener extends ListenerAdapter {

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        // Bots (inkl. sich selbst) ignorieren
        if (event.getAuthor().isBot()) return;

        String nachricht = event.getMessage().getContentRaw();

        if (!nachricht.startsWith("!")) return;

        switch (nachricht) {
            case "!ping"   -> event.getChannel().sendMessage("Pong!").queue();
            case "!status" -> event.getChannel().sendMessage(
                    "Agent aktiv. Koordinaten werden übertragen.").queue();
            case "!hilfe"  -> event.getChannel().sendMessage(
                    "Verfügbare Befehle: `!ping`, `!status`, `!hilfe`").queue();
            default        -> event.getChannel().sendMessage(
                    "Unbekannter Befehl. Schreib `!hilfe` für eine Übersicht.").queue();
        }
    }
}
```

**Warum `isBot()` statt direkter Vergleich mit `getSelfUser()`?**
`isBot()` schließt auch andere Bots aus — nicht nur den eigenen. Das ist in den meisten Fällen das gewünschte Verhalten, da Bots keine Commands an andere Bots schicken sollten.

</details>

**Zum Nachdenken:** Gibt es Szenarien, in denen ein Bot absichtlich auf Nachrichten eines anderen Bots reagieren sollte? Was wäre ein konkretes Beispiel?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Listener ist registriert, Bot reagiert auf `!ping` mit `Pong!` _(Grundprinzip verstanden)_
- [ ] **Gut-Ziel** — Drei Befehle funktionieren, Prefix-Prüfung ist eingebaut _(sicherer Umgang mit Events)_
- [ ] **Profi-Ziel** — Bot ignoriert sich selbst und andere Bots, unbekannte Befehle werden sinnvoll beantwortet _(robuster, produktionsnaher Code)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Prefix-Commands (`!befehl`) sind einfach umzusetzen — aber warum haben die meisten modernen Discord-Bots auf Slash Commands umgestellt? Was könnte der Grund sein?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Der Agent antwortet — aber nur auf einfache Textnachrichten. Discord hat seit 2021 **Slash Commands** als offiziellen Standard für Bot-Befehle eingeführt: `/befehl` statt `!befehl`, direkt ins Discord-Interface integriert, mit Autovervollständigung und Berechtigungssteuerung. Im nächsten Modul rüstet Hartwoods Agent auf diesen Standard um.

→ Weiter zu: [[03-slash-commands|Modul 03: Slash Commands]]

---

> _„Halt den Hut fest und die Endlosschleifen kurz."_
> — Old Finn McGraw
