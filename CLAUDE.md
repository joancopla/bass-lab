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
- So amb la Web Audio API, generat per síntesi. Sense samples externs. Excepció: les bases de bateria són fitxers d'àudio reals de `/drums`; només si un estil no en té, sona una bateria sintètica de reserva.

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
- **Graus de l'escala**: amb la fonamental marcada al màstil, toca un grau d'una escala o arpegi, o digues quin grau és un punt marcat. Les notes i els graus surten d'una lletra per grau, com a la pantalla d'Escales.
- **Lectura de frases**: 4–8 notes en clau de fa per tocar seguides a l'altura exacta. En acabar es mostren els noms i es pot escoltar la frase.
- **Intervals al màstil**: des d'una nota marcada, toca l'interval demanat (fins a l'octava, amunt o avall) a l'altura exacta. En respondre es dibuixa la forma.
- **Repàs d'errors**: sessió que barreja mòduls amb els ítems de pes alt (errors no compensats) que encaixen amb la configuració; s'acaba sola quan ja no en queden.

### Eines sense preguntes
- **Mapa de connexions**: tria una nota i mostra totes les seves posicions, amb colors per octava i les formes d'octava.
- **Toca lliure**: tot el mànec (fins al trast 20 o més); en tocar una posició sona la nota i es mostra el nom en les dues notacions, l'altura real, la freqüència i la nota escrita al pentagrama.
- **Escales i arpegis** (dades a `bass-lab-escales.json`): mànec sempre de 4 cordes i 24 trasts (`instrument`), amb el patró calculat com a fonamental + offsets `[corda, trast]`. Es tria escala o arpegi, to i corda de la fonamental (Mi o La), i es mostren els graus o els noms a cada nota. Inclou el botó «Toca l'escala» (pujant i baixant, una nota per temps), el pla de 52 setmanes amb la tasca del dia i un metrònom amb el BPM del dia (`bpm_start + bpm_delta`, sense acumular).
  - Les dades van incrustades a l'`index.html` (`<script id="scales-data">`) perquè l'app funcioni obrint el fitxer directament. Si canvia el JSON, cal tornar a enganxar-ne la versió compactada.
  - Noms per grau (una lletra per grau). Si el to té dues grafies, s'agafa la que té menys alteracions (Re♭ menor → Do♯ menor) i, si empaten, la regla `flat_keys_pc`.
  - Si el patró cau per sota del trast 0, la fonamental puja al trast 12.
  - El pla compta des del primer dia que s'obre la pantalla; es pot marcar un altre dia com a avui.
  - A l'inici és el **curs destacat**, a dalt de tot: setmana i fase, tema, tasca i BPM del dia. «Practica la d'avui» obre Escales amb l'escala i el to de la setmana carregats al mànec.
  - **Pentagrama enllaçat** sota el mànec: les notes del patró pujant, en clau de fa i a l'altura escrita (+12), amb la grafia de l'escala (Mi♯, Do♭…). Tocar una nota al mànec, al pentagrama o a la tira la ressalta als tres llocs, i «Toca l'escala» les recorre alhora.
  - Alteracions commutables: a cada nota (per defecte, amb becaire si la lletra torna natural) o amb armadura. L'armadura és la de la major o la menor natural de referència (els modes, la seva; el 7 de dominant, la del mixolidi). Re♭ menor melòdica no en té d'estàndard i va sense.
  - **Notació pròpia** (llatina o anglesa) compartida per Escales i la Roda, a part de la dels exercicis.
- **Roda harmònica** (cercle de quintes en SVG propi): fora, les 12 majors en ordre de quintes amb el nombre d'alteracions; dins, les relatives menors. En triar una tonalitat es ressalten el I, el IV i el V i les relatives ii, iii i vi (en una menor: i, iv, v, III, VI, VII), i es mostren els 7 acords diatònics. «Veure al mànec» obre l'escala major (o la menor natural) a la pantalla d'escales. La roda és fixa, amb Do a dalt.
  - `key_spelling` decideix només la fonamental de la major. La relativa i els acords surten de l'escala, una lletra per grau (Mi → Do♯ m, i no Re♭ m). Fa♯/Sol♭ mostra les dues grafies, però s'escriu amb bemolls.
- **12 patrons que tot baixista ha de saber** (dades a `PT` dins l'`index.html`): targeta destacada a l'inici, sota el curs, amb la barra d'apresos i el patró següent; pantalla pròpia amb una targeta per patró, ordenades per nivell (1–5) i filtrables per nivell, estil i estat.
  - Cada patró s'escriu en La, amb la fonamental a la corda Mi, trast 5: `corda+trast/durada` en semicorxeres, `x` nota morta, `r` silenci, `T`/`P` polze i pop, `|` compàs. Es transporta a qualsevol to amb la fonamental a la corda Mi o La (mateixa regla del trast 12). Graus i noms surten d'una lletra per grau des de la fonamental.
  - Cada targeta té tablatura amb els comptes del ritme, el mànec, la partitura en clau de fa plegable (figures, barres, silencis i notes mortes amb cap en x) i un reproductor amb metrònom i BPM propis. El swing va a 2:1; les notes mortes i el slap tenen veus pròpies.
  - Afegits validats per l'usuari: selector major/menor a la tríada; als patrons 8 i 9, segon compàs sobre el IV que acaba a mig to de la fonamental per tornar a començar.
- **Bases de bateria** (`/drums`, format a `drums/README.md`): fitxers d'àudio de bateria real descrits a `drums.json` (nom, estil, BPM original, compàs i compassos).
  - Bateria sintètica de reserva (decidit per l'usuari el 25/10/2026): si un estil no té cap fitxer real, sona un compàs generat amb Web Audio (bombo, caixa, cop de vora i hi-hat) amb un patró per estil (`DR_SYNTH`), al tempo exacte i amb l'avís a la targeta. Els fitxers reals sempre tenen prioritat.
  - Tempo (decidit per l'usuari el 25/10/2026): amb fitxers reals de l'estil, en lloc de −5/−1/+1/+5 hi ha un selector amb els BPM de les gravacions, que sonen al seu tempo original, sense estirar. En triar l'estil s'agafa la més propera al BPM del patró o del dia. Amb bateria sintètica o clic, el tempo és lliure (30–240) amb els botons ±. Si l'estil té fitxers reals, un commutador «Real / Sintètica» permet triar la sintètica igualment (per targeta de patró i al metrònom).
  - El time-stretch (versió de BPM més proper, ±15% com a màxim i avís al límit) només entra si se li demana un tempo que no és d'una gravació.
  - Time-stretch propi (sense dependències): WSOLA que talla la base a cada cop, col·loca cada cop al seu temps nou i no en duplica cap, circular i amb encadenaments de 2 ms perquè el bucle (`loop=true`) no tingui cap tall. Verificat amb un to de prova: to ±1,2 cèntims, cops a menys de 2 ms del temps.
  - **Volum del baix** (100% · 50% · 25% · Mut), per tocar-hi a sobre: afecta només el baix que toca l'app en reproduir (patrons i «Toca l'escala»), no la bateria ni les notes en tocar el màstil. És global, es desa a la configuració i surt a cada targeta de patró i al metrònom de les escales.
  - Compte enrere d'un compàs (clic del metrònom) i després baix i bateria amb el mateix rellotge. Cada patró suggereix un estil (rock, funk, reggae, shuffle); al metrònom de les escales es tria Clic o un estil.
  - Obrint el fitxer directament (`file://`) no es pot llegir `/drums`: hi ha un botó per triar la carpeta a mà. A GitHub Pages funciona sol.
  - Els fitxers es publiquen al repo: només gravacions pròpies o loops amb llicència que en permeti la redistribució.

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
1. ~~**Exportar i importar el progrés**~~ — fet el 25/10/2026: targeta «Còpia de seguretat» a Configuració. Desa totes les claus `basslab.*` en un JSON (`{ app: 'bass-lab', format: 1, date, data }`); en recuperar, mostra la data i un resum i demana confirmació abans de substituir-ho tot, i després recarrega l'app.
2. **Afinador pel micròfon** (excepció a "Detecció de notes pel micròfon"): YIN, suposant la corda a l'aire més propera per evitar errors d'octava, mode "harmònic del trast 12", agulla de ±50 cèntims i verd a ±3. Desactivar echoCancellation, noiseSuppression i autoGainControl. Apagar el micròfon en sortir de la pantalla.
3. ~~**Escales i arpegis al màstil**~~ — fet el 25/10/2026: veure-les (pantalla «Escales i arpegis») i l'exercici **Graus de l'escala** (mòdul `sd`). Amb la fonamental marcada, «toca la ♭3 de La menor» (qualsevol posició de la nota dins el rang) o «quin grau és?» amb un punt a prop (fins a 2 cordes i 4 trasts). Escales: major, menor natural, pentatòniques major i menor, i arpegis maj7, 7 i m7. Pes per escala × grau × sentit.
4. ~~**Lectura de frases curtes**~~ — fet el 25/10/2026: mòdul `ph`. 4–8 notes en una tonalitat major o menor (amb armadura, fins a 5 alteracions i segons el filtre d'alteracions de la configuració), que comencen en una nota de l'acord, van sobretot per graus conjunts i acaben en 1, 3 o 5. Es toquen seguides a l'altura exacta. Una nota fallada es marca, sona la correcta i es continua. Pes per tonalitat.
5. ~~**Intervals al màstil**~~ — fet el 25/10/2026: al Mapa de connexions, commutador «Octaves / Intervals» (tries nota i interval, i es veuen totes les posicions a l'altura exacta amunt i avall, unides a la fonamental, amb la forma descrita; tocar un punt el fa fonamental). Exercici **Intervals al màstil** (mòdul `iv`, ±1–12 semitons, altura exacta). L'oïda dibuixa la forma en respondre.

Pendent de l'usuari: fer-la servir una setmana, provar-la al mòbil real i escoltar el so amb auriculars.

## Reordenar la usabilitat (acordat el 25/10/2026, quan acabin els pendents 1–5)
L'inici i la navegació s'han anat omplint per capes: exercicis, eines, curs, patrons, bateria… Cal repensar-ho tot junt: agrupar l'inici (entrenar / eines / curs), simplificar les targetes de patrons i el metrònom, i revisar el mòbil.

## Idees per més endavant (anotades el 25/10/2026, després dels pendents 1–5)
- **Lectura de ritme**: pentagrama amb figures (negres, corxeres, semicorxeres, silencis) sobre una sola nota, per tocar-lo amb la bateria. Aprofita el motor de ritme dels patrons.
- **Progressions i línies de baix**: una progressió (I–IV–V, ii–V–I, blues de 12 compassos) amb bateria, i l'usuari hi posa la línia. Es mostren les fonamentals al mànec i, si es vol, una línia de mostra amb aproximacions. Enllaça la Roda harmònica amb els patrons.
- **Notes de l'acord al mànec**: surt un acord (Do7, Lam7…) i cal trobar-ne totes les notes dins la zona. Base per improvisar i per al walking.
