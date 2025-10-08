# google-cloud-ctr-prediction

### Click-Through Rate (CTR) Prediction on Google Cloud Vertex AI

This project demonstrates an end-to-end machine learning deployment pipeline on Google Cloud Platform (GCP) using the Avazu Click-Through-Rate dataset. It integrates BigQuery, Vertex AI Workbench, Cloud Storage, and Vertex AI Endpoints to train, register, and deploy an XGBoost model for real-time prediction.

### Objective

The goal was to build and deploy an ML model capable of predicting whether an ad click occurs, using:

→ BigQuery for data preprocessing 
→ Vertex AI Workbench for model training  
→ Cloud Storage for artifact management  
→ Vertex AI Model Registry & Endpoints for deployment and inference  

### Architecture Overview

BigQuery → Vertex AI Workbench (training)   
         → Cloud Storage (model artifacts)  
         → Vertex AI Model Registry   
         → Vertex AI Endpoint (real-time inference)  



| Component            | Service Used                                                                          |
| -------------------- | ------------------------------------------------------------------------------------- |
| Data Storage         | BigQuery                                                                              |
| Model Training       | Vertex AI Workbench (JupyterLab)                                                      |
| Model Deployment     | Vertex AI Model Registry + Endpoints                                                  |
| Model Type           | XGBoost (binary:logistic)                                                             |
| Programming Language | Python 3.10                                                                           |
| Libraries            | pandas, xgboost, google-cloud-aiplatform, google-cloud-storage, google-cloud-bigquery |


### Key Features

Feature-engineered Avazu CTR dataset with 13 engineered features (time, frequency, encoding, and hashed interactions)

Trained XGBoost model exported as model.bst

Artifacts stored in Cloud Storage

Model deployed to Vertex AI Endpoint for online inference

Verified predictions via API call using PredictionServiceClient

Cloud-native workflow — no manual file transfers, fully integrated


### Model Deployment Flow

#### Data Preprocessing
SQL transformations in BigQuery to create derived tables:  

→ train_v2_base, freq_features, target_encode_features, etc.  

#### Training
Run Jupyter notebook in Vertex AI Workbench:

→ Trains XGBoost model  
→ Saves model.bst and feature_list.json    
→ Uploads artifacts to GCS bucket (gs://avazu-ctr/models/ctr_xgb/2025-10-08T06-28-20_named)  

####  Model Registration & Deployment

→ Model uploaded to Vertex AI Model Registry as ctr_xgb_v2_named  
→ Deployed to endpoint ctr_xgb_endpoint  
→ Inference Test    
→ BigQuery row fetched  
→ Prediction made using

pred = endpoint.predict(instances=[row], parameters={"feature_names": FEATURES})
print(pred.predictions)
