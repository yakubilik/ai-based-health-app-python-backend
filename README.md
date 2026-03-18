# AI-Based Health App - Python Backend

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.0-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Selenium](https://img.shields.io/badge/Selenium-Web%20Scraping-43B02A?logo=selenium&logoColor=white)](https://www.selenium.dev/)
[![Heroku](https://img.shields.io/badge/Heroku-Deployed-430098?logo=heroku&logoColor=white)](https://heroku.com/)

A multi-service Python backend for an AI-powered health application. The system provides **disease prediction** using a trained machine learning model and **drug information lookup** via barcode scanning.

## Features

- **Disease Prediction** - Predicts diseases from symptom data using a pre-trained Logistic Regression model (scikit-learn), with automatic Turkish translation of results.
- **Drug Information API** - Retrieves drug prospectus information by barcode number, scraping data from [ilacprospektusu.com](https://www.ilacprospektusu.com) using Selenium.
- **Heroku-Ready** - Both services include Procfiles for seamless Heroku deployment with Gunicorn.

## Project Structure

```
ai-based-health-app-python-backend/
├── diseases-prediction/
│   ├── app.py                 # Flask API for disease prediction
│   ├── finalized_model.sav    # Pre-trained scikit-learn model
│   ├── requirements.txt       # Python dependencies
│   └── Procfile               # Heroku deployment config
├── drugs-api/
│   ├── app.py                 # Flask API for drug info lookup
│   ├── requirements.txt       # Python dependencies
│   └── Procfile               # Heroku deployment config
├── .gitignore
├── LICENSE
└── README.md
```

## Tech Stack

| Component          | Technology                          |
|--------------------|-------------------------------------|
| Web Framework      | Flask                               |
| ML Model           | scikit-learn (Logistic Regression)   |
| Web Scraping       | Selenium + ChromeDriver             |
| Translation        | `translate` library                 |
| WSGI Server        | Gunicorn                            |
| Deployment         | Heroku                              |

## Installation & Setup

### Prerequisites

- Python 3.8+
- pip
- Google Chrome & ChromeDriver (for the drugs-api service)

### Disease Prediction Service

```bash
cd diseases-prediction
pip install -r requirements.txt
python app.py
```

The service will start on `http://localhost:5000`.

### Drug Information API

```bash
cd drugs-api
pip install -r requirements.txt
```

Set the required environment variables for Selenium (Heroku buildpack style):

```bash
export GOOGLE_CHROME_BIN=/usr/bin/google-chrome
export CHROMEDRIVER_PATH=/usr/bin/chromedriver
```

```bash
python app.py
```

The service will start on `http://localhost:5000`.

## API Endpoints

### Disease Prediction Service

| Method | Endpoint       | Description                        |
|--------|----------------|------------------------------------|
| GET    | `/`            | Health check / home page           |
| GET    | `/predict/`    | Predict disease from symptoms      |

**Predict Disease**

```
GET /predict/?predict=1,0,1,0,1,...
```

Pass a comma-separated list of symptom feature values as the `predict` query parameter. Returns the predicted disease name (translated to Turkish).

**Example Response:**

```json
{
  "Page": "Request",
  "Message": " USer Success loaded request page for <predicted_disease>",
  "TimeStamp": 1640000000.0
}
```

### Drug Information API

| Method | Endpoint | Description                              |
|--------|----------|------------------------------------------|
| POST   | `/`      | Look up drug prospectus info by barcode  |

**Look Up Drug Info**

```
POST /
Content-Type: application/json

{
  "barcode": "8699586090115"
}
```

**Example Response:**

```json
{
  "head": "Drug Name - Prospectus Title",
  "Value": ["Line 1 of prospectus", "Line 2 of prospectus", "..."]
}
```

## Deployment

Both services are configured for Heroku deployment. Each subdirectory is deployed as an independent Heroku app:

```bash
# Example for diseases-prediction
cd diseases-prediction
heroku create your-app-name
git push heroku main
```

> **Note:** The drugs-api service requires the Heroku buildpacks for Google Chrome and ChromeDriver.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
