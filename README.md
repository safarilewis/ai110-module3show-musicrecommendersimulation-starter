# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

This project simulates a simple content-based music recommender. Real platforms like Spotify, TikTok, and YouTube combine many signals, including listening history, likes, skips, watch time, playlists, and what similar users enjoyed, to predict what a person might want next. My version keeps the idea small and transparent by focusing on song attributes inside the dataset, then comparing them against a user taste profile to rank songs that best match the user's vibe.

---

## How The System Works

Real-world recommenders usually mix two big approaches. Collaborative filtering looks at behavior patterns across many users, such as "people who liked this also liked that," while content-based filtering looks directly at the item's attributes, such as genre, mood, energy, tempo, or other audio features. My simulator uses the content-based approach because it is easier to explain: each song is treated like a bundle of features, each user profile stores target preferences, and the recommender calculates a weighted score for every song before sorting the catalog from highest to lowest.

This version will prioritize genre and mood as the strongest categorical signals, then use energy as a numeric "closeness" score. Instead of rewarding songs for having higher energy in general, the system rewards songs that are closer to the user's target energy. That matters because a user who wants chill lofi should not get high-energy rock just because the energy value is large. After every song gets a score, a ranking rule sorts the list so the top `k` songs become the recommendations.

### Features Used

`Song` features:

- `title`
- `artist`
- `genre`
- `mood`
- `energy`
- `tempo_bpm`
- `valence`
- `danceability`
- `acousticness`

`UserProfile` features:

- `favorite_genre`
- `favorite_mood`
- `target_energy`
- `likes_acoustic`

### Algorithm Recipe

- Add strong points for a genre match because genre usually shapes the overall listening intent.
- Add smaller points for a mood match because songs can still feel right even across neighboring moods.
- Add similarity points for energy based on how close the song's energy is to the user's target.
- Optionally reward or penalize acousticness depending on whether the user prefers more acoustic songs.
- Sort songs by total score from highest to lowest and return the top results.

### Why We Need Scoring and Ranking

- A scoring rule explains how one song is judged against one user profile.
- A ranking rule compares all scored songs against each other so the system can choose the best recommendations.

Without scoring, the system has no way to judge a single song. Without ranking, it has no way to turn many scored songs into a final recommendation list.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this


---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card - Music Recommender Simulation

## 1. Model Name

Give your recommender a name, for example:

> VibeFinder 1.0

---

## 2. Intended Use

- What is this system trying to do
- Who is it for

Example:

> This model suggests 3 to 5 songs from a small catalog based on a user's preferred genre, mood, and energy level. It is for classroom exploration only, not for real users.

---

## 3. How It Works (Short Explanation)

Describe your scoring logic in plain language.

- What features of each song does it consider
- What information about the user does it use
- How does it turn those into a number

Try to avoid code in this section, treat it like an explanation to a non programmer.

---

## 4. Data

Describe your dataset.

- How many songs are in `data/songs.csv`
- Did you add or remove any songs
- What kinds of genres or moods are represented
- Whose taste does this data mostly reflect

---

## 5. Strengths

Where does your recommender work well

You can think about:
- Situations where the top results "felt right"
- Particular user profiles it served well
- Simplicity or transparency benefits

---

## 6. Limitations and Bias

Where does your recommender struggle

Some prompts:
- Does it ignore some genres or moods
- Does it treat all users as if they have the same taste shape
- Is it biased toward high energy or one genre by default
- How could this be unfair if used in a real product

---

## 7. Evaluation

How did you check your system

Examples:
- You tried multiple user profiles and wrote down whether the results matched your expectations
- You compared your simulation to what a real app like Spotify or YouTube tends to recommend
- You wrote tests for your scoring logic

You do not need a numeric metric, but if you used one, explain what it measures.

---

## 8. Future Work

If you had more time, how would you improve this recommender

Examples:

- Add support for multiple users and "group vibe" recommendations
- Balance diversity of songs instead of always picking the closest match
- Use more features, like tempo ranges or lyric themes

---

## 9. Personal Reflection

A few sentences about what you learned:

- What surprised you about how your system behaved
- How did building this change how you think about real music recommenders
- Where do you think human judgment still matters, even if the model seems "smart"
