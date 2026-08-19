# Online Courses Analysis — Power BI Dashboard

An interactive Power BI dashboard analyzing 2,693 online courses scraped from Coursera, exploring course popularity, category performance, instructor ratings, languages, and in-demand skills across the e-learning market.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data%20Analysis-blue)
![Power Query](https://img.shields.io/badge/Power%20Query-M%20Language-green)

## 📊 Overview

This project transforms a raw Coursera course-listing dataset into a two-page executive dashboard that answers:
- Which course categories and formats are most popular?
- How does viewership vary by language, sub-category, and course duration?
- Which instructors are top-rated, and which skills are most in demand?
- How does subtitle availability relate to viewership?

## 🗂️ Dataset

- **Source:** Coursera online course listings (`Online_Courses.csv`)
- **Records:** 2,693 courses (after cleaning) across **11 categories** and **46 sub-categories**
- **Languages covered:** 7 (English, Spanish, French, Japanese, Chinese (Simplified), Russian, Portuguese (Brazilian))
- **Data model:** Star-style model with two tables
  - `Online_Courses` — course-level facts (title, category, language, rating, viewers, duration, skills)
  - `Teachers` — unpivoted instructor-per-course table, related to `Online_Courses` on `Course Id` (many-to-one)

## 🧹 Data Preparation (Power Query / M)

- Removed duplicate rows, blank rows, and parsing errors from the raw CSV (45 → 16 relevant columns)
- Extracted numeric `Rating` from text (e.g., `"4.7 stars"` → `4.7`)
- Parsed free-text `Duration` (hours / weeks / months) into a standardized `Duration in Hours` column
- Derived `Subtitle_language_count` and `Count of Skills Provided.` from comma-separated text fields
- Unpivoted a multi-column instructor list into a normalized `Teachers` table (one row per instructor per course)

## 🧮 Key DAX Measures

```dax
Rank_categiry_by_avg_views =
IF(
    RANKX(ALL(Online_Courses[Category]), CALCULATE(AVERAGE(Online_Courses[Number of viewers]))) < 6,
    CALCULATE(AVERAGE(Online_Courses[Number of viewers])),
    BLANK()
)

Instructor_rating =
IF(
    RANKX(ALL(Teachers[Instructors.]), CALCULATE(AVERAGE(Teachers[Rating]))) <= 3,
    CALCULATE(AVERAGE(Teachers[Rating])),
    BLANK()
)
```

## 📈 Dashboard Pages

**Page 1 — Main Analysis**
| Visual | Insight |
|---|---|
| Bar Chart | Course Type Popularity (Course vs. Specialization vs. Professional Certificate) |
| Column Chart | Courses by Language and Sub-Category |
| Word Cloud | Most in-demand Skills |
| Pie Chart | Most prominent Languages |
| Pivot Table | Top 5 Categories ranked by average viewers |
| Line Chart | Viewers vs. Subtitle Count |
| Clustered Column | Top-rated Instructors |
| Line Chart | Viewership vs. Lecture Duration |
| Pivot Table | Skills Provided & Duration by Category / Sub-Category |
| Slicers | Filter by Category, Sub-Category |

**Page 2** — Detail table view

## 💡 Key Insights

- **Business dominates volume:** Business is the largest category (871 courses, 32%), followed by Computer Science (434) and Data Science (421).
- **Data Science drives engagement:** Despite lower course counts, Data Science courses average ~6,380 viewers per course — nearly 2x the overall average (~3,240) and the highest of any category.
- **English-first market:** 97% of courses (2,621 of 2,693) are in English; Spanish is a distant second at 52 courses.
- **Course > Specialization:** Standalone Courses make up 86% of listings (2,307), vs. 14% Specializations and a handful of Professional Certificates.
- **Quality is consistently high:** Overall average rating is 4.66/5, with Social Sciences (4.77), Language Learning (4.75), and Health (4.72) rating highest.
- **Subtitles correlate with reach:** Number of subtitle languages shows a mild positive correlation with viewership (r ≈ 0.23), more than duration (r ≈ 0.04) or rating (r ≈ 0.10) — accessibility matters more than length or star rating for reach.
- **Top in-demand skills:** Data Analysis, Python Programming, Machine Learning, Communication, and Data Science lead the skills word cloud.

## 🛠️ Tools & Skills Demonstrated

- Power BI Desktop (data modeling, report design)
- Power Query (M) for ETL and text/data cleaning
- DAX for ranking and dynamic top-N measures
- Data storytelling with bar, line, pie, word cloud, and pivot table visuals
- Star-schema relationship modeling (fact + unpivoted dimension table)

## 🚀 How to Use

1. Clone/download this repo
2. Open `Edtec_Analysis_Final.pbix` in Power BI Desktop
3. Explore filters via the Category and Sub-Category slicers on Page 1

