# Proper 2D to 3D Extrusion Tool

## 🧊 PROPER 2D → 3D SOLID EXTRUDER

A fully dynamic Python tool that converts **any 2D image shape** into a true **3D solid model** with:

* ✅ Sharp, pointy corners (no rounding)
* ✅ Solid interior (no hollow frames)
* ✅ Works with logos, icons, text, geometric shapes
* ✅ Preserves original color
* ✅ Automatic background removal & smart mask detection

Exports a clean, watertight **.glb 3D model** ready for viewing or use in Blender, Unity, Unreal, or 3D printers.

---

## 📌 Key Features

* Dynamic object detection (no manual masking required)
* Supports any shape & any color
* AI background removal when available (`rembg`)
* Fallback intelligent thresholding system
* Delaunay triangulation for solid interior
* Adjustable thickness & scale
* Optimized for sharp geometry (no smoothing)
* Auto-opens in Windows 3D Viewer

---

## 📂 Output

* ✅ `.glb` 3D model
* ✅ Fully watertight mesh
* ✅ Colored & centered model

---

## 🛠 Requirements

Install dependencies:

```bash
pip install numpy opencv-python pillow trimesh scipy
```

Optional (for better background removal):

```bash
pip install rembg
```

---

## 🚀 Usage

### Command Line

```bash
python ultimate_3d_generator.py path/to/image.png --thickness 0.5 --scale 2.0
```

### Parameters

| Parameter   | Description              | Default  |
| ----------- | ------------------------ | -------- |
| image       | Path to the 2D image     | required |
| --thickness | Depth of extrusion in 3D | 0.3      |
| --scale     | Overall size multiplier  | 2.0      |

---

## 📦 Example

```bash
python ultimate_3d_generator.py logo.png --thickness 1 --scale 1.5
```

This produces:

```
logo.glb
```

An extruded 3D solid model of the logo with sharp edges.

---

## 🧠 How It Works

### 1. Object Detection

The script intelligently detects the main object by:

* Using `rembg` AI if available
* Falling back to:

  * OTSU threshold
  * Adaptive threshold
  * Median threshold

It selects the most reliable mask using heuristic scoring.

### 2. Contour Extraction

* Extracts the largest contour
* Preserves corners using minimal approximation

### 3. Interior Filling

* Samples up to 5000 random interior points
* Combines them with boundary points
* Uses **Delaunay triangulation** to build real solid geometry

### 4. 3D Extrusion

Creates:

* Front face
* Back face
* Side faces (using boundary edges)

### 5. Cleanup & Optimization

* Merges duplicate vertices
* Fixes normals
* Removes degenerate & duplicate faces
* Ensures watertight solid mesh

---

## 🔧 Adjustable Settings

Inside code:

```python
if len(xs) > 5000:
    np.random.seed(42)
    indices = np.random.choice(len(xs), 5000, replace=False)
```

* This controls interior point density
* Higher = smoother interior but slower

---

## 📊 Sample Output Report

```
✅ 3D Generation complete!
Vertices: 4,312
Faces: 8,604
Color: RGB(255, 100, 20)
Watertight: YES
```

---

## 🖼 Supported Input Formats

* PNG
* JPG
* JPEG
* WebP

Supports transparent and non-transparent images.

---

## ❗ Notes

* Designed for SHARP geometry – smoothing is intentionally disabled
* Complex images may take longer to triangulate
* Best results with high-contrast shapes

---

## 🧩 Troubleshooting

### "No object detected"

* Use higher contrast image
* Add transparent background
* Install rembg for AI masking

### Slow performance

* Reduce max interior points (5000 → 2000)
* Use simpler shapes

---

## 💡 Ideal Use Cases

* Logo extrusion
* 2D to 3D conversion
* Game assets
* 3D printing shapes
* Educational visualization

---

## 👩‍💻 Author

Developed for dynamic 2D ➜ 3D conversion with full geometry control and precision.

---

## ✅ License

Free to use and modify for educational and personal projects.

---

If you need:

* STL export
* Smoothing versions
* UI version
* Batch processing

Just ask!# 🧊 Proper 2D → 3D Extrusion Tool

A powerful Python-based system that converts **any 2D image into a true solid 3D model** with **sharp corners, no hollows, and watertight geometry**. Designed for shapes, logos, icons, and silhouettes, this tool dynamically detects the object and creates a realistic 3D extrusion.

---

## 🚀 Features

✅ Converts ANY 2D shape into real 3D solid geometry
✅ Preserves SHARP corners and edges (no smoothing)
✅ Automatic background removal (AI or heuristic-based)
✅ Adaptive object detection for all colors & shapes
✅ Interior filled using Delaunay triangulation
✅ Generates watertight mesh (no gaps or hollow frames)
✅ Exports to `.glb` (ready for Blender, Unity, 3D Viewer, etc.)

---

## 📦 Output

* ✅ Automatically opens in **Windows 3D Viewer** after generation for instant preview

* ✅ Sample input image **triangle.JPG** is included with this repository for testing and demonstration

* ✅ Automatically opens in **Windows 3D Viewer** after generation for instant preview

- A solid 3D model in `.glb` format
- Correct depth extrusion
- Accurate object color preserved
- Automatically centered in 3D space

---

## 🛠 Requirements

Install the required libraries:

```bash
pip install numpy opencv-python pillow trimesh scipy rembg
```

> ⚠️ `rembg` is optional; the script will fall back to adaptive thresholding if not installed.

---

## ▶️ Usage

Run from command line:

```bash
python your_script.py input_image.png --thickness 0.5 --scale 2.5
```

### Arguments

| Parameter | Description                     | Default  |
| --------- | ------------------------------- | -------- |
| image     | Path to 2D input image          | required |
| thickness | Extrusion depth (front to back) | 0.3      |
| scale     | Overall size scaling of model   | 2.0      |

---

## 🧠 How It Works

### 1. Smart Object Detection

* Uses `rembg` if available for AI background removal
* Falls back to adaptive / OTSU thresholding
* Chooses the best mask using heuristic scoring

### 2. Sharp Contour Extraction

* Finds the main contour
* Uses ultra-low epsilon in `cv2.approxPolyDP` to retain pointy edges

### 3. Interior Filling

* Fills the mask
* Randomly samples up to **5000 interior points** (for performance)
* Combines them with boundary points
* Applies **Delaunay triangulation** for solid interior mesh

### 4. 3D Extrusion

* Front face at +thickness/2
* Back face at -thickness/2
* Side walls created only from boundary points

### 5. Mesh Cleanup

* Removes duplicates & degenerate faces
* Repairs holes
* Fixes normals
* Ensures watertight geometry

---

## 📐 Why 5000 Interior Points?

Sampling every pixel can create millions of points → extremely slow triangulation.

So we cap at 5000 to:

* Maintain performance ✅
* Preserve shape detail ✅
* Avoid crashes & memory spikes ✅

---

## 🖼 Sample Input & Output

### 📥 Sample Input Image (2D)

> The repository includes a ready-to-use sample image:

**triangle.JPG**

⬇️ Place your input image preview here:

```
<img width="344" height="274" alt="image" src="https://github.com/user-attachments/assets/fbcde476-9a7c-49ee-aa7b-89ca937bb62a" />
```

---

### 📤 Generated 3D Output

After running the tool, the extruded 3D model will be generated and opened automatically in Windows 3D Viewer.

⬇️ Place your 3D model preview screenshot here:

```
<img width="338" height="308" alt="image" src="https://github.com/user-attachments/assets/b7237a5d-d279-4c62-b793-7ffca81d9cd4" />

```

---

## 📂 Example Workflow

1. Input image: logo.png
2. Script detects object
3. Shape is triangulated
4. Solid mesh created
5. Output: `logo.glb`
6. Opens automatically in Windows 3D Viewer

---

## 🖼 Supported Input Types

* PNG (transparent or solid background)
* JPG
* Logos, icons, silhouettes, text shapes, symbols

---

## 🧪 Sample Command

```bash
python create_3d.py triangle.png --thickness 1 --scale 3
```

---

## 🔍 Troubleshooting

| Problem            | Solution                                          |
| ------------------ | ------------------------------------------------- |
| No object detected | Ensure clear contrast between object & background |
| Jagged edges       | Increase image resolution                         |
| Too many vertices  | Slightly increase epsilon value                   |
| Slow processing    | Reduce image size or sampling cap                 |

---

## 💡 Customization Tips

* Change detail level:

```python
epsilon = 0.002 * perimeter  # higher = smoother edges
```

* Adjust sampling:

```python
if len(xs) > 3000:  # lower for speed
```

---

## 📜 License

Free for educational and personal use.

---

## 👩‍💻 Author

Developed for dynamic 2D ➜ 3D shape generation with professional geometry output and sharp fidelity.

---

## ✅ Summary

This tool transforms flat images into accurate solid 3D models while preserving every corner and detail — perfect for FYPs, AR/VR, prototyping, and game asset creation.

---

🎉 Ready to convert your 2D art into real 3D geometry!
