# Screenshots

Project screenshots live in this folder. The homepage loads the `_thumb` version of each image;
the full-size original is kept alongside it for reference.

| Project    | Original (full size)  | Used on the page                 |
|------------|-----------------------|----------------------------------|
| Lemmix VR  | `lemmix_vr.png`       | `lemmix_vr_thumb.png` (1280 px)  |

To regenerate a thumbnail from an original (1280 px wide, 256-color palette, well suited to pixel art):

```sh
python3 -c "
from PIL import Image
im = Image.open('lemmix_vr.png').convert('RGB')
im = im.resize((1280, round(im.height * 1280 / im.width)), Image.LANCZOS)
im.quantize(256, dither=Image.Dither.NONE).save('lemmix_vr_thumb.png', optimize=True)
"
```

If the thumbnail file is missing, the tile shows a "Screenshot coming soon" placeholder instead.
