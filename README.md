# Job Resume Skill Matching Fine-Tuned Model

## Project Overview

This project develops a machine-learning system that classifies how well a candidate's resume matches a job description.

The system predicts one of three categories:

- **Strong:** High resume–job match.
- **Moderate:** Partial resume–job match.
- **Weak:** Low resume–job match.

It compares a TF-IDF and Logistic Regression baseline with a fine-tuned DistilBERT model. It also explores class-weighted cross-entropy loss and provides an interactive Gradio demonstration.

**Important:** This is an educational project. Its predictions are experimental and must not be used as hiring decisions.

## Problem Statement

Matching resumes to job descriptions manually can be time-consuming. This project explores how natural language processing can help estimate resume–job compatibility and identify relevant skills that appear to be missing.

## Dataset

- **Source:** [Hugging Face – batuhanmtl/job_resume_fit](https://huggingface.co/datasets/batuhanmtl/job_resume_fit)
- **Working sample:** 1,000 resume–job pairs.
- **Input:** Resume text and job description text.
- **Target:** Weak, Moderate, or Strong.

The working sample is cleaned and split into:

| Split | Examples |
|---|---:|
| Training | 700 |
| Validation | 150 |
| Testing | 150 |
| **Total** | **1,000** |

The labels are derived from the dataset's match score using these project thresholds:

- Weak: score below 40.
- Moderate: score from 40 to below 70.
- Strong: score from 70 to 100.

The split is stratified to preserve class proportions.

**Data privacy:** The raw dataset is not included in this repository. Review the dataset's licensing and privacy conditions before downloading or redistributing it. Do not upload real personal resumes or identifiable candidate information.

## Models and Methodology

### 1. Baseline: TF-IDF + Logistic Regression

TF-IDF converts text into numerical features. Logistic Regression uses those features to classify the resume–job pair.

### 2. Fine-tuned model: DistilBERT

The project uses `distilbert-base-uncased`, a compact pretrained transformer model, with a three-class classification head.

The resume and job description are combined into one input sequence and tokenized with a maximum sequence length of 256.

### 3. Loss Function

The primary fine-tuning experiment uses multiclass cross-entropy loss:


L = -(1/N) × Σ log(pᵢ,ᵧᵢ)


It penalizes the model when it assigns a low probability to the correct class.

A second experiment uses class-weighted cross-entropy loss to give greater importance to less frequent classes.

### 4. Experiments

Two learning rates are compared:

- 2e-5
- 5e-5

Both experiments use two epochs and the same validation-based model-selection approach.

A separate class-weighted experiment uses two epochs and balanced class weights calculated from the training data.

## Evaluation

The models are evaluated on the held-out test set using:

- Accuracy
- Precision
- Recall
- Macro F1-score
- Classification report
- Confusion matrix

### Experimental Results

| Model | Test Accuracy | Macro F1 |
|---|---:|---:|
| TF-IDF + Logistic Regression | 0.7133 | 0.6750 |
| DistilBERT (learning rate 2e-5) | 0.6400 | 0.6242 |
| Class-weighted DistilBERT | 0.6467 | 0.6343 |

The TF-IDF baseline achieved the best overall test performance. Class weighting improved DistilBERT's accuracy and macro F1 compared with its ordinary fine-tuning experiment, but did not outperform the baseline.

These results show that a more complex model does not necessarily perform better on every dataset.

## Interactive Demonstration

The project includes a Gradio application that supports:

- Resume text input.
- Text-based PDF upload.
- DOCX upload.
- Job description input.
- TF-IDF and DistilBERT predictions.
- Class probabilities.
- Matched and missing skill lists.

Three illustrative resume–job pairs are used to demonstrate strong, partial, and weak skill overlap.

**Skill-matching limitation:** Matched and missing skills are identified using a predefined keyword list. This is not semantic skill extraction, and a skill not detected in the text is not necessarily absent from the candidate's actual abilities.

Scanned PDFs that contain only images are not supported because OCR is not implemented.

## How to Run

1. Open the project notebook: `Job_Resume_Matching_1000.ipynb`.
2. Open it in Google Colab.
3. Install the required libraries using the installation cells in the notebook.
4. Load the dataset from Hugging Face, subject to its applicable access, licensing, and privacy conditions.
5. Run the notebook cells in order to preprocess the data, train the models, and evaluate the results.
6. Run the PDF/DOCX extraction and Gradio demo cells to launch the interactive application.

A GPU runtime is recommended for DistilBERT fine-tuning.

## Limitations

- The dataset may not represent all industries, job roles, or candidate backgrounds.
- Model performance depends on the quality and distribution of the training labels.
- The keyword-based skill comparison is limited to a predefined vocabulary.
- Truncated inputs may omit information from long resumes or job descriptions.
- Softmax probabilities are not calibrated measures of real-world certainty.
- The system does not evaluate a person's complete qualifications, experience, or potential.

## Future Improvements

- Expand and diversify the dataset after privacy and licensing review.
- Improve semantic skill extraction.
- Explore more fine-tuning settings and loss functions.
- Improve performance across all three classes.
- Evaluate probability calibration and fairness.
- Add OCR support for scanned resumes.

## Technologies Used

- Python
- Google Colab
- PyTorch
- Hugging Face Transformers
- Hugging Face Datasets
- Scikit-learn
- Pandas and NumPy
- Matplotlib
- Gradio
- PyPDF
- python-docx

## Disclaimer

This project is developed for academic and educational purposes. It is an experimental resume–job matching tool, not a recruitment decision system. Human review is essential for any real-world employment assessment.
