# Sandra app-icon pipeline (CraftDeck fork)

One trade dress for every Sandra craft app icon: same doll, same style, different
motif + robe color. That consistency is what makes the set protectable; the motifs
keep them distinguishable at 128 px.

## Spec (from upstream `catalog.rs`)

- `assets/app-icons/{id}.png`, **128x128 PNG**, baked via `include_bytes!`
- Generate big (1024 square), review, downscale to 128 with high-quality bicubic.
- Add one `include_bytes!` arm per id in `crates/core/src/catalog.rs`; the
  `every_app_has_its_icon` test goes green per id.

## Generation (craft-anything, fal.ai Nano Banana Pro, ~$0.15 each)

A Craft Anything **project** must exist (empty folders do not count — the server
gates on its project registry). Create one `craft-icons` project once, then:

```
prompt = "Trademarkable app icon, flat vector-illustration style. Original character:
a small cute Japanese kokeshi doll with {MOTIF}, wearing a {COLOR} robe, on a dark
charcoal background. Centered composition, no text, no letters, no logos, no watermark,
full-bleed, bold simple silhouette readable at tiny icon size, warm minimal shading"
shape  = square  →  Assets/Icons/{id}-doll-1024.png
```

Rules for trademarkability: original AI art only; no third-party marks, no text in
the image, no emoji, no artist names in prompts. Keep the doll + robe system identical
across the set; vary only motif + robe color.

## Review gate (mandatory, per icon)

AI generates, a human approves. Never auto-commit unreviewed art. Pilot one icon,
lock the style, then batch the rest.

## Downscale (PowerShell, high quality)

```powershell
Add-Type -AssemblyName System.Drawing
$src = [System.Drawing.Image]::FromFile('.\{id}-doll-1024.png')
$dst = New-Object System.Drawing.Bitmap(128, 128)
$g = [System.Drawing.Graphics]::FromImage($dst)
$g.InterpolationMode = [System.Drawing.Drawing2D.InterpolationMode]::HighQualityBicubic
$g.DrawImage($src, 0, 0, 128, 128)
$dst.Save('D:\Dev\repos\craft-deck\assets\app-icons\{id}.png', [System.Drawing.Imaging.ImageFormat]::Png)
```

## Motif table (locked)

| app | robe | motif (hat / prop) |
|---|---|---|
| appcraft | mint | doctor's head-mirror |
| bookcraft | amber | open book as hat |
| codercraft | teal | tiny terminal window as hat |
| craft-sandbox-vbox | slate gray | small open box prop |
| databasecraft | deep blue | database cylinder prop |
| djcraft | magenta | chunky headphones |
| gamecraft | violet | d20 die prop |
| lookcraft | sky blue | envelope as hat |
| makercraft | steel gray | brass gear prop |
| modcraft | lime | small cube prop |
| moneycraft | green | gold coin prop |
| qgiscraft | earth green | map pin prop |
| robotcraft | orange-red | antenna headband |
| speechcraft | purple | vintage microphone prop |
| sportscraft | energetic red | whistle prop |
| studiocraft | indigo | paintbrush prop |
| travelcraft | sunny yellow | tiny suitcase prop |
| writercraft | crimson | fountain pen prop |

## Later: Python MCP repos

Same pipeline, second family color note (e.g. warm paper background instead of
charcoal) so the two fleets read as related but distinct. Only for repos worth
keeping — no icons for dupes or runts.
