# 03 · Beleg-Erfassung aus E-Mail-Anhängen

Holt neue Mails aus einem Postfach, nimmt die PDF-Anhänge, liest den Text heraus und erkennt **Lieferant, Rechnungsnummer, Datum, Betrag** sowie IBAN und USt-IdNr. Sicher Erkanntes geht in die Tabelle, Unsicheres in die Prüfspur.

```
IMAP (ungelesen) → Nur PDF-Anhänge → Text aus PDF → Felder erkennen → Sicher erkannt?
                                                                     ├─ ja   → In Tabelle
                                                                     └─ nein → Prüfen
```

## Ohne Cloud

Kein OCR-Dienst, kein externer Anbieter. Der PDF-Text wird auf dem eigenen Server gelesen, die Felder per Mustererkennung gezogen. Bei Belegen ist genau das der Unterschied.

## Grenzen

- Funktioniert bei **Text-PDFs**. Eine eingescannte Papierrechnung ist ein Bild – dafür braucht es zusätzlich OCR (z. B. Tesseract oder OCRmyPDF auf demselben Server).
- Die Muster sind auf **deutsche Rechnungen** ausgelegt.
- `pruefen = true` heißt: weniger als drei der Kernfelder erkannt oder kein Betrag. **Diese Belege laufen nicht automatisch weiter.**

## Rechtlicher Rahmen

Dieser Workflow **erfasst**, er bucht nicht. Erlaubt ist nach **§ 6 Nr. 3 StBerG** nur die mechanische Erfassung. Das Kontieren von Belegen und das Einrichten der Buchführung gehören ausdrücklich nicht dazu und bleiben Steuerberatern vorbehalten.

Für die Aufbewahrung gelten die **GoBD**: Original-PDF unverändert archivieren – die erfassten Felder sind nur ein Index.

## Anschließen

- **In Tabelle:** Google Sheets, Excel, Baserow, NocoDB oder PostgreSQL – eine Zeile je Beleg.
- **Prüfen:** Mail an die Buchhaltung mit dem PDF im Anhang, oder eine Aufgabe.

**Credentials:** IMAP.
