# 02 · Job-Radar – RSS, Merkliste, Meldung

Liest einmal täglich einen Projekt- oder Job-Feed, verwirft alles unter den eigenen Schwellen und schickt eine kurze Telegram-Meldung – ohne dieselben Einträge am nächsten Tag erneut zu melden.

```
Täglich 06:00 → RSS lesen → Feld-Normalisierung → Vorfilter v2 → Meldung bauen → Telegram
```

## Der Kniff: die Merkliste

Jeder geprüfte Eintrag landet mit Zeitstempel in `$getWorkflowStaticData('global')` und wird beim nächsten Lauf übersprungen. Einträge älter als 30 Tage werden automatisch gelöscht, damit die Liste nicht unbegrenzt wächst.

## Was die Meldung enthält

- neu geprüft / schon bekannt / durchgelassen
- Ablehnungsgründe, gezählt (z. B. `7× Budget zu niedrig`)
- die Treffer mit Budget, Titel und Link
- **Knapp verfehlt:** die drei Abgewiesenen, die der Schwelle am nächsten kamen. Ohne diese Zeile ist ein Radar, der jeden Morgen „0" meldet, nicht von einem kaputten zu unterscheiden.

## Vor dem ersten Lauf

| Wo | Was |
|---|---|
| Feed lesen | eigene Feed-URL |
| Feld-Normalisierung | an die Felder des Feeds anpassen – jeder Feed liefert andere; Umrechnungskurse in `KURS_ZU_USD` |
| Vorfilter v2 | `MIN_FESTPREIS_USD`, `MIN_STUNDE_USD` |
| Telegram senden | Credential und `chatId` – besser als Umgebungsvariable: `{{ $env.TELEGRAM_CHAT_ID }}` |

## Gut zu wissen

- `staticData` wird **nur bei produktiven Ausführungen** gespeichert. Beim Testen im Editor merkt sich der Workflow nichts.
- Meldungen werden als HTML formatiert (`parse_mode: HTML`).

**Credentials:** Telegram API.
