# Solar PV Panel Anomaly Detection Using Detectron2

## About the Project

This project focuses on detecting anomalies in solar panels using deep learning-based instance segmentation. Leveraging UAV-captured orthophotos and a trained Detectron2 model, the system identifies eight types of solar anomalies and links them to individual solar panels. The output is structured for spatial analysis and GIS integration, enabling improved inspection and maintenance workflows.

The approach combines computer vision, geospatial data processing, and model training using COCO-format annotations.

## Model Overview

- Framework: Detectron2 (by Facebook AI)
- Model: Mask R-CNN or Faster R-CNN
- Input: Orthophoto tiles (512×512 RGB)
- Output: Instance masks and bounding boxes for anomalies
- Number of Classes: 8
  - Single Hotspot
  - Multi Hotspots
  - Single Diode
  - Multi Diode
  - Single Bypassed Substring
  - Multi Bypassed Substring
  - String (Open Circuit)
  - String (Reversed Polarity)

## Project Workflow

1. Prepare georeferenced orthophoto
2. Tile the image into fixed-size patches
3. Run inference using the trained Detectron2 model
4. Extract bounding boxes and polygon masks
5. Export results to GeoJSON
6. Spatially join with segmented solar panel polygons
7. Generate a summary of anomalies per solar panel

## Project Structure
```
├── ortho/ # Input orthophoto
├── solar_polygon/ # Solar panel GeoJSON polygons
├── outputs/ # Model outputs: predictions and GeoJSONs
├── training_data/ # COCO-style training and validation data
  ├── train/
  │   ├── images/
  │   │   ├── img_0001.jpg
  │   │   ├── img_0002.jpg
  │   │   └── ...
  │   └── annotations/
  │       └── result.json         # COCO-format annotations for training images
  ├── valid/
  │   ├── images/
  │   │   ├── img_1001.jpg
  │   │   ├── img_1002.jpg
  │   │   └── ...
  │   └── annotations/
  │       └── result.json           # COCO-format annotations for validation images
├── test.ipynb # Jupyter Notebook for inference and export
├── train_model.py # Training script
├── environment.yml # Conda environment definition
```


## Setup Instructions

1. Clone the repository:
```
   git clone https://github.com/geomatupen/solaroly.git
   cd solaroly
```
3. Create and activate the environment:
```
   conda env create -f environment.yml
   conda activate thermal-detector
```
5. Run training or inference using the notebook or scripts.

## Training

To train the model on your own dataset:

- Prepare annotations in COCO format.
- Update paths in `train_model.py`.
- Run:
```
  python train_model.py
```
Trained weights will be saved to the `training_data/output/` directory.

## Inference

To run inference using the trained model:

- Make sure your trained weights (model_final.pth) are saved under output/.
- Place your orthophoto or test tiles inside a folder (e.g., ortho/).
- Update paths inside inference.py if needed (e.g., tiles_info, predictor setup, output folder).
- Run the script

## Output Files

- `anomalies_polygons.geojson`: Polygon masks of detected anomalies
- `anomalies_bbox.geojson`: Bounding box detections
- `anomalies_with_solar_id.geojson`: Anomalies linked to solar panel IDs
- `anomaly_summary.csv`: Summary of anomalies per solar panel and class

## Dependencies

- Python 3.10
- Detectron2
- PyTorch
- Rasterio
- GeoPandas
- Shapely
- OpenCV
- Matplotlib
- tqdm

Install all dependencies with:
```
   conda env create -f environment.yml
```
## License

This project is licensed under the GNU GENERAL PUBLIC LICENSE.

## Acknowledgements
- Special thanks to Termatics, Austria for providing the opportunity and support to develop this project.

Developed by Upendra Oli.
