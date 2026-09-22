# 01 · Kontakt-Vorfilter mit Honeypot und Drosselung

Nimmt ein Kontaktformular per Webhook an, antwortet **sofort mit 200** und verwirft Bot-Einsendungen, bevor sie im Postfach oder CRM landen.

```
Webhook (antwortet sofort) → Code „Vorfilter" → Saubere Anfrage → [hier anschließen]
```

## Vier Prüfungen

1. **Honeypot** – ein für Menschen unsichtbares Feld `website`. Ist es ausgefüllt, war es ein Bot.
2. **Zeitfalle** – das Formular schickt `dauer` (ms zwischen Laden und Absenden). Unter 2,5 s ist das kein Mensch.
3. **Plausibilität** – Pflichtfelder, E-Mail-Format, Feldlängen, Anzahl Links.
4. **Drosselung** – höchstens 3 Anfragen je IP in 60 Minuten, gemerkt in `staticData`, mit automatischer Aufräumung.

Alle Schwellen stehen als Konstanten oben im Code-Node.

## Gut zu wissen

- Der Webhook steht auf **„Respond Immediately"**. Würde er auf eine Antwort warten und der Code-Node gibt `[]` zurück, hinge der Absender bis zum Timeout.
- Abgewiesene Anfragen bekommen trotzdem **200** – ein Bot soll nicht lernen, dass er erkannt wurde.
- `staticData` wird **nur bei produktiven Ausführungen** gespeichert. Im Editor merkt sich die Drosselung nichts – das ist kein Fehler.
- Den Webhook-Pfad vor dem Produktivbetrieb durch eine **eigene UUID** ersetzen.

## Formular-Seite

Nicht `type="hidden"` für den Honeypot verwenden – solche Felder lassen Bots aus.

```html
<div aria-hidden="true" style="position:absolute;left:-9999px">
  <label>Website</label>
  <input type="text" name="website" tabindex="-1" autocomplete="off">
</div>
<input type="hidden" name="dauer" id="dauer">
<script>
  const T0 = Date.now();
  document.querySelector('form').addEventListener('submit', () => {
    document.getElementById('dauer').value = Date.now() - T0;
  });
</script>
```

## Anschließen

Am Ausgang „Saubere Anfrage": Mail an dich, Eintrag ins CRM, Eingangsbestätigung an den Absender.

**Credentials:** keine.
