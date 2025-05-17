# 📽️ Recommendation System on the KuaiRec Dataset

This project aims to build a video recommendation system using the [KuaiRec dataset](https://kuairec.github.io/), which contains rich interaction data between users and short videos. The model's goal is to predict the **watch ratio** of users on specific videos.

---

## 📊 1. Data Analysis

The dataset is **large and detailed**:

- **1.5 GB** interaction matrix for training.
- **10,728 videos**
- **7,176 users**

Key observations:

- The `items.csv` file contains a wide variety of features for each video (e.g., tags, duration, views, likes).
- The `users.csv` file already includes one-hot encoded categorical features.
- Memory management is critical due to the dataset size.

---

## 🛠️ 2. Data Processing

To prepare the data:

- **Watch Ratio:**
  - Removed outliers with watch ratios > 3.
  - Applied Min-Max normalization (to scale between 0 and 1).
- **Other Numerical Features:**
  - Applied outlier filtering.
  - Used log scaling or Min-Max normalization.
- **Categorical and One-Hot Vectors:**
  - Padded user one-hot features (some had `NaN` values).
  - Padded video tag sequences to length 31.

---

## 🧠 3. Feature Engineering

### User Features:

- Used the existing one-hot vectors.
- Did **not** include followers/followings for simplicity.

### Video Features:

- Included:
  - Duration
  - Author ID
  - Average watch ratio
  - Tags (IDs and strings)
- Excluded:
  - Views and likes (to avoid bias toward popularity).

> Some additional features were left out intentionally for future experimentation with weighting or additional sub-networks.

---

## 🤖 4. Models and Performance

### Tried Models:

| Model               | Result                         |
| ------------------- | ------------------------------ |
| ALS (Spark)         | Didn't scale well — too large. |
| Linear Regression   | Poor performance (baseline).   |
| **Two-Tower Model** | ✅ Best results so far.        |

### Two-Tower Model:

- Architecture: separate neural nets for users and videos.
- Output: predicted normalized watch ratio.
- Results:
  - **Loss**: `0.02`
  - **MAE**: `0.12`

---

## 🚀 5. Future Improvements

Planned steps to enhance the recommender:

1. ✅ Get ALS / matrix factorization working properly.
2. ✅ Extract embeddings from:
   - Two-tower model
   - Matrix factorization
3. 🔄 Combine them into a hybrid model (e.g., late fusion or embedding concat).
4. 🔬 Explore a **transformer-based model** that:
   - Encodes video sequences with positional encoding.
   - Uses attention mechanisms for sequential recommendation.

---

## ⚠️ 6. Challenges Faced

- ⚡ Very large datasets (memory & performance issues).
- 🔍 High number of outliers in interaction values.
- 🧮 Limited compute power.
- 🧠 Beginner mistakes (e.g., forgot feature normalization at first).
- 🤯 Feature overload — selecting relevant ones took time.

Despite the challenges, this project was a great opportunity to learn about recommender systems, machine learning pipelines, and handling real-world-scale data.

---

## 📬 Contact

If you have any questions or feedback, feel free to open an issue or contact me on GitHub!
