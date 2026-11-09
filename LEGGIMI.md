# REAWISE

**Step sequencer MIDI per REAPER — 16 tracce, 64 step, 16 pattern, LFO, generatore di ritmi euclidei, probabilità, trig condizionali e quantizzatore di scale...**

## Cos'è REAWISE

REAWISE è un **MIDI step sequencer** che vive dentro REAPER come effetto JSFX. Non produce audio: genera note, pan e controlli MIDI e li manda allo strumento (synth, sampler, drum machine virtuale).

Ogni **traccia** è una voce monofonica con la sua nota, il suo canale MIDI, la sua risoluzione e la sua lunghezza. Sopra la griglia di step c'è tutto quello che serve a rendere vivo un pattern: velocity, gate, pan, micro-timing, ratchet, probabilità e trig condizionali per ogni singolo step. In più ci sono 16 LFO, un generatore di ritmi euclidei, un quantizzatore con 54 scale, 16 pattern con concatenazione e undo/redo.

Il sequencer è **agganciato al trasporto di REAPER**: parte, si ferma e segue il tempo e la posizione del progetto.

---

**Installa il font per vedere l'interfaccia corretta.**

---

## Caratteristiche in breve

**Struttura:** 16 tracce (8 per pagina), 64 step per traccia (16 per pagina), lunghezza di ogni traccia da 1 a 64 step.
**Pattern:** 16 pattern in memoria, copia/taglia/incolla, cambio in coda a fine ciclo ed editing in background. **CHAINING** fino a 64.
**Per traccia:** canale MIDI (1–16 o ALL), nota, rate (11 risoluzioni), lunghezza, solo, mute, volume, densità, swing, pan, gate, modalità di riproduzione (avanti, indietro, ping-pong, casuale).
**Per step:** on/off, velocity, gate (70 valori, con INF e LEG), pan, micro-timing, ratchet (1–4), chance (0–100%), trig condizionali.
**Trig condizionali:** oltre 270 condizioni: rapporti `A:B` fino a 16:16, `1ST`, `PRE`, `NEI` e versioni inverse `/A:B`, `/1ST`, `/PRE`, `/NEI`.
**Euclideo:** generatore di ritmi euclidei (algoritmo di Bjorklund) per traccia.
**Quantizzatore:** 54 scale (modi, pentatoniche, blues, scale del mondo, jazz/simmetriche) + scala personalizzata disegnata sulla tastiera.
**LFO:** un LFO per traccia, 6 forme d'onda, sync al tempo o frequenza libera, 64 destinazioni (anche altri LFO e il Master).
**Master:** volume, densità, swing/gate, offset di lunghezza, trasposizione (ottave e semitoni in scala) 
**Comodità:** Undo/Redo a 16 livelli, tooltip integrati in inglese e in italiano, interfaccia ridimensionabile (ha una dimensione iniziale di 1324 × 538 pixel e scala con il ridimensionamento).

---

## Concetti fondamentali

### Una traccia, una nota

Ogni traccia suona **una sola nota MIDI**, scelta nel campo *Nota* della traccia. Gli step decidono *quando* quella nota suona e *come* (velocity, gate, pan, timing...), ma non *quale* nota. Non esiste un campo "nota" per singolo step.

Se vuoi una linea melodica, la costruisci distribuendo le note su più tracce (sullo stesso canale MIDI) oppure facendole muovere con un LFO sul `PITCH`. Per un kit di batteria è il modello ideale: una traccia per ogni suono.

Ogni traccia è inoltre **monofonica**: quando parte una nuova nota, la precedente della stessa traccia viene chiusa.

### Rate e lunghezza: ogni traccia ha il suo orologio

Ogni traccia ha un **rate** (quanto dura uno step) e una **lunghezza** (quanti step fa prima di ricominciare). Tracce con rate o lunghezze diverse creano poliritmi e pattern che si sfasano.

### Cicli

Un **ciclo** è un giro completo del pattern di una traccia. Ogni traccia ha il proprio contatore di cicli, che alimenta i trig condizionali. 
Il contatore riparte da zero all'avvio del trasporto e a ogni cambio di pattern.

### Cosa succede a ogni step (in ordine)

1. La traccia deve poter suonare (non in mute; se almeno una traccia è in solo, suonano solo le tracce in solo).
2. Lo step deve essere attivo (o attivo per via del ritmo euclideo).
3. Deve passare il **trig condizionale** dello step.
4. Deve passare la **probabilità** (chance dello step, densità della traccia, densità master).
5. Si calcola la nota (con eventuale modulazione LFO) e la si **quantizza** sulla scala attiva.
6. La velocity viene scalata dal volume della traccia e dal volume master.
7. Il pan viene mandato (CC 10) prima della nota, solo se è cambiato.
8. Swing e micro-timing spostano l'istante della nota; il ratchet la divide in sotto-step; il gate ne decide la durata.

---

## L'interfaccia

Ecco le aree principali, descritte per funzione.

**Quantizzatore:** mini tastiera di 12 note, `ROOT NOTE`, `SCALE`.
**Pagine:** `1-8` / `9-16` per le tracce; `1-16`, `17-32`, `33-48`, `49-64` per gli step.
**Barra delle modalità:** `VELOCITY`, `GATE`, `PAN`, `TIMING`, `RATCHETING`, `CHANCE`, `COND`: decidono cosa modifica la rotella sugli step. Ne è attiva **una alla volta**.
**Strip delle tracce:** per ogni traccia: `S` (solo), `M` (mute), canale, nota, rate, lunghezza, beat euclidei, la fila di 16 step, due slot modulari, pulsanti di shift/flip/random, modalità di riproduzione.
**Barra LFO:** selezione traccia LFO, forma d'onda, sync/freq, velocità, retrigger, destinazione, ampiezza.
**EUC:** interruttore globale della modalità euclidea.
**Trasposizione:** `OCT` (ottave), `ST` (semitoni), `RST` (reset).
**Master:** volume con meter, densità, swing/gate, lunghezza.
**Pattern:** 16 pulsanti, `CHAINING`.
**Utility:** `UNDO`, `REDO`, `DEL` (Clear All), `TIPS`, `ENG`/`ITA`.

I **tooltip** (pulsante `TIPS`) descrivono ogni controllo e riportano anche le scorciatoie del mouse. La lingua dei tooltip si cambia con il pulsante `ENG`/`ITA`.
Il miglior manuale è già lì dentro.

---

## Limiti e cose da sapere

- **Una nota per traccia.** Non esiste una nota per singolo step: per le melodie si usano più tracce o l'LFO sul pitch.
- **Solo MIDI.** REAWISE non genera suono e non ha ingressi/uscite audio.
- **Serve il trasporto.** Con REAPER fermo non parte nulla.
- Il **pan** è inviato come CC 10: dipende dallo strumento.
- Il **legato** richiede lo stesso canale MIDI tra le tracce coinvolte e non funziona con ratchet maggiore di 1.
- L'**LFO** agisce solo quando ha una destinazione e il trasporto è in marcia.
- Il quantizzatore sposta le note **verso il basso** a parità di distanza dalla nota ammessa.
- **Undo/Redo** coprono 16 modifiche.

Il plugin è ancora in fase di sviluppo, potrebbe presentare bug o comportamenti non previsti.
La versione 1.0 e 1.5 erano un disastro, realizzate troppo di fretta e con troppa superficialità, colpa mia!
Ho lavorato sodo fino alla versione 1.8 per rendere il sequencer stabile e utilizzabile, non è ancora perfetto però.
Continuerò a lavorarci prendendomi il tempo necessario per migliorarlo sempre di più.

---
