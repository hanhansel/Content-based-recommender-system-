<a id="top"></a>

<div align="center">

# **Video Game Recommender System**

<b><ins>A content-based recommender for a gaming marketplace: type in a game you like and get five similar games, based on text features and cosine similarity.</ins></b>

<br>

![Status](https://img.shields.io/badge/status-completed-green?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

</div>

<br>

<details open>
<summary><b>Table of Contents</b></summary>

1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [How It Works](#how-it-works)
4. [Skills Learned](#skills-learned)
5. [Challenges and Solutions](#challenges-and-solutions)
6. [Project Structure](#project-structure)
7. [Getting Started](#getting-started)
8. [Extra Things](#extra-things)
9. [Credits and Contact](#credits-and-contact)

</details>

---

## <ins>Project Overview</ins>

> **TL;DR:** I cleaned a dataset of 16,928 video games, turned each game's name, platform, genre, developer and rating into **TF-IDF** vectors, and used **cosine similarity** to recommend the five most similar games to any title a user types in.

### ▸ Problem
A gaming marketplace with thousands of titles needs a way to help players find games they'll like. When there's no user rating history to learn from, the simplest and most useful approach is to recommend games that are similar to one the player already enjoys. This project builds that recommender from a real-world video game dataset.

### ▸ Objectives
- *Explore and clean the dataset*: missing values, skewed distributions, and anomalies.
- *Turn each game's attributes into features* that can be compared.
- *Recommend five similar games* for any title the user enters, even with small typos.
- *Test the recommendations* on different games and reflect on the strengths and weaknesses of the approach.

### ▸ Context
Built as my final exam for the **Machine Learning** course in my third semester at Bina Nusantara University, using the exam's provided video game dataset.

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Tech Stack</ins>

> The tools I **used** and what I used them **for**.

| Category | Tool | What I Used It For |
|:---|:---|:---|
| **Language** | `Python` | Data cleaning, feature building and the recommendation function |
| **Data Processing** | `pandas`, `NumPy` | Loading the TSV dataset, fixing data types and filling missing values |
| **AI / ML** | `scikit-learn` | TF-IDF vectorization of game attributes and cosine similarity between every pair of games |
| **Visualization** | `matplotlib`, `seaborn` | Histograms and box plots for each numeric column, and the genre distribution chart |
| **Libraries** | `difflib` | Matching the user's input to the closest game title in the dataset |
| **Tools** | Jupyter | Running the full analysis in one notebook |

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>How It Works</ins>

> Clean the data → combine each game's attributes into text → TF-IDF → cosine similarity → top 5 matches for the user's game.

```
Game Dataset → EDA and Cleaning → Combined Text Features → TF-IDF Vectors → Cosine Similarity → Top 5 Games
```

1. **Explore.** Check missing values in every column, and plot a histogram and box plot with skewness and kurtosis for each numeric column.
2. **Clean.** Convert *User_Score* to numbers, so text like "tbd" becomes missing, then fill it with the median. Fill *Critic_Score* and *Critic_Count* with the median, and fill missing categorical values with the most common value.
3. **Build features.** Combine each game's *Name*, *Platform*, *Genre*, *Developer* and *Rating* into one text string, for example *"Mario Kart Wii Wii Racing Nintendo E"*.
4. **Vectorize.** Convert those strings into TF-IDF vectors, which give more weight to words that are distinctive for a game and less to words that appear everywhere.
5. **Compare.** Compute the cosine similarity between every pair of games, giving a 16,928 × 16,928 similarity matrix.
6. **Recommend.** The user types a game name. `difflib` finds the closest matching title, even with typos, and the five games with the highest similarity scores are returned.

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Skills Learned</ins>

> What this project taught me, technically and beyond.

- **Exploratory data analysis:** Checking missing values, distributions, skewness and kurtosis before deciding how to clean each column. See `Codes.ipynb`.
- **Data cleaning:** Converting mixed text and number columns like *User_Score* and choosing between median and mode imputation. See `Codes.ipynb`.
- **Text vectorization with TF-IDF:** Turning categorical attributes into weighted feature vectors that can be compared. See `Codes.ipynb`.
- **Similarity modeling:** Using cosine similarity to rank every game against every other game. See `Codes.ipynb`.
- **Fuzzy matching:** Using `difflib` so users don't need to type a title exactly. See `Codes.ipynb`.
- **Critical evaluation:** Testing the recommender on different games and spotting where cleaning choices hurt the results. See `Codes.ipynb`.
- **Building interpretable models:** Making a recommender where every suggestion can be explained by shared words, like the same series, platform or developer.

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Challenges and Solutions</ins>

> What went wrong, how I fixed it, and what I took away from the project.

**Messy numeric columns.** *User_Score* was stored as text because 2,441 games had "tbd" instead of a score, and about half the games had no *Critic_Score*. I converted *User_Score* to numbers so "tbd" became missing, then filled the gaps with the median, since several columns were heavily skewed. Some anomalies were never fixed, though: 9,210 negative *User_Count* values and 273 games with impossible release years like 1. They don't affect the recommendations, because the final model only uses text features, but they would matter for any model that uses the numbers. The lesson: check value ranges as well as missing values.

**Mode imputation hurt the recommendations.** 39.5% of games had no *Developer*, and every one of them was filled with the most common value, *Ubisoft*. *Rating* was filled the same way with *E*. Because *Developer* and *Rating* are part of the features, thousands of unrelated games now share the word *Ubisoft*. For example, *Super Mario Bros.* is treated as a Ubisoft game. Filling those gaps with *"Unknown"*, or leaving the column out, would have kept the similarities honest. The lesson: an imputation that's fine for statistics can create false patterns in a similarity model.

**Recommendations that weren't really new.** The input game always comes back as recommendation #1, because a game is always most similar to itself. On top of that, the dataset has 214 duplicate name and platform rows, and the same game can appear once per platform. Searching *Super Mario Bros.* returned *Super Mario Bros. 3* twice. In practice, users got three or four new games instead of five. Skipping the input game and removing duplicate titles before taking the top five would fix both.

**Matching what users actually type.** Users rarely type a title exactly as it appears in the dataset. I used `difflib.get_close_matches` to find the closest title, so small typos and different capitalization still work. If nothing is close enough, the function currently fails, so a "game not found" message is a good next step.

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Project Structure</ins>

```
Content-based-recommender-system-/
├── 3A.tsv          # video game dataset (16,928 games)
├── Codes.ipynb     # EDA, cleaning, TF-IDF, cosine similarity and recommendations
└── README.md
```

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Getting Started</ins>

```bash
# 1. Clone
git clone https://github.com/hanhansel/Content-based-recommender-system-.git
cd Content-based-recommender-system-

# 2. Install
pip install pandas numpy scikit-learn matplotlib seaborn jupyter

# 3. Run
jupyter notebook Codes.ipynb
```

> **Requirements:** Python 3. Before running, change the dataset path in the second cell from `E:/files/semester 3/...` to `3A.tsv`. The first cell also imports libraries the recommender doesn't use (`surprise`, `xgboost`, `graphviz`). Either install them with `pip install scikit-surprise xgboost graphviz`, or delete those import lines.

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Extra Things</ins>

<details>
<summary><b>Dataset</b></summary>
<br>

- **Source:** `3A.tsv`, the video game dataset provided for the exam
- **Size:** 16,928 games × 17 columns, across 31 platforms and 12 genres
- **Columns:** *Name*, *Platform*, *Year_of_Release*, *Genre*, *Publisher*, sales by region (*NA*, *EU*, *JP*, *Other*, *Global*), *Critic_Score*, *Critic_Count*, *User_Score*, *User_Count*, *Developer*, *Rating*
- **Missing values:** *Critic_Score* and *Critic_Count* (8,668 each), *User_Score* (6,769, plus 2,441 "tbd"), *Rating* (6,836), *Developer* (6,688), *Publisher* (55)
- **Preprocessing:** "tbd" converted to missing; median imputation for *User_Score*, *Critic_Score* and *Critic_Count*; mode imputation for categorical columns

</details>

<details>
<summary><b>Results and Key Findings</b></summary>
<br>

> **Headline:** The recommender works best for games in a series, where the shared title words make the matches obvious.

| Input Game | Top Recommendations |
|:---|:---|
| *Uncharted: The Nathan Drake Collection* | Uncharted 3: Drake's Deception, God of War Collection, Uncharted: Drake's Fortune, ICO |
| *Super Mario Bros.* | Super Mario Bros. 3, Super Mario Bros. 2, Mario Bros., Super Mario Bros. 3 |
| *Jak II* | Jak 3, Jak X: Combat Racing, Jak and Daxter: The Precursor Legacy, The Last of Us |

*The input game itself, which always came back as #1, is left out of the table.*

1. **Series are matched well:** shared words in the title dominate the similarity, so sequels and spin-offs rank at the top.
2. **Beyond the series, matches come from platform and developer:** *God of War Collection* and *ICO* for *Uncharted*, and *The Last of Us* for *Jak II*, are all Sony-published PlayStation games, some made by the same developer.
3. **Weak spots:** duplicate titles, the input game being returned, and the *Ubisoft* fill for missing developers all reduce the quality of the list. See [Challenges and Solutions](#challenges-and-solutions).

</details>

<details>
<summary><b>Model Details</b></summary>
<br>

- **Approach:** content-based filtering, with no user ratings or training step
- **Features:** *Name* + *Platform* + *Genre* + *Developer* + *Rating*, combined into one text string per game
- **Vectorizer:** `TfidfVectorizer` with default settings
- **Similarity:** `cosine_similarity` across all games (16,928 × 16,928 matrix)
- **Input matching:** `difflib.get_close_matches` on game titles
- **Output:** the 5 games with the highest similarity to the matched title

</details>

<p align="right">(<a href="#top">back to top</a>)</p>

---

## <ins>Credits and Contact</ins>

**Acknowledgements:** Dataset provided as part of the Machine Learning course at Bina Nusantara University.

**License:** No license. Personal use only.

**Author:** **Hansel Sebastian**, *Data Science Student @ Bina Nusantara University*

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/hanhansel)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/hansel-sebastian-5a19b8245)

<p align="right">(<a href="#top">back to top</a>)</p>
