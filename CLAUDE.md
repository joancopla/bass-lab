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
- Moviment només com a resposta a una acció, curt (150–400 ms) i desactivat amb `prefers-reduced-motion`: la corda tocada vibra en sonar, el punt de la pregunta llisca a la nova posició, transicions de pantalla amb el màstil com a element compartit. Sense confeti ni celebracions exagerades.
- Tema **fosc per defecte**, amb degradats molt subtils que donin profunditat. A la configuració es pot triar clar o automàtic (segons `prefers-color-scheme`). El màstil és de banús en tots dos temes.

## Mòduls d'exercici
1. **Màstil → nota**: es marca una posició al màstil i l'usuari tria el nom de la nota.
2. **Nota → màstil**: surt un nom de nota i l'usuari la toca al màstil. Opció "totes les posicions": ha de trobar-la a totes les posicions dins el rang triat.
3. **Partitura → nota**: surt una nota en clau de fa i l'usuari diu el nom.
4. **Partitura → màstil**: surt una nota en clau de fa i l'usuari la toca al màstil, a l'altura correcta (no val la mateixa nota en una altra octava).
5. **Notació**: traducció entre notació llatina i anglesa, en els dos sentits.
6. **Connexions del màstil**: en triar una nota, es mostren totes les seves posicions alhora i es remarquen les formes d'octava. Exercici: es marca una posició i l'usuari ha de trobar la mateixa nota una octava amunt o avall en una altra zona.
7. **Zones**: qualsevol exercici es pot limitar a una zona concreta (trasts 0–4, 5–9, 10–15, 12–20) per conquerir el màstil per blocs.

### Mòduls afegits després
- **Oïda (relativa)**: sona una nota marcada al màstil i després una altra a prop (fins a una quinta amunt o avall); l'usuari la troba al màstil. Cal el so activat.
- **Repàs d'errors**: sessió que barreja mòduls amb els ítems de pes alt (errors no compensats) que encaixen amb la configuració; s'acaba sola quan ja no en queden.

### Eines sense preguntes
- **Mapa de connexions**: tria una nota i mostra totes les seves posicions, amb colors per octava i les formes d'octava.
- **Toca lliure**: tot el mànec (fins al trast 20 o més); en tocar una posició sona la nota i es mostra el nom en les dues notacions, l'altura real, la freqüència i la nota escrita al pentagrama.
- **Escales i arpegis** (dades a `bass-lab-escales.json`): mànec sempre de 4 cordes i 24 trasts (`instrument`), amb el patró calculat com a fonamental + offsets `[corda, trast]`. Es tria escala o arpegi, to i corda de la fonamental (Mi o La), i es mostren els graus o els noms a cada nota. Inclou el botó «Toca l'escala» (pujant i baixant, una nota per temps), el pla de 52 setmanes amb la tasca del dia i un metrònom amb el BPM del dia (`bpm_start + bpm_delta`, sense acumular).
  - Les dades van incrustades a l'`index.html` (`<script id="scales-data">`) perquè l'app funcioni obrint el fitxer directament. Si canvia el JSON, cal tornar a enganxar-ne la versió compactada.
  - Noms per grau (una lletra per grau). Si el to té dues grafies, s'agafa la que té menys alteracions (Re♭ menor → Do♯ menor) i, si empaten, la regla `flat_keys_pc`.
  - Si el patró cau per sota del trast 0, la fonamental puja al trast 12.
  - El pla compta des del primer dia que s'obre la pantalla; es pot marcar un altre dia com a avui.

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

## Pendent (anotat el 30/09/2026, per ordre de prioritat)
1. **Exportar i importar el progrés** (configuració, pesos, estadístiques i historial) com a fitxer JSON des de la configuració. Motiu: Safari esborra el `localStorage` després de 7 dies sense visitar la web; també serveix per passar el progrés entre aparells.
2. **Afinador pel micròfon** (excepció a "Detecció de notes pel micròfon"): YIN, suposant la corda a l'aire més propera per evitar errors d'octava, mode "harmònic del trast 12", agulla de ±50 cèntims i verd a ±3. Desactivar echoCancellation, noiseSuppression i autoGainControl. Apagar el micròfon en sortir de la pantalla.
3. **Escales i arpegis al màstil**: veure-les ja està fet (25/10/2026, mòdul «Escales i arpegis»). Falta l'exercici amb preguntes ("toca la tercera de Sol").
4. **Lectura de frases curtes**: 4–8 notes en clau de fa per llegir i tocar seguides.
5. **Intervals al màstil**: la forma de cada interval, enllaçada amb l'exercici d'oïda.

Pendent de l'usuari: fer-la servir una setmana, provar-la al mòbil real i escoltar el so amb auriculars.
