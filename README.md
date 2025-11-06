# Computer-Code-STABGCN-Inversion
Computer-Code-STABGCN-Inversion
Code implementation for the paper "Prediction of Natural Gamma and Neutron Porosity Based on Waveform Structures of Elastic Parameters Using a Closed-Loop Deep Learning Fusion Network". This repository contains the STABGCN forward models, a pre-trained inverse model, and well log data for reproducing the results presented in the paper.

Project Overview
This project aims to predict key well logs (GR and CNL) from rock physics parameters (Vp, Vs, and Dn) using deep learning techniques.

The core idea is to compare the effectiveness of a semi-supervised learning method based on physical consistency constraints against a traditional fully supervised learning method. The semi-supervised approach leverages a pre-trained inverse model (inverse_prediction_model.h5) as a physical constraint, enabling it to utilize a large amount of unlabeled data to improve prediction accuracy and generalization.

This repository provides all the key components needed to reproduce the paper's results:

inverse_prediction_model.h5: A pre-trained inverse model for physical constraint.
forward_model_supervised.py.ipynb: The script for training the fully supervised model and generating a performance baseline.
forward_model_semisupervised.py.ipynb: The script for training the semi-supervised model and performing the final comparison.
model data/: Contains all necessary well log data.
Workflow
Please execute the scripts in the following order to fully reproduce the comparison results from the paper.

Prerequisites: Model and Data Setup
Ensure that the inverse_prediction_model.h5 file and the model data folder are located at the correct relative path to your Jupyter Notebook scripts. inverse_prediction_model.h5 is a critical pre-trained model that will be directly called by the semi-supervised learning process. You do not need to train this model yourself.

Step 1: Run forward_model_supervised.py.ipynb
This script will train a fully supervised forward model. It uses only 5 labeled wells for training, simulating a scenario with limited data.

Role: Serves as a performance baseline for comparison.
Output: The script will generate gr_predicted_supervised.mat and cnl_predicted_supervised.mat files, which contain the prediction results from the supervised model. These files will be used in the final comparison visualization.
Step 2: Run forward_model_semisupervised.py.ipynb
This script is the core of the project and will train the semi-supervised forward model.

Role:
Uses 5 labeled wells for supervised learning.
Leverages all other unlabeled wells and the pre-trained inverse_prediction_model.h5 to enforce a physical consistency constraint.
Loads the supervised prediction results from Step 1 and generates a 2x3 comparison plot, visually demonstrating the performance difference among True Values, Supervised Predictions, and Semi-Supervised Predictions.
Model and Script Descriptions
inverse_prediction_model.h5 (Pre-trained Inverse Model)
Function: This is a pre-trained inverse model that has learned the mapping from well logs to rock physics parameters: f(GR, CNL) -> (Vp, Vs, Dn).
Role: In the semi-supervised learning process, it acts as a known and reliable "physics law simulator." Its weights are frozen and do not participate in training; it is used only to verify whether the "pseudo" well logs predicted by the forward model are physically consistent.
forward_model_supervised.py.ipynb (Supervised Forward Model Training)
Objective: To establish a standard, fully supervised forward prediction model as a performance baseline.
Method: Uses only a small labeled dataset from 5 wells for training. The model learns to predict (GR, CNL) at a center point from a sequence of (Vp, Vs, Dn).
forward_model_semisupervised.py.ipynb (Semi-Supervised Forward Model Training)
Objective: To demonstrate that semi-supervised learning can outperform the fully supervised model by leveraging unlabeled data.
Method:
Supervised Part: Computes the supervised loss on the 5 labeled wells, just like the supervised model.
Semi-Supervised Part: Executes a "forward-inverse" cycle on all unlabeled data, using inverse_prediction_model.h5 to calculate a physical consistency loss. This loss is added to the total loss to guide the model's training.
Comparison: Finally, it visually demonstrates the performance improvement of the semi-supervised method over the fully supervised method through comparison plots.
