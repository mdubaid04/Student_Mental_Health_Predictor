<div align="center">

# 🧠 Student Mental Health Predictor

**A machine learning web app that reads your daily habits — screen time, sleep, study load, and stress — and predicts a mental wellness score out of 10.**

[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen)](https://mental-health-predict.netlify.app/)
[![API](https://img.shields.io/badge/API-FastAPI-009688)](https://student-mental-health-predictor-21ox.onrender.com)
[![Model](https://img.shields.io/badge/model-RandomForest-orange)](#-model-details)

[🌐 Live App](https://mental-health-predict.netlify.app/) · [⚙️ API](https://student-mental-health-predictor-21ox.onrender.com) · [📓 Notebook](#-model-details)

</div>

---

> ⚠️ **Disclaimer**
> This project is built for learning and informational purposes only. It is **not a clinical or diagnostic tool**, and the prediction should never be treated as medical advice. If you're struggling, please reach out to someone you trust or a mental health professional.

---

## 📖 About

Students today juggle academics, screens, and sleepless nights — and it quietly adds up. This project explores whether everyday, easily-reportable habits (study hours, sleep, physical activity, social media usage, and perceived stress) can meaningfully predict a student's overall mental wellness.

It's a complete, end-to-end ML application — from data preprocessing and model training in a Jupyter notebook, to a trained model served through a FastAPI backend, to a live, interactive frontend anyone can use.

---

## 🔗 Live Links

| Layer | Link | Notes |
|---|---|---|
| 🌐 **Frontend** | [mental-health-predict.netlify.app](https://mental-health-predict.netlify.app/) | Hosted on Netlify |
| ⚙️ **Backend API** | [student-mental-health-predictor-21ox.onrender.com](https://student-mental-health-predictor-21ox.onrender.com) | Hosted on Render (free tier) |

> 💤 The backend sleeps after inactivity on Render's free tier — the first request after a while may take 20–30 seconds to wake it up. Please be patient on first load.

---

## ✨ Features

- 🧾 Clean, single-page form covering profile, digital habits, and lifestyle factors
- ⚡ Real-time prediction powered by a trained Random Forest model
- 🔌 Decoupled architecture — static frontend talks to a REST API, so either side can evolve independently
- 📱 Responsive design, usable on both desktop and mobile
- 🧪 Fully reproducible model training, documented in the included notebook

---

## 🛠️ Tech Stack

**Frontend**
- HTML, CSS, JavaScript — no framework, kept lightweight
- Deployed on **Netlify**

**Backend**
- **FastAPI** (Python)
- **scikit-learn** — Random Forest Regressor wrapped in a preprocessing pipeline
- **joblib** for model serialization
- Deployed on **Render**

**Modeling**
- Pandas, NumPy for data handling
- Seaborn, Matplotlib for EDA
- scikit-learn `Pipeline` + `ColumnTransformer` for preprocessing
- `RandomizedSearchCV` for hyperparameter tuning

---

## ⚙️ How It Works

```
 ┌────────────┐        POST /predict         ┌──────────────┐        ┌────────────────────┐
 │  Frontend  │ ────────────────────────────▶ │   FastAPI    │ ─────▶ │  RF Pipeline (.pkl)  │
 │ (Netlify)  │ ◀──────────────────────────── │  (Render)    │ ◀───── │  scikit-learn model  │
 └────────────┘        { score: 7.2 }         └──────────────┘        └────────────────────┘
```

1. User fills in their profile and daily habits on the frontend
2. Form data is sent as a `POST` request to the FastAPI backend
3. The backend runs the input through the same preprocessing pipeline used during training, then the Random Forest model
4. A predicted **mental health score (0–10)** is returned and displayed instantly

---

## 📂 Project Structure

```
Student_Mental_Health_Predictor/
├── index.html                             # Main frontend page
├── style.css                              # Styling
├── script.js                              # Form handling + API calls
├── main.py                                # FastAPI app & /predict endpoint
├── Mental_health_model.pkl                # Trained Random Forest pipeline (joblib)
├── Student_Mental_State_Prediction.ipynb  # Data cleaning, EDA & model training
├── requirements.txt                       # Python dependencies
└── README.md
```

*(Update this structure if your actual repo layout differs.)*

---

## 🚀 Getting Started Locally

### 1. Clone the repo
```bash
git clone https://github.com/mdubaid04/Student_Mental_Health_Predictor.git
cd Student_Mental_Health_Predictor
```

### 2. Run the backend
```bash
pip install -r requirements.txt
uvicorn main:app --reload
```
FastAPI will start at `http://127.0.0.1:8000` — interactive API docs available at `http://127.0.0.1:8000/docs`.

### 3. Run the frontend
```bash
npx serve .
```
Update the API base URL inside `script.js` to point to `http://127.0.0.1:8000` while testing locally.

---

## 📊 Model Details

The model is trained on a **student social media & mental health dataset**, using the following inputs:

`Age`, `Gender`, `Country`, `Academic Level`, `Most-Used Platform`, `Purpose of Use`, `Avg. Daily Usage Hours`, `Daily Unlocks`, `Study Hours`, `Physical Activity Hours`, `Sleep Hours per Night`, `Stress Level`

**Target:** `Mental_Health_Score` (continuous, 0–10)

**Preprocessing pipeline** (`scikit-learn ColumnTransformer`)
| Step | Applied to |
|---|---|
| `log1p` transform + `StandardScaler` | Skewed feature: `Study_Hours` |
| `StandardScaler` | `Age`, `Avg_Daily_Usage_Hours`, `Daily_Unlocks`, `Physical_Activity_Hours`, `Sleep_Hours_Per_Night` |
| `OrdinalEncoder` | `Stress_Level` (Low → Very High) |
| `OneHotEncoder` | `Gender`, `Academic_Level`, `Most_Used_Platform`, `Purpose_Of_Use`, `Country` |

**Model:** `RandomForestRegressor`, tuned with `RandomizedSearchCV`

| Hyperparameter | Value |
|---|---|
| `n_estimators` | 200 |
| `max_depth` | 15 |
| `min_samples_split` | 5 |
| `min_samples_leaf` | 2 |
| `random_state` | 42 |

**Performance (R² Score)**

| Set | Score |
|---|---|
| Training | 0.955 |
| Testing | 0.865 |

The full training process — data cleaning, EDA, feature engineering, and model comparison — is documented in `Student_Mental_State_Prediction.ipynb`.

---

## 🙋 Author

Built by **Ubaid** — [@mdubaid04](https://github.com/mdubaid04)

Feel free to ⭐ the repo if you found it useful, or open an issue if you spot a bug!

---

## 📄 License

Open-source, built for learning purposes.
