# Deep Learning for Computer Vision using Python and MATLAB
This repository shows an example of how to integrate MATLAB apps into a Python deep learning workflow for computer vision and image analysis tasks, with emphasis on the *data preparation* stage of the traditional deep learning workflow.

![](figs/Fig1.jpg)

I will assume that: (1) you have a deep learning pipeline for computer vision in Python that you plan to adapt and reuse for a new (set of) task(s); and (2) the images associated with the new task(s) will require interactive actions, such as annotation, labeling, and segmentation. 

## The basic recipe
Assuming that you have [MATLAB installed and configured in your machine](https://www.mathworks.com/products/get-matlab.html) and your favorite Python setup (e.g., using Jupyter notebooks), calling MATLAB from a Python script is a straightforward process, whose main steps are:

1.	(In MATLAB) Install the [MATLAB Engine API for Python](https://www.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html), which provides a Python package called `matlab` that allows you to call MATLAB functions and exchange data between Python and MATLAB.
2.	(In Python) Configure paths and working directory. 
3.	(In Python) Start a new MATLAB process in the background:
`import matlab.engine`
`eng = matlab.engine.start_matlab('-desktop')`
4.	(In Python) Set up your variables (e.g., path to image folders).
5.	(In Python) Call a MATLAB app of your choice (e.g., Image Labeler app).
6.	(In MATLAB) Work (interactively) with the selected app and export results to variables in the workspace.
7.	(In Python) Save the variables needed for the rest of the workflow, e.g., image filenames and associated labels (and their bounding boxes).
8.	(In Python) Use the variables as needed, e.g., processing tabular data using pandas and using image-related labels as ground truth.
9.	Repeat steps 3 through 7 as many times as needed in your workflow.
10.	(In Python) Quit the MATLAB engine:
`eng.exit()`

## An example
Here is an example of how to use Python and MATLAB together for two different tasks within the scope of medical image analysis (using deep learning): skin lesion segmentation and (medical) image (ROI) labeling.

### Task 1: Skin Lesion Segmentation
The Task: Given a dataset of images containing skin lesions, we want to build a deep learning solution for segmenting each image, i.e., classifying each pixel as belonging to either the lesion (foreground) or the rest of the image (background). 
The Problem: In order to train and validate a deep neural network for image segmentation, we need to input both the images as well as the segmentation masks (Figure 2), which are essentially binary images where foreground pixels (in this case corresponding to the lesion) are labeled white and background pixels are marked as black. The job of the network is to learn the segmentation masks for new images.


### Task 2: Useful pre- and post-processing operations on WSIs in MATLAB

Since the goal of using deep learning techniques in CPATH is to produce solutions that are clinically translatable, i.e., capable of working across large patient populations, it is advisable to deal with some of the most likely WSI artifacts upfront, thereby increasing the abilities of the resulting model to generalize over image artifacts found in other test sets. 

The second part of this example shows examples of preprocessing operations to handle commonly found artifacts in histopathology images as well as postprocessing morphological operations for improving the quality of the results at pixel level. Essentially, this example should help the medical image analysis community to create an image analysis pipeline for WSIs (and, as bonus, the ability to reproduce the code and examples described in a [recent paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8057393/) on this topic) using MATLAB.

It highlights the usefulness of MATLAB (and Image Processing Toolbox) functions such as: 
- Image thresholding and filtering: `imbinarize`,`bwareafilt`, and `imlincomb`
- Morphological image processing operations: `imclose`, `imopen`, `imdilate`, `imerode`, `imfill`, and `strel`
- Feature extraction: `bwlabel` and `regionprops`
- Visualization: `montage`, `imoverlay`, `plot` and `rectangle`

![](figures/Fig7.png)

## Requirements
- [X]  [MATLAB 2021a](https://www.mathworks.com/products/matlab.html) or later
- [X]  [Image Processing Toolbox](https://www.mathworks.com/products/image.html)

## Suggested steps
1. Download or clone the repository.
2. Open MATLAB.
3. Ensure that the files `AT2Scan.jpg` and `FakePred2.jpg` containing the test images<sup>[1](#myfootnote1)</sup> for Part 2 are in the same folder as the `cpath_matlab.mlx` file. 
4. Run the `cpath_matlab.mlx` script and inspect results.
## Additional remarks

- You are encouraged to expand and adapt the example to your needs.
- The image used for Part 1 is part of MATLAB.
## Notes
<a name="myfootnote1">[1]</a> These images are publicly available at (https://github.com/BHSmith3/Image-Cleaning). 
