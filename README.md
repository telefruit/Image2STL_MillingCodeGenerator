# Image2STL – G-Code Generator

**Image to STL, Relief- and Engrave Milling G-Code Generator and Visualizer**

> **Source code is proprietary and available under a commercial license.**  
> Contact: [www.mycncmill.de](https://www.mycncmill.de)

Image2STL converts a grayscale image (or any image) into a 3-D height-field STL model and generates CNC milling G-Code for roughing and finishing passes.  
It visualizes the resulting tool path in 3-D, simulates the machining process step-by-step and shows a material-removal preview — all inside one desktop application.

---

## Screenshots

### STL Model View

![STL Model View](docs/screenshots/01_stl_view.png)

---

### Tool Dialog

![Tool Dialog](docs/screenshots/02_tool_dialog.png)

---

### G-Code Visualizer

![G-Code Visualizer](docs/screenshots/03_gcode_visualizer.png)

---

### NC Simulation

![NC Simulation](docs/screenshots/04_simulation.png)

---

### Material Removal Simulation

![Material Removal Simulation](docs/screenshots/05_material_removal.png)

---

## Features

- Converts any image to a **height-field STL** model
- Generates **two separate G-Code files** in one step:
  - `*_roughing.nc` – layer-by-layer stock removal with the roughing tool
  - `*_finishing.nc` – single-pass contour tracing with the finishing tool
- Full **3-D tool-path visualization** with feed/rapid distinction
- **Step-by-step NC simulation** with seek slider and play/pause
- **Material removal simulation** to preview the machined surface
- Supports **G54** (work coordinate system offset) and **G92** (machine-origin zeroing)
- **Project files** (`.i2s`) save and restore all settings, both tools and both G-Code programs
- Backward-compatible with old single-tool project files

---

## Workflow

1. **Load Image** – open any PNG/JPG/BMP image.
2. **Set workpiece dimensions** – enter width, length and maximum depth in mm.
3. **Configure tools** – edit the roughing and finishing tools (type, diameter, geometry).
4. **Set feed rates** – enter separate feed rates for roughing and finishing.
5. **Choose milling mode** – *Engrave* or *Relief*.
6. **Generate STL** – preview the 3-D model on the *STL Model* tab.
7. **Generate Both G-Codes** – creates and saves `*_roughing.nc` and `*_finishing.nc`.
8. **Visualize** – switch to the *G-Code Path* tab and select roughing or finishing.
9. **Simulate** – switch to the *Simulation* tab for the step-by-step playback.
10. **Check material removal** – switch to the *Material Removal* tab.
11. **Save Project** – store all settings as a `.i2s` project file for later use.
12. **Machine** – load both `.nc` files into your CNC controller (e.g. [MyCncMill](https://www.mycncmill.de)) and run them in order: roughing first, finishing second.

---

## Settings Reference

### Workpiece Dimensions

| Setting | Unit | Description |
|---|---|---|
| **Width (X)** | mm | Physical width of the workpiece along the X axis |
| **Length (Y)** | mm | Physical length of the workpiece along the Y axis |
| **Maximum Depth (Z)** | mm | Maximum milling depth; maps to the darkest (or brightest) pixel, depending on direction |
| **Safe Height (Z)** | mm | Z height for all rapid (G0) traverse moves above the workpiece surface; must be high enough to clear all clamps and stock |

### Image Settings

| Setting | Description |
|---|---|
| **Image Smoothing** | Gaussian blur radius in pixels (0 = off). Smooths hard pixel edges before converting to a height map. Useful for low-resolution source images. |
| **Conversion Direction** | **Bright areas higher** – light pixels become peaks, dark pixels become valleys. **Dark areas higher** – inverted mapping. |

### Coordinate System

| Option | Behaviour |
|---|---|
| **G54 (Work Offset)** | The G-Code activates the G54 work coordinate system. The WCS offset must be set once on the machine (e.g. via `G10 L20 P1`) before running the program. Recommended for repeatable setups. |
| **G92 (Machine Origin)** | Jog the spindle manually to the lower-left corner of the workpiece at the surface (Z = 0) and start the job. The program issues `G92 X0 Y0 Z0` to define that position as the origin on-the-fly. |

### Tools

Two independent tools can be configured — one for roughing, one for finishing:

| Parameter | Description |
|---|---|
| **Type** | **Flat End Mill** – flat bottom cutter, ideal for roughing. **Ball Nose** – hemispherical tip, ideal for smooth 3-D surface finishing. |
| **Cut Diameter (mm)** | Cutting diameter in mm. The cutting radius (= diameter ÷ 2) is derived automatically and used for G-Code cutter-radius compensation. |
| **Cut Length (mm)** | Usable cutting length (flute length). |
| **Shank Ø / Length (mm)** | Shank geometry – used for the 3-D tool model in the simulation. |

> **Roughing tool** (default: Flat End Mill Ø 3.0 mm) – clears material layer by layer.  
> **Finishing tool** (default: Ball Nose Ø 2.0 mm) – follows the 3-D contour in a single pass.

### Feed Rates

| Setting | Unit | Description |
|---|---|
| **Roughing Feed (F)** | mm/min | Feed rate for all cutting moves in the roughing G-Code |
| **Finishing Feed (F)** | mm/min | Feed rate for all cutting moves in the finishing G-Code |

### Advanced Parameters

| Setting | Unit | Description |
|---|---|
| **Z-Offset** | mm | Constant offset added to all Z coordinates. Use to compensate for fixture height. |
| **Max. Z-Infeed** | mm | Maximum depth per roughing layer. Smaller = more passes, better quality. Does **not** apply to the finishing pass. |

### Milling Mode

| Mode | Description |
|---|---|
| **Engrave** | The relief is carved *into* a flat blank. Image features stand *out* above a fully removed background. |
| **Relief** | Material is removed *around* the features. Image features remain as high points; the background is cut away. |

---

## G-Code Output

Clicking **Generate Both G-Codes** opens a single save dialog. Both files are saved automatically:

```
my_project_roughing.nc   – roughing program (layer-by-layer raster)
my_project_finishing.nc  – finishing program (single-pass 3-D contour)
```

Use them in sequence in your CNC controller — roughing first, then finishing.

---

## Visualization Tabs

### G-Code Path
3-D display of the selected tool path:
- **Red** – cutting (feed) moves
- **Blue / transparent** – rapid (G0) traverse moves

### Simulation
Step-by-step playback with a 3-D tool model, seek slider, speed control and play/pause.

### Material Removal
Voxel-based preview of the machined surface for roughing or finishing independently.

---

## Project Files (`.i2s`)

Saves and restores: image path, workpiece dimensions, both tools, both G-Code programs, all settings.  
Old single-tool project files are automatically migrated.

---

## License

Source code is proprietary. See [LICENSE](LICENSE) for details.  
Contact: [www.mycncmill.de](https://www.mycncmill.de)
