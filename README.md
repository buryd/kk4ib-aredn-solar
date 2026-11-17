# KK4IB AREDN Solar

Wiring and performance notes for a 24 V solar node built from the parts below. The hookup drawing is `KK4IB-AREDN-solar-wiring.jpeg`.

## Parts requested

- 1 × 24 V 12 Ah AGM battery (Battery Source)
- 1 × 25 W solar panel, model BC-20W-M24, with an AGM charge controller mounted on the back
- 2 × Ubiquiti NanoStations

## Parts added

The panel controller only charges. The battery voltage rises above the NanoStation limit while charging, and each radio takes power over Ethernet. Four parts were added so the original three can run the radios.

| Added part | Role |
|---|---|
| 5 A fuse on the battery positive | A 12 Ah AGM can dump a large current into a short. The controller and both radios sit on the far side of the fuse, so a fault in either lead opens the fuse. |
| Low-voltage disconnect, load positive only | Nothing in the original list stops the radios from running the battery flat. It opens at 22.4 V and closes at 25.0 V. The panel stays connected, so charging continues after the radios drop. |
| Buck converter, output 24.0 V, 2 A or more, input up to 32 V | A NanoStation M is specified for 22–27 V. AGM absorption on this bank is about 28–29 V, and float is about 27.0–27.6 V. The buck holds 24.0 V. Set it with a meter before the radios are plugged in. |
| Two passive PoE injectors | Each NanoStation is powered over Ethernet: pins 4 and 5 positive, pins 7 and 8 return. One injector per radio, fed from the buck output. |

Set the onboard controller to AGM. A full 24 V AGM at rest reads about 25–26 V. Keep the battery shaded and vented. Face the panel true south, tilted near site latitude, with a clear view of the sun from about 9 to 3.

## Wiring

Red is positive. Black is negative.

1. Panel controller battery leads land on the fused bus, positive through the 5 A fuse at the battery post.
2. The load positive leaves that same bus, through the low-voltage disconnect, into the buck converter.
3. The buck’s 24.0 V output splits to the two passive PoE injectors.
4. Each injector feeds one NanoStation.
5. Panel negative, battery negative, buck return, and both PoE returns share one common negative. The disconnect does not switch that wire.

## Performance

The battery stores 24 V × 12 Ah = **288 Wh**. The working window for AGM life is half of that, **144 Wh**. The disconnect opens near 22.4 V, around **230 Wh** used.

A linked NanoStation is usually about 6 W. The datasheet maximum is 8 W. After the buck converter (about 92% efficient), two radios at 6 W each pull about **13 W** from the battery, **313 Wh per day**.

The 25 W panel is derated to 0.70 for heat, dust, and a fixed mount.

| Both radios | Sun on a fixed south-facing panel | Energy from the panel | Hours to 50% | Hours to cutoff |
|---|---|---|---|---|
| 6 W each | Northern winter, 2.5 h | 44 Wh | 13 h | 21 h |
| 6 W each | Good summer, 5.5 h | 96 Wh | 16 h | 26 h |
| 4.5 W each | Northern winter, 2.5 h | 44 Wh | 18 h | 29 h |
| 8 W each | Northern winter, 2.5 h | 44 Wh | 9 h | 15 h |

Full sun aimed square-on can put about 21 W into the battery, so a clear midday runs ahead of a 13 W load. Morning, evening, and the night erase that lead. In that sun the charge current is about **0.8 A** (0.07C). That is a healthy float rate for a 12 Ah AGM. With the radios off, a half-discharged battery takes about three or four winter days to refill.

Two radios left on do not reach a daily balance. On a northern winter day the panel replaces about **14%** of the energy they use. On a good summer day it replaces about **31%**.

Holding a typical 12 W radio load through a 2.5-hour winter takes about a **180 W** panel, closer to **250 W** if cloudy days have to be covered, and about an **80 Ah** 24 V AGM for three nights at 50% discharge. This 25 W panel is about 14% of that winter array. This 12 Ah battery is about 15% of that bank. With the radios off, the same panel is the right size to hold the AGM against self-discharge.

At freezing, AGM capacity is about 80% of the 25 °C rating, so the runtimes above shorten by about a fifth. The sun hours are site averages.

The model BC-20W-M24 matches the 20 W 24 V maintainer panels that carry an onboard AGM controller (published charge current about 0.56–0.83 A). The table uses the stated 25 W nameplate. A true 20 W panel lowers the sun column by 20% and leaves the same result.

## Originator’s comments on the added parts

The person who built the original three-part system read the “Parts added” section and replied that one objective of the experiment was simplicity.

- The buck converter adds a net loss.
- The charge controller already includes fuse-type protection.
- The low-voltage disconnect adds a very small loss and an unnecessary complication.
- PoE injectors are not needed.

Those four parts work against that simplicity goal. They also do not change the energy result above: two NanoStations left on still outrun the 25 W panel.

**Buck converter.** The loss is real. At about 92% efficiency, 12 W at the radios pulls about 13 W from the battery, roughly 1 W of continuous loss, about 25 Wh over a full day. A northern winter day collects about 44 Wh, so that loss is more than half of one day’s solar input. Runtime to a 50% discharge grows by about an hour if the buck is left out, from roughly 13 hours to about 14 hours. The radios then sit on the battery voltage. Float while the sun is holding a near-full battery is about 27.0–27.6 V, and absorption is about 28–29 V. The NanoStation M range is 22–27 V. That high-voltage window is midday, and it is longer in summer, whenever the panel is producing more than the radios use and the battery is still near full.

**Fuse.** The controller’s protection is real on the charge leads. A short in the radio wiring is across the battery posts, so that fault current comes from the battery. The controller sits in parallel and does not open that path.

**Low-voltage disconnect.** Its loss is tiny. The radios already pass a 50% discharge in about half a day of continuous running, and a NanoStation’s own low end is about 22 V, next to the 22.4 V setting. The disconnect only drops the load after the AGM is already deeply discharged.

**PoE injectors.** A separate injector box is optional. Each NanoStation still needs 24 V on Ethernet pins 4 and 5, with the return on pins 7 and 8. The AC adapters that came with the radios do that from mains. From the battery, the same connection is a splice onto those pairs.

### What to remove

Remove the low-voltage disconnect and the separate PoE injector boxes. Keep the fuse and the buck converter.

The disconnect does not fix the daily deficit. A boxed injector can be replaced by a splice onto pins 4 and 5 and 7 and 8, or by the AC adapters supplied with the radios when mains power is available.

The fuse has no meaningful running loss and covers a battery-side short the controller does not. The buck is the part that costs energy, about 25 Wh on a day the radios never turn off, and it is what keeps 28–29 V absorption off the radio input. In strong sun the panel can exceed the radio load and hold a near-full AGM at that voltage.

### Mobile use

This node is a mobile system, used for outings, not left running continuously. The battery is the power source. The panel is the charger between outings.

Two radios at 6 W each draw 12 W with no buck converter. A full battery holds 288 Wh. Staying inside a 50% discharge leaves 144 Wh.

| Outing | Energy used | Share of the 50% window |
|---|---|---|
| 4 hours | 48 Wh | One third |
| 8 hours, no sun counted | 96 Wh | Two thirds |
| 8 hours in winter sun | about 52 Wh from the battery | About one third |
| 8 hours in summer sun | about even with the panel | Little net drain |

An 8-hour winter outing still takes about 52 Wh from the battery, because the panel only puts back about 44 Wh that day. A summer day of similar length collects about 96 Wh, which matches an 8-hour run if the radios are on while the sun is up. A night outing gets nothing from the panel until the next day.

With the radios off afterward, about 0.8 A from the panel will maintain the AGM. Refilling a 50% discharge takes about two sunny summer days, or about four winter days.

On a mobile outing the buck’s loss is about 1 W, roughly 8 Wh over 8 hours, against a 144 Wh working window. The voltage exposure is the same: in sun, with a battery still near full, the radios can see 27–29 V during float or absorption.
