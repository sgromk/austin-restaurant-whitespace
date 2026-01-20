# Unstructured Data Analytics: Austin Restaurant Whitespace
### Team Members: Gabriel Kinshuk, Carol Le, Vyshnavi Maringanti, Niladri Nath, Radha Pawar

This repository contains the analytical framework and implementation code for identifying underserved market segments in the Austin restaurant sector. The project analyzes restaurant visual presentation, captured through unstructured image data, as a leading indicator of consumer sentiment and market positioning. 

By transforming thousands of Google Maps photos into semantic labels, we evaluate whether high-rated establishments share distinct visual characteristics that differentiate them from lower-performing competitors.

---

## Project Overview

We employ a computer vision to natural language processing pipeline to bridge the gap between unstructured visual data and structured statistical analysis. This study addresses the limitations of traditional market research by looking beyond metadata such as price and cuisine into the physical environment and aesthetic attributes of the venues.

### Analytical Framework
The analysis identifies three primary technical challenges in restaurant market analysis:
* **Search Bias**: Traditional API searches favor top-ranked results. We bypass this using a coordinate-based grid search to ensure geographic equity and capture comprehensive market data.
* **Semantic Mapping**: Converting raw pixel data into a bag-of-labels using the Google Cloud Vision API to allow for mathematical comparison of atmospheres.
* **The Consistency Hypothesis**: Testing if high-rated restaurants (Top 25%) exhibit higher visual coherence compared to the high-variance bottom quartile.

---

## Methodology and Pipeline

### 1. Spatial Data Acquisition
Instead of keyword-based queries, we implemented a coordinate-based grid search across the Austin metropolitan area.
* **Mechanism**: A custom class generates a lattice of latitude and longitude nodes.
* **Objective**: To capture a dataset of approximately 2,000 unique restaurants, neutralizing the impact of Google's recommendation algorithms.

### 2. Feature Extraction
Using the Google Cloud Vision API, we extracted high-level semantic labels from restaurant photo sets. This process transforms an image into a vector of descriptive terms such as industrial design, plating, or ambient lighting.

### 3. Statistical Analysis
To isolate which visual features drive consumer sentiment, we adapted text-mining techniques for image labels.

* **TF-IDF Scoring**: Used to penalize ubiquitous labels like "food" and highlight distinctive ones such as "craft cocktail."
* **Lift Analysis**: We calculate the probability of a label appearing in a high-rated restaurant versus a low-rated one:

$$Lift = \frac{P(\text{Label} \mid \text{High Rating})}{P(\text{Label} \mid \text{Low Rating})}$$

---

## Technical Evaluation

The project evaluates the visual coherence of restaurant groups using Cosine Similarity. We represent each restaurant as a TF-IDF vector $A$ and $B$, then measure the angle between them to determine aesthetic alignment:

$$\text{similarity} = \cos(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}$$

### Performance Metrics and Validation

| Metric | Target | Hypothesis | Finding |
| :--- | :--- | :--- | :--- |
| **Label Lift** | Top Quartile | Predictive visual cues exist. | High-rated spots share unique labels like plating and interior design. |
| **Cosine Similarity**| Consistency | High-rated spots show higher similarity. | **Statistically Significant**: Top 25% show higher visual synergy. |
| **Geographic Density**| Coverage | Grid search avoids bias. | Captured 40% more peripheral data points than keyword search. |

---

## Repository Structure

- **`austin_reviews.ipynb`**: The data engine. Handles grid generation, API rate limiting, and automated data flattening.
- **`image_label_analysis.ipynb`**: The statistical core. Implements the TF-IDF vectorizer, Cosine Similarity matrix, and hypothesis testing.
- **`data/`**: 
    - `austin_restaurants_full.csv`: Raw API output from the spatial grid search.
    - `restaurants_with_labels.csv`: The primary dataset post-Vision API processing.
    - `ATX_restaurants_cuisine_nonchain.csv`: Supplemental dataset for cuisine-specific analysis, excluding major chains to focus on local market variations.

---

## Key Findings

* **Visual Consistency**: High-rated restaurants are more visually consistent. Our t-test confirmed that the top 25% have a significantly higher internal cosine similarity than the bottom 25%, suggesting that successful establishments follow specific aesthetic patterns.
* **Market Gap Identification**: By mapping high-lift labels that are underrepresented in specific Austin neighborhoods, we can identify market opportunities where a specific visual style is missing despite high potential demand.
* **Differentiating Factors**: Basic food-related labels yielded low TF-IDF scores, suggesting that food quality is a baseline requirement, whereas environmental and atmospheric labels are the primary differentiators for high-rated status.