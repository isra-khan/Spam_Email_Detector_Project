# Spam Email Detection

This project classifies email text as **Spam** or **Ham** (legitimate email) using a trained machine-learning model. It includes a Streamlit web app for checking one email at a time or processing an MBOX archive, plus a separate pipeline for training and evaluating models.

## Features

- Classify pasted email text and view the prediction and confidence score.
- Process emails from an `.mbox` archive and download the results as a CSV.
- Train and compare Logistic Regression, Decision Tree, SVM, KNN, and Random Forest models.
- Save trained models, vectorizers, and evaluation results under `outputs/`.

## Requirements

- Python 3.13 or newer (as specified in `pyproject.toml`).
- The project dependencies listed in `requirements.txt`.
- The included model and vectorizer files under `outputs/` for the web app.

## Setup

Open a terminal in the project directory (the directory containing `app.py`), then create and activate a virtual environment.

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell**

```powershell
py -3 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Run the web app

From the project directory, with the virtual environment activated:

```bash
streamlit run app.py
```

Streamlit prints a local URL in the terminal, usually <http://localhost:8501>. Open that URL in your browser. If the port is already in use by another app, start this project on another port:

```bash
streamlit run app.py --server.port 8502
```

Then open <http://localhost:8502>.

### Classify one email

Choose **Single Email**, paste the email text into the text box, and select **Classify Email**. You do not need to upload a file for a single email.

### Process an email archive (optional)

Choose **Batch MBOX Processing** and upload an `.mbox` archive. The app classifies the messages in the archive and lets you download the results as a CSV.

An MBOX file is an email archive, not the training dataset. For example, Google Takeout can export Gmail mail as MBOX: request a Gmail export, download and extract the Takeout archive, then select the `.mbox` file inside it. A `.csv`, `.pst`, or individual `.eml` file is not an MBOX archive.

## Train the models (optional)

The web app uses the saved model and vectorizer paths in `src/config/config.py`. They are configured to use the included artifacts in `outputs/`, so you do not need to train the models just to run the app.

To retrain:

1. Make sure `data/dataset/dataset.csv` exists and has `Message` and `Category` columns. The supported category values are `spam` and `ham`.
2. From the project directory, run:

   ```bash
   python -m src.pipeline.training_pipeline
   ```

3. Training evaluates several models with cross-validation and writes artifacts and observations under a new timestamped directory in `outputs/`.
4. To use the newly trained model in the web app, update `model_path` and `feature_path` in `src/config/config.py` to point to the model and `vectorizer.pkl` in that new output directory.

Training can take considerably longer than starting the web app because it searches model parameters.

## Project layout

```text
app.py                       Streamlit web application
src/components/              Data ingestion, transformation, and model training
src/config/config.py         Dataset and saved model paths; model parameters
src/pipeline/                 Training and prediction pipelines
src/utils/                    Email processing, logging, and state helpers
data/dataset/                 Training dataset
outputs/                      Saved models, vectorizers, and evaluation results
```
