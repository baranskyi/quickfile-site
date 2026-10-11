# QuickFile promo site

One-page promo site for [QuickFile](https://github.com/baranskyi/omarchy-quickfile), the
IDE-style file manager sidebar for Omarchy. English only. Pixel-art and terminal styling
in the colors of Omarchy's **Osaka Jade** theme (`/usr/share/omarchy/themes/osaka-jade/colors.toml`).

Every install button leads to the plugin's page on the official Omarchy Plugin Marketplace:
<https://plugins.omarchy.org/plugin.html?id=m0sthatedman.quickfile>.

## Structure

```text
index.html                 page, styles and scripts (no build step, no dependencies)
assets/quickfile-demo.mp4  2:05 SUPER+B smart-search demo, 1280×800, H.264 + AAC
assets/quickfile-loop.mp4  first 34 s of the demo, muted, for the hero window
assets/demo-poster.jpg     poster frame for both videos
assets/og.png              1200×630 social preview
assets/favicon.svg         pixel version of the QuickFile tree icon
vercel.json                static hosting headers
```

Fonts come from Google Fonts (Silkscreen, JetBrains Mono). Pixel icons and the skyline are
drawn by the inline script from ASCII maps and a seeded generator; without JavaScript the
page still reads completely.

## Local preview

Video chapters seek inside the MP4, which needs a server with HTTP Range support
(`python3 -m http.server` has none, so seeking jumps back to the start):

```bash
npx --yes serve -l 8478 .
```

## Demo video source

Rendered by the Remotion project `Projects/remotion-video` (`out/quickfile-smart-search.mp4`).
The web copies were re-encoded with ffmpeg; the AI-credit balance widget in the recorded
Omarchy bar is blurred (64×26 px at x 1626, y 1 of the 1920×1200 source):

```bash
BLUR="[0:v]split[a][b];[b]crop=64:26:1626:1,boxblur=6:2[bl];[a][bl]overlay=1626:1"
ffmpeg -i quickfile-smart-search.mp4 -filter_complex "$BLUR,scale=1280:800:flags=lanczos,format=yuv420p[v]" \
  -map "[v]" -map 0:a -c:v libx264 -profile:v high -preset slow -crf 25 -c:a aac -b:a 96k \
  -movflags +faststart assets/quickfile-demo.mp4
```

## Deployment

Static site, deployable as-is to Vercel (project name `omarchy-quickfile`, giving
`https://omarchy-quickfile.vercel.app/`). The canonical URL, Open Graph URLs and JSON-LD
`@id`s in `index.html` use that address; change them together if the site moves to
another domain.

## Author

Slava Baranskyi — [LinkedIn](https://www.linkedin.com/in/baranskiy/) ·
[Restless Brain](https://www.restless-brain.com/about) · [GitHub](https://github.com/baranskyi).
The author block and Person JSON-LD follow the approved public bio in
`Docs/About Me/slava-baranskyi-search-profile.md` (same Person `@id` as easygym.me).
