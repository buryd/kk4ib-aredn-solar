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
