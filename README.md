# Interactive Image Segmentation using Graph Cuts and Superpixels

An interactive foreground-background image segmentation tool that combines **SLIC superpixels** with **graph-cut optimization** (Boykov-Kolmogorov max-flow/min-cut) to extract objects from images based on user-drawn scribbles.

## How It Works

1. **User Interaction** — Mark foreground (object) and background regions by drawing scribbles directly on the image using the mouse.
2. **Superpixel Generation** — The image is over-segmented into superpixels using the SLIC algorithm (`cv2.ximgproc.createSuperpixelSLIC`), grouping perceptually similar pixels into compact regions.
3. **Graph Construction** — A graph is built where:
   - Each superpixel is a node
   - Neighboring superpixels are connected with edges weighted by their similarity (CIE Lab color histogram comparison + spatial proximity)
   - Source (s) and sink (t) terminal nodes encode user-provided foreground/background labels
4. **Graph Cut Optimization** — The Boykov-Kolmogorov max-flow/min-cut algorithm finds the optimal partition that separates foreground from background by minimizing an energy function that balances region similarity and boundary smoothness.
5. **Output** — The segmented foreground is extracted and displayed alongside the original image.

## Pipeline

```
Input Image → User Scribbles → SLIC Superpixels → Graph Construction → Min-Cut → Segmented Output
```

## Features

- **Interactive GUI** — Draw object markers (red) with `O` key and background markers (blue) with `B` key
- **CIE Lab Color Space** — Operates in perceptually uniform Lab space for more accurate color similarity
- **3D Color Histograms** — Superpixel similarity computed via Bhattacharyya distance on Lab histograms
- **Custom Boykov-Kolmogorov Implementation** — Max-flow/min-cut solver built on top of NetworkX's residual graph utilities

## Usage

```bash
pip install opencv-contrib-python numpy networkx matplotlib
python main.py
```

1. Enter the image filename when prompted
2. Press `O` and draw over the **object** (red strokes)
3. Press `B` and draw over the **background** (blue strokes)
4. Press `Esc` to run segmentation

## Project Structure

```
├── main.py          # Main pipeline: GUI, superpixel extraction, graph construction, segmentation
├── Min_Cut.py       # Boykov-Kolmogorov max-flow/min-cut algorithm
└── README.md
```

## Tech Stack

- **OpenCV** — Image I/O, SLIC superpixels, Lab color conversion, histogram computation
- **NetworkX** — Graph data structure and residual network utilities
- **NumPy** — Array operations and mask generation
- **Matplotlib** — Result visualization

## References

- Boykov, Y., & Kolmogorov, V. (2004). *An Experimental Comparison of Min-Cut/Max-Flow Algorithms for Energy Minimization in Vision.* IEEE TPAMI.
- Achanta, R., et al. (2012). *SLIC Superpixels Compared to State-of-the-Art Superpixel Methods.* IEEE TPAMI.
