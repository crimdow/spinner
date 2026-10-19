# Spinner (spin.jermins.com) — notes for Claude

A wheel that picks the order of people for a meeting or game. It is one static file, `index.html`. There is no login and no server; everything is saved in the browser.

## Saved settings (localStorage)
- `spinner-v1`: people (who's in), what's done, and timer settings (on/off, minutes each) and sound.
- `spinner-theme`: `light`, `dark` or empty. Empty means Auto, which follows the phone.
- `spinner-wheel`: empty for Classic (six colours) or `gray`. Gray also turns the blue buttons gray.
- A script in `<head>` applies the theme and wheel before first paint.

## Layout and decisions
- **☰ menu:** Timer, Minutes each, Sound. Then **Appearance** (Light / Dark / Auto, a `.seg3` like Home Board's), then **Theme** (Classic / Gray cards with colour dots), then People.
- **Smooth spins:** the wheel is four stacked layers: rim and face, the slices (`#rotorBox`), the hub, and the pointer (`#flapBox`). A spin or drag only changes the CSS `transform` of `#rotorBox` and `#flapBox` (with `will-change`), so the phone rotates finished pictures instead of redrawing every name and peg. Don't go back to setting a `transform` attribute inside the SVG.
- **Upright names:** after a spin or a hand turn, `relevel()` fades names that ended up upside down and flips them, instead of redrawing the wheel.
- **No menu jumps:** the menu sheet is drawn once (`drawSettings`). Taps update only what changed: the ticked row in place, `paintPeople()` for the list and the buttons under it, and the timer's minutes row shown or hidden. Redrawing the whole sheet scrolls it to the top and replays its slide-up.
- **No redraws on switch:** changing theme or wheel updates the buttons in place (`paintLookCtl()`). The wheel colours are CSS variables (`--w0..4`, `--i0..4`), so nothing is redrawn and the menu doesn't jump.
- **Locked page:** it never scrolls in the web app: `html{overflow:hidden}` and `body{position:fixed;inset:0}`.

## Look
- Same as Home Board: light background `#e4e5e8`, dark `#1d1e22`, accent blue `#4f7bd4`, system font, round grey buttons, solid pills, slide-up sheets.
- The icon is a six-colour wheel with a dark pointer.

## Testing
- Serve the folder with any static server and drive it with Playwright on a phone-sized viewport. Seed `spinner-v1` with a list of names.
- Check Light, Dark, Auto, Classic and Gray, and that the menu node isn't re-rendered when they're switched.
