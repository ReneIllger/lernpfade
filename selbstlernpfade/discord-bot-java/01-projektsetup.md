---
title: "Modul 01 — Projektsetup & Bot registrieren"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
modul-nr: 1
dauer: 45 min
voraussetzungen:
  - keine (erstes Modul der Reihe)
---

# Modul 01 — Projektsetup & Bot registrieren
_„Das elektrische Auge der Wüste"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟩 Einsteiger |
| **Voraussetzungen** | IntelliJ IDEA installiert & Edu-Lizenz aktiviert · OpenJDK 25 installiert · Discord-Account vorhanden |
| **Lernziel** | Du kannst ein Java-Maven-Projekt anlegen, die JDA-Bibliothek einbinden und einen Discord-Bot zum ersten Mal online bringen. |

---

## Worum geht es?

Kairo, Frühjahr 1938. Dr. Vincent Hartwood sitzt im Hinterzimmer eines staubigen Teehändlers nahe dem Basar und breitet eine verwitterte Karte auf dem Tisch aus. Drei Teams — eines in Luxor, eines am Sinai, eines hier in der Stadt — sollen in den nächsten Wochen koordiniert graben. Das Problem: Telegramme kommen zu spät, Boten verschwinden spurlos, und Old Finn traut keinem der örtlichen Funkamateurfunker.

„Ich kenn' ein Netz", sagt Old Finn und zieht nachdenklich an seiner Pfeife. „Elektrisch. Sicher. Die Schmuggler zwischen Alexandria und Bagdad nutzen es schon seit Monaten." Er schiebt dir einen zerknitterten Zettel rüber. Darauf steht: **Discord**.

Lila lehnt sich über deine Schulter. „Wir bräuchten einen automatischen Agenten im Netz — jemanden, der Nachrichten entgegennimmt und weitergibt, auch wenn keiner von uns gerade am Gerät sitzt." Sie tippt mit dem Finger auf den Zettel. „Das bist du."

**Deine Mission:** Registriere den Bot-Agenten im Netz, richte das Werkzeug ein und bringe ihn zum ersten Mal online. Die Crews in Luxor und am Sinai warten.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | Den Agenten registrieren | 🟢 | ~8 Min. |
| 2 | Werkzeugkasten einrichten — Maven-Projekt anlegen | 🟢 | ~8 Min. |
| 3 | JDA einbinden — die `pom.xml` verstehen | 🟡 | ~10 Min. |
| 4 | Den Bot zum ersten Mal starten | 🟡 | ~10 Min. |
| 5 | Bot in den Expeditions-Server einladen | 🔴 | ~9 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — Den Agenten registrieren 🟢

> Old Finn schiebt dir einen weiteren Zettel rüber — diesmal mit einer Adresse: **discord.com/developers**. „Jeder Agent braucht eine Akte. Leg eine an. Und pass auf den Token auf — wer den hat, spricht in deinem Namen."

**Deine Aufgabe:**
Öffne das Discord Developer Portal und lege dort eine neue Applikation an. Erstelle darin einen Bot und kopiere den **Bot-Token** in eine sichere Textdatei auf deinem Rechner. Aktiviere außerdem die Option **Message Content Intent** in den Bot-Einstellungen.

**Was du dafür brauchst:**

1. Gehe auf [discord.com/developers/applications](https://discord.com/developers/applications) und melde dich an
2. Klicke „New Application" → vergib einen Namen (z.B. `Hartwood-Agent`)
3. Wechsle links zu „Bot" → klicke „Add Bot"
4. Unter „Token": klicke **Reset Token** und kopiere den angezeigten Token sofort — er wird nur einmal im Klartext angezeigt
5. Scrolle auf derselben Seite nach unten zu **Privileged Gateway Intents** → aktiviere **Message Content Intent**

> [!warning] Token = Schlüssel
> Behandle den Token wie ein Passwort. Teile ihn niemals, committe ihn nicht in Git, schreibe ihn nicht in den Quellcode. Für dieses Modul reicht eine einfache Textdatei.

> [!tip] Tipp
> Wenn du den Token verpasst hast, ist das kein Problem — einfach erneut auf „Reset Token" klicken. Der alte Token wird dabei ungültig.

<details>
<summary>📜 Lösungsvorschlag</summary>

Kein Code nötig — du solltest am Ende haben:
- Eine Applikation im Developer Portal mit dem Namen deiner Wahl
- Einen Bot, der zu dieser Applikation gehört
- Einen kopierten Token (sicher verwahrt)
- Message Content Intent aktiviert ✓

</details>

**Zum Nachdenken:** Warum stellt Discord unterschiedliche „Intents" zur Verfügung, anstatt allen Bots automatisch Zugriff auf alle Nachrichten zu geben?

---

## Aufgabe 2 — Werkzeugkasten einrichten — Maven-Projekt anlegen 🟢

> Lila schiebt die Karte zur Seite und macht Platz auf dem Tisch. „Bevor wir senden können, brauchst du eine Relaisstation. Und die bauen wir jetzt." Sie klopft auf ihr zerlesenes Mechaniker-Notizbuch. „IntelliJ. Maven. Los."

**Deine Aufgabe:**
Lege in IntelliJ IDEA ein neues **Maven-Projekt** mit OpenJDK 25 an.

**Was du dafür brauchst:**

Maven ist ein **Build-Tool** für Java. Es erledigt zwei Dinge, die du sonst manuell machen müsstest:
- Es lädt automatisch externe Bibliotheken (sogenannte *Dependencies*) aus dem Internet herunter
- Es kompiliert und verpackt dein Projekt nach klaren Regeln

Die gesamte Konfiguration steckt in einer einzigen Datei: der `pom.xml` im Projektstamm.

**Schritte in IntelliJ:**
1. `File → New → Project`
2. Links „New Project" wählen, als Build-System **Maven** auswählen
3. Java-Version: **17** (oder höher, falls auf deinem Rechner installiert)
4. Projektname: `hartwood-agent`, Speicherort nach Wahl
5. Klicke „Create"

IntelliJ legt automatisch folgende Struktur an:
```
hartwood-agent/
├── pom.xml
└── src/
    ├── main/
    │   └── java/         ← hier kommt dein Code
    └── test/
        └── java/         ← hier kommen später Tests hin
```

> [!tip] Tipp
> Falls IntelliJ fragt, ob du das Maven-Projekt importieren möchtest, bestätige das. IntelliJ synchronisiert sich damit automatisch mit der `pom.xml`.

<details>
<summary>📜 Lösungsvorschlag</summary>

Nach diesem Schritt solltest du eine funktionierende IntelliJ-Projektstruktur haben. Die `pom.xml` sieht ungefähr so aus:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>de.hartwood</groupId>
    <artifactId>hartwood-agent</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>25</maven.compiler.source>
        <maven.compiler.target>25</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```

</details>

**Zum Nachdenken:** Was ist der Unterschied zwischen `groupId` und `artifactId` in der `pom.xml`? Wofür würde man diese Felder in einem echten Firmenprojekt nutzen?

---

## Aufgabe 3 — JDA einbinden — die `pom.xml` verstehen 🟡

> Hartwood lehnt sich zurück und schlägt sein Notizbuch auf. „Wir bauen nicht alles selbst. Old Finn hat Kontakte — jemanden, der das Discord-Protokoll bereits vollständig übersetzt hat. Wir nutzen seine Arbeit." Er schreibt einen Namen auf: **JDA — Java Discord API**.

**Deine Aufgabe:**
Füge JDA als Dependency in deine `pom.xml` ein und lass Maven die Bibliothek herunterladen.

**Was du dafür brauchst:**

Eine *Dependency* in Maven ist eine externe Bibliothek, die dein Projekt verwendet. Du trägst sie in die `pom.xml` ein, und Maven lädt sie automatisch aus dem Internet (Maven Central Repository) herunter.

Jede Dependency hat drei Pflichtfelder:
- `groupId` — die Organisation (wie ein Nachname)
- `artifactId` — die Bibliothek selbst (wie ein Vorname)
- `version` — welche Version du nutzen möchtest

Füge folgenden Block in deine `pom.xml` ein — **innerhalb** des `<project>`-Tags, nach dem `<properties>`-Block:

```xml
<dependencies>
    <dependency>
        <groupId>net.dv8tion</groupId>
        <artifactId>JDA</artifactId>
        <version>5.3.2</version>
    </dependency>
</dependencies>
```

> [!info] Aktuelle JDA-Version prüfen
> Die hier angegebene Version ist zum Zeitpunkt dieser Einheit aktuell. Ob eine neuere stabile Version verfügbar ist, siehst du immer hier:
> 🔗 [github.com/discord-jda/JDA/releases](https://github.com/discord-jda/JDA/releases)

Nachdem du die `pom.xml` gespeichert hast: Klicke in IntelliJ oben rechts auf das **Maven-Symbol** (der Elefant) und dann auf „Reload All Maven Projects". IntelliJ lädt JDA jetzt herunter.

> [!tip] Tipp 1
> Wenn IntelliJ nach dem Speichern ein kleines Symbol in der oberen rechten Ecke zeigt (Elefant mit Pfeil), klicke darauf — das ist die Kurzform von „Maven neu laden".

> [!tip] Tipp 2
> Falls der Download fehlschlägt: Prüfe deine Internetverbindung und ob du dich ggf. hinter einem Proxy befindest.

<details>
<summary>📜 Lösungsvorschlag</summary>

Vollständige `pom.xml` nach diesem Schritt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>de.hartwood</groupId>
    <artifactId>hartwood-agent</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>25</maven.compiler.source>
        <maven.compiler.target>25</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>net.dv8tion</groupId>
            <artifactId>JDA</artifactId>
            <version>5.3.2</version>
        </dependency>
    </dependencies>
</project>
```

Im Maven-Fenster von IntelliJ (rechte Seite) sollte unter `Dependencies` jetzt `JDA` erscheinen.

</details>

**Zum Nachdenken:** Maven lädt Bibliotheken aus dem Internet herunter — aber wohin genau? Schau mal nach, ob du einen Ordner `.m2` in deinem Home-Verzeichnis findest. Was könnte der Zweck dieses Ordners sein?

---

## Aufgabe 4 — Den Bot zum ersten Mal starten 🟡

> Hartwood schreibt die letzten Zeilen in sein Notizbuch und schiebt es dir rüber. „Das ist alles, was der Agent braucht, um sich zu melden. Dein Token. Und diese sechs Zeilen." Er tippt auf die Seite. „Wenn die grüne Lampe leuchtet, ist er online."

**Deine Aufgabe:**
Schreibe eine `Main`-Klasse, die den Bot mit deinem Token startet. Der Bot soll in der Konsole „Bot ist online!" ausgeben und dann aktiv bleiben.

**Was du dafür brauchst:**

JDA stellt einen `JDABuilder` bereit, mit dem du den Bot konfigurierst und startest. Das grundlegende Muster:

```java
JDA jda = JDABuilder.createDefault("DEIN_TOKEN_HIER")
        .build();
```

- `createDefault()` — erstellt einen Bot mit sinnvollen Standardeinstellungen
- `.build()` — startet die Verbindung zu Discord

Der Builder kann außerdem mit `awaitReady()` warten, bis die Verbindung vollständig aufgebaut ist:
```java
jda.awaitReady();
System.out.println("Bot ist online!");
```

**Schritte:**
1. Erstelle im Ordner `src/main/java` ein neues Package, z.B. `de.hartwood`
2. Erstelle darin eine Klasse `Main` mit einer `main`-Methode
3. Baue den Bot wie oben beschrieben — ersetze `"DEIN_TOKEN_HIER"` durch deinen echten Token

> [!tip] Tipp 1
> IntelliJ zeigt JDA-Klassen in der Autovervollständigung, sobald die Dependency korrekt geladen wurde. Tippe `JDABuilder` und drücke `Strg+Leertaste` — IntelliJ ergänzt den passenden Import automatisch.

> [!tip] Tipp 2
> `awaitReady()` wirft eine `InterruptedException`. IntelliJ wird dich darauf hinweisen — lass sie mit `throws InterruptedException` in der Methodensignatur zu.

<details>
<summary>📜 Lösungsvorschlag</summary>

```java
package de.hartwood;

import net.dv8tion.jda.api.JDA;
import net.dv8tion.jda.api.JDABuilder;
import net.dv8tion.jda.api.requests.GatewayIntent;

public class Main {

    public static void main(String[] args) throws InterruptedException {
        JDA jda = JDABuilder.createDefault("DEIN_TOKEN_HIER")
                .enableIntents(GatewayIntent.MESSAGE_CONTENT)
                .build();

        jda.awaitReady();
        System.out.println("Bot ist online!");
    }
}
```

Wenn du das Programm startest und in der Konsole `Bot ist online!` siehst — ist dein Agent aktiv. Im Discord Developer Portal unter „Bot" sollte jetzt auch der Status auf „Online" wechseln.

</details>

**Zum Nachdenken:** Was passiert, wenn du das Programm beendest? Und was würde das in einem echten Einsatz bedeuten — z.B. wenn der Bot auf einem Server laufen soll?

---

## Aufgabe 5 — Bot in den Expeditions-Server einladen 🔴

> [!example] Expedition ins Unbekannte
> Old Finn lehnt sich vor und senkt die Stimme. „Ein Agent, der nur für sich selbst sendet, nützt niemandem. Er muss in die Kanäle. In die Räume, wo die Crews zusammenkommen." Er zieht ein zerknittertes Papier aus seinem Mantel — eine Einladungs-URL, handgeschrieben. „Du musst lernen, wie man diese Einladungen baut."

**Deine Aufgabe:**
Erstelle einen eigenen Discord-Testserver, generiere eine OAuth2-Einladungs-URL und lade deinen Bot dort ein. Lies dabei die Berechtigungen genau durch, die du vergibst, und begründe deine Wahl.

**Was du dafür brauchst:**

**Schritt 1 — Eigenen Testserver erstellen:**
In Discord auf das **+** in der linken Seitenleiste klicken → „Eigenen Server erstellen" → „Nur für mich und meine Freunde" → Namen vergeben (z.B. `Hartwood-Testlabor`). Dieser Server gehört nur dir und ist deine sichere Spielwiese für die gesamte Modulreihe.

**Schritt 2 — Einladungs-URL generieren:**
Im Developer Portal unter deiner Applikation: **OAuth2 → URL Generator**. Dort wählst du:
- **Scope**: `bot` (und optional `applications.commands` für spätere Slash Commands)
- **Bot Permissions**: Die konkreten Rechte, die dein Bot im Server haben soll

Für den Anfang reicht: `View Channels` (unter *General Channel Permissions*) + `Send Messages` (unter *Text Permissions*).

> [!info] Weiterführende Quelle
> **Discord Developer Docs — OAuth2** — Erklärt das Berechtigungsmodell und alle verfügbaren Scopes ausführlich.
> 🔗 [discord.com/developers/docs/topics/oauth2](https://discord.com/developers/docs/topics/oauth2)

<details>
<summary>📜 Lösungsvorschlag</summary>

**Weg im Developer Portal:**
1. Deine Applikation → OAuth2 → URL Generator
2. Scopes: ✓ `bot` + ✓ `applications.commands`
3. Bot Permissions: ✓ `View Channels` + ✓ `Send Messages`
4. Generierte URL kopieren → im Browser öffnen → Server auswählen → Autorisieren

**Warum diese Berechtigungen?**
Das Prinzip des geringsten Privilegs (Principle of Least Privilege): Ein Bot bekommt nur die Rechte, die er tatsächlich braucht. Zu viele Rechte (z.B. `Administrator`) sind ein Sicherheitsrisiko — gerade wenn der Token je kompromittiert werden sollte.

</details>

**Zum Nachdenken:** Was könnte passieren, wenn du deinem Bot beim Einladen `Administrator`-Rechte gibst und dein Token später in falsche Hände gerät?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — Bot im Developer Portal angelegt, Token gesichert, Maven-Projekt läuft _(Grundlagen verstanden)_
- [ ] **Gut-Ziel** — JDA eingebunden, Bot startet und meldet sich in der Konsole als online _(sicherer Umgang mit dem Setup)_
- [ ] **Profi-Ziel** — Bot läuft in einem eigenen Testserver, Berechtigungen bewusst gewählt und begründet _(Transfer und Vertiefung geschafft)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Warum sollte ein Bot-Token niemals direkt im Quellcode stehen — und wie könnte man das in einem echten Projekt besser lösen?**

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Was kommt als Nächstes?

Der Agent ist online — aber er schweigt noch. Er empfängt keine Nachrichten und antwortet auf nichts. Im nächsten Modul lernst du, wie das **Event-System** von JDA funktioniert: Hartwoods Crew sendet die erste Testnachricht, und der Agent wird antworten.

→ Weiter zu: [[02-nachrichten|Modul 02: Nachrichten empfangen & antworten]]

---

> _„Halt den Hut fest und die Tokens sicher verwahrt."_
> — Old Finn McGraw
