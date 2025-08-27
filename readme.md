# image2ascii

A tiny CLI that renders images as **true-color ASCII art** using the Unicode half block `▀`.  
Supports PNG/JPG out of the box, and **SVG** via optional CairoSVG.

https://github.com/vroby65/image2ascii

---

## Features

- True-color output (24-bit ANSI) using `▀` (top = FG, bottom = BG)
- Keeps aspect ratio; height is ~½ of the pixel height
- Simple width control with `-w`
- Reads **SVG** when CairoSVG is installed
- Prints to **stdout** (pipe/redirect as you like)

---

## Requirements

- Python **3.8+**
- [Pillow](https://pillow.readthedocs.io/) (`pip install pillow`)
- **Optional (for .svg):** [CairoSVG](https://cairosvg.org/) (`pip install cairosvg`)

> Terminal must support **24-bit color** (most modern terminals do: Windows Terminal, iTerm2, GNOME Terminal, Alacritty, etc.).

---

## Install

```bash
# Clone
git clone https://github.com/vroby65/image2ascii.git
cd image2ascii

# Install deps
python3 -m pip install --upgrade pillow
# Optional for SVG input:
python3 -m pip install --upgrade cairosvg

# Make it executable (Linux/macOS)
chmod +x image2ascii
```

On Windows:

```powershell
py -3 -m pip install --upgrade pillow cairosvg   # cairosvg optional
# Run with: 
py image2ascii <args>
```

---

## Usage

```bash
image2ascii [-w WIDTH] imagefile
```

* `-w WIDTH` → output width **in characters** (default: `40`)
* `imagefile` → PNG/JPG by default, **SVG** if CairoSVG is installed

Examples:

```bash
# Basic
./image2ascii cat.jpg

# Wider output
./image2ascii -w 120 photo.png

# SVG input (requires cairosvg)
./image2ascii -w 160 logo.svg
```

---

## Tips

* Save the colored output to a file and view it preserving ANSI codes:
  
  ```bash
  ./image2ascii -w 100 pic.jpg > out.ans
  less -R out.ans
  ```

* Strip ANSI color codes if you want plain text:
  
  ```bash
  ./image2ascii -w 80 img.png | sed -r 's/\x1b\[[0-9;]*m//g' > plain.txt
  ```

---

## How it works (short)

The image is resized, then every **2×2** pixel block becomes one `▀`:

* Top average color → **foreground**
* Bottom average color → **background**

This halves the text height while keeping more vertical detail.

---

## Known notes

* Some SVGs with external resources may need additional Cairo dependencies.
* When redirecting to a file, ANSI escape codes are included by design.

---

## License

MIT (or your preferred license)
