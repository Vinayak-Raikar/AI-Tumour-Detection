# Brain Tumor Detection from MRI Images

A Python and OpenCV project that segments brain MRI images and flags tumor-like regions. The user uploads a brain scan, and the program outlines the suspected region and reports whether one was found.

> **Disclaimer:** This is a learning/academic project. It is not a medical device and must not be used for diagnosis or any health decision.

## Features

- Upload an MRI image through a simple file-picker window
- Brain area isolation (removes background and skull edges)
- Detection of unusually bright regions inside the brain
- Visual output: original image, detected region, and a red outline over the suspected tumor
- Segmentation pipeline for experimenting with different methods:
  - Median filtering and texture filtering
  - Otsu thresholding with morphological operations
  - Local thresholding
  - Watershed segmentation
  - K-Means clustering (k = 4)
  - MSER region detection
- Handles TIF, BMP, JPG and PNG images, including 16-bit scans

## How it works

1. The image is converted to grayscale and resized.
2. Otsu thresholding finds the brain, and erosion removes the skull edge.
3. Pixels much brighter than the average brain intensity (mean + 2 standard deviations) are marked as candidates.
4. Morphological opening and closing clean the result.
5. The largest connected bright region is measured. If it covers enough of the brain area, it is reported as a tumor-like region.

This is a rule-based, segmentation approach and does not use a trained model, so results can include false positives and misses, and it works best on tumors that appear bright.

## Project structure

```
├── main.py             # Full segmentation pipeline (Otsu, watershed, K-Means, MSER)
├── quick_tumor.py      # Upload an image and detect a tumor-like region
├── tumor_predict.py    # Optional: SVM classifier (needs a labeled yes/no dataset)
└── dataset/            # MRI images
```

## Installation

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
pip install opencv-python numpy matplotlib scikit-image scikit-learn scipy pillow
```

## Usage

```bash
python quick_tumor.py
```

Choose an MRI image in the window that opens. The result is shown with a verdict and the suspected region outlined in red.

To run the full segmentation pipeline on one image:

```bash
python main.py
```

## Tuning

In `quick_tumor.py`, change `MIN_RATIO` to control sensitivity:

- Too many false alarms: increase it (for example `0.02`)
- Missing tumors: decrease it (for example `0.005`)

## Optional: trained classifier

`tumor_predict.py` trains an SVM on HOG features. It needs a labeled dataset with `yes` (tumor) and `no` (no tumor) folders placed in a `tumor_data/` directory. It prints accuracy measured on held-out test images.

## Limitations

- Rule-based detection, not a trained deep learning model
- Mainly detects bright tumors; dark or subtle ones may be missed
- Sensitive to image quality, scan type, and slice orientation
- Not validated clinically

## Tech stack

Python, OpenCV, NumPy, scikit-image, scikit-learn, SciPy, Matplotlib, Tkinter

## License

MIT (or choose your own)
