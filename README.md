# TAIHLS-Final-Project

## Project Overview:

The leading cause of death in patients that have cancer is often the spread of cancer to other parts of the body, also known as cancer metastasis. The main force driving cancer metastasis lies in the Epithelial-Mesenchymal Transition (EMT), where cells transition from the epithelial state (stationary and structured) to the mesenchymal state (loosely organized, very mobile, and invasive). 

The problem is that this transition process is hard to predict, as it is affected by gene regulation, and it is not clear what regulation patterns control this transition. As a result, it is extremely difficult to say with certainty when a cell will go into the EMT phase. 

To address this issue, we take a three-step approach:

1. Simulate EMT biological dynamics using the ODE model, by following the time-dependent evolution of a vector containing biomarkers.
2. Train an LSTM to classify the "fates" and find patterns in the EMT from the time-series trajectories
3. Trains a Neural ODE (NODE) to learn continuous latent dynamics.

## Setup Instructions


```bash
git clone https://github.com/YOUR_USERNAME/TAIHLS-Final-Project.git
cd TAIHLS-Final-Project
python -m venv .venv
source .venv/bin/activate     
pip install -r requirements.txt
notebooks/EMT_Transition.ipynb
python src/emt_project.py




