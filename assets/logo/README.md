Simple generated logo mark for Nesttuntorget Fysioterapi (not the clinic's original/official logo — none could be found; see `agent/PLAN.md` status log). Colors match the site's palette exactly (`css/style.css` `:root` variables), hardcoded here since standalone SVG files can't read the page's CSS variables.

- `icon.svg` — mark only (rounded plus in a circle, partial accent ring). Used as the site favicon and in the header next to the text logo.
- `logo-horizontal.svg` — icon + wordmark lockup, for use outside the site (e.g. print, social profiles). Text uses a generic bold sans-serif fallback rather than the site's Plus Jakarta Sans/Inter, since that font isn't guaranteed to be available wherever this file gets opened — swap the `font-family` if you edit it somewhere the font is installed.

Replace either file with the real logo (once available) — same filenames, or update the `src`/`href` references in the HTML files.
