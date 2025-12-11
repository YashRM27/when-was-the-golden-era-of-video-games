# Video Game Sales & Ratings Analysis

### Identifying Industry Trends, Best-Selling Games, and the Golden Years of Gaming

This project explores the relationship between video game sales, critic ratings, and user ratings using a curated dataset of the top 400 games released since 1977. The objective is to determine whether a “golden age” of video games exists by analyzing release years that performed exceptionally well according to both critics and users. Additionally, the project examines the commercial side of gaming by ranking the best-selling video games.

The project was completed inside a DataCamp notebook using SQL and Python, and the outputs are stored as DataFrames:

* `best_selling_games`
* `critics_top_ten_years`
* `golden_years`

---

## 📂 Dataset Overview

The project uses four tables:

### **1. game_sales**

Contains metadata and sales data for the top 400 games.
Columns:

* `name`
* `platform`
* `publisher`
* `developer`
* `games_sold` (in millions)
* `year`

### **2. reviews**

Contains critic and user ratings from Metacritic.
Columns:

* `name`
* `critic_score`
* `user_score`

### **3. users_avg_year_rating**

Pre-aggregated user rating statistics.
Columns:

* `year`
* `num_games`
* `avg_user_score`

### **4. critics_avg_year_rating**

Pre-aggregated critic rating statistics.
Columns:

* `year`
* `num_games`
* `avg_critic_score`

---

## 🎯 Project Goals

1. **Identify the ten best-selling video games**
2. **Find the ten strongest years according to critic scores** (with sample-size filtering)
3. **Detect years where critics and users broadly agreed games were highly rated (avg score > 9)**
4. **Study trends that could indicate a golden era of gaming**

---

## 🧠 SQL Queries Used

### **1. Ten Best-Selling Games (`best_selling_games`)**

```sql
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```

---

### **2. Top Ten Years by Critic Rating (`critics_top_ten_years`)**

```sql
SELECT 
    year,
    COUNT(name) AS num_games,
    ROUND(AVG(critic_score), 2) AS avg_critic_score
FROM game_sales AS gs
JOIN reviews AS r
    ON gs.name = r.name
GROUP BY year
HAVING COUNT(name) >= 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```

---

### **3. Golden Years – When Critics & Users Agreed (`golden_years`)**

```sql
SELECT 
    u.year,
    u.num_games,
    c.avg_critic_score,
    u.avg_user_score,
    (c.avg_critic_score - u.avg_user_score) AS diff
FROM users_avg_year_rating AS u
JOIN critics_avg_year_rating AS c
    ON u.year = c.year
WHERE c.avg_critic_score > 9 OR u.avg_user_score > 9
ORDER BY u.year ASC;
```

---

## 📊 Key Insights

* Several iconic years stood out, including **1998**, often considered one of the strongest years in gaming history.
* The commercial market continues to be dominated by Nintendo titles and modern PC hits.
* Golden years are characterized by strong alignment between critics and users, indicating universally well-received releases.
* Sales performance and critical performance do not always correlate, offering interesting contrasts.

---

## 🛠️ Tools and Technologies

* **SQL** (aggregation, joins, filtering, sorting)
* **Python** (DataFrame handling and analysis in DataCamp)
* **DataCamp Workspace**
* **GitHub** (version control and sharing)

---

## 📁 Repository Structure

```
├── README.md
├── notebook.ipynb   # Full DataCamp exported notebook
├── assets/          # Images or exports (optional)
└── results/         # CSV or JSON outputs (optional)
```

---

## 🚀 How to Use This Project

1. Open the notebook to view SQL queries and analysis.
2. Explore the DataFrames generated (`best_selling_games`, `critics_top_ten_years`, `golden_years`).
3. Extend the analysis by applying additional business questions or visualizations.

---

## 📎 Credits

* Dataset adapted from Kaggle’s video game dataset.
* Analysis completed in DataCamp’s DataLab environment.

---
