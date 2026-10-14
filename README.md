# DEATH NOTE // Forensic Console — Case DN-03

A single-file, offline escape-room puzzle. Open `DEATHNOTE.html` in any modern
browser — no server, no build step, no network, no dependencies.

## The flow

1. **Select Recovery Route** — the player picks one of two damaged copies of an
   investigation program. The routes differ in length; the handicap attached to
   each is deliberately not revealed until the player commits.
2. **Restore the Investigation Source** — the chosen C program is shown in a
   read-only listing on the right. The editor on the left starts empty and the
   player must retype the program by hand. Copy, paste, selection and drag-drop
   are all disabled.
3. **Evidence recovery** — a staged "archive restoration", then a breach warning.
4. **Recovered System Logs** — 90 log records. The player reads the packets from
   the rows that are both `STATUS 200` and on the `RECOVERY` channel, in
   timestamp order, to recover a location code.

## The two routes

| Route | Program | Handicap |
|---|---|---|
| Long Code | 42 lines of C | Typing faster than 35 WPM makes the text decay, erasing a word at a time until the player slows down. |
| Short Code | 27 lines of C | The keyboard is remapped on a 5s-stable / 5s-scrambled cycle. During a scramble window each letter produces a different letter; the live mapping is displayed above the editor. |

## Validation

The submitted source is compared to the reference as a **stream of C tokens**,
not as text, so indentation, blank lines and spacing around operators and braces
are irrelevant. Spelling, symbols, values and string contents must match exactly.
Errors report the line number of the reference listing plus the expected and
found token.

Both embedded programs compile cleanly under:

```
gcc -std=c11 -Wall -Wextra -pedantic
```

## Tuning

All the knobs sit together near the top of the `<script>` block:

- `WPM_LIMIT` — speed ceiling for the long route (default 35)
- `ERASE_EVERY` — ms between erased words while over the limit (default 420)
- `STABLE_MS` / `SCRAMBLE_MS` — keyboard cycle for the short route (5000 / 5000)

## Notes for running an event

- Use real laptops. The keyboard scramble hooks `keydown`, which phone and
  tablet virtual keyboards report unreliably.
- A window of at least 900px wide keeps the editor and listing side by side.
- The log data is generated from a fixed seed, so every machine produces the
  same records and the same answer.
- Protection is client-side only; view-source reveals the program and the code.
