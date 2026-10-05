# DSA 4060 – Recommender Systems
## Week 3 Practical Lab: Building a Simple Content-Based Recommender

## Project Overview

This project implements a simple **content-based movie recommender system** using structured movie features and user ratings.

Movies are represented using binary genre features, while user ratings are used to construct a preference profile. The recommender compares the user's learned preferences with the features of each movie, calculates recommendation scores, removes movies the user has already watched, and returns the highest-ranked unseen movies.

The project demonstrates the basic workflow of content-based filtering without using TF-IDF or cosine similarity.

---

## Lab Objectives

The objectives of this practical are to:

- Represent movies using structured item features.
- Create and interpret an item-feature matrix.
- Build a user preference profile from ratings.
- Weight movie features using user ratings.
- Calculate recommendation scores for candidate movies.
- Remove movies already watched by the user.
- Generate Top-N recommendations.
- Compare recommendations for different users.
- Examine the limitations of a basic content-based recommender.

---

## Dataset

The initial dataset contains **10 movies** represented using five structured genre features:

- Action
- Comedy
- Drama
- Romance
- SciFi

Each genre is represented using binary encoding:

- `1` = genre/feature is present
- `0` = genre/feature is absent

For example, **Galaxy Mission** has both Action and SciFi features.

The main dataset columns are:

| Column | Description |
|---|---|
| `movie_id` | Unique identifier for each movie |
| `title` | Movie title |
| `Action` | Action genre indicator |
| `Comedy` | Comedy genre indicator |
| `Drama` | Drama genre indicator |
| `Romance` | Romance genre indicator |
| `SciFi` | Science-fiction genre indicator |

---

## Content-Based Filtering Workflow

The recommender follows this workflow:

**Item Metadata → Item Profiles → User Ratings → User Profile → Candidate Scores → Remove Seen Items → Rank Items → Top-N Recommendations**

### 1. Item Profiles

Each movie is represented by its five binary genre features.

For example:

`Galaxy Mission = [1, 0, 0, 0, 1]`

This indicates that Galaxy Mission contains **Action** and **SciFi** features.

### 2. User Ratings

User ratings are joined with the corresponding movie features.

### 3. Rating-Weighted Features

Each rated movie's features are multiplied by the rating given by the user.

This allows highly rated movies to contribute more strongly to the user's preference profile.

### 4. User Profile

The weighted features are summed to create a user preference profile.

For User A, the two strongest preferences were:

- **Action**
- **SciFi**

### 5. Profile Normalization

The user profile is normalized into proportions to make the feature weights easier to interpret.

### 6. Recommendation Scoring

Each movie receives a recommendation score based on how strongly its features match the user's normalized preference profile.

### 7. Remove Watched Movies

Movies already rated by the user are removed from the candidate recommendations.

### 8. Top-N Recommendations

The remaining movies are ranked from highest to lowest recommendation score.

---

## User A

User A rated four movies:

| Movie | Rating |
|---|---:|
| Galaxy Mission | 5 |
| Love in Nairobi | 2 |
| Final Battle | 4 |
| Future World | 5 |

The resulting profile showed that **Action and SciFi** were the user's strongest preferences.

### User A Top Recommendations

The recommender identified movies with features that most closely matched User A's preference profile.

**Space Warriors** ranked highly because it contains both Action and SciFi, while **City Detectives** matched the user's strong preference for Action.

---

## Testing a Second User

A second user was created with different ratings:

| Movie ID | Rating |
|---:|---:|
| 2 | 5 |
| 4 | 4 |
| 6 | 5 |
| 9 | 4 |

User A and User B do not necessarily receive the same recommendations because their ratings produce different feature weights and therefore different user profiles.

This demonstrates that ratings influence personalization even when the same structured movie features are used.

---

## Custom Movie

A unique movie was added to extend the original dataset:

### Nairobi After Dark

| Feature | Value |
|---|---:|
| Action | 1 |
| Comedy | 0 |
| Drama | 1 |
| Romance | 0 |
| SciFi | 1 |

The movie therefore combines **Action, Drama, and SciFi**.

After adding the custom movie, the updated dataset is saved to `movies.csv`.

---

## Custom User Profile

A custom user was created using five movie ratings:

| Movie ID | Rating |
|---:|---:|
| 1 | 5 |
| 2 | 2 |
| 6 | 3 |
| 8 | 5 |
| 10 | 2 |

The custom user's strongest preferences were **Action and SciFi**.

---

## Custom User – Top 5 Recommendations

The recommender produced the following Top 5 recommendations:

| Rank | Movie | Recommendation Score |
|---:|---|---:|
| 1 | Nairobi After Dark | 0.84375 |
| 2 | Future World | 0.62500 |
| 3 | City Detectives | 0.53125 |
| 4 | Final Battle | 0.31250 |
| 5 | Wedding Weekend | 0.15625 |

### Interpretation

**Nairobi After Dark** ranked first with a recommendation score of **0.84375** because its Action, Drama, and SciFi features strongly overlap with the user's learned preference profile.

**Future World** ranked second because its Action and SciFi features match the user's two strongest preferences.

**City Detectives** ranked third because it contains Action and Drama, giving it a strong partial match with the user's profile.

---

## Reusable Recommendation Function

The notebook implements a reusable function:

```python
recommend_movies(movies, ratings, features, n=3)
```

The function:

1. Joins ratings with movie features.
2. Extracts the rated movie features.
3. Weights features using ratings.
4. Builds and normalizes the user profile.
5. Calculates recommendation scores.
6. Removes movies already watched.
7. Sorts candidate movies by recommendation score.
8. Returns the Top-N recommendations.

This makes it possible to generate recommendations for different users without repeating the entire calculation manually.

---

## Cold-Start Example

The notebook also considers a user who has rated only **Galaxy Mission**.

Because Galaxy Mission contains Action and SciFi, these two genres would dominate the user's profile.

However, one rating provides very limited information about a user's overall preferences. This can lead to overly narrow or inaccurate recommendations.

This illustrates the **new-user/cold-start problem**.

---

## Limitations

The recommender has several limitations:

1. **Limited features**  
   Only five genre features are used to describe movies.

2. **Missing metadata**  
   The model ignores actors, directors, release year, language, descriptions, and other potentially useful information.

3. **Cold-start problem**  
   Users with very few or no ratings provide insufficient information for building reliable preference profiles.

4. **Overspecialization**  
   Content-based systems can repeatedly recommend movies that are very similar to those the user already likes.

5. **Metadata dependency**  
   Poor or incomplete movie metadata can result in poor recommendations.

6. **Simple preference modelling**  
   The model uses weighted genre features and does not capture more complex relationships between movies and user interests.

---

## Technologies Used

- Python
- pandas
- Jupyter Notebook
- Git
- GitHub

---

## Repository Structure

```text
DSA4060-Week3-StudentID/
│
├── content_based_lab.ipynb
├── movies.csv
└── README.md
```

### Files

- **`content_based_lab.ipynb`** – Complete implementation, experiments, interpretations, and answers.
- **`movies.csv`** – Movie dataset including the custom movie.
- **`README.md`** – Project documentation and summary.

---

## How to Run the Project

### 1. Clone the repository

```bash
git clone YOUR_REPOSITORY_URL
```

### 2. Open the project directory

```bash
cd DSA4060-Week3-StudentID
```

### 3. Start Jupyter Notebook

```bash
jupyter notebook
```

### 4. Open

```text
content_based_lab.ipynb
```

Run the notebook cells from top to bottom.

### Requirement

The notebook requires the pandas library:

```bash
pip install pandas
```

---

## Key Findings

The practical demonstrates that:

- Structured movie metadata can be used to represent item profiles.
- User ratings can be used to learn feature preferences.
- Higher ratings give greater weight to the corresponding movie features.
- Different rating patterns can produce different recommendations.
- Already watched movies should be excluded from recommendation candidates.
- Content-based filtering can provide personalized recommendations without requiring ratings from other users.

---

## Final Question

**If two users have watched exactly the same movies but gave them different ratings, should this recommender produce the same recommendations for both users?**

No. The ratings are used to weight the features of the movies when constructing each user's preference profile. If two users assign different ratings to the same movies, their feature weights can differ.

Therefore, the users can have different preference profiles, resulting in different recommendation scores and rankings even though they watched exactly the same movies.

---

## Conclusion

This practical successfully demonstrates the implementation of a simple content-based recommender using structured movie features and user ratings.

The system constructs item profiles, learns user preferences from rating-weighted features, calculates recommendation scores, removes previously watched movies, and generates personalized Top-N recommendations.

Testing multiple users demonstrates how differences in ratings affect the resulting user profiles and recommendations. The project also highlights important limitations of basic content-based filtering, including limited metadata, overspecialization, and the cold-start problem.