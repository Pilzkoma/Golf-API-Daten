# Recherche-Plan: Berlin/Brandenburg Golfplätze (Abgeschlossen)

> **Status 2026-07-30:** Alle Punkte erledigt (Commit „Add per-hole data for
> Kallin, Mahlow, Großbeeren; complete Motzener See Platz C"). Dokument bleibt
> als Referenz für Datenquellen und Konventionen erhalten.

## Datenquellen (Priorität)
1. **mScorecard.com** – oft vollständige Lochweiten in Metern
2. **1golf.eu** – CR/SR-Daten, Gesamtdistanzen, Lochbeschreibungen
3. **GolfPass / Hole19** – Lochweiten (oft in Yards → ×0.9144 = Meter)
4. **Offizielle Club-Website / Scorecard-PDF** – autoritative Quelle
5. **Where2golf.com** – Backup für CR/SR und Distanzen

## Einheit-Konvention
- **mScorecard & offizielle Sites**: meist echte Meter → direkt verwenden
- **GolfPass / Hole19**: oft Yards (auch wenn "m" steht) → ×0.9144 prüfen
- **Prüfung**: Gesamtdistanz gegen 1golf.eu abgleichen (≤1% Abweichung ok)

---

## Erledigt

### 1. Golfanlage Kallin ✅
- **Ort:** Nauen OT Börnicke (~55 km westlich)
- **Web:** golf-kallin.de
- **Plätze:** 18-Loch (Gelb, Blau ×2, Rot ×2) · 9-Loch (Gelb, Rot)
- **Offen geblieben:** Orange-Tees (18h + 9h) nicht recherchiert/aufgenommen

### 2. Golf Club Mahlow e.V. ✅
- **Ort:** Blankenfelde-Mahlow (~15 km südlich)
- **Web:** gcmahlow.de
- **Platz:** 9-Loch — Gelb CR61.2/SR115, Rot CR61.4/SR113

### 3. GolfRange Berlin-Großbeeren ✅
- **Ort:** Großbeeren (~25 km südlich)
- **Web:** berlin-grossbeeren.golfrange.de
- **Platz:** 9-Loch — Gelb CR62.0/SR102, Rot CR62.8/SR100

### 4. Motzener See – Platz C Blau/Rot (Ergänzung) ✅
- Blau (Damen) und Rot (Damen) aus GolfPass B/C-Kombo eingefügt;
  CR/SR=0 analog zu bestehenden C-Kurs-Tees

---

## JSON-Struktur (aktuell, seit GolfDataEditor-TeeColor-Migration)

```json
{
  "clubName" : "...",
  "courses" : [{
    "courseName" : "...",
    "teeSets" : [{
      "id" : "UUID",
      "name" : "Gelb",
      "color" : "gelb",
      "courseRating" : 72.3,
      "slopeRating" : 129,
      "holes" : [
        {"number" : 1, "par" : 4, "meters" : 360, "handicap" : 11}
      ]
    }]
  }]
}
```

- **`color`** ersetzt das frühere `category` (`"Herren"`/`"Damen"`/`"Profi"`).
  Der Editor liest Altdaten mit `category` noch, schreibt aber nur `color`.
- ⚠️ **Verlust bei der Migration:** Kurse mit gleichfarbigen Herren-/Damen-Tees
  (z. B. Kallin 18h Blau, Wilkendorf, Gatow, Bad Saarow) sind seither nur noch
  über CR/SR unterscheidbar. Vor-Migrations-Stand mit `category` liegt auf
  `main` (Commit 6ae6eea).
- Club- und Kurs-`id`s werden vom Editor nicht mehr geschrieben (nur noch
  TeeSet-`id`s). Die alte UUID-Nummerierung (0024–0027) ist damit obsolet.

## Bekannte Daten-Fallstricke
- GolfPass/Hole19 labeln Yards oft als "m" → immer Gesamtsumme prüfen
- AP Bad Saarow H16: 168m par-4 (Datenfehler, trotzdem so übernommen)
