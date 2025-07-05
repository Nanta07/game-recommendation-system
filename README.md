## 🕹️ Game Recommendation System

This repository contains a Machine Learning project aimed at building a **Game Recommendation System** that helps users discover video games that match their preferences, using both **Content-Based Filtering** and **Collaborative Filtering** approaches.

### 📌 Project Overview

With the growing number of video games released every year, users often struggle to find titles that truly match their interests. This project leverages Machine Learning techniques to build an intelligent recommendation system that enhances user experience on game distribution platforms.

### 🎯 Objectives

* Develop a **Content-Based Filtering (CBF)** model using game metadata and cosine similarity.
* Build a **Collaborative Filtering (CF)** model using the **LightFM** library.
* Evaluate model performance using **Precision\@k** and **Recall\@k**.
* Provide interactive recommendations based on user input.

### 📚 Dataset

* **Source**: [Kaggle - Video Game Sales with Ratings](https://www.kaggle.com/datasets/rush4ratio/video-game-sales-with-ratings)
* Contains game metadata including genre, platform, publisher, rating, and user scores.

### 💡 Why This Project Matters

* Helps users find relevant games quickly and accurately.
* Improves engagement and satisfaction on gaming platforms.
* Supports publishers and developers in reaching target audiences.
* Informs product and marketing strategies through user insights.

### 🛠️ Tech Stack

* **Python**
* **Jupyter Notebook**
* **LightFM**
* **Scikit-learn**
* **Pandas & Numpy**

### 🗂️ Project Structure

```
game-recommendation-system/
│
├── notebooks/             # Jupyter Notebook for experimentation and visualization
│   └── recommendation-system_Ananta_Boemi_Adji.ipynb
│
├── src/                   # Core Python scripts for preprocessing and modeling
│   └── recommendation-system_Ananta_Boemi_Adji.py
│
├── laporan/               # Project report and documentation
│   └── laporan_recommend-system_AnantaBoemiAdji.md
│
└── README.md              # Project overview and usage instructions
```

### 🚀 Getting Started

1. Clone this repository.
2. Install the required packages listed in `requirements.txt`.
3. Run the notebook or script to generate recommendations.

Note: The current project is still written in Bahasa Indonesia. An updated version in English will be provided soon.
