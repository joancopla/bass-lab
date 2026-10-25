# Bases de bateria

Fitxers d'àudio de bateria real (wav o mp3) per tocar amb els patrons i el pla d'escales.
L'app no genera bateria: només fa sonar els fitxers d'aquesta carpeta.

## Com afegir una base

1. Copia el fitxer aquí (o en una subcarpeta, per exemple `funk/funk-95.wav`).
2. Afegeix-lo a `loops` de `drums.json`:

```json
{ "file": "funk/funk-95.wav", "name": "Funk sec", "style": "funk", "bpm": 95, "meter": "4/4", "bars": 2 }
```

- `style`: `rock`, `funk`, `reggae` o `shuffle` (són els que suggereixen els exercicis). Se'n poden fer servir d'altres.
- `bpm`: el tempo original de la gravació.
- `meter`: el compàs. Els patrons i el pla fan servir 4/4.
- `bars`: quants compassos dura el fitxer. Si no hi és, es calcula a partir de la durada i el BPM.

La clau `exemple` només mostra el format; l'app la ignora.

## Requisits dels fitxers

- Tallats just a l'inici del primer temps i al final de l'últim compàs, sense silenci al davant ni cua de reverberació: el bucle enllaça el final amb el principi.
- Diverses versions del mateix estil a tempos diferents (per exemple 75, 90 i 105 BPM). L'app tria la més propera al tempo de l'exercici i només l'estira ±15% com a màxim.
- Gravacions pròpies o loops amb una llicència que en permeti la redistribució: aquesta carpeta es publica amb l'app a GitHub Pages.
