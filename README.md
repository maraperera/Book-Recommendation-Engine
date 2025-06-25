# Book Recommendation System (K-Nearest Neighbors)

This project implements a book recommendation system using the K-Nearest Neighbors (KNN) algorithm. It leverages the Book-Crossings dataset to suggest similar books based on user ratings.

## Table of Contents

-   [Overview](#overview)
-   [Features](#features)
-   [Dataset](#dataset)
-   [How it Works](#how-it-works)
-   [Getting Started](#getting-started)
    -   [Prerequisites](#prerequisites)
    -   [Installation](#installation)
    -   [Usage](#usage)
-   [Expected Output](#expected-output)

## Overview

This project provides a Python-based solution to recommend books to users. By analyzing historical user ratings, the system identifies books that are "close" to a given book based on their rating patterns across users. This is achieved using the `NearestNeighbors` algorithm from `scikit-learn`, which calculates the cosine distance between book vectors in a high-dimensional space.

The core of the system is the `get_recommends` function, which takes a book title as input and returns a list of 5 similar books along with their calculated distances.

## Features

* **K-Nearest Neighbors (KNN)**: Utilizes the KNN algorithm to find the most similar books.
* **Cosine Similarity**: Employs cosine distance as the metric for determining book similarity, effective for rating data.
* **Data Preprocessing**: Includes steps to filter out infrequent users and less-rated books to improve recommendation quality.
* **Sparse Matrix Optimization**: Uses `scipy.sparse.csr_matrix` for efficient handling of the large and sparse rating data.

## Dataset

The project uses the **Book-Crossings dataset**, which contains:
* **1.1 million ratings** (scale of 1-10)
* **270,000 books**
* **90,000 users**

The dataset is provided in `BX-Books.csv` and `BX-Book-Ratings.csv`.

## How it Works

1.  **Data Loading**: The `BX-Books.csv` (containing ISBN, title, author) and `BX-Book-Ratings.csv` (containing user ID, ISBN, rating) files are loaded into pandas DataFrames.
2.  **Data Preprocessing**:
    * Users with fewer than 200 ratings are removed.
    * Books with fewer than 100 ratings are removed.
    * Book information and ratings are merged.
    * Duplicate book titles are handled to ensure each unique book is represented once.
3.  **Pivot Table Creation**: A pivot table is created where:
    * Rows represent **book titles**.
    * Columns represent **user IDs**.
    * Values represent **ratings**.
    * Missing ratings are filled with zeros.
4.  **Sparse Matrix Conversion**: The pivot table is converted into a `scipy.sparse.csr_matrix` for memory efficiency and faster computation with `NearestNeighbors`.
5.  **Model Training**: A `NearestNeighbors` model is initialized with `metric='cosine'` and `algorithm='brute'` and trained on the sparse book-user rating matrix.
6.  **Recommendation Generation**: The `get_recommends` function, given a book title, finds its corresponding vector in the sparse matrix and uses the trained KNN model to identify the 5 closest books (excluding itself) based on cosine distance.

## Getting Started

### Prerequisites

* Python 3.x
* Jupyter Notebook or Google Colaboratory (recommended)
* Required Python libraries:
    * `numpy`
    * `pandas`
    * `scipy`
    * `scikit-learn`
    * `matplotlib` (optional, for plotting/visualization if implemented)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```
2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: `venv\Scripts\activate`
    ```
3.  **Install the required libraries:**
    ```bash
    pip install numpy pandas scipy scikit-learn matplotlib
    ```
4.  **Download the dataset:**
    The dataset files (`BX-Books.csv` and `BX-Book-Ratings.csv`) are typically downloaded directly within the notebook environment from the provided URL, or you can download them manually and place them in the project directory.

    If running in a notebook, you can use:
    ```python
    !wget [https://cdn.freecodecamp.org/project-data/books/book-crossings.zip](https://cdn.freecodecamp.org/project-data/books/book-crossings.zip)
    !unzip book-crossings.zip
    ```

### Usage

1.  **Open the Jupyter Notebook (or Google Colab notebook):**
    Navigate to the project directory and open the notebook file (e.g., `book_recommendation.ipynb`).
2.  **Run cells sequentially:**
    Execute each cell in the notebook, from importing libraries and loading data to model training and defining the `get_recommends` function.
3.  **Call `get_recommends`:**
    You can then call the `get_recommends` function with any book title from the dataset to get recommendations.

    ```python
    # Example usage within your notebook or a Python script
    # Assuming the get_recommends function is defined and data loaded
    # in the current execution environment.

    # Example 1:
    recommendations_1 = get_recommends("The Queen of the Damned (Vampire Chronicles (Paperback))")
    print(recommendations_1)

    # Example 2:
    recommendations_2 = get_recommends("Where the Heart Is (Oprah's Book Club (Paperback))")
    print(recommendations_2)
    ```

## Expected Output

The `get_recommends` function returns a list where:
* The first element is the input book title.
* The second element is a list of five recommended books.
* Each recommended book is itself a list containing the book title and its distance from the input book.

**Example for "The Queen of the Damned (Vampire Chronicles (Paperback))"**:
