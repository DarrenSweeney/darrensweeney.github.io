# Ape Escape PS1 Decomp - Live Dashboard

This is the live website hosted at:
- **GitHub Pages**: https://darrensweeney.github.io/ApeEscapeDecomp/
- **Custom Domain**: https://darrensweeney.net/apescape/

## Features

### Interactive Function Dependency Graph
- **D3.js force-directed layout** — Functions arrange themselves by their call relationships
- **Real-time visualization** — See all 1914 PS1 functions and their dependencies
- **Zoom & Pan** — Use mouse wheel to zoom, click+drag to pan around
- **Smooth animations** — Watch matched functions pulse with green glow

### Color Legend
- 🟢 **Green** = Matched (byte-exact decompilation)
- 🟠 **Orange** = Partial (close match, needs refinement)
- ⚫ **Gray** = Unmatched (still being worked on)

### Hover Inspection (Obsidian-style Spider Web)
Hover over any function node to see:
- Function name & address
- Status (matched/partial/unmatched)
- Size in bytes
- Match percentage
- When it was decompiled
- Number of functions it calls
- Quick links to decomp.me and GitHub

### Live Connections
Connected functions show glowing edges:
- Matched function pairs have **glowing cyan/green connections** that animate
- Partial connections show as dim orange
- Hover highlights all connected nodes

### Controls
- **+ Zoom In** — Magnify the graph
- **- Zoom Out** — Zoom out to see the whole graph
- **Reset View** — Return to original layout
- **Labels** — Toggle function address labels on/off
- **Click any node** — Opens that function on decomp.me

### Auto-Refresh
The dashboard checks for new matches every 3 seconds and updates automatically. Watch the decomposition happen in real-time!

## Data

The dashboard reads from `manifest.json` which contains:
- All 1914 PS1 functions
- Their current status (matched/partial/unmatched)
- Decompilation metadata
- Function dependencies and call relationships

## Keeping the Website Updated

The website is served from the `/docs` folder, while the actual decomposition work happens in `/progress/manifest.json`.

### Automatic Sync (Recommended)

Run the sync watcher to keep the website data fresh:

**Python:**
```bash
python tools/sync_website.py watch
```

**PowerShell (Windows):**
```powershell
.\tools\sync_website.ps1 -watch
```

Or just run once to sync now:
```bash
python tools/sync_website.py
```

### Manual Sync

Copy the latest manifest to the website:
```bash
cp progress/manifest.json docs/manifest.json
```

Then git commit and push:
```bash
git add docs/manifest.json
git commit -m "Update website data"
git push
```

GitHub Pages will rebuild automatically within a few seconds.

## Technical Details

- **Framework**: Vanilla HTML5/CSS3 + D3.js v7
- **Data Format**: JSON manifest
- **Browser Compatibility**: Modern browsers (Chrome, Firefox, Safari, Edge)
- **Responsive**: Works on desktop, tablet, mobile
- **Performance**: Optimized for 1900+ nodes and connections

## Customization

Edit `docs/index.html` to customize:
- Colors (search for hex codes like `#00d4ff`, `#00ff88`)
- Graph physics (CONFIG object at top of script)
- Refresh interval (currently 3 seconds)
- Node size and animation speeds

### Graph Physics Tuning

```javascript
const CONFIG = {
    nodeRadius: 4,           // Node circle size
    linkDistance: 40,        // Target distance between connected nodes
    chargeStrength: -80,     // Repulsion between nodes (-more = more spread)
    collideRadius: 7,        // Collision radius to prevent overlap
};
```

## Tips for Viewing

1. **Zoom into clusters** — Use zoom to focus on specific areas
2. **Hover before clicking** — Get function details first
3. **Watch the animations** — Green pulsing = new match!
4. **Look for patterns** — Orange clusters need work
5. **Use reset button** — Easy way to see the whole graph

## Troubleshooting

**Graph not loading?**
- Check browser console (F12) for errors
- Ensure `manifest.json` is in the same folder
- Try a hard refresh (Ctrl+Shift+R)

**No functions showing?**
- Make sure `manifest.json` exists and has data
- Check that data synced properly from `/progress/`

**Graph moving too slowly?**
- Try zooming in or resetting view
- Reduce total nodes by filtering (advanced)

**Website not updating?**
- Run sync script: `python tools/sync_website.py`
- Commit and push: `git add docs/manifest.json && git commit -m "update" && git push`
- GitHub Pages rebuilds within 30 seconds
