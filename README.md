# Hyperbolic Book Recommender System

## Overview

This project implements a book recommender system using hyperbolic embeddings. It processes book data from the Library of Congress, creates embeddings based on genre information and book descriptions, and visualizes the results in hyperbolic space. The system uses both Poincaré embeddings and Graph Factorization methods to create and compare different embedding models.

## Files and Their Functions

1. `analysis.py`: The main script that orchestrates the entire process.
2. `ingestion.py`: Handles data loading and preprocessing.
3. `model.py`: Contains functions for training Poincaré and Graph Factorization models.
4. `utils.py`: Provides utility functions for data manipulation and visualization.
5. `process_loc_jsons.py`: Preprocesses the Library of Congress JSON data.
6. `reduce_loc_dataset.py`: Reduces the dataset to a manageable size based on shelf ID frequency.

## Dependencies

The project requires the following Python libraries:
- pandas
- numpy
- matplotlib
- sklearn
- gensim
- gem (Graph Embedding Methods)

## Data Preprocessing

Before running the main analysis, the raw data from the Library of Congress needs to be processed and reduced. This is done in two steps:

1. **Processing LOC JSONs** (`process_loc_jsons.py`):
   - Loads JSON files obtained from the Library of Congress API.
   - Cleans and formats the data, extracting relevant fields (title, shelf_id, description).
   - Outputs a CSV file containing the full dataset.

2. **Reducing the Dataset** (`reduce_loc_dataset.py`):
   - Reads the full dataset created by `process_loc_jsons.py`.
   - Reduces the dataset by keeping only shelf IDs that appear more than 30 times.
   - Removes certain shelf IDs (e.g., those that are websites).
   - Maps shelf IDs to subclass names using a provided class names file.
   - Outputs a reduced CSV file containing only the relevant data.

## Workflow

1. **Data Preprocessing**:
   - Run `process_loc_jsons.py` to process the raw Library of Congress data.
   - Run `reduce_loc_dataset.py` to create a manageable dataset for analysis.

2. **Data Loading and Processing** (`ingestion.py`):
   - Loads the reduced book data CSV file.
   - Computes TF-IDF vectors for book descriptions.
   - Creates edge lists based on genre information and description similarities.

3. **Model Training** (`model.py`):
   - Trains Poincaré embedding models using genre-only and genre+description data.
   - Trains Graph Factorization models using the same data.

4. **Visualization** (`utils.py`):
   - Provides functions to visualize the embeddings in 2D space.
   - Uses t-SNE for dimensionality reduction when necessary.

5. **Analysis and Execution** (`analysis.py`):
   - Configures the system using a JSON configuration file.
   - Executes the main pipeline:
     a. Loads and processes the preprocessed data.
     b. Trains various embedding models (Poincaré and Graph Factorization).
     c. Generates visualizations for each model.

## Usage

1. Ensure all dependencies are installed.
2. Run `process_loc_jsons.py` to process the raw Library of Congress data.
3. Run `reduce_loc_dataset.py` to create the final dataset for analysis.
4. Update the `production.json` configuration file with appropriate file paths and parameters.
5. Run `analysis.py` to execute the main analysis pipeline.

## Configuration

The `production.json` file should contain the following keys:
- `PREPROCESSING`:
  - `FULL_DATASET`: Path to save the full processed dataset.
  - `SP_MIN` and `SP_MAX`: Range of JSON files to process.
  - `LOC_JSON_PATH`: Path to the directory containing LOC JSON files.
  - `CLASS_NAMES`: Path to the file mapping shelf IDs to class names.
  - `REDUCED_DATASET`: Path to save the reduced dataset.
- `ANALYSIS`:
  - `LITERATURE_FILE`: Path to the reduced dataset CSV file.
  - `CLASS_FILE`: Path to the file containing genre classification data.
  - `MIN_CUT` and `MAX_CUT`: Thresholds for similarity scores.
  - `N_CLS`: Number of genres/subgenres.
  - `GENRE_TSV_FNAME` and `GENPDES_TSV_FNAME`: Output file names for genre-only and genre+description edge lists.

## Output

The system generates several PDF files visualizing the embeddings:
- `poincare_genre.pdf`: Poincaré embedding using genre-only data.
- `GF_genre.pdf`: Graph Factorization embedding using genre-only data.
- `poincare_d2.pdf` and `poincare_d10.pdf`: Poincaré embeddings using genre+description data in 2D and 10D.
- `GF_d2.pdf` and `GF_d10.pdf`: Graph Factorization embeddings using genre+description data in 2D and 10D.

These visualizations show how books are clustered in the hyperbolic space based on their genres and content similarities.

## Extending the System

To extend or modify the system:
1. Add new embedding methods in `model.py`.
2. Implement additional preprocessing steps in `ingestion.py`.
3. Create new visualization techniques in `utils.py`.
4. Update `analysis.py` to incorporate new features or models.
5. Modify the preprocessing scripts to handle different data sources or formats.
