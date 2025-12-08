# TAIHLS-Final-Project

## Project Overview:

The leading cause of death in patients that have cancer is often the spread of cancer to other parts of the body, also known as cancer metastasis. The main force driving cancer metastasis lies in the Epithelial-Mesenchymal Transition (EMT), where cells transition from the epithelial state (stationary and structured) to the mesenchymal state (loosely organized, very mobile, and invasive). 

The problem is that this transition process is hard to predict, as it is affected by gene regulation, and it is not clear what regulation patterns control this transition. As a result, it is extremely difficult to say with certainty when a cell will go into the EMT phase. 

To address this issue, we take a three-step approach:

1. Simulate EMT biological dynamics using the ODE model, by following the time-dependent evolution of a vector containing biomarkers.
2. Train an LSTM to classify the "fates" and find patterns in the EMT from the time-series trajectories
3. Trains a Neural ODE (NODE) to learn continuous latent dynamics.


##  Documentation

The way we did this was through combining 1. Mechanistic modeling and 2. Deep learning. In our context, this was the Neural ODE framework and an LSTM baseline. The first step was deriving a minimal three-dimensional ODE system representing the dynamics of ZEB (Z), miR-200 (M), and an external signal (S). The equations included Hill-type nonlinearities (that we understood through our prior experience in multivariable and linear algebra) to model repression/activation, decay terms for natural degradation, and a signal input function representing environmental cues.
For the simulation itself, we generated 1,200 trajectories with 161 time points each, introducing Gaussian noise (σ = 0.02) to mimic experimental variability that you would see in real life. The trajectories were labeled based on final Z and M levels to indicate mesenchymal versus epithelial outcomes. The ODE itself consisted of an encoder that compressed the first 6 observed time points into a latent state, followed by a two-hidden-layer ODE function with 128 hidden units and Tanh activations modeling the continuous evolution, and a classifier with 128 hidden units and sigmoid output predicting EMT probability. These activations and modeling features were derived from a past successful project we found in the literature, but for a different cause.
The training used mean squared error (MSE) for trajectory reconstruction and binary cross-entropy (BCE) for classification. The model was trained on a batch size of 64 for 12 epochs using the Adam optimizer with a learning rate of 0.001. We tuned the hyperparameters to balance both the classification loss and the reconstruction such that we were given accurate predictions of trajectories and the EMT.
To compare to a baseline, we used a very simple LSTM model that had 128 hidden units over the same observed window (6 time points) for 6 epochs. The Neural ODE model achieved a test AUC of 0.92 and mean trajectory MSE = 0.004, while the LSTM baseline achieved a test AUC of 0.88. To take the analysis further, we developed a gradient-based minimal intervention algorithm. This method looks at the last observed state of a cell’s trajectory and finds the smallest change, or “perturbation,” that can flip the predicted outcome from undergoing EMT to not undergoing EMT, or vice versa. Essentially, it tells us the minimal tweak needed to alter the cell’s future behavior according to the model. Our perturbations ended up being quite small, with L2 norms from 0.1-0.3, meaning only minor adjustments to the cell’s state are required to shift its predicted fate.

## Setup Instructions


```bash
git clone https://github.com/pi271828/TAIHLS-Final-Project.git
cd TAIHLS-Final-Project
python -m venv .venv
source .venv/bin/activate     
pip install -r requirements.txt
notebooks/EMT_Transition.ipynb
python src/emt_project.py
```
## Research Paper

The research paper is linked below for further reading:

https://docs.google.com/document/d/1R2718bcPXRZ2-zwnY36jBjQtu-BtHdCIze_TAIvV4OE/edit?usp=sharing

