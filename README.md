# Generate DEM other Terrain Features from USGS 3D Elevation Program (3DEP) Lidar Data for User-defined Area of Interest

## Author
Bishal Dhungana
Geospatial Data Scientist

## Overview

This workflow generates a Digital Elevation Model (DEM) and topographic products from USGS 3D Elevation Program (3DEP) lidar data for a user-defined area of interest (AOI).

### Steps Involved
1. **Data Source**: Retrieve 3DEP lidar data from AWS EPT bucket for the selected AOI.
2. **Environment Setup**: Clone the GitHub repository, set up the Python environment, and install necessary libraries like GDAL and PDAL.
3. **Data Access and Preprocessing**: Import required libraries, project 3DEP dataset polygons, and define the AOI using shapefiles or an interactive map.
4. **Point Cloud Data Processing (PDAL)**: Build and execute PDAL pipelines to process the lidar data.
5. **DEM Generation (GDAL)**: Create a DEM using filtered point cloud data and save it in the desired format.
6. **Topographic Products Generation**: Generate additional topographic products from the DEM (slope, contours, etc).

## Detailed Instructions

### Install and Run on Local File System

Because of the file size, it is recommended to run the Jupyter Notebook on your local machine:

1. **Create Directory**:
    ```bash
    $ mkdir post_lidar
    ```

2. **Clone Repository**:
    ```bash
    $ cd post_lidar
    $ git clone -b usgs_lidar_products https://github.com/NextEraEnergy/ds905-LiDAR_Processing.git
    ```

3. **Set Up Esri Python Environment**:
    - Identify the path to Esri's Python environment clone (typically located in the ArcGIS Pro installation directory).
    - Change to the directory and activate the environment:
      ```bash
      $ cd "C:\Program Files\ArcGIS\Pro\bin\Python\envs\arcgispro-py3-clone"
      $ conda activate arcgispro-py3-clone
      ```
    - Install necessary libraries:
      ```bash
      $ conda install -c conda-forge gdal pdal
      ```

4. **Create and Activate Virtual Environment**:
    ```bash
    $ conda env create -n venv --file environment.yml
    $ conda activate venv
    ```

5. **Launch Jupyter Notebook**:
    Refer to this [guide](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/execute.html) if unsure how to launch a notebook.

### Access the Full Workflow

For detailed steps and code implementation, please refer to the Jupyter Notebooks in the GitHub repository. The notebooks include all necessary functions, library imports, and processing steps required to generate the DEM and topographic products:

- [Generate DEM from 3DEP Lidar Data - Jupyter Notebook](https://github.com/DBishal13/Lidar-Products-USGS3DEP/blob/main/notebooks/Generate_DEM_AOI.ipynb)
- [Generate Topographic Products from DEM - Jupyter Notebook](https://github.com/DBishal13/Lidar-Products-USGS3DEP/blob/main/notebooks/Generate_DEM_AOI.ipynb)

## Additional Resources
- https://github.com/OpenTopography/OT_3DEP_Workflows/tree/main/notebooks
- The USGS 3DEP lidar point cloud data are accessible in Entwine Point Tile (EPT) format from this [Amazon Web Services S3 Bucket](https://s3-us-west-2.amazonaws.com/usgs-lidar-public/index.html).
- Documentation for open-source Python libraries used by these workflows include [PDAL](https://pdal.io/) and [GDAL](https://gdal.org/)
- Access USGS 3DEP via the [OpenTopography portal](https://opentopography.org/)

### Workflow for Generating Other Topographic Products

The second notebook, `Generate_DEM_Products.ipynb`, extends the workflow by creating additional topographic products from the generated DEM. Follow these steps:

1. **Load DEM**: Import the generated DEM file into the notebook. This step is crucial as it serves as the base for generating other topographic products.


2. **Generate Slope**: Calculate the percentage rise slope from the DEM using GDAL and richdem. Slope represents the rate of change of elevation and is useful for various analyses, such as identifying steep areas.


3. **Generate Contours**: Extract contour lines from the DEM using GDAL. Contours are lines that connect points of equal elevation and are essential for understanding the terrain's shape.
4. **Generate Hillshade**: Create a hillshade from the DEM to visualize the terrain in a 3D-like effect. Hillshade is useful for enhancing the visual interpretation of the terrain.


5. **Save Outputs**: Save the generated topographic products, such as slope, contours, and hillshade, in the desired formats for further use or analysis.



### Reprojecting Files to State Plane for Mapbooks

For creating mapbooks, it is often necessary to reproject the generated DEM and other topographic products to a specific coordinate system, such as the State Plane Coordinate System. This ensures that the data aligns correctly with other spatial datasets used in the mapbook.

1. **Reproject DEM**: Use GDAL to reproject the DEM to the desired State Plane coordinate system.
    ```bash
    $ gdalwarp -t_srs EPSG:<State_Plane_EPSG_Code> input_dem.tif output_dem_stateplane.tif
    ```

2. **Reproject Other Products**: Similarly, reproject other topographic products like slope, contours, and hillshade to the State Plane coordinate system.
    ```bash
    $ gdalwarp -t_srs EPSG:<State_Plane_EPSG_Code> input_slope.tif output_slope_stateplane.tif
    ```

### Extending the Workflow for Additional Products

After generating the basic topographic products, you can extend the workflow to create additional products such as:

- **Slope Exclusion**: Identify and exclude areas with slopes above a certain threshold, which can be useful for various analyses, such as site suitability studies.
- **Aspect**: Calculate the aspect (direction of slope) from the DEM, which is important for understanding sunlight exposure and drainage patterns.
- **Viewshed Analysis**: Determine visible areas from a specific location using the DEM, which is useful for telecommunications and landscape planning.
- **Landform Classification**: Classify the terrain into different landforms (e.g., ridges, valleys) based on the DEM and derived products.

For detailed code and implementation of these additional products, you can extend the existing Jupyter Notebooks or create new ones as needed.

