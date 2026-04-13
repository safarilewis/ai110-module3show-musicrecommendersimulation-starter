# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name

**NextPlay CLI 1.0**

---

## 2. Intended Use

This recommender is being developed for a small music company that wants to understand how platforms like Spotify and TikTok predict what to play next. The system suggests songs from a small catalog based on a user's preferred genre, mood, energy level, and whether they like acoustic music. Its main purpose is to simulate and explain the recommendation process in a simple, transparent way so the team can study how user preferences and song features turn into ranked suggestions. It is useful as an internal prototype and learning tool, but it is still too small and simplified for production use with real listeners.

---

## 3. How the Model Works

The model uses a content-based scoring system. Each song has features like genre, mood, energy, tempo, valence, danceability, and acousticness. The user profile stores a favorite genre, favorite mood, target energy value, and a simple acoustic preference. The recommender gives the biggest bonus for a genre match, a smaller bonus for a mood match, and then adds points when the song's energy is close to the user's target energy. It also adds a small bonus or penalty depending on whether the user prefers more acoustic songs. After every song gets a score, the system sorts the list from highest to lowest and returns the top results.

---

## 4. Data

The dataset contains 18 songs in `data/songs.csv`. I started with the 10-song starter catalog and expanded it with 8 more songs to cover more genres and moods, including EDM, folk, blues, metal, acoustic, reggaeton, and world. The data includes categorical features like genre and mood plus numeric features like energy, tempo, valence, danceability, and acousticness. Even after expansion, the dataset is still very small and hand-written, so it reflects a narrow and simplified view of musical taste.

---

## 5. Strengths

The recommender works well when the user's taste is clear and consistent. The chill lofi profile ranked `Library Rain` and `Midnight Coding` at the top, which matches the profile's low energy, chill mood, and acoustic preference. The deep intense rock profile also behaved well because `Storm Runner` clearly matched the rock, intense, and high-energy combination. Another strength is transparency: every recommendation comes with a score and reasons, so it is easy to explain why a song ranked highly.

---

## 6. Limitations and Bias

One weakness I noticed is that the system can over-reward exact category matches even when the rest of the vibe is not perfect. In the edge-case profile with `favorite_genre="ambient"`, `favorite_mood="sad"`, and `target_energy=0.9`, `Spacewalk Thoughts` ranked first mostly because it matched the ambient genre and acoustic preference, even though its energy was far from the requested high-energy target and its mood was `chill`, not `sad`. This shows a bias created by the hand-picked weights: exact genre matches can overpower more subtle mismatch signals. The system also has a filter-bubble risk because the catalog is tiny and some moods like `sad` are barely represented, so users with unusual or conflicting preferences get weak recommendations instead of truly good ones.

---

## 7. Evaluation

I tested four profiles:

- `High-Energy Pop`
- `Chill Lofi`
- `Deep Intense Rock`
- `Edge Case: Sad but Energetic`

I looked at whether the top 5 results felt musically reasonable and whether the reasons matched the ranking. The main profiles behaved well. `Sunrise City` ranked first for high-energy pop because it matched genre, mood, and energy very closely. `Library Rain` and `Midnight Coding` rose to the top for chill lofi because they matched both category features and stayed close to the low target energy. `Storm Runner` ranked first for deep intense rock, which also matched my expectations.

The biggest surprise came from the edge-case profile. Because the dataset has no truly sad, high-energy ambient song, the recommender stitched together partial matches. `Spacewalk Thoughts` won mainly from genre plus acousticness, while several intense high-energy songs appeared below it because they matched energy but not genre. That result made sense mathematically, but it did not fully feel right as a recommendation.

I also ran one weight-shift experiment: I cut the genre weight in half and doubled the energy weight. For the high-energy pop profile, this moved `Rooftop Lights` above `Gym Hero`. That change made the results more energy-sensitive and slightly more flexible across genres, but it also made the model less loyal to the user's stated favorite genre. The experiment showed that the recommendations were not just different because of the songs; they were strongly shaped by the scoring weights.

---

## 8. Future Work

- Add more songs and more balanced coverage across moods so edge-case users are not forced into weak matches.
- Use more than one numeric vibe feature in scoring, especially valence and danceability, so the system can distinguish songs that have similar energy but very different emotional feel.
- Add a diversity rule so the top 5 recommendations are not all near-duplicates of the same genre or style.
- Let users express multiple favorite genres or mixed moods instead of assuming taste is always one clear category.

---

## 9. Personal Reflection

This project made me see how quickly a simple set of rules can start to feel like a real recommender. Even with only a few features, the top results often looked believable because the system was consistent about what it rewarded. At the same time, building it made me more aware that "smart" behavior can come from simple math plus a carefully chosen dataset, not necessarily from deep understanding.

The most interesting part was seeing how much the hand-picked weights changed the story. When I doubled the energy weight and cut the genre weight, the recommender immediately started valuing cross-genre energy matches more. That reminded me that recommendation systems are not neutral. Human judgment still matters a lot because someone has to decide what counts as a "good" match in the first place.
