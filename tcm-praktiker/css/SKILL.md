---
name: css
description: CSS-Richtlinien pruefen und anwenden. Verwende diesen Skill IMMER wenn CSS geschrieben, geaendert oder geprueft wird — auch bei HTML/JS-Tasks die Styling beinhalten.
argument-hint: [datei.css]
---

# CSS-Richtlinien

Pruefe CSS-Code gegen die Richtlinien. Wenn `$ARGUMENTS` eine Datei enthaelt, lies und pruefe diese. Sonst wende die Regeln beim Schreiben von neuem CSS an.

---

## 1. Layout: Grid first, Flexbox als Ausnahme

**Immer CSS Grid verwenden.** Flexbox nur wenn es keine andere Wahl gibt.

```css
/* RICHTIG */
.dashboard {
	display: grid;
	grid-template-columns: 260px 1fr;
	grid-template-rows: 64px 1fr;
	min-height: 100vh;
}

.card-grid {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
	gap: 16px;
}

/* FALSCH */
.dashboard {
	display: flex;
}
.card-grid {
	display: flex;
	flex-wrap: wrap;
}
```

**Flexbox NUR erlaubt fuer:**

- Button-Gruppen / Tag-Chips (dynamische Breite)
- Zentrierung eines einzelnen Elements
- Navigation-Items in einer Zeile
- Icon + Text Kombination

---

## 2. Design Tokens (CSS Custom Properties)

Alle Farben, Schriften und Abstaende als CSS-Variablen. **Keine hardcoded Werte.**

```css
:root {
	/* Farben */
	--color-primary: #0d9488;
	--color-primary-hover: #0b7c72;
	--color-accent: #1a3b47;
	--color-text-dark: #1f2937;
	--color-text-muted: #6b7280;
	--color-border: #d0d5da;
	--color-bg-light: #f8f9fa;
	--color-white: #ffffff;
	--color-success: #10b981;
	--color-warning: #f59e0b;
	--color-error: #ef4444;

	/* Typografie */
	--font-primary: 'Montserrat', sans-serif;
	--font-size-xs: 12px;
	--font-size-sm: 14px;
	--font-size-base: 16px;
	--font-size-lg: 18px;
	--font-size-xl: 20px;
	--font-size-2xl: 24px;
	--font-size-3xl: 28px;

	/* Abstaende — 4px Basis-Grid */
	--space-1: 4px;
	--space-2: 8px;
	--space-3: 12px;
	--space-4: 16px;
	--space-5: 20px;
	--space-6: 24px;
	--space-8: 32px;
	--space-10: 40px;
	--space-12: 48px;

	/* Border Radius */
	--radius-sm: 6px;
	--radius-md: 8px;
	--radius-lg: 12px;
	--radius-full: 9999px;

	/* Schatten */
	--shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
	--shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);
}
```

---

## 3. Typografie

- Mindestgroesse Fliesstext/Inputs: **16px**
- Labels duerfen 14px sein
- Font: **Montserrat** durchgaengig
- Keine `em`/`rem` ohne definierten Basis-Wert

```css
body {
	font-family: var(--font-primary);
	font-size: var(--font-size-base);
	color: var(--color-text-dark);
	line-height: 1.5;
}
h1 {
	font-size: var(--font-size-3xl);
	font-weight: 700;
}
h2 {
	font-size: var(--font-size-2xl);
	font-weight: 700;
}
h3 {
	font-size: var(--font-size-xl);
	font-weight: 700;
}
h4 {
	font-size: var(--font-size-lg);
	font-weight: 600;
}
```

---

## 4. Responsive Design

### Breakpoints

- **Desktop**: Default (1024px+)
- **Tablet**: < 1024px
- **Mobile**: < 768px (nur ausgewaehlte Bereiche)

### Admin: Desktop-first

```css
.admin-layout {
	display: grid;
	grid-template-columns: 260px 1fr;
	min-height: 100vh;
}
@media (max-width: 1023px) {
	.admin-layout {
		grid-template-columns: 1fr;
	}
}
```

### WordPress (meine-tcm.com): Mobile-first

```css
.content {
	display: grid;
	grid-template-columns: 1fr;
	gap: var(--space-4);
}
@media (min-width: 768px) {
	.content {
		grid-template-columns: repeat(2, 1fr);
	}
}
@media (min-width: 1024px) {
	.content {
		grid-template-columns: repeat(3, 1fr);
	}
}
```

### Responsive-Pflicht

| Bereich                     | Mobile           | Tablet         | Desktop |
| --------------------------- | ---------------- | -------------- | ------- |
| meine-tcm.com (WordPress)   | Pflicht          | Pflicht        | Pflicht |
| Admin: Dashboard & Anfragen | Ja (vereinfacht) | Ja             | Ja      |
| Admin: Kalender             | Nein             | Eingeschraenkt | Ja      |
| Admin: Patienten            | Nein             | Eingeschraenkt | Ja      |
| Admin: Rechnungen           | Nein             | Nein           | Ja      |
| Admin: Einstellungen        | Nein             | Ja             | Ja      |

---

## 5. Container Queries

Fuer Komponenten in verschiedenen Kontexten (z.B. Cards in Sidebar vs. Hauptbereich).

```css
.card-container {
	container-type: inline-size;
}
.card {
	display: grid;
	grid-template-columns: 1fr;
}
@container (min-width: 400px) {
	.card {
		grid-template-columns: auto 1fr;
	}
}
```

---

## 6. Keine Magic Numbers

Alle Werte aus dem Design-Token-System oder begruendet.

```css
/* RICHTIG */
.card {
	padding: var(--space-6);
	border-radius: var(--radius-md);
	gap: var(--space-4);
}

/* FALSCH */
.card {
	padding: 23px;
	border-radius: 7px;
	gap: 13px;
}
```

---

## 7. Naming: BEM

```css
.patient-card {
} /* Block */
.patient-card__header {
} /* Element */
.patient-card__body {
} /* Element */
.patient-card--active {
} /* Modifier */
.patient-card--paused {
} /* Modifier */
```

---

## 8. Kein `!important`

Wenn `!important` noetig erscheint, ist die Spezifitaet falsch. Einzige Ausnahme: Utility-Klassen.

---

## 9. Performance

- `will-change` nur gezielt (Animationen)
- Kein Layout-Thrashing (lesen + schreiben im selben Frame)
- `content-visibility: auto` fuer lange Listen
- Bilder: `aspect-ratio` statt Padding-Hack, `loading="lazy"`

```css
.patient-row {
	content-visibility: auto;
	contain-intrinsic-size: 0 64px;
}
```

---

## 10. Animationen

Nur `transform` und `opacity` animieren. **Keine Layout-Properties** (width, height, margin, padding).

```css
/* RICHTIG */
.modal {
	transition:
		transform 200ms ease,
		opacity 200ms ease;
}

/* FALSCH */
.modal {
	transition: height 200ms ease;
}
```

---

## Pruef-Checkliste

Wenn eine CSS-Datei geprueft wird, auf folgende Verstoesse achten:

1. **Layout**: `display: flex` wo Grid besser waere?
2. **Hardcoded Werte**: Farben/Abstaende ohne `var(--...)`?
3. **Magic Numbers**: Werte die nicht aus dem Token-System kommen?
4. **Typografie**: Schriftgroesse < 16px fuer Fliesstext/Inputs?
5. **BEM**: Klassen die nicht dem BEM-Schema folgen?
6. **!important**: Unnoetige `!important` Deklarationen?
7. **Animationen**: Layout-Properties animiert statt transform/opacity?
8. **Performance**: Fehlende `content-visibility` bei langen Listen?
9. **Responsive**: Falscher Ansatz (mobile-first vs. desktop-first)?
10. **Container Queries**: Fehlend bei wiederverwendbaren Komponenten?

Fuer jeden Verstoss: Zeilennummer, Problem, und korrekten Code zeigen.
