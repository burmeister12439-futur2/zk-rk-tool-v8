# ZK-RK Analysis Tool v8.0 - Multi-Konflikt-Detection

## 🎯 Neue Features in v8.0

### 1. **Multi-Konflikt-Detection (Option A + C)**
- Identifiziert **ALLE** Zielkonflikte im Text (nicht nur einen)
- Rankt Konflikte nach **Zentralität im Text** (0.0 - 1.0)
- Zeigt Hauptkonflikt prominent + weitere Konflikte ausklappbar

### 2. **Verbessertes UI**
- **Hauptkonflikt** wird hervorgehoben (🏆 Badge)
- **Weitere Konflikte** in ausklappbarer Sektion
- **Statistik-Dashboard**: Zeigt Gesamtanzahl und Kategorien
- **Zentralitäts-Score** für jeden Konflikt sichtbar

### 3. **Erweiterte Funktionalität**
- Einzelne Konflikte speicherbar (Button pro Konflikt)
- "Schärfen"-Button vorbereitet (für zukünftige Implementierung)
- CSV-Export aller identifizierten Konflikte
- Verbesserte 3-YES-Bewertung mit visuellen Indikatoren

## 📋 Technische Änderungen

### Backend (main_v8.py)
- Neuer Endpoint: `/analyze-multi` 
- Alter Endpoint `/analyze` bleibt für Rückwärtskompatibilität
- Erweitertes Prompt-Engineering für Multi-Detection
- Automatisches Ranking nach Zentralität
- Robusteres Error-Handling

### Frontend (index_v8.html)
- Responsive Card-Design für mehrere Konflikte
- Collapsible/Expandable Sekundär-Konflikte
- Statistik-Übersicht oben
- Individuelle Action-Buttons pro Konflikt

## 🚀 Deployment auf Railway.app

### Option 1: Update bestehende Deployment

1. **Backup erstellen**
```bash
# Alte Version sichern
git tag v7.1-backup
git push origin v7.1-backup
```

2. **Neue Version deployen**
```bash
cd zk-rk-analysis
git pull  # falls nötig

# Ersetze main.py mit main_v8.py
cp main_v8.py main.py

# Ersetze index.html mit index_v8.html
cp index_v8.html index.html

# Commit und Push
git add .
git commit -m "Update to v8.0: Multi-conflict detection with ranking"
git push origin main
```

Railway.app wird automatisch neu deployen.

### Option 2: Neue Deployment (parallel testen)

1. **Neues GitHub Repo erstellen** (z.B. `zk-rk-analysis-v8`)
2. **Files hochladen**:
   - `main_v8.py` → `main.py`
   - `index_v8.html` → `index.html`
   - `requirements.txt`
3. **Neues Railway Projekt erstellen**
4. **Environment Variable setzen**: `ANTHROPIC_API_KEY`
5. **Deploy**

## 🧪 Testing

### Lokales Testing
```bash
# Backend starten
python main_v8.py

# In anderem Terminal - Test Request
curl -X POST http://localhost:8000/analyze-multi \
  -H "Content-Type: application/json" \
  -d '{"policy_text": "Ihr Testtext hier..."}'
```

### Frontend lokal testen
```bash
# Einfach index_v8.html im Browser öffnen
# API_URL in der Datei auf localhost:8000 ändern für lokales Testing
```

## 📊 Beispiel-Output

```json
{
  "conflicts": [
    {
      "conflict": "Zielkonflikt zwischen X und Y",
      "function_a": "Funktion A",
      "function_b": "Funktion B",
      "implementation_collision": "Konkrete Kollision",
      "centrality_score": 0.95,
      "three_yes": {
        "system_function": true,
        "system_function_reasoning": "...",
        "implementation_collision": true,
        "implementation_reasoning": "...",
        "current_pressure": true,
        "pressure_reasoning": "..."
      },
      "category": "ZENTRAL"
    },
    {
      "conflict": "Weiterer Zielkonflikt...",
      "centrality_score": 0.65,
      ...
    }
  ],
  "total_count": 2
}
```

## 🔄 Kompatibilität

- **Rückwärtskompatibel**: Alter `/analyze` Endpoint funktioniert weiter
- **Bestehende Workflows**: Können schrittweise auf `/analyze-multi` migriert werden
- **CSV-Export**: Format erweitert um Rang und Zentralität

## 🎯 Nächste Schritte (für v8.1+)

1. **Schärfungs-Dialog implementieren** (siehe Punkt 2 der ursprünglichen Frage)
2. **Permanente Speicherfunktion** (siehe Punkt 3)
3. **ZK-Bibliothek** mit Tagging und Suche
4. **Dubletten-Check** beim Speichern
5. **Team-Kollaborations-Features**

## 📝 Verwendung

### Einfacher Text
```javascript
const response = await fetch('API_URL/analyze-multi', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    policy_text: "Ihr Politiktext hier..."
  })
});

const data = await response.json();
console.log(`Gefunden: ${data.total_count} Konflikte`);
```

## 🐛 Troubleshooting

**Problem**: Backend findet keine Konflikte
- **Lösung**: Text muss mindestens 50 Zeichen haben und tatsächliche Policy-Inhalte enthalten

**Problem**: Frontend zeigt Fehler
- **Lösung**: Prüfe Browser Console (F12) und Network Tab für detaillierte Fehler

**Problem**: CSV-Export funktioniert nicht
- **Lösung**: Stelle sicher, dass mindestens ein Konflikt analysiert wurde

## 📞 Support

Bei Fragen oder Problemen: Klaus kontaktieren oder Issue in GitHub erstellen.

---

**Version**: 8.0  
**Datum**: November 2025  
**Status**: Production Ready
