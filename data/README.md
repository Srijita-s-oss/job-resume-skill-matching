# Dataset Information

## Dataset Source

This project uses the Hugging Face dataset:

[batuhanmtl/job_resume_fit](https://huggingface.co/datasets/batuhanmtl/job_resume_fit)

## Working Dataset

A cleaned working sample of 1,000 resume–job description pairs is used in the project.

Each example contains resume text, job description text, and a match category.

## Dataset Split

| Split | Number of examples |
|---|---:|
| Training | 700 |
| Validation | 150 |
| Testing | 150 |
| **Total** | **1,000** |

The data is split using stratified sampling to preserve class proportions.

## Labels

The three project labels are derived from the dataset's match score:

- **Weak:** Score below 40.
- **Moderate:** Score from 40 to below 70.
- **Strong:** Score from 70 to 100.

## Privacy and Responsible Use

Raw resume data is not included in this repository.

Resume information can contain personal or identifying details. Do not upload real personal resumes, names, email addresses, phone numbers, or other identifiable information.

The dataset's licensing and privacy conditions must be reviewed before downloading or redistributing its contents. Public availability does not automatically grant permission to republish the data.

This dataset is used for academic experimentation only.
