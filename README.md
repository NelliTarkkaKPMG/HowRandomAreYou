# How Random Are You? / Kuinka satunnainen olet?

An interactive tablet demo. The player tries to press **A** and **B** as randomly as they can while a
small predictor guesses each press in advance. The result is compared with a
**pseudo-random** (PRNG) and a **cryptographically secure** (CSPRNG) sequence of the same length.

The point: **looking random is not the same as being unpredictable.**

## How to run

It is one self-contained file: open `index.html` in any modern browser. No build step, no server,
no internet needed. For an exhibition tablet, copy the file to the device or host it as a static page
and add it to the home screen / run it in kiosk mode.

- Length: 50 / 100 / 200 presses
- Languages: Finnish and English (FI/EN toggle)
- Keyboard: ← / → or A / B also work
- Kiosk mode: after 2 minutes without interaction it returns to the start screen and clears everything

## Privacy

The app collects nothing:

- no cookies, `localStorage`, analytics or accounts
- a Content-Security-Policy blocks **all** network connections, so nothing can be sent anywhere
- results live only in memory and are wiped on "Play again", on idle timeout or when the page is closed

## What it shows

| Part | What it does |
|---|---|
| **Predictor** | Several "experts" look at the last 1–6 choices and predict what usually came next. The predictor follows whichever expert is currently doing best. Against truly random input it averages 50%; against people it usually does better. |
| **Statistical tests** | Share of B, share of switches, longest streak, and evenness of three-choice patterns (χ²). The "random range" for each is the middle 95% of 2,000 simulated cryptographically random sequences, computed on the device. |
| **PRNG** | `mulberry32` with a 16-bit seed. It's deterministic but passes the same tests. |
| **CSPRNG** | `crypto.getRandomValues()`. |
| **Seed attack** | The attacker sees the first 32 values and tries all 65,536 seeds. The PRNG is cracked in milliseconds and the rest is predicted 100% correctly. The same attack finds nothing in the CSPRNG sequence. |

The 16-bit seed is deliberately small so the brute force can run live. Real PRNGs such as V8's
`Math.random()` (xorshift128+) have larger state, but that state can still be reconstructed from
observed outputs. That's why they must never be used for passwords, keys or tokens.
