# Bad Teacher Unlearning

## Dataset Descriptions

This was tested on two data sets for the machine unlearning process.

### Emotion Dataset

The Emotion dataset was obtained from Kaggle and contains English-language textual samples labeled with one of six emotion categories: joy, sadness, anger, fear, love, and surprise.

For our analysis, we used this multi-class for forgetting **surprise** as the minority class and **joy** as the majority class.

### HOT Dataset

The Hinglish Offensive Tweets (HOT) dataset is a collection of code-mixed social media posts primarily intended to classify offensive language in the Hinglish domain, a mix of Hindi and English.

The dataset comprises tweets collected using specific offensive and profane hashtags. Each tweet is annotated with one of three class labels:

* Non Offensive
* Abusive
* Hate-Inducing

Owing to its code-mixed nature, the dataset poses significant linguistic challenges due to:

* variations in transliteration
* inconsistent grammar
* non-standard language usage common in informal social media text

---

# Methodology

The proposed approach uses the **Bad Teacher framework** for targeted machine unlearning. The goal is to make the model forget specific classes or data without retraining the entire model from scratch.

## Models Used

### Smart Teacher (`M'`)

* Trained on the full dataset
* Learns all classes normally

### Bad Teacher (`Mb`)

* Randomly initialized
* Not trained on any data
* Produces random predictions

### Student Model (`Ms'`)

* Initialized using the smart teacher’s weights
* Learns from both teachers during unlearning

---

## Unlearning Process

The dataset is divided into:

* **Retain Set (`R'`)** → data the model should remember
* **Forget Set (`D'`)** → data the model should forget

During training:

* For retain data, the student model learns from the smart teacher
* For forget data, the student model follows the bad teacher

This helps the student model:

* keep useful knowledge from retained classes
* gradually forget the targeted class/data

---

## Training Objective

The student model is optimized using **KL-divergence loss** between teacher and student predictions.

Additional loss functions such as:

* Cosine Embedding Loss
* Mean Squared Error (MSE)
* Huber Loss
* Focal Loss

were also tested to improve forgetting performance and reduce shared representations.

---

## Evaluation Metrics

The model is evaluated using:

* **Zero-Retain Forgetting (ZRF):** Measures how well the model forgets the target data
* **Forget Accuracy:** Should decrease after unlearning
* **Retain Accuracy:** Should remain high
* **Validation Accuracy:** Measures overall performance after unlearning
