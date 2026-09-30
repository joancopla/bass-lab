# CLAUDE.md — Bass Lab: app per aprendre les notes del baix

## Què és aquest projecte
App web d'ús personal per aprendre:
- Les notes del màstil del baix elèctric, a totes les zones i no només a les 2 o 3 posicions habituals.
- La notació anglesa (C, D, E…) i la llatina (Do, Re, Mi…), i a passar d'una a l'altra.
- La lectura de partitura tradicional en clau de fa.

L'usuari toca el baix des de fa 3 anys, però no domina el màstil ni la lectura. No és un curs per a principiants: és una eina d'entrenament diari amb sessions curtes i repetició dels errors.

Idioma de la interfície: **català**.

## Tecnologia
- Un sol fitxer `index.html` amb HTML, CSS i JavaScript vanilla.
- Sense backend, sense build, sense frameworks i sense dependències externes (ni CDN).
- Ha de funcionar obrint el fitxer directament al navegador.
- Responsive: ordinador i mòbil, amb botons i trasts prou grans per tocar amb el dit.
- Persistència amb `localStorage`: configuració, progrés i estadístiques.
- So amb la Web Audio API, generat per síntesi. Sense samples externs.

## Disseny
- Les captures de referència són a `/design/`. Inspira't en el seu estil visual (paleta, tipografia, espais, sensació general) sense copiar-lo literalment ni fer servir cap logotip, nom o text de la marca original.
- Estètica neta i visual: el màstil és el protagonista.
- Màstil en horitzontal, amb la corda greu a baix (com el baixista el veu mirant avall), marcadors als trasts 3, 5, 7, 9 i 12 (doble punt), i números de trast visibles.
- Tema **fosc per defecte**, amb degradats molt subtils que donin profunditat. A la configuració es pot triar clar o automàtic (segons `prefers-color-scheme`).

## Mòduls d'exercici
1. **Màstil → nota**: es marca una posició al màstil i l'usuari tria el nom de la nota.
2. **Nota → màstil**: surt un nom de nota i l'usuari la toca al màstil. Opció "totes les posicions": ha de trobar-la a totes les posicions dins el rang triat.
3. **Partitura → nota**: surt una nota en clau de fa i l'usuari diu el nom.
4. **Partitura → màstil**: surt una nota en clau de fa i l'usuari la toca al màstil, a l'altura correcta (no val la mateixa nota en una altra octava).
5. **Notació**: traducció entre notació llatina i anglesa, en els dos sentits.
6. **Connexions del màstil**: en triar una nota, es mostren totes les seves posicions alhora i es remarquen les formes d'octava. Exercici: es marca una posició i l'usuari ha de trobar la mateixa nota una octava amunt o avall en una altra zona.
7. **Zones**: qualsevol exercici es pot limitar a una zona concreta (trasts 0–4, 5–9, 10–15, 12–20) per conquerir el màstil per blocs.

## Configuració
- Baix de **4 cordes** (E A D G) o **5 cordes** (B E A D G), en afinació estàndard.
- Rang de trasts personalitzable (per exemple 0–5, 0–12, 0–20) i presets de zona (mòdul 7).
- Activar i desactivar cordes concretes.
- Alteracions: només naturals / sostinguts / bemolls / tots dos.
- Notació de resposta: llatina, anglesa o barrejada.
- So activat o desactivat.
- Tema: fosc (per defecte), clar o automàtic.
- Mida de sessió: 10, 20 o 50 preguntes, o mode lliure.

## Referència musical (verificar sempre contra aquesta taula)

### Noms de notes
| Llatí | Anglès |
|---|---|
| Do | C |
| Re | D |
| Mi | E |
| Fa | F |
| Sol | G |
| La | A |
| Si | B |

Alteracions: ♯ = sostingut, ♭ = bemoll (Do♯ = C♯, Re♭ = D♭). En mode "tots dos", s'accepten les dues grafies enharmòniques (C♯ = D♭). Evita per defecte notes com Mi♯, Fa♭, Si♯ o Do♭.

### Cordes a l'aire (altura real, notació científica, MIDI)
| Corda | Nota | MIDI | Freqüència aprox. |
|---|---|---|---|
| B (5a corda) | B0 | 23 | 30,9 Hz |
| E | E1 | 28 | 41,2 Hz |
| A | A1 | 33 | 55,0 Hz |
| D | D2 | 38 | 73,4 Hz |
| G | G2 | 43 | 98,0 Hz |

- Nota a un trast: `MIDI = MIDI_corda_aire + trast`.
- Freqüència: `f = 440 × 2^((MIDI − 69) / 12)`.

### Formes d'octava (mòdul 6)
- 2 cordes amunt + 2 trasts. Exemple: E a l'aire (E1) → corda D, trast 2 (E2).
- 3 cordes amunt − 3 trasts. Exemple: corda E, trast 5 (A1) → corda G, trast 2 (A2).
- Mateixa corda + 12 trasts.

## Partitura en clau de fa
- Pentagrama dibuixat amb **SVG propi**, sense llibreries (res de VexFlow ni similars).
- **El baix és un instrument transpositor d'octava: s'escriu una octava per sobre del que sona.**
  - Nota escrita = nota real + 12 semitons.
  - Exemple: la corda E a l'aire sona E1 però s'escriu E2, a la **primera línia addicional per sota** del pentagrama en clau de fa.
- Línies del pentagrama en clau de fa (de baix a dalt): G2, B2, D3, F3, A3. Espais: A2, C3, E3, G3.
- Per sota: F2 (espai sota el pentagrama), E2 (1a línia addicional), D2, C2 (2a línia addicional)…
- Per sobre: B3 (espai), C4 (1a línia addicional)…
- Dibuixa les línies addicionals que calguin i les alteracions a l'esquerra de la nota.
- Als mòduls 3 i 4, limita les notes al rang que permeten la configuració de cordes i trasts.

## Sistema d'aprenentatge
- Cada combinació (posició o nota × mòdul) té un pes. Els errors i les respostes lentes n'augmenten el pes i els encerts ràpids el redueixen. Les preguntes se sorteja segons el pes.
- Evita repetir la mateixa pregunta dues vegades seguides.
- Retorn immediat:
  - Si és correcte: marca verda, so de la nota i pas a la següent.
  - Si és incorrecte: es mostra la resposta correcta al màstil i/o al pentagrama, i sona la nota correcta.
- Estadístiques:
  - Percentatge d'encerts per mòdul.
  - Temps mitjà de resposta.
  - **Mapa de calor del màstil** amb les posicions on es falla més.
  - Historial de sessions (data, mòdul, encerts).
- Opció per reiniciar les estadístiques, amb confirmació.

## So
- So de baix per **model físic de corda pinçada (Karplus-Strong)**, calculat en JavaScript: excitació de dit, línia de retard amb pèrdues, retard fraccionari (allpass) per a l'afinació exacta i filtre de pinta de pastilla. Sense samples externs.
- L'afinació s'ha de mantenir dins de ±2 cèntims (verificable per autocorrelació).
- Ha de sonar a l'**altura real** (no a l'escrita).
- L'àudio s'ha d'activar amb la primera interacció de l'usuari (restricció dels navegadors).

## Com treballar en aquest projecte
1. **Abans d'escriure codi**, proposa l'estructura i un esbós de la interfície, i espera validació.
2. Implementa per fases:
   - **Fase 1**: màstil + configuració + mòduls 1 i 2 + persistència.
   - **Fase 2**: pentagrama SVG + mòduls 3 i 4.
   - **Fase 3**: mòduls 5, 6 i 7.
   - **Fase 4**: estadístiques, mapa de calor i so afinat.
3. Al final de cada fase, verifica el càlcul de notes amb la taula de referència: comprova almenys les cordes a l'aire, el trast 5, el trast 12 i les notes escrites E2 i C4 al pentagrama.
4. No afegeixis dependències ni funcionalitats fora d'aquest document sense preguntar.

## Fora d'abast (de moment)
- Detecció de notes pel micròfon.
- Cançons, tabs o contingut amb drets d'autor.
- Comptes d'usuari, sincronització o backend.
