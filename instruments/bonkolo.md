[← Back to Instrument Index](../index.md)

# Bonkolo — Technical Documentation

**Bonkolo** is a real-time **probabilistic sequencer** built in [Csound](https://csound.com/) and [Cabbage](https://cabbageaudio.com/), designed for improvisational performance with evolving rhythmic patterns, shifting harmonic textures, and dynamic timbral modulation. It features two independently controlled layers—**SOLO** and **CHORD**—each with probabilistic triggering, modulation shaping, and voice articulation.

Inspired by the behavior of bouncing “balls”, Bonkolo models motion-like dynamics through time-varying envelopes, stereo positioning, and per-note decay shaping. Each note behaves as a virtual object with properties such as **jump height**, **gravity-based decay**, **sustain**, and **pan**, producing animated, polyrhythmic textures that shift continuously across the sequence.

Unlike traditional step sequencers or generative plugins, Bonkolo is conceived as a **performance instrument**, not a composition tool. Its unique combination of **bounce-based articulation**, **dual-layer probabilistic control**, and **real-time modulation shaping** enables continuous musical transformation without breaking flow. All parameters are designed for hands-on use via MIDI or OSC interfaces, with no reliance on screen-based editing during play.

<p align="left">
  <img src="../images/bonkolo.png" style="max-width: 100%;" width="700">
  <br>
  <em><strong>Bonkolo interface</strong>, organized by functional groups: SEQUENCE, SCALE & TUNING, SOLO & CHORD modulation, FILTER and bounce shaping, NOISOLO subsystem, TIMBRE modulation, FM synthesis, and the eight-slot preset system. Key parameters are MIDI-mapped for real-time control.</em>
</p>
## Sequencing and Temporal Structure

Bonkolo’s sequencing engine combines rhythmic precision with evolving variation. A global clock (**METRO**) defines the pulse rate, while **RND** introduces smooth, randomized deviations—either freely or synced to the grid—allowing fluid timing variation without breaking continuity. **SWING** adds timing asymmetry using **TYPE** and **PHASE**, shaping the space between notes to produce off-center grooves and microtiming shifts.

Each sequence layer (SOLO and CHORD) advances through a set of pitches using a **directional walk**, which can behave like a random walk, a strict ordered traversal, or anything in between. The behavior is defined by:

- **WALK** – controls how directional or random the traversal is: centered = fully random; left/right = increasingly ordered (ascending or descending),
- **STEP** – pitch interval size per move,
- **RANGE** – the maximum pitch span the walk may traverse,
- **W-SPD** – the rate at which walk direction and shape are re-randomized.

The **DISO** toggle enables periodic reshuffling of the pitch sequence, with **DIS-F** controlling how often this occurs. When **WRAP** is active, the walk cycles continuously; when disabled, it reflects at the top or bottom of the range.

## Scale and Tuning Architecture

Bonkolo’s scale system is built around **diatonic and pentatonic material**—specifically 7- and 5-tone pitch-class structures—drawn from a library of 196 structured scales, derived from **28 curated pitch-class sets** and their **modal rotations**. These include both familiar Church modes and less common modes used for harmonic and melodic variety.

Each scale can be **rotated**, **inverted**, and **transposed** in real time, with key changes following the **circle of fifths**. Both **equal division of the octave (EDO)** and **Just Intonation (JI)** (7-limit) are supported. In JI mode, tuning is aligned to match the selected tonic from the EDO system, allowing the harmonic center to remain consistent while switching tuning systems.

A **global tuning** control applies a continuous offset to all frequencies. A **queueing mechanism** allows scale, mode, and key changes to be deferred and applied only when the toggle is released, enabling coordinated tuning transitions during performance.

## Bouncing Balls

Each tone in Bonkolo represents a virtual “ball” whose articulation is shaped by rhythmic and spatial modulation. The bounce behavior is modeled using a **band-limited square wave** with adjustable **duty cycle**:

- **Short duty cycles** produce sharp, percussive transients—similar to impact sounds.
- **Intermediate values** create layered, polyrhythmic articulations.
- A **100% duty cycle** removes the bounce effect entirely.

As each note progresses, its modulation waveform undergoes **controlled amplitude decay and increasing frequency**, simulating the compression of time between bounces as energy dissipates. A user-defined **gravity field** shapes this decay rate and bounce compression globally.

Additional **spatial mappings** affect per-note parameters including:

- **Sustain/Damping**,
- **Jump height** (velocity-dependent),
- **Stereo pan**.

These mappings interact to produce a **dynamic, spatially structured, multi-voiced percussive texture** that evolves continuously during performance.

## Articulation and Layering

Bonkolo provides independent expressive control over the **SOLO** and **CHORD** layers. Each line has its own articulation logic, allowing differentiated rhythmic phrasing and texture.

- **Trigger probability** (**DICE**) determines the likelihood that a given step will activate. Higher values create denser textures; lower values introduce sparsity and unpredictability. SOLO and CHORD layers are triggered independently.
- **Octave** (**OCT**) shifts each layer to a distinct register, useful for foreground/background separation.
- **Amplitude modulation** (**AM**) shapes dynamic variation in loudness, with a global rate control (**AMF**).
- **Sustain duration** (**SUS**) defines note length per layer. A global **SUS** parameter adjusts overall envelope length.
- **Pitch modulation** (**µ-MOD**) introduces fine-grained pitch variation, with global speed control via **µ-SPD**.
- **Duty cycle** (**π**) controls the bouncing ball effect. Low values produce sharp, percussive hits; intermediate values yield layered, polyrhythmic motion; a value of 1 removes the effect entirely. The global **π-RND** control introduces random variation to this articulation.
- **Envelope softness** (**SOFT**) adjusts the attack curve—from sharp to gradual.
- **Gravity** (**GRAV**) defines bounce decay across all tones.

## Chord-Specific Behavior

The CHORD line includes additional controls that shape harmonic structure, voicing, and temporal spread:

- **Dedicated volume** control adjusts the overall loudness of the CHORD layer independently from SOLO.
- **Inversion probability** determines how often chord voicings are rearranged internally, introducing harmonic variation.
- **Chord density** sets the number of voices per chord (ranging from 2 to 5), allowing control over harmonic thickness.
- **Doubling** (**`-|+`**) adds a second chord transposed either down or up. The farther the control moves from center, the higher the probability that the extra voicing will be triggered.
- **SCATTER** offsets the timing of individual chord notes. Leftward values introduce random staggering; rightward values produce regular, arpeggio-like spreads.

## Synthesis and Timbre Architecture

Bonkolo’s sound engine is based on a flexible **FM architecture** with two modulators and adjustable cross-feedback. Each note’s timbre evolves dynamically through modulation shaping, supporting a wide range of textures from pure to complex.

An additional **overtone control** blends a clean sine-like waveform with a brighter, harmonically rich signal (**BUZZ**), allowing control over spectral brightness.

A probabilistic **noise subsystem** (**NOISOLO**) adds colored bursts shaped by stochastic spline-based waveforms. These waveforms allow for gradual adjustment of randomness, supporting textures that range from pitchless noise to **pitched, noise-inflected gestures**. Formant shaping introduces breathy, explosive, or rumbling character.

## Synthesis and Timbre Architecture

Bonkolo’s sound engine is based on a flexible **FM architecture** with two modulators and adjustable cross-feedback. Each note’s timbre evolves dynamically through modulation shaping, supporting a wide range of textures from pure to complex.

An additional **overtone control** blends a clean sine-like waveform with a brighter, harmonically rich signal (**BUZZ**), allowing control over spectral brightness.

A probabilistic **noise subsystem** (**NOISOLO**) adds colored bursts shaped by stochastic spline-based waveforms, with formant shaping that produces breathy, explosive, or rumbling transients.

## Global Modulation Controls

Bonkolo features a set of global modulation controls that shape expressive behavior across both SOLO and CHORD layers:

- **AM** / **AMF** – amplitude modulation depth and frequency
- **µ-SPD**, **µ-RND** – speed and randomness of pitch modulation
- **π-RND** – random variation of the bounce duty cycle (**π**)
- **GRAV** – global gravity strength controlling bounce decay and frequency
- **PHYS**, **REL**, **SYNC** – global timing modifiers: disable gravity, scale behavior relative to the metro, or synchronize to metro rate

The **bounce effect** can be further shaped using spatial modulation curves for **SUSTAIN**, **GRAVITY**, **JUMP**, and **PAN**, each controlled by three parameters:

- **BVAR** sets the curve’s shape or bias
- **SHIFT** offsets the position along the modulation axis
- **AMP** controls the overall depth or influence

These curves determine how each virtual “ball” behaves over time and space, modulating:

- **SUSTAIN** – per-note envelope length
- **JUMP** – perceived bounce height or energy
- **PAN** – stereo position of each note
- **GRAVITY** – decay rate and bounce compression

## Presets as a Compositional Tool

Bonkolo features an eight-slot **preset system** designed not just for parameter recall, but as a tool for **real-time compositional control**. Each preset captures the full state of the instrument, including tuning, modulation, articulation, sequencing, and all expressive parameters.

**Presets can be saved, recalled, and overwritten on the fly**, allowing performers to build and navigate musical structures dynamically, without interrupting play. Presets can be switched instantly via MIDI, enabling smooth transitions between contrasting musical contexts—such as sparse vs. dense textures, harmonic centers, or articulation styles.

A complete set of eight presets can be written to file, preserving a customized collection of performance states. These sets can be reused across sessions or performances, functioning as **signature vocabularies** that support open-ended variation and structural reuse.

[← Back to Instrument Index](../index.md)
