**Install the font to prevent text display issues.**

### What is REAWISE:

**An advanced polyphonic, polymetric, probabilistic, Euclidean sequencer...** *JSFX for REAPER*

Inspired by Bitwig's *STEPWISE*, crossed with HoRNet's *RhyGenerator*, stealing and in some cases adapting features from *Elektron* sequencers.
If you make electronic music in Reaper, you might (*should*) like it.

### What is currently in REAWISE v1.5:

* 16 tracks split into two pages of 8.

* SOLO / MUTE per track.

* MIDI channel selection per track (from 1 to 16 or ALL).

* PITCH per track (it is enough to use multiple tracks on the same synth to create chords / melodies without changing the note on the single step... 
  much more convenient and faster... dragging or rolling the mouse wheel modifies semitones, with left click + wheel it modifies octaves).

* PITCH QUANTIZER with visual keyboard to turn notes on / off and create custom scales, 
  ROOT NOTE parameter to set the root note and SCALE to choose from available ones (currently only Chromatic, Major, and Minor).

* LFO per track (sine, triangle, sawtooth, inverted sawtooth, square, sample & hold. 
  In sync with REAPER or freely settable via frequency. With or without phase retrig). 
  LFOs can modulate almost all sequencer parameters and modulate each other: it is possible to link an LFO to any other LFO, except to itself.

* Time DIVISION and MULTIPLICATION from /16 to x16.

* PATTERN LENGTH per track up to 64 steps split into 4 pages of 16 
  (non-destructive, if you write a 32-step sequence and go down to 16, returning to 32 you will still have your sequence intact).

* Non-destructive EUCLIDEAN MODE to preserve manually written sequences 
  (the sequence is overwritten by the Euclidean algorithm only if the FILL (F) parameter is modified, 
  and even with Euclidean mode active, sequences remain manually editable).

* TOOLTIPS in both English and Italian (because I'm Italian), most information is already present inside the plugin itself, 
  what is not found here is already found there (and even many things found here are already found there).

* UNDO / REDO with 16 states.

* DEL to instantly turn off all steps and reset dedicated parameters.

* Bidirectional painting function to generate chords with a single click dragging the mouse vertically, 
  or to fill 16 steps with a single click dragging the mouse horizontally.

* SWING per track (-100% / +100%).

* SHIFT (arrow buttons) to move the sequence forward or backward by one step.

* FLIP to mirror the sequence.

* RANDOM (?) to generate pseudo-random sequences.

* PLAYBACK MODE: forward, backward, ping pong, stochastic.

* Parameter reset with double click.

* Buttons that activate the mouse wheel to modify parameters per single step 
  (in this case parameter reset happens with a single middle click, to avoid conflicts with step on/off):

    * VELOCITY (standard 0-127, excursion +/-5, holding down the right click allows fine adjustment +/-1).
    * Visual feedback with gradual step transparency. Always visible.

    * GATE (ranging from 0.125 up to all 64 steps, or INF: notes run freely until they encounter a subsequent active step).
    * Visual feedback with a horizontal line inside the step that lengthens / shortens. Visible when the button is active.

    * PAN (standard -100 / +100, excursion +/-5, holding down the right click allows fine adjustment +/-1).
    * Visual feedback with a vertical line moving left / right. Visible when the button is active.

    * TIMING (-50% / +50%, excursion +/-5, holding down the right click allows fine adjustment +/-1).
    * Visual feedback same as Pan. Visible when the button is active.

    * RATCHETING (sub-step division up to 4).
    * Visual feedback with vertical lines dividing the step (4 sub-steps = 3 lines). Visible when the button is active.

    * CHANCE (in collaboration with DENSITY on the track which further scales percentages, 
    if a step has a 50% chance to play and the track has 50%, that single step has a 0% chance / 
    if a step has an 80% chance and the track has 90%, that single step has a 70% chance... etc...
    Excursion +/-5, holding down the right click allows fine adjustment +/-1).
    * Visual feedback with a vertical line lowering and raising. Visible when the button is active.

    * TRIG CONDITIONS (entire Elektron list, 43 conditions + off. NEI and /NEI conditions if used on track 1 will take track 16 as the
    "neighbor track").
    * Textual visual feedback inside the steps. Visible when the button is active. Maybe not very readable, but I have a solution...

* Textual / numerical values of each parameter per step, printed in the bottom bar when the mouse is over a step.

* Ability to directly type desired values inside many parameters, activating the text box with a right click.
To know which parameters support this, turn on TIPS and hover over the parameters or simply right click randomly... certainly nothing will break!
For the PITCH parameter, you can write the MIDI value (e.g., 60 = C4), standard notation (c, d, e), Italian notation (do, re, mi).
For black notes (C#, D#, F#) use cs, ds, fs because "#" is intercepted by REAPER. In Italian notation add b or d for flat and sharp.
It is also possible to write just the note if you want to preserve the octave or just the octave if you want to preserve the note.
Any input is converted to standard notation.

* Right-click context menu on empty parts of the interface, to set a sync offset for drawing the playhead, both globally and per individual track. 
Different plugins introduce different latencies shifting playback by a few steps; this only happens if MIDI sends are used and not if VSTis are cascaded on the same track.
Furthermore, changing the time division can also introduce playhead shifts.
I managed to implement under-the-hood offsets to auto-adjust the playhead when there are no VSTis, but I cannot calculate offsets for every VSTi,
so it remains the end user's task to adjust the playhead when necessary.
In any case, it is only a drawing issue; the audio/midi engine is completely synchronized.
Offsets range from +16 to -16, which is most likely more than needed, but this way I am sure to cover every requirement.
It's a bit clunky, I know; at the moment I don't have other solutions and I don't know if there will be in the future,
it's the only problem that is really driving me crazy, I don't think there are elegant solutions to auto-compensate everything behind the scenes.

---

### What is planned for v2:

* Unipolar option for the LFO, currently only bipolar.
* Addition of many scales in the quantizer.
* Sequence transposition by +/-1 semitone and +/-1 octave.
* 16 Patterns.
* Bug fixing, if any.
* Optimizations wherever possible.

---

### A couple of extra words:

* Do whatever you want with this plugin. Use it, don't use it, modify it if you want, steal my ideas to make another one, whatever you like—consider it yours!

* I'm not asking for donations, I'm not asking to "buy me a coffee." 
  I didn't make this sequencer for you; I made it for myself, but if it turns out to be useful to someone else, I'm glad. 
  Maybe I'm not the only one who has been looking for a sequencer like this for a long time, especially in REAPER.

* If you're interested in the project, the only thing I ask for is feedback, of any kind.

* If you have feature requests, you can make them, but I don't guarantee they will be implemented. 
  I'll try to do it if they are features that interest me too and maybe hadn't crossed my mind yet.

* There shouldn't be any bugs, besides the playhead quirks, but it's possible there might be some I haven't noticed.
  So I guarantee the sequencer works at 90 - 95%, I can't guarantee it 100% just yet.

* Note: As a fan of *Reapertips* and a user of his theme, the plugin was designed to fit his theme. 
  If you use different themes and it ends up looking bad aesthetically, I feel sorry for you—go ahead and tweak the GUI. <3

**Thanks for your attention.**
