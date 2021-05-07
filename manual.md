# FamiTracker to Capcom 2 Converter

ft2c2 is a tool for hackers of games using the Capcom 2 sound engine. It allows composers to compose their music in the widely-used FamiTracker software, then converts the music automatically to the Capcom 2 format with minimal differences.

It is critical to note, however, that it cannot convert arbitrary FamiTracker modules. This is because key to the conversion process is supporting only a limited subset of the features FamiTracker supports - only those which have a corresponding feature in the Capcom 2 engine. Additionally, there are a number of features the converter supports which should only be used sparingly, as using them ubiquitously will produce a Capcom 2 track which is impractically large.

# The Capcom 2 Engine
A few forewords on the engine are necessary to understand the features and limitations of the converter.

Unlike FamiTracker, in Capcom 2 time durations are expressed as in musical notation: divisive note times that can be added together using tie. Things such as volume and envelope can change in between tied notes, but there is a limit. Only 8 notes may be tied together, which must include all time extensions and changes to volume or envelope.

Second, the first frame of every non-tied note in Capcom 2 is silence. This allows two identical consecutive notes to not blend together and removes the need to have explicit note cuts/releases when note creating rests. Additionally, it produces an audible pop at the start of each note that gives the illusion having an attack and decay envelope, which greatly reduces the need for explicit attack in envelopes.

# Supported Features and Limitations
Tracker:
- Notes A0 to B7
- Tie limit of 8 tied notes
- Note cut and note release
- Implicit 1-frame gap between notes
- Volume control
- Instruments
- Instrument changes are possible in the middle of notes, but invoke tie

Effects:
- Pitch slide (1xx and 2xx)
- Jump to frame (Bxx)
- Stop (Cxx)
- Skip frame (Dxx)
- Set speed (but not tempo) with (Fxx)

Instruments:
- Mandatory 1-frame silence at the start of volume envelopes. Note that this is NOT necessary for tie envelopes.
- Delayed onset vibrato possible through tie envelopes
- ASR envelopes with sustain level at 15, D envelopes
- Vibrato or pitch slide via pitch envelope
- Tremolo is not supported
- Single-value duty cycle and noise mode envelopes

Module Properties:
- Single song per file
- No expansion sound chips
- Linear period pitch mode

Song Settings:
- Speed and frame size settings
- Fixed tempo of 150

## Feature Recommendations
One of the key principles of composing for Capcom 2 games is that just because you can do something doesn't mean you should. Every tracker, effect, and instrument feature consumes space, and while FamiTracker modules can be many kilobytes in size, Capcom 2 tracks are best made under 1 KB. To put this in perspective, a single simple note is 1 byte, but a single note with attack and release (at note release) consumes a full 10 bytes (1% of your budget). Space can go very quickly.

- To the greatest extent possible note and rest durations should be powers of two rows (1, 2, 4, etc.) or dotted versions of the same (3, 6, 12, etc.).
- Volume should be specified for every note but volume changes should be used sparingly as they force note splitting and tie which eats up bytes and note tie slots.
- Instrument should be specified for every note
- Speed changes should be used sparingly
- Simple constant volume envelopes (plus the mandatory 1-frame gap) should be overwhelmingly common. D envelopes are fine for percussion if many consecutive notes use the same envelope, but ASR envelopes should be used only when a long fade out/in is absolutely necessary, as they eat space fast. Additionally, the one frame gap between notes produces the audial illusion of attack and decay, greatly reducing the need for an explicit envelope.