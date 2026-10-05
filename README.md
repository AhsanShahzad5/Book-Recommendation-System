# Book Recommendation System

A notebook-based machine learning project that explores two practical ways to recommend books: a **popularity-based recommender** and an **item-based collaborative filtering recommender**.

The project uses reader ratings from the Book-Crossing dataset to identify highly rated books and find titles with similar rating patterns.

## What the project does

### 1. Popularity-based recommendations

The popularity model ranks books using their average rating while requiring enough feedback to make the ranking meaningful.

- Keeps books with at least **250 ratings**
- Sorts them by average rating
- Returns the **top 50 books**
- Includes the title, author, cover image, rating count, and average rating

### 2. Collaborative filtering

The collaborative model recommends books based on similarities in user-rating behavior.

- Keeps active users who rated more than **200 books**
- Keeps books with at least **50 ratings** from those users
- Builds a book-user ratings matrix
- Replaces missing ratings with zero
- Calculates book-to-book similarity using **cosine similarity**
- Returns the four most similar books for a selected title

For example, the notebook recommends the following titles for `1984`:

1. _Animal Farm_
2. _The Handmaid's Tale_
3. _Brave New World_
4. _The Vampire Lestat_

## Project structure

```text
.
|-- code.ipynb          # Data exploration and recommender implementation
|-- notes.md            # Learning notes about recommendation approaches
|-- README.md
`-- data/
    |-- Books.csv       # Book metadata and cover image URLs
    |-- Ratings.csv     # User-book ratings
    |-- Users.csv       # User demographic information
    |-- classicRec.png  # Reference diagram
    |-- DeepRec.png     # Reference diagram
    `-- recsys_taxonomy2.png
```

## Dataset

The project uses three CSV files from the Book-Crossing dataset:

- `Books.csv`: ISBN, title, author, publication year, publisher, and cover URLs
- `Ratings.csv`: user ID, ISBN, and book rating
- `Users.csv`: user ID, location, and age

The current model uses book metadata and ratings. User age and location are not used for recommendation generation.

> A rating of `0` is present in the source data and represents implicit feedback. The current notebook retains these values when building the ratings matrix.

## Getting started

### Prerequisites

- Python 3.9 or newer
- Jupyter Notebook or JupyterLab

### Installation

Clone the repository, move into the project directory, and create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
pip install jupyter numpy pandas scikit-learn
```

Start Jupyter:

```bash
jupyter notebook code.ipynb
```

Run the cells from top to bottom to load the data, build both recommenders, and test the `recommend()` function.

> **Linux note:** the dataset files in `data/` use capitalized names (`Books.csv`, `Users.csv`, and `Ratings.csv`), while the notebook currently references lowercase filenames. Change those three paths in the loading cell when running on a case-sensitive file system.

## Usage

After running all notebook cells, request recommendations by passing an eligible book title exactly as it appears in the pivot table:

```python
recommend("1984")
```

The function returns a list containing each recommended book's title, author, and medium-sized cover image URL.

## How it works

```text
CSV data
   |
   v
Merge ratings with book metadata
   |
   +--> Aggregate rating counts and averages --> Popularity top 50
   |
   `--> Filter active users and well-rated books
            |
            v
       Book-user matrix
            |
            v
       Cosine similarity
            |
            v
       Four similar books
```

The collaborative pipeline produces a similarity matrix for **706 books** after filtering the dataset.

## Tech stack

- Python
- NumPy
- pandas
- scikit-learn
- Jupyter Notebook

## Current limitations

- Recommendations are available only for titles that remain after filtering.
- The function expects an exact title and does not yet handle unknown or misspelled input.
- Zero-valued implicit ratings are treated as numeric zeros.
- The model is created in memory each time the notebook runs.
- A web interface, API, model persistence, and deployment are not implemented yet.

## Possible improvements

- Add friendly handling for missing book titles
- Separate explicit and implicit feedback
- Evaluate recommendation quality with ranking metrics
- Save the processed data and similarity matrix
- Build an API and a simple web interface
- Add content-based signals and create a hybrid recommender
