# Face Landmark & Segmentation Analyzer

CSC 126 Computer Vision Midterm Project, Caraga State University (CCIS)

## Project description

A Python + OpenCV application that takes a **static image** of a face and runs
three techniques in one pipeline:

| Topic | What the program does |
|-------|-----------------------|
| **a. Image Operations** | Resize, BGR to grayscale conversion, histogram equalization, Gaussian blur |
| **b. Image Segmentation** | YCrCb skin thresholding, morphological opening/closing, contours, face-region and lip masks |
| **f. Facial Landmark Detection** | Dense face mesh landmarks using MediaPipe Face Landmarker |

**Pipeline:** load image -> preprocess -> detect face landmarks -> segment
face skin and lips using the landmarks -> draw results -> save and display.

**Output:** a 6-panel summary figure (original, grayscale + equalized, skin mask,
face region from landmarks, segmented face skin, final annotated result with
bounding box, label, landmarks and lip overlay) and an annotated image, saved
to `outputs/`.

## Project structure

```
face-landmark-cv/
|-- main.py             # the full application
|-- requirements.txt    # dependencies
|-- README.md
|-- images/             # sample input images
|-- outputs/            # generated results and screenshots
|-- models/             # landmark model (downloaded automatically)
```

## Setup and how to run

The first run downloads the landmark model file, so an internet connection is
needed. Choose one option.

### Option A: Google Colab

1. Open [colab.research.google.com](https://colab.research.google.com) and create a new notebook.
2. Upload `main.py`, `requirements.txt` and the sample images, and move the images into `images/`:
   ```python
   from google.colab import files
   import os, shutil

   uploaded = files.upload()   # select main.py, requirements.txt and the sample images together

   os.makedirs("images", exist_ok=True)
   for name in uploaded:
       if name.lower().endswith((".jpg", ".jpeg", ".png")):
           shutil.move(name, os.path.join("images", name))
   ```
3. Install the dependencies:
   ```
   !pip install -q -r requirements.txt
   ```
   If Colab asks you to restart, use **Runtime > Restart session**.
4. Run the program:
   ```
   !python main.py --image images/sample1.jpg --no-show
   ```
5. Show the results:
   ```python
   from IPython.display import Image, display
   display(Image("outputs/sample1_summary.png"))
   display(Image("outputs/sample1_annotated.png"))
   ```
6. Download the results before closing, because Colab deletes its files when the session ends:
   ```
   !zip -r outputs.zip outputs
   files.download("outputs.zip")
   ```

### Option B: Local computer (VS Code)

Requires Python 3.

1. Clone the repository and open the folder in VS Code.
2. Create and activate a virtual environment:
   - Windows: `python -m venv .venv` then `.venv\Scripts\Activate.ps1`
   - macOS/Linux: `python3 -m venv .venv` then `source .venv/bin/activate`
3. Install the dependencies:
   ```
   pip install -r requirements.txt
   ```
4. Run the program:
   ```
   python main.py --image images/sample1.jpg
   ```
   A window shows the six-panel result. Add `--no-show` to only save the results.

If the model download fails, the program prints the download link and where to
save the file (`models/face_landmarker.task`).

## Limitations

- Works best on clear, well-lit, front-facing faces.
- The skin threshold uses a fixed YCrCb range, so strong shadows, unusual
  lighting or skin-like background colors can affect the skin mask; the face
  region from the landmarks keeps the final result limited to the face.
- Processes one static image at a time.

## Screenshots

Add your screenshots from `outputs/` here, for example:

![Summary result](outputs/sample1_summary.png)
