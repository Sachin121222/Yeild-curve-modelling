# 🚀 GitHub Repository Setup Guide

Follow these steps to publish this project to GitHub and share the Colab notebook.

---

## Step 1 — Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Fill in:
   - **Repository name**: `stochastic-yield-curve`
   - **Description**: `Stochastic Yield Curve Modelling with CIR Processes (Single-Factor MLE + Two-Factor EKF)`
   - **Visibility**: Public ✅ (required so the Colab link is accessible to anyone)
3. ✅ Check **Add a README file** — we'll replace it
4. Click **Create repository**

---

## Step 2 — Upload Files via GitHub Web UI (Easiest)

In your new repository:

1. Click **Add file → Upload files**
2. Drag and drop everything from this folder:
   - `README.md`
   - `requirements.txt`
   - `.gitignore`
   - `notebooks/Stochastic_Yield_Curve_CIR.ipynb`
   - `data/train_data.csv`
   - `data/test_data.csv`
   - `data/test_data_3M.csv`
3. Write a commit message like `Initial commit — CIR yield curve project`
4. Click **Commit changes**

> **Note**: GitHub may warn that `.gitignore` is a hidden file. Upload it anyway.

---

## Step 3 — Upload Notebook to Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click **File → Upload notebook**
3. Select `notebooks/Stochastic_Yield_Curve_CIR.ipynb`
4. Upload the three data files to the Colab session:
   - In the left sidebar, click the 📁 **Files** icon
   - Click **Upload** and select all three CSV files from `data/`

---

## Step 4 — Set Colab Sharing to "Anyone with the link"

1. In Colab, click **Share** (top-right)
2. Under **General access**, change to **"Anyone with the link"**
3. Set the role to **Viewer**
4. Click **Copy link** and save the URL

---

## Step 5 — Add Colab Link to README

Back on GitHub, edit `README.md` and add this badge at the top (replace `<COLAB_URL>` with your link):

```markdown
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](<COLAB_URL>)
```

---

## Step 6 — (Optional) Open Directly from GitHub in Colab

You can also open the notebook directly from GitHub in Colab using this URL pattern:

```
https://colab.research.google.com/github/<YOUR_USERNAME>/stochastic-yield-curve/blob/main/notebooks/Stochastic_Yield_Curve_CIR.ipynb
```

Replace `<YOUR_USERNAME>` with your GitHub username.

---

## ✅ Submission Checklist

- [ ] Repository is **Public**
- [ ] All three CSV files are in `data/`
- [ ] Notebook is in `notebooks/`
- [ ] `README.md` is complete
- [ ] Notebook runs **top-to-bottom without errors** (test with Runtime → Run all in Colab)
- [ ] Colab link sharing set to **"Anyone with the link can view"**
- [ ] Colab link added to README
