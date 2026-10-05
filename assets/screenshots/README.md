# Screenshots

Project screenshots live in this folder. The homepage loads the `_thumb` version of each image;
the full-size original is kept alongside it for reference.

| Project    | Original (full size)  | Used on the page                 |
|------------|-----------------------|----------------------------------|
| Lemmix VR  | `lemmix_vr.png`       | `lemmix_vr_thumb.png` (1280 px)  |
| Lemmix VR (icon) | `lemmix_vr_logo.jpeg` | `lemmix_vr_logo.png` (256 px, transparent) |
| LateralAI  | `lateralAI_screenshot.png` | `lateralai_thumb.png` (1280x720) |
| LateralAI (icon) | `lateralAI_logo.png`  | `lateralai_logo_thumb.png` (256 px) |
| Stream Frame | `streamframe_screenshot.png` | `streamframe_thumb.png` (1280x720) |
| Stream Frame (icon) | `streamframe_logo.jpeg` | `streamframe_logo_thumb.png` (256 px) |
| VPinball VR | `vpinball_vr_hero.png` + `vpinball_vr_logo_text.png` | `vpinball_vr_thumb.png` (1280x720) |
| VPinball VR (icon) | `vpinball_vr_logo.png` | `vpinball_vr_logo_thumb.png` (256 px, transparent) |

The Lemmix VR title icon is the app logo with its white background removed (flood-filled from the
edges) and cropped to the icon, so it sits cleanly on the dark tile. The LateralAI logo is already a
rounded app icon with transparent corners, so it is only resized. The Stream Frame icon is a square
crop of the headset from the full logo (leaving out the wordmark), on its own dark background; the
tile's rounded corners frame it. The Visual Pinball logo already has a transparent background, so it is
trimmed to the ball, centred on a square canvas and resized.

To regenerate a thumbnail from an original (1280 px wide, 256-color palette, well suited to pixel art):

```sh
python3 -c "
from PIL import Image
im = Image.open('lemmix_vr.png').convert('RGB')
im = im.resize((1280, round(im.height * 1280 / im.width)), Image.LANCZOS)
im.quantize(256, dither=Image.Dither.NONE).save('lemmix_vr_thumb.png', optimize=True)
"
```

For a UI screenshot, keep the full color range instead and crop to an exact 16:9 frame before
resizing, so nothing is cut off by the tile's `object-fit: cover`:

```sh
python3 -c "
from PIL import Image
im = Image.open('lateralAI_screenshot.png').convert('RGB')
w, h = im.size
tw = round(h * 16 / 9)
left = (w - tw) // 2
im.crop((left, 0, left + tw, h)).resize((1280, 720), Image.LANCZOS).save('lateralai_thumb.png', optimize=True)
"
```

The Stream Frame screenshot is wider than 16:9, so instead of cropping off the outer service tiles
it is padded to 16:9: the top edge row is stretched upward and the bottom is filled with the page's
background colour.

```sh
python3 -c "
from PIL import Image
im = Image.open('streamframe_screenshot.png').convert('RGB')
w, h = im.size
th = round(w * 9 / 16)
top = (th - h) // 2
out = Image.new('RGB', (w, th), im.getpixel((w // 2, h - 1)))
out.paste(im.crop((0, 0, w, 1)).resize((w, top)), (0, 0))
out.paste(im, (0, top))
out.resize((1280, 720), Image.LANCZOS).save('streamframe_thumb.png', optimize=True)
"
```

The VPinball VR image is not a screenshot: it is built from the Steam library art that the
Steam Frame install package uses (`Doc/steam-frame-build/library-art/` in the fork), the same way
the Steam library shows it. The left 16:9 part of the hero background (without the ball) gets the
"Visual Pinball / Steam Frame Edition" logo centred on top:

```sh
python3 -c "
from PIL import Image
hero = Image.open('vpinball_vr_hero.png').convert('RGB')
logo = Image.open('vpinball_vr_logo_text.png').convert('RGBA')
h = hero.height
bg = hero.crop((0, 0, round(h * 16 / 9), h)).resize((1280, 720), Image.LANCZOS)
lw = 1000
logo = logo.resize((lw, round(logo.height * lw / logo.width)), Image.LANCZOS)
bg.paste(logo, ((1280 - lw) // 2, (720 - logo.height) // 2), logo)
bg.save('vpinball_vr_thumb.png', optimize=True)
"
```

If the thumbnail file is missing, the tile shows a "Screenshot coming soon" placeholder instead.
