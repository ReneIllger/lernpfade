---
title: "Modul 05 — Externe API anbinden"
tags:
  - ausbildung/selbstlernpfad
  - ausbildung/programmierung
  - ausbildung/java
lernpfad: Discord Bot mit Java
modul-nr: 5
dauer: 45 min
voraussetzungen:
  - "[[04-embeds|Modul 04]] abgeschlossen — Embeds funktionieren, `/expedition`-Command läuft"
---

# Modul 05 — Externe API anbinden
_„Old Finns Kontaktnetz"_

| | |
|---|---|
| **Fachbereich** | Programmierung / Java |
| **Dauer** | 45 Minuten |
| **Zielgruppe** | FIAE & FISI |
| **Komplexität** | 🟥 Experte |
| **Voraussetzungen** | [[04-embeds\|Modul 04]] abgeschlossen · IntelliJ IDEA mit Edu-Lizenz · OpenJDK 25 · JDA 5.x · Gson-Bibliothek (wird in diesem Modul eingebunden) · Internetverbindung |
| **Lernziel** | Du kannst aus einem Java-Programm heraus einen HTTP-GET-Request absetzen, die JSON-Antwort mit Gson parsen und die Daten in einem Discord-Bot-Command verwenden. |

---

## Worum geht es?

Lila breitet eine Karte über Hartwoods Tisch aus. Flugroute Kairo → Luxor → Assuan. Mit dem Finger fährt sie die gestrichelten Linien entlang. „Ich brauche Wetterdaten. Bevor ich starte, muss ich wissen: Gegenwind? Böen? Sichtweite?" Sie schaut auf. „Kann der Agent das abrufen?"

Old Finn grinst und klopft die Asche aus seiner Pfeife. „Ich kenn' jemanden." Er schreibt eine Adresse auf einen Bierdeckel: **Open-Meteo**. „Kostenloser Wetterdienst. Antwortet auf Anfragen in einem codierten Format — JSON heißt das. Du schickst eine Anfrage mit Koordinaten. Er schickt dir Temperatur, Wind, alles. Kein Klopfen, kein Passwort. Einfach fragen."

Hartwood notiert: `GET /v1/forecast?latitude=...&longitude=...`

„Das ist eine API", sagt er. „Eine Schnittstelle. Eine definierte Art, mit einem fremden System zu reden."

**In diesem Modul** baust du den `/wetter`-Command: Lila wählt eine Expeditionsstadt, der Agent fragt Open-Meteo an und antwortet mit einem formatierten Wetterbericht.

---

## Aufgaben im Überblick

| # | Aufgabe | Schwierigkeit | Zeit |
|---|---------|:---:|---:|
| 1 | HTTP & APIs verstehen — Anfrage, Antwort, JSON | 🟢 | ~6 Min. |
| 2 | Gson einbinden & ersten HTTP-Request absetzen | 🟢 | ~9 Min. |
| 3 | JSON parsen — Wetterdaten extrahieren | 🟡 | ~10 Min. |
| 4 | `/wetter`-Command mit Embed-Ausgabe | 🟡 | ~10 Min. |
| 5 | Fehlerbehandlung — Was tun, wenn der Kontakt schweigt? | 🔴 | ~10 Min. |

> 🟢 **Pflicht** — 🟡 **Pflicht (Hilfe erlaubt)** — 🔴 **Optional / Vertiefung**

---

## Aufgabe 1 — HTTP & APIs verstehen — Anfrage, Antwort, JSON 🟢

> Old Finn lehnt sich zurück. „Stell dir vor, du schickst einen Brief an einen Informanten. Der Brief hat eine feste Adresse — eine URL. Er enthält eine Frage — den Request. Der Informant antwortet mit einem Zettel — der Response. Und weil der Informant ordentlich ist, schreibt er seinen Zettel immer im gleichen Format. Das nennt sich JSON."

**Deine Aufgabe:**
Lies die Erklärung und beantworte die Frage am Ende in eigenen Worten.

**Was du dafür brauchst:**

**HTTP-Request/Response:**
```
du (Client)          Informant (Server / API)
    │                        │
    │── GET /v1/forecast?... ─▶│   Request: Was willst du wissen?
    │                        │   (Methode + URL + Parameter)
    │◀─ 200 OK + JSON ────────│   Response: Status + Antwort-Daten
    │                        │
```

Wichtige HTTP-Statuscodes:

| Code | Bedeutung |
|---|---|
| `200 OK` | Alles gut, hier sind die Daten |
| `400 Bad Request` | Deine Anfrage war fehlerhaft |
| `404 Not Found` | Diese Adresse existiert nicht |
| `500 Internal Server Error` | Fehler auf der Serverseite |

**JSON** (JavaScript Object Notation) ist ein Textformat für strukturierte Daten:

```json
{
  "current": {
    "temperature_2m": 34.5,
    "wind_speed_10m": 18.2
  }
}
```

Verschachtelte Objekte (`{}`), Arrays (`[]`), Strings (`"..."`), Zahlen und Booleans — das sind die einzigen Bausteine.

**Die API, die du nutzen wirst:**
[Open-Meteo](https://open-meteo.com/) liefert kostenlose Wetterdaten ohne Registrierung oder API-Key.

Beispiel-URL für Kairo:
```
https://api.open-meteo.com/v1/forecast
  ?latitude=30.06
  &longitude=31.24
  &current=temperature_2m,wind_speed_10m,weathercode
```

> [!tip] Tipp
> Öffne die URL direkt im Browser — du siehst die rohe JSON-Antwort. Das ist genau das, was dein Java-Code später empfangen und verarbeiten wird.

<details>
<summary>📜 Lösungsvorschlag</summary>

Keine Code-Aufgabe. Eine gute Antwort:

_„Eine API ist eine definierte Schnittstelle, über die zwei Programme miteinander kommunizieren. Ich schicke einen HTTP-Request an eine URL mit Parametern. Der Server antwortet mit einem Statuscode und Daten — meist im JSON-Format. JSON ist ein Textformat, das ich in Java in Objekte umwandeln kann."_

</details>

**Zum Nachdenken:** Open-Meteo braucht keinen API-Key — du fragst einfach an. Warum haben viele andere APIs dennoch einen API-Key? Was würde ohne Key-Pflicht passieren, wenn ein Service sehr teuer im Betrieb ist?

---

## Aufgabe 2 — Gson einbinden & ersten HTTP-Request absetzen 🟢

> Hartwood schreibt die URL auf und reicht den Zettel rüber. „Schick eine Anfrage. Lass dir zeigen, was der Informant antwortet. Wir müssen wissen, wie er redet, bevor wir seine Antworten verstehen können."

**Deine Aufgabe:**
Füge Gson als Dependency in die `pom.xml` ein. Schreibe dann eine Java-Klasse `WetterService` mit einer Methode `wetterAbfragen(double lat, double lon)`, die einen HTTP-GET-Request an Open-Meteo schickt und den Response-Body als `String` zurückgibt. Gib den rohen JSON-String in der Konsole aus.

**Was du dafür brauchst:**

**Gson in `pom.xml`:**
```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.11.0</version>
</dependency>
```

**Java HttpClient** (seit Java 11 eingebaut — keine zusätzliche Dependency):
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

HttpClient client = HttpClient.newHttpClient();

HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.open-meteo.com/v1/forecast" +
                "?latitude=" + lat +
                "&longitude=" + lon +
                "&current=temperature_2m,wind_speed_10m,weathercode"))
        .GET()
        .build();

HttpResponse<String> response = client.send(request,
        HttpResponse.BodyHandlers.ofString());

String body = response.body();
int statusCode = response.statusCode();
```

> [!tip] Tipp 1
> `client.send()` blockiert — das bedeutet, der Thread wartet, bis die Antwort da ist. Das ist für einen ersten Test in Ordnung. Für produktiven Code wäre `sendAsync()` besser (Vertiefung Aufgabe 5).

> [!tip] Tipp 2
> `send()` wirft `IOException` und `InterruptedException`. Lass IntelliJ den try-catch-Block automatisch einfügen — klicke auf die rote Markierung → „Surround with try/catch".

<details>
<summary>📜 Lösungsvorschlag</summary>

**WetterService.java:**
```java
package de.hartwood;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class WetterService {

    private final HttpClient client = HttpClient.newHttpClient();

    public String wetterAbfragen(double lat, double lon) throws IOException, InterruptedException {
        String url = "https://api.open-meteo.com/v1/forecast"
                + "?latitude=" + lat
                + "&longitude=" + lon
                + "&current=temperature_2m,wind_speed_10m,weathercode";

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .GET()
                .build();

        HttpResponse<String> response = client.send(request,
                HttpResponse.BodyHandlers.ofString());

        return response.body();
    }

    public static void main(String[] args) throws Exception {
        WetterService service = new WetterService();
        // Kairo: 30.06° N, 31.24° O
        System.out.println(service.wetterAbfragen(30.06, 31.24));
    }
}
```

In der Konsole siehst du den rohen JSON-String von Open-Meteo.

</details>

**Zum Nachdenken:** `HttpClient.newHttpClient()` erzeugt ein neues Client-Objekt. Warum ist es sinnvoll, diesen Client als Instanzvariable zu speichern und wiederzuverwenden — statt ihn bei jedem Aufruf neu zu erstellen?

---

## Aufgabe 3 — JSON parsen — Wetterdaten extrahieren 🟡

> Old Finn legt einen Zettel auf den Tisch — vollgeschrieben mit Zahlen und Schlüsselbegriffen. „Der Informant antwortet. Aber seine Antwort ist roh. Du musst sie lesen. `temperature_2m` — das ist die Temperatur. `wind_speed_10m` — der Wind. `weathercode` — ein Zahlencode für das Wetter." Er schiebt einen zweiten Zettel dazu: eine Übersetzungstabelle. „Ich hab' die Codes schon nachgeschlagen."

**Deine Aufgabe:**
Erweitere `WetterService` um eine Methode `wetterParsen(String json)`, die aus dem JSON-String Temperatur, Windgeschwindigkeit und Wettercode extrahiert und als einfaches Datenhaltungs-Objekt zurückgibt. Erstelle dafür eine Klasse `Wetterdaten`.

**Was du dafür brauchst:**

**Gson JSON-Parsing:**
```java
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

JsonObject root    = JsonParser.parseString(json).getAsJsonObject();
JsonObject current = root.getAsJsonObject("current");

double temperatur = current.get("temperature_2m").getAsDouble();
double wind       = current.get("wind_speed_10m").getAsDouble();
int    code       = current.get("weathercode").getAsInt();
```

**Wettercode → lesbare Beschreibung** (vereinfachte Tabelle):

| Code | Beschreibung |
|---|---|
| 0 | Klarer Himmel |
| 1–3 | Überwiegend klar bis bewölkt |
| 45, 48 | Nebel |
| 51–67 | Regen (leicht bis stark) |
| 71–77 | Schnee |
| 80–82 | Schauer |
| 95–99 | Gewitter |

> [!tip] Tipp 1
> Erstelle eine private Hilfsmethode `codeZuBeschreibung(int code)`, die mit einem `switch` den passenden Text zurückgibt. Nutze `default` für alle unbekannten Codes.

> [!tip] Tipp 2
> Eine einfache Klasse `Wetterdaten` mit drei Feldern (`double temperatur`, `double wind`, `String beschreibung`) und einem Konstruktor reicht völlig aus.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Wetterdaten.java:**
```java
package de.hartwood;

public class Wetterdaten {
    public final double temperatur;
    public final double wind;
    public final String beschreibung;

    public Wetterdaten(double temperatur, double wind, String beschreibung) {
        this.temperatur  = temperatur;
        this.wind        = wind;
        this.beschreibung = beschreibung;
    }
}
```

**WetterService.java** (Erweiterung):
```java
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public Wetterdaten wetterParsen(String json) {
    JsonObject root    = JsonParser.parseString(json).getAsJsonObject();
    JsonObject current = root.getAsJsonObject("current");

    double temperatur = current.get("temperature_2m").getAsDouble();
    double wind       = current.get("wind_speed_10m").getAsDouble();
    int    code       = current.get("weathercode").getAsInt();

    return new Wetterdaten(temperatur, wind, codeZuBeschreibung(code));
}

private String codeZuBeschreibung(int code) {
    if (code == 0)                          return "Klarer Himmel";
    if (code >= 1  && code <= 3)            return "Teils bewölkt";
    if (code == 45 || code == 48)           return "Nebel";
    if (code >= 51 && code <= 67)           return "Regen";
    if (code >= 71 && code <= 77)           return "Schnee";
    if (code >= 80 && code <= 82)           return "Schauer";
    if (code >= 95 && code <= 99)           return "Gewitter";
    return "Unbekannt (Code " + code + ")";
}
```

</details>

**Zum Nachdenken:** Du speicherst die Felder in `Wetterdaten` als `public final`. Was wäre der Vorteil, stattdessen private Felder mit Getter-Methoden zu verwenden — und wann lohnt sich dieser Aufwand?

---

## Aufgabe 4 — `/wetter`-Command mit Embed-Ausgabe 🟡

> Lila klopft an den Türrahmen. „Fertig mit der Theorie?" Sie wirft ihre Fliegerkappe auf den Tisch. „Ich will `/wetter` eingeben, eine Stadt wählen, und der Agent sagt mir, was mich erwartet. Kairo, Luxor, Assuan — die drei Strecken, die ich fliege."

**Deine Aufgabe:**
Registriere einen neuen Slash Command `/wetter` mit einem Pflicht-Parameter `stadt` als Choice-Dropdown. Die drei Optionen sind `Kairo`, `Luxor` und `Assuan` — jede mit festen Koordinaten. Der Bot fragt Open-Meteo an, parst die Antwort und gibt einen formatierten Embed zurück.

Verwende diese Koordinaten:

| Stadt | Latitude | Longitude |
|---|---|---|
| Kairo | 30.06 | 31.24 |
| Luxor | 25.70 | 32.64 |
| Assuan | 24.09 | 32.90 |

Der Embed soll enthalten: Stadtname als Titel, Wetterbeschreibung als Beschreibung, Temperatur und Wind als inline-Felder, Footer mit Quellenangabe.

**Was du dafür brauchst:**

Koordinaten aus dem Choice-Wert ermitteln — ein einfaches Mapping reicht:

```java
double[] koordinaten = switch (stadtWert) {
    case "luxor"  -> new double[]{25.70, 32.64};
    case "assuan" -> new double[]{24.09, 32.90};
    default       -> new double[]{30.06, 31.24};  // Kairo
};
```

Denke daran: `wetterAbfragen()` wirft Checked Exceptions — fange sie im Handler ab.

> [!tip] Tipp 1
> Lagere den Embed-Bau in eine Methode `erstelleWetterEmbed(String stadt, Wetterdaten daten)` aus — analog zu Modul 04.

> [!tip] Tipp 2
> Wähle die Embed-Farbe passend zur Wetterlage: blau für Regen, gelb für Sonne, grau für Bewölkung. Du hast das Muster aus Modul 04 bereits kennengelernt.

<details>
<summary>📜 Lösungsvorschlag</summary>

**Command-Registrierung (Main.java, Ergänzung):**
```java
Commands.slash("wetter", "Wetterbericht für Expeditionsstädte")
    .addOptions(new OptionData(OptionType.STRING, "stadt", "Zielstadt", true)
        .addChoices(
            new Command.Choice("Kairo",  "kairo"),
            new Command.Choice("Luxor",  "luxor"),
            new Command.Choice("Assuan", "assuan")
        ))
```

**SlashCommandListener.java** (Ergänzungen):
```java
private final WetterService wetterService = new WetterService();

// Im switch:
case "wetter" -> {
    String stadt = event.getOption("stadt").getAsString();
    double[] koord = switch (stadt) {
        case "luxor"  -> new double[]{25.70, 32.64};
        case "assuan" -> new double[]{24.09, 32.90};
        default       -> new double[]{30.06, 31.24};
    };
    try {
        String json     = wetterService.wetterAbfragen(koord[0], koord[1]);
        Wetterdaten daten = wetterService.wetterParsen(json);
        event.replyEmbeds(erstelleWetterEmbed(stadt, daten)).queue();
    } catch (Exception e) {
        event.reply("Kontakt nicht erreichbar. Versuch es später nochmal.").queue();
    }
}

private MessageEmbed erstelleWetterEmbed(String stadt, Wetterdaten daten) {
    int farbe = switch (daten.beschreibung) {
        case "Klarer Himmel", "Teils bewölkt" -> 0xFFD700;
        case "Regen", "Schauer"               -> 0x3498DB;
        case "Gewitter"                        -> 0x8E44AD;
        default                                -> 0x95A5A6;
    };

    String titel = Character.toUpperCase(stadt.charAt(0)) + stadt.substring(1);

    EmbedBuilder embed = new EmbedBuilder();
    embed.setTitle("Wetterbericht — " + titel);
    embed.setDescription(daten.beschreibung);
    embed.setColor(farbe);
    embed.addField("Temperatur", daten.temperatur + " °C",   true);
    embed.addField("Wind",       daten.wind       + " km/h", true);
    embed.setFooter("Quelle: Open-Meteo (open-meteo.com)");
    embed.setTimestamp(java.time.Instant.now());
    return embed.build();
}
```

</details>

**Zum Nachdenken:** Du rufst `wetterAbfragen()` direkt im JDA-Event-Thread auf — ein blockierender Aufruf. Was könnte dabei schiefgehen, wenn die API langsam antwortet oder kurz nicht erreichbar ist?

---

## Aufgabe 5 — Fehlerbehandlung — Was tun, wenn der Kontakt schweigt? 🔴

> [!example] Expedition ins Unbekannte
> Hartwood tippt `/wetter Luxor` — und wartet. Und wartet. Dreißig Sekunden. Eine Minute. Der Agent antwortet nicht. Old Finn schaut auf seine Taschenuhr. „Der Informant schläft manchmal. Oder die Leitung ist tot." Er lehnt sich vor. „Ein guter Agent wartet nicht ewig. Er gibt nach zehn Sekunden auf und meldet: Kein Signal. Das nennt sich Timeout."

**Deine Aufgabe:**
Drei Dinge sollen abgesichert werden:

1. **Timeout**: Der HTTP-Request soll nach **8 Sekunden** abbrechen, wenn keine Antwort kommt.
2. **HTTP-Fehler**: Falls der Statuscode nicht `200` ist, soll eine sprechende Fehlermeldung zurückgegeben werden — kein leerer Embed, kein Crash.
3. **Async-Antwort**: JDA erwartet innerhalb von 3 Sekunden eine erste Antwort. Da der HTTP-Request bis zu 8 Sekunden dauern kann, musst du Discord zuerst mit `deferReply()` mitteilen: „Ich bin noch dabei" — und später mit `editOriginal()` die echten Daten nachliefern.

**Was du dafür brauchst:**

**Timeout beim HttpClient:**
```java
HttpClient client = HttpClient.newBuilder()
        .connectTimeout(Duration.ofSeconds(8))
        .build();
```

**Statuscode prüfen:**
```java
if (response.statusCode() != 200) {
    throw new IOException("API-Fehler: HTTP " + response.statusCode());
}
```

**`deferReply()` + `editOriginal()`** — für Commands, die länger als 3 Sekunden dauern:
```java
// Sofort: Discord mitteilen, dass wir arbeiten (zeigt "denkt nach..." an)
event.deferReply().queue();

// Später: echte Antwort nachliefern
event.getHook().editOriginalEmbeds(embed).queue();
// oder bei Fehler:
event.getHook().editOriginal("Fehlertext").queue();
```

> [!info] Weiterführende Quelle
> **JDA Wiki — Interactions** — erklärt `deferReply`, `editOriginal` und das Zusammenspiel mit dem 3-Sekunden-Limit ausführlich.
> 🔗 [jda.wiki/using-jda/interactions](https://jda.wiki/using-jda/interactions/)

> [!tip] Tipp
> Sobald du `deferReply()` verwendet hast, musst du **immer** mit `event.getHook()` antworten — `event.reply()` funktioniert danach nicht mehr.

<details>
<summary>📜 Lösungsvorschlag</summary>

**WetterService.java** — mit Timeout und Statuscode-Prüfung:
```java
public class WetterService {

    private final HttpClient client = HttpClient.newBuilder()
            .connectTimeout(Duration.ofSeconds(8))
            .build();

    public String wetterAbfragen(double lat, double lon) throws IOException, InterruptedException {
        String url = "https://api.open-meteo.com/v1/forecast"
                + "?latitude=" + lat
                + "&longitude=" + lon
                + "&current=temperature_2m,wind_speed_10m,weathercode";

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .timeout(Duration.ofSeconds(8))
                .GET()
                .build();

        HttpResponse<String> response = client.send(request,
                HttpResponse.BodyHandlers.ofString());

        if (response.statusCode() != 200) {
            throw new IOException("API-Fehler: HTTP " + response.statusCode());
        }

        return response.body();
    }

    // wetterParsen() bleibt unverändert
}
```

**SlashCommandListener.java** — `case "wetter"` mit deferReply:
```java
case "wetter" -> {
    String stadt = event.getOption("stadt").getAsString();
    double[] koord = switch (stadt) {
        case "luxor"  -> new double[]{25.70, 32.64};
        case "assuan" -> new double[]{24.09, 32.90};
        default       -> new double[]{30.06, 31.24};
    };

    // Sofort antworten — dann in Ruhe die API anfragen
    event.deferReply().queue();

    try {
        String json       = wetterService.wetterAbfragen(koord[0], koord[1]);
        Wetterdaten daten = wetterService.wetterParsen(json);
        event.getHook().editOriginalEmbeds(erstelleWetterEmbed(stadt, daten)).queue();
    } catch (IOException e) {
        event.getHook().editOriginal(
            "Kein Signal vom Informanten. Versuche es später erneut.").queue();
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
        event.getHook().editOriginal(
            "Verbindung unterbrochen.").queue();
    }
}
```

</details>

**Zum Nachdenken:** `deferReply()` + `editOriginal()` löst das 3-Sekunden-Problem — aber der HTTP-Request blockiert immer noch den JDA-Event-Thread. Wie könnte man das mit `HttpClient.sendAsync()` und einem eigenen Thread-Pool sauber lösen? Was wäre der nächste Schritt?

---

## Selbstkontrolle

Wie weit bist du gekommen?

- [ ] **Mindest-Ziel** — HTTP-Request wird abgesetzt, rohe JSON-Antwort erscheint in der Konsole _(Grundprinzip verstanden)_
- [ ] **Gut-Ziel** — `/wetter`-Command funktioniert, Embed zeigt Temperatur, Wind und Beschreibung _(sicherer Umgang mit APIs)_
- [ ] **Profi-Ziel** — Timeout, Statuscode-Prüfung und `deferReply` sind eingebaut, Fehler werden dem Nutzer klar kommuniziert _(produktionsnaher Code)_

---

## Reflexion

Nimm dir 2 Minuten und beantworte für dich:

> **Dein Bot hängt jetzt davon ab, dass Open-Meteo erreichbar ist. Was bedeutet das für die Zuverlässigkeit deines Bots — und wie könntest du dieses Risiko in einem echten Projekt minimieren?** _(Stichwort: Caching, Fallback, Monitoring)_

Was ist dir noch unklar? Notiere es für das nächste Gespräch mit deinem Ausbilder.

---

## Rückblick auf die gesamte Modulreihe

Du hast alle fünf Module abgeschlossen. Hier ist, was der Agent jetzt kann — und was dahintersteckt:

| Modul | Fähigkeit | Konzept |
|---|---|---|
| 01 | Bot starten & registrieren | Maven, Dependencies, Token-Sicherheit |
| 02 | Nachrichten empfangen & antworten | Event-System, Observer-Pattern |
| 03 | Slash Commands | API-Registrierung, Interactions, Parameter |
| 04 | Formatierte Ausgaben | Builder-Pattern, Embeds, Choices |
| 05 | Externe Daten einbinden | HTTP, JSON, Fehlerbehandlung, Timeouts |

> [!success] Expedition abgeschlossen
> Hartwood schlägt sein Notizbuch zu und lehnt sich zurück. Lila nickt anerkennend. Old Finn zündet sich eine neue Pfeife an. „Nicht schlecht", sagt er. „Nicht schlecht."

---

> _„Halt den Hut fest und die Timeouts kurz."_
> — Old Finn McGraw
