# fmfix - a Flying Circus flight-model overhaul

fmfix 1.26.0 playtest build · 20 Oct 2026 · by Dusty

fmfix makes the Flying Circus fighters fly closer to how the real aircraft flew, measured against period flight tests. It is a small plugin for GB Mod Loader that adjusts the game's own flight-model code as it runs; it does not replace any plane file. The playtest build is **fmfix 1.26.0**.

- **26 fighters** are covered (all the Flying Circus single-seaters plus the S.E.5a); two-seaters and bombers are untouched for now.
- **AI planes get every change too**, unless a change is marked player-only, so the planes you fight fly by the same rules as yours.
- **Every change can be switched off** in `fmfix.ini`, and fmfix checks the game's code before touching it: if a game update moves something, that one feature switches itself off and the log says why.
- **It is made for single player.** In multiplayer everyone needs the same fmfix files, with Mods ON.

## The philosophy

The aim is historical behaviour for the right reason, not numbers forced to fit. Five rules have guided every change.

1. **Period flight tests come first.** Where a 1917-1919 test of the real aircraft exists, that is the target. Holtzauge's performance models come second: they fill the gaps (no period test ever timed a sustained turn) and are treated as strong directions, not hard targets. The developers' own spec sheet is never a target.
2. **Fix the cause, not the symptom.** A plane that is too slow could be given more thrust, but if the real fault is a wrong wing area or a rich mixture, that is what gets fixed. Several of the biggest changes came from finding a hidden error that earlier tuning had papered over: the Albatros D.II's wing area, the mixture formula, the propeller blades.
3. **Nothing is sacred, and sources are weighed.** Any value can be re-examined, including the turn times. A single 1918 flight with an engine of unknown condition counts for less than repeated tests; climb times that include the take-off are corrected for it.
4. **The game's own data is the reference for what the game does.** Every change was worked out from the game's own flight-model data, so the predictions use the same physics the game uses.
5. **Character matters as much as numbers.** The Camel should still bite, the Dr.I should still climb, and a stall should still be dangerous. Fixes keep what each plane was known for, and only remove behaviour that no pilot of the time described.

## At a glance

Compared with vanilla, most fixed planes are a little faster and climb better, turn a little wider, and stall, dive and run their engines more like the real thing.

| Area | What is different from vanilla | Planes |
| --- | --- | --- |
| Speed, climb and turns | Each plane refitted to period tests: more drag when pulling hard, so sustained turns are wider; speed and climb usually up | 26 fighters |
| Propellers | Honest propeller blades at the real size and pitch, so the engine revs right and the plane no longer holds energy too well in glides and dives | SPAD XIII, Dolphin, D.VII, D.VII F, D.XII (more to come) |
| Mixture | The automatic mixture lever now finds the true best mixture at every height | All 39 Flying Circus planes (AI and auto-mixture players) |
| Engine character | High-compression D.IIIau throttle gate, misfires, BMW knock, Camel power at height, rotary cold oil fixed, plug fouling, rev limiter below the red line | Per engine, see Engines |
| New hangar engines | Pfalz D.XII flies its common Mercedes by default (BMW and D.IIIau as hangar options); Camel Le Rhone 9Jb option; D.IIIau for the D.VII and Pfalz D.IIIa | D.XII, Camel, D.VII, Pfalz D.IIIa, D.Va |
| Stalls | Stall breaks reshaped from wind-tunnel data: the D.VII now really stalls; the Camel, Dolphin, Pup and N28 mush instead of falling away | 6 planes |
| Control forces | The game's 'dead stick at speed' replaced by a pilot-strength limit; full back stick can reach the stall on 11 more planes | All 26 fighters |
| Handling and bug fixes | Fin and rudder sizes, rudder and tail sign bugs, Pfalz and Halberstadt control bugs, stronger rotary gyro (halfway to the real engines' spinning mass), negative-g engine cut | Per plane, see Handling |
| Structure | Wings now fail the way the real ones did: Albatros lower wings in long fast dives, Nieuport lower wings at very high speed, Triplane wings in very hard pulls; a D.Va that loses a lower wing keeps its top wing | D.Va, D.III, N11, N17, Triplane, D.VII |

## Performance: speed, climb and turns

The biggest change is the one the community argued about most: in vanilla, many Flying Circus planes turn tighter than any period evidence supports, while flying slower and climbing worse than their period tests. fmfix refits each plane so that its top speeds, climbs and turns sit on its targets together.

**How a refit works, in plain words.** A wing has two kinds of drag: a fixed part, and a part that grows as the wing lifts harder. In vanilla the second part was too small on most planes, which makes hard sustained turns too cheap. fmfix adds it back, then trims the fixed drag and the propeller's push so that speed and climb land on the period figures. Each plane has its own numbers, worked out from the game's own flight-model data.

**What you will feel.** Most planes keep more speed in a straight line and climb a little better. Hard sustained turns bleed more energy, so a long flat turning fight is wider and slower than in vanilla. Instant turns, rolls and the first seconds of a break are much less affected. Measured in game so far (recorder, before the newest builds): the D.Va's speed and turn, the Pfalz D.IIIa's top speed, and the Camel, D.VII, D.VIII, S.E.5a and Schuckert all match the bench prediction; the D.Va at 5 km and the Dr.I at 2.9 km are within 1 %.

**Plane-specific refits worth knowing about:**

| Plane | What changed | Why |
| --- | --- | --- |
| Albatros D.II | Wing area raised from 22.9 to the real 24.5 m2; much better climb high up | The game used the D.I's wing by mistake |
| Pfalz D.IIIa | Flies on its real wing section; ~184 km/h at sea level | The game's section did not match the Pfalz's ribs |
| SPAD XIII | Fitted as the 220 hp aircraft it carries, to the September 1917 test | The engine in the game is the 220 hp 8Be |
| Fokker Dr.I | Speeds kept, better climb, a little better turn | Its climb figures are from the 1918 factory description |
| Sopwith Pup, Nieuport 11 | Back on the real 80 hp Le Rhone 9C; less torque, lower rpm, better glide | Vanilla gave them 16-27 % more power than the engine had |
| Siemens-Schuckert D.IV | Engine compression raised to the real 6.0 | The game's engine was 12-17 % weak above 1 km |
| Pfalz D.XII | Flies the Mercedes D.IIIa by default; the BMW is a hangar option | Most production D.XIIs had the Mercedes |
| Fokker D.VII F, D.VIII | Softer lift ramps near the stall | Their sustained turns were far ahead of the period-based models (the D.VIII 'monstrous') |

What is not changed: the two-seaters and bombers, and the planes' weights (except where an engine swap changes the engine).

## Engines and propellers

Engines now behave like their period manuals describe, and the first planes have honest propellers. The common engine stays each plane's default; other historical engines are new hangar options.

**Mixture at height (all 39 Flying Circus planes).** The game's automatic best-mixture helper used the wrong formula for every WWI engine with a mixture lever, so AI planes and auto-mixture players ran rich up high, and the AI's lever had only 4 positions. It now holds the true best mixture at every height. This was the real reason the S.E.5a and SPADs lost too much speed with height. AI planes and auto-mixture players gain up to ~8 % power low and 5-13 % above 4 km. If you set the mixture by hand, nothing changes: leaning is still your job (best is usually near 55 % at ~4.5 km on the Camel).

**Honest propellers.** The game's propeller blades turned anywhere from 51 % to 90 % of the engine's power into thrust; real WWI propellers managed about 78-80 %. The fits had hidden this by making airframes too clean or too draggy, which shows when the engine is not pulling: in glides, dives and zooms. Rebuilt so far, each on its real blade, size and pitch, with the airframe refitted:

| Plane | Propeller now | What you notice |
| --- | --- | --- |
| SPAD XIII | Its own blade, the excess drag removed | Revs ~2,150-2,170 at top speed instead of ~2,090; loses energy a little faster power-off |
| Sopwith Dolphin | Its own blade, the excess drag removed; refitted to the clean two-Vickers plane | Turns ~1 s quicker at 1 km; same take-off pull |
| Fokker D.VII | The tested aircraft's Wolf propeller, 2.80 m | Engine rpm matches the June 1918 test at every height; no longer ~32 % too clean |
| Fokker D.VII F, Pfalz D.XII (BMW) | Honest blade at the period pitch | Ground run-up ~1,340 rpm as in the 1918 BMW manual |
| Pfalz D.XII (Mercedes) | The D.VII's Wolf propeller | One wing refit shared by both engines |

**Engine character, per engine:**

- **Mercedes D.IIIau (high compression; D.Va, D.VII, Pfalz D.IIIa and D.XII options).** The throttle has a ground mark at 92.5 %, as documented in 1919. Low down, opening past the mark leans the engine and it can cut out; above ~2-3 km full throttle is usable. It runs rough below ~1,400 rpm. The AI holds the mark and never leans.
- **Misfires (player only).** Inline engines can misfire when lean, knocking, or (D.IIIau) when lugged below 1,400 rpm.
- **BMW IIIa (D.VII F, D.XII BMW).** The D.XII's altitude throttle follows the 1918 manual (closed below ~2 km, fully open from ~4 km). Opening it fully near the ground makes it knock and misfire after a few minutes. A small trim takes off the excess power above ~4 km that a 1921 NACA altitude test shows.
- **Camel at height.** The Camel's Clerget had ten times the high-altitude friction of the identical engine in the Triplane, so it lost too much power with height. Its friction now thins out with the air like every other rotary (value provisional until the mixture test flight).
- **Rotary 'overcooling' (13 rotaries, AI too).** In vanilla, cold oil after a dive or long glide made a rotary stiffen and take damage at far lower rpm (the Camel at ~1,165 instead of 1,550). Real rotaries used total-loss oil, so that no longer happens. Instead, the period problem is modelled (player only): a long glide blipping with the fuel on fouls the plugs, and the engine stutters for 10-30 s when opened up. Gliding with the fuel off avoids it, as pilots did.
- **Rev limiter below the red line (DH.2, E.III, Strutter, Triplane).** The automatic engine control cut the ignition above the damage rpm, so AI engines over-revved into damage in dives. The cut now sits just below the red line. Players without the engine assist are not affected.
- **Negative g (inline engines).** A push to negative g starves the carburettor and the engine dies within about a second and picks up again about half a second after; a rotary rides out a short push.

**New hangar engine options** (they need the JSGME folder `TBS - Engine Mods V1.25.0` enabled):

| Plane | Default | Hangar options |
| --- | --- | --- |
| Pfalz D.XII | Mercedes D.IIIa (the common production engine) | Slot 10 BMW IIIa (vanilla's engine), slot 11 Mercedes D.IIIau |
| Sopwith Camel | Clerget 9B | Slot 5 Le Rhone 9Jb: slower low, faster above ~3 km, a little less torque and gyro, 27 kg lighter cowl |
| Albatros D.Va, Fokker D.VII, Pfalz D.IIIa | Mercedes D.IIIa | Mercedes D.IIIau |

Without the slot files (or with Mods OFF) the D.XII keeps the vanilla BMW, so it stays compatible with unmodded players' files.

## Handling

The planes keep their character, but stalls, heavy controls at speed and several control bugs now follow period and wind-tunnel evidence.

**Stalls.** Below the stall nothing changes; past it, the lift now falls away the way wind-tunnel tests of these wing sections show.

| Plane | Vanilla | Now |
| --- | --- | --- |
| Fokker D.VII, D.VII F | The wing barely stalled (97 % of peak lift kept) | A real but gentle break: the nose drops or mushes clearly; full back stick in a hard turn gives a mild buffet |
| Sopwith Camel, Dolphin | Lift collapsed 2-3 times faster than any test of their wing section | The stall mushes or drops a wing gently; both still flick and spin readily, and the developed spin is unchanged |
| Sopwith Pup, Nieuport 28 | Harsh break | Softer, matching 'docile' (Pup) and 'straightforward' (N28) in period accounts |
| Pfalz D.IIIa, D.XII | The D.IIIa never really stalled | A clearer break at the stall |

**Controls at speed.** In vanilla each plane's controls fade to almost nothing above a set airspeed (a 'dead stick'): a light touch at speed did nothing, and the D.XII pitched up in dives at ~300 km/h because the push it needed was taken away. fmfix replaces that with a pilot-strength limit on all three axes of all 26 fighters. The control follows the stick exactly until the force needed is more than a pilot could hold (push 27 kg, pull 35 kg, one-hand roll 14 kg, pedals 68 kg), sized from each plane's own control surfaces and 1919-21 NACA flight tests. Balanced surfaces (the Fokkers, the D.XII) are lighter. You will feel it as ailerons that get heavy smoothly from ~220 km/h on the Camel and D.Va, and the D.XII now holds a 45 deg dive.

**Elevator reach.** On 17 planes full back stick now reaches the stall in a hard turn; before, the elevator ran out first (the forum's 'Albatros and Pfalz can't cross the line'). The catch: a little more elevator at 1 g too, so slow-flight stalls come a little easier on those planes.

**Bugs fixed:**

- Pfalz D.IIIa: the pitch stick also moved part of the right aileron, rolling the plane on every pull.
- Halberstadt D.II: the all-moving tailplane got weaker past half stick; now the nose keeps coming up as you pull further.
- Siemens-Schuckert D.IV: the rudder worked against itself, so full pedal was barely stronger than half.
- Nieuport 11 / 17: the tail's speed fade had the wrong sign.
- Fin and rudder sizes: the Albatros, Pfalz and E.III fins worked about a third too hard for their shape (the nose swung back too quickly after a rudder kick); the Snipe gets its larger production tail; the N17's fin and rudder were only half as effective as the N11's and are brought up to it.

**Rotary gyro (Camel, Triplane, N17 and the other rotaries).** The real engines' spinning mass was larger than the game's, so the gyroscopic swing is now stronger: halfway between vanilla and the full estimate, after the full size made the Camel 'nearly unflyable'. It may still be reverted after the playtest.

## Structure

In vanilla the wings of these planes almost never failed the way the real ones did. fmfix adjusts a few joint strengths so the historical failure modes come back, without making normal hard flying any more dangerous.

| Plane | Now | History behind it |
| --- | --- | --- |
| Albatros D.Va, D.III | A long fast dive (from ~260 km/h IAS) can twist off a lower wing; hard pulls are unchanged | The V-strut lower wing's spar failures of 1917 |
| Nieuport 11, 17 | The single-spar lower wing can twist off at very high speed (~305 km/h IAS and up) | The sesquiplane's known lower-wing weakness |
| Sopwith Triplane | Wings break in very hard pulls (~5-6 g) | Reported sand-load test figures of 4.7-5.5 g (source still being confirmed) |
| Fokker D.VII | A dive limit near ~390 km/h IAS | Before, it had none at all |

**Survivable failure (D.Va).** When an fmfix-weakened lower wing breaks, the top wing on that side now stays on, as in the real Albatros failures; you may be able to nurse it home. Any other damage (bullets) breaks wings exactly as vanilla does.

## Installing and playing

1. fmfix runs inside **GB Mod Loader**: `fmfix.dll` and `fmfix.ini` go into the game's `bin\game\gbml\` folder. Install with the game closed.
2. For the hangar engine options, enable **`TBS - Engine Mods V1.25.0`** in JSGME and play with **Mods ON**. Without them, every plane still gets its fixes; the D.XII then simply keeps the vanilla BMW.
3. Start the game once and look at `bin\game\gbml_log.txt`: it should say **fmfix 1.26.0**, "all ... checks passed", and nothing "NOT installed". A line that says a feature is off names the reason; send that log with any report.
4. **Multiplayer:** fmfix changes the flight model, so every player on a server needs the same `fmfix.dll` and `fmfix.ini`, with Mods ON. Mods ON keeps you off the normal public servers.
5. **Turning things off:** every feature has its own switch in `fmfix.ini` (each plane's section, or the feature's own section). Restart the game after a change.

## Still uncertain, and what feedback helps

This is a playtest build: the changes in builds 1.17-1.26 have only been predicted from the game's own flight-model data, not yet flown. Earlier builds have been flown and checked against recordings.

**Known uncertainties:**

- The control-force sizes come from 1919-21 trainer and DH-4 tests; British WWI fighter stick-force reports are still to be read and may adjust them. A few planes' control-surface sizes were set by hand.
- The Camel's power at height is provisional until a mixture test flight.
- The stronger rotary gyro may be too much; it may be reverted.
- Only some planes have honest propellers so far. The rest (Camel, S.E.5a, SPAD VII, Snipe, Pup, Nieuports, DH.2, D.VIII, Triplane and others) still use the game's blades, with fits that compensate.
- The Pfalz D.IIIa and Albatros D.III now run out of forward stick a little earlier in steep dives (~264 / ~278 km/h). For the D.IIIa this may be historical (a designed nose-rise in dives); it is adjustable.
- Two-seaters and bombers are untouched.

**Feedback that helps most** (Mods ON, the game's flight recorder on if you can):

- Does a plane feel wrong for its type? Say which plane, what you did and at what speed and height.
- Stalls and spins: was there warning, did a wing drop, how much height to recover?
- Heavy controls at speed: too heavy, too light, or about right for a WWI fighter?
- Anything that feels too easy: a manoeuvre no WWI pilot could have flown.
- Engine behaviour: misfires, cut-outs, stutters after a glide: when, and how it sounded.
- Always attach `gbml_log.txt` and say which fmfix version it shows.

## Sources

- **Period flight tests:** Adlershof and Martlesham Heath captured-aircraft reports (1917-1918), RAE report B.A.100 (S.E.5 and SPAD VII, 1917), STAé French test reports (SPAD XIII No. 512, Tableau Standard 1919), the June 1918 Fokker D.VII No. 2009 test, NACA TN 147 (the 1918 Dr.I test), McCook Field test P-154 (SPAD XIII, 1921).
- **Engines:** Flight, 20 Feb 1919 (the high-compression Mercedes D.IIIau report), the 1918 BMW IIIa manual, NACA Reports 103, 108 and 135 (altitude-chamber engine tests).
- **Wind-tunnel and flight mechanics:** NACA TR 254 (Fokker D.VII wing), TR 296, 279, 233, 273 (sections past the stall), TR 70 and TR 112 (control forces in flight), NACA Report 124 (Göttingen sections).
- **Books and models:** Holtzauge's performance models and book, Gray & Thetford, Bruce, King, Mason, Davilla & Soltan, Herris.
- **Propellers:** period propeller stamps and drawings as read by collectors (Axial, Wolff, Lang, Air Board AB 8080) and an operator's notes on a flying S.E.5a.
- **The game itself:** every change worked out from the game's own flight-model data.
