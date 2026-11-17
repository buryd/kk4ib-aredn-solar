# KK4IB AREDN Solar

Mobile wiring and performance notes for a 24 V node built from three parts. The hookup drawing is `KK4IB-AREDN-solar-wiring.jpeg`.

## Parts

- 1 × 24 V 12 Ah AGM battery (Battery Source)
- 1 × 25 W solar panel, model BC-20W-M24, with an AGM charge controller mounted on the back
- 2 × Ubiquiti NanoStations

Set the onboard controller to AGM. A full 24 V AGM at rest reads about 25–26 V. Keep the battery shaded and vented. Face the panel true south, tilted near site latitude, with a clear view of the sun from about 9 to 3.

## Wiring

Red is positive. Black is negative.

1. The controller’s battery leads land on the battery posts.
2. Both NanoStations connect to those same posts, in parallel.
3. Positive is Ethernet pins 4 and 5. Return is pins 7 and 8.
4. Panel negative, battery negative, and both radio returns are one common negative.

## Performance

This node is used for outings. The battery is the power source. The panel is the charger between outings.

The battery stores 24 V × 12 Ah = **288 Wh**. Staying inside a 50% discharge leaves **144 Wh**. A NanoStation’s own low end is about 22 V, around **230 Wh** used.

A linked NanoStation is usually about 6 W. Two radios draw **12 W** from the battery. The datasheet maximum is 8 W each, **16 W** together.

| Outing, both radios at 6 W | Energy from the battery | Share of the 50% window |
|---|---|---|
| 4 hours | 48 Wh | One third |
| 8 hours, no sun counted | 96 Wh | Two thirds |
| 8 hours in winter sun | about 52 Wh | About one third |
| 8 hours in summer sun | about even with the panel | Little net drain |

An 8-hour winter outing still takes about 52 Wh from the battery, because a fixed south-facing panel only puts back about 44 Wh that day (25 W × 2.5 full-sun hours × 0.70 derate). A summer day of similar length collects about 96 Wh (5.5 full-sun hours), which matches an 8-hour run if the radios are on while the sun is up. A night outing gets nothing from the panel until the next day.

At the 8 W datasheet maximum, an 8-hour outing with no sun counted is 128 Wh, still inside the 144 Wh window if the battery started full.

From a full battery, with the radios left on and no sun counted, a 50% discharge is about **12 hours** at 6 W each and about **9 hours** at 8 W each. The radios’ own cutoff, near 22 V, is about **19 hours** at 6 W each.

In full sun the controller delivers about **0.8 A**. With the radios off, refilling a 50% discharge takes about two sunny summer days, or about four winter days.

The radios sit on the battery voltage:

| Battery state | Voltage | NanoStation range is 22–27 V |
|---|---|---|
| Near the radio cutoff | about 22 V | At the bottom of the range |
| About half charged | about 24 V | Inside the range |
| Full, at rest | 25–26 V | Inside the range |
| Float, sun holding a near-full battery | 27.0–27.6 V | At or above the top of the range |
| Absorption | 28–29 V | Above the published maximum |

Float and absorption show up in the middle of a sunny outing when the panel is producing more than the radios use and the battery is still near full. That window is longer in summer.

At freezing, AGM capacity is about 80% of the 25 °C rating, so the runtimes shorten by about a fifth. Sun hours are site averages. The model BC-20W-M24 matches 20 W 24 V maintainer panels (published charge current about 0.56–0.83 A). These figures use the stated 25 W nameplate. A true 20 W panel lowers the sun column by 20%.

Leaving both radios on day and night still misses a daily balance. They draw 12 W × 24 h = **288 Wh**. A winter day puts back about 44 Wh, about 15% of that. A summer day puts back about 96 Wh, about 33%.

## Parts left out

A fuse, a low-voltage disconnect, a buck converter, and separate PoE injector boxes were drawn into an earlier hookup. The person who built the original system asked for them to come back out. One objective of the experiment is simplicity.

- The buck converter adds a net loss of about 1 W, about 8 Wh over an 8-hour outing.
- The charge controller already includes fuse-type protection on the charge leads.
- The low-voltage disconnect adds a very small loss. On a mobile outing the radios are shut off by the operator. Their own low end is about 22 V.
- A boxed PoE injector is unnecessary. The battery leads are spliced onto pins 4 and 5 and pins 7 and 8.

The controller’s protection is on the charge leads. A short in the radio wiring is across the battery posts. The radios also see 27–29 V when the sun is holding a near-full battery in float or absorption.
