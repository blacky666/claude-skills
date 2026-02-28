---
name: locale
description: Waehrungs- und Locale-Richtlinien anwenden. Verwende diesen Skill IMMER wenn Waehrungen, Geldbetraege, Zahlenformate oder Locale-spezifische Formatierung geschrieben oder geaendert wird.
argument-hint: [datei]
---

# Waehrung & Locale

Wende die Waehrungs- und Locale-Regeln an. Wenn `$ARGUMENTS` eine Datei enthaelt, lies und pruefe diese. Sonst wende die Regeln beim Schreiben von neuem Code an.

---

## Standardwaehrung

- **EUR** (Euro) ist die Standardwaehrung
- Schweizer koennen **CHF** waehlen

## Zahlenformate

| | Oesterreich (EUR) | Schweiz (CHF) |
|---|---|---|
| **Locale** | `de-AT` | `de-CH` |
| **Tausender** | Punkt (`.`) | Apostroph (`'`) |
| **Dezimal** | Komma (`,`) | Punkt (`.`) |
| **Beispiel** | `€ 1.250,00` | `CHF 1'250.00` |

## Frontend-Formatierung

**IMMER `Intl.NumberFormat` verwenden.** Keine manuelle String-Formatierung.

```javascript
// Oesterreich (Standard)
new Intl.NumberFormat('de-AT', {
	style: 'currency',
	currency: 'EUR',
}).format(1250.0); // → '€ 1.250,00'

// Schweiz
new Intl.NumberFormat('de-CH', {
	style: 'currency',
	currency: 'CHF',
}).format(1250.0); // → 'CHF 1'250.00'
```

## Regeln

1. **Keine hardcoded Waehrungssymbole** — immer ueber `Intl.NumberFormat` mit `style: 'currency'`
2. **Keine manuelle Tausender-/Dezimal-Formatierung** — `Intl.NumberFormat` uebernimmt das
3. **Locale aus User-Settings** laden, nicht hardcoden (Default: `de-AT`)
4. **Backend**: Geldbetraege immer als Integer (Cent) speichern, erst im Frontend formatieren
5. **Eingabefelder**: Komma UND Punkt als Dezimal-Eingabe akzeptieren, intern als Float/Integer verarbeiten

## Pruef-Checkliste

1. Wird `Intl.NumberFormat` mit korrekter Locale verwendet?
2. Sind Waehrungssymbole hardcoded statt dynamisch?
3. Werden Geldbetraege korrekt als Cent (Integer) gespeichert?
4. Akzeptieren Eingabefelder beide Dezimal-Trennzeichen?
5. Ist die Locale aus den User-Settings geladen?
