# REAWISE

**MIDI step sequencer for REAPER — 16 tracks, 64 steps, 16 patterns, LFOs, a Euclidean rhythm generator, probability, conditional trigs and a scale quantizer...**

## What is REAWISE?

REAWISE is a **MIDI step sequencer** that runs inside REAPER as a JSFX effect. It does not produce audio: it generates MIDI notes, pan and controls and sends them to an instrument (synth, sampler or virtual drum machine).

Each **track** is a monophonic voice with its own note, MIDI channel, resolution and length. Above the step grid is everything you need to bring a pattern to life: velocity, gate, pan, micro-timing, ratchet, probability and conditional trigs for each individual step. There are also 16 LFOs, a Euclidean rhythm generator, a quantizer with 54 scales, 16 patterns with chaining, and undo/redo.

The sequencer is **tied to REAPER's transport**: it starts, stops and follows the project's tempo and position.

---

**Install the font to display the interface correctly.**

**Disable Anticipative FX in REAPER's settings to display the playhead correctly.**

---

## Features at a glance

**Structure:** 16 tracks (8 per page), 64 steps per track (16 per page), track length from 1 to 64 steps.  
**Patterns:** 16 patterns in memory, copy/cut/paste, queued changes at the end of a cycle, and background editing. **CHAINING** up to 64.  
**Per track:** MIDI channel (1–16 or ALL), note, rate (11 resolutions), length, solo, mute, volume, density, swing, pan, gate, playback mode (forward, backward, ping-pong, random).  
**Per step:** on/off, velocity, gate (70 values, including INF and LEG), pan, micro-timing, ratchet (1–4), chance (0–100%), conditional trigs.  
**Conditional trigs:** over 270 conditions: `A:B` ratios up to 16:16, `1ST`, `PRE`, `NEI` and inverse versions `/A:B`, `/1ST`, `/PRE`, `/NEI`.  
**Euclidean:** Euclidean rhythm generator (Bjorklund algorithm) per track.  
**Quantizer:** 54 scales (modes, pentatonic, blues, world, jazz/symmetric scales) + a custom scale drawn on the keyboard.  
**LFO:** one LFO per track, 6 waveforms, tempo sync or free-running frequency, 64 destinations (including other LFOs and the Master).  
**Master:** volume, density, swing/gate, length offset, transposition (octaves and scale semitones).  
**Conveniences:** 16-level Undo/Redo, built-in tooltips in English and Italian, resizable interface (initial size 1324 × 538 pixels, scales when resized).

---

## Core concepts

### One track, one note

Each track plays **one MIDI note**, selected in the track's *Note* field. The steps decide *when* that note plays and *how* (velocity, gate, pan, timing...), but not *which* note. There is no per-step note field.

To create a melodic line, spread the notes across multiple tracks (on the same MIDI channel) or use an LFO on `PITCH` to move them. For a drum kit, this is the ideal model: one track per sound.

Each track is also **monophonic**: when a new note starts, the previous note on the same track is stopped.

### Rate and length: each track has its own clock

Each track has a **rate** (how long a step lasts) and a **length** (how many steps it plays before starting over). Tracks with different rates or lengths create polyrhythms and patterns that drift out of phase.

### Cycles

A **cycle** is one complete pass through a track's pattern. Each track has its own cycle counter, which drives conditional trigs.  
The counter resets to zero when the transport starts and whenever the pattern changes.

### What happens on each step (in order)

1. The track must be allowed to play (not muted; if at least one track is soloed, only soloed tracks play).
2. The step must be active (or active because of the Euclidean rhythm).
3. The step's **conditional trig** must pass.
4. The **probability** check must pass (step chance, track density, master density).
5. The note is calculated (with any LFO modulation) and **quantized** to the active scale.
6. Velocity is scaled by the track volume and master volume.
7. Pan is sent (CC 10) before the note, only if it has changed.
8. Swing and micro-timing shift the note's timing; ratchet divides it into sub-steps; gate determines its duration.

---

## The interface

Here are the main areas, described by function.

**Quantizer:** 12-note mini keyboard, `ROOT NOTE`, `SCALE`.  
**Pages:** `1-8` / `9-16` for tracks; `1-16`, `17-32`, `33-48`, `49-64` for steps.  
**Mode bar:** `VELOCITY`, `GATE`, `PAN`, `TIMING`, `RATCHETING`, `CHANCE`, `COND`: determine what the mouse wheel edits on the steps. **One is active at a time.**  
**Track strips:** for each track: `S` (solo), `M` (mute), channel, note, rate, length, Euclidean beats, the row of 16 steps, two modular slots, shift/flip/random buttons, playback mode.  
**LFO bar:** LFO track selection, waveform, sync/freq, speed, retrigger, destination, amplitude.  
**EUC:** global switch for Euclidean mode.  
**Transposition:** `OCT` (octaves), `ST` (semitones), `RST` (reset).  
**Master:** volume with meter, density, swing/gate, length.  
**Pattern:** 16 buttons, `CHAINING`.  
**Utility:** `UNDO`, `REDO`, `DEL` (Clear All), `TIPS`, `ENG`/`ITA`.

The **tooltips** (`TIPS` button) describe each control and also show mouse shortcuts. Change the tooltip language with the `ENG`/`ITA` button.  
The best manual is already in there.

---

## Limitations and things to know

- **One note per track.** There is no per-step note: use multiple tracks or the pitch LFO for melodies.
- **MIDI only.** REAWISE does not generate sound and has no audio inputs or outputs.
- **Transport required.** Nothing plays while REAPER is stopped.
- **Pan** is sent as CC 10: its effect depends on the instrument.
- **Legato** requires the same MIDI channel across the tracks involved and does not work with ratchet greater than 1.
- The **LFO** only operates when it has a destination and the transport is running.
- The quantizer moves notes **downward** when they are equally distant from two allowed notes.
- **Undo/Redo** cover 16 changes.

The plugin is still in development and may have bugs or unexpected behavior.  
Versions 1.0 and 1.5 were a disaster, made too hastily and with too little care — my fault!  
I worked hard through version 1.8 to make the sequencer stable and usable, though it is not perfect yet.  
I will keep working on it, taking the time needed to make it better and better.

---
