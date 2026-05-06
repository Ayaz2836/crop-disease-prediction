
# Fertilizer Prediction Model

This project develops a machine learning model to predict the most suitable fertilizer based on various environmental and soil conditions. It also includes a user-friendly Gradio-based Graphical User Interface (GUI) for interactive predictions.

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Data Preprocessing](#data-preprocessing)
- [Model Training](#model-training)
- [GUI Application](#gui-application)
- [How to Use the GUI](#how-to-use-the-gui)

## Project Overview
The goal of this project is to assist farmers and agricultural professionals in making informed decisions about fertilizer usage. By inputting key environmental and soil parameters, the model recommends an appropriate fertilizer, along with a rationale for the recommendation.

## Dataset
The dataset used for this project, `Fertilizer Prediction.csv`, was sourced from KaggleHub (`gdabhishek/fertilizer-prediction`). It contains the following features:
- `Temparature`: Environmental temperature.
- `Humidity`: Air humidity.
- `Moisture`: Soil moisture content.
- `Nitrogen`: Nitrogen content in the soil.
- `Potassium`: Potassium content in the soil.
- `Phosphorous`: Phosphorous content in the soil.
- `Soil Type`: Type of soil (categorical: Black, Clayey, Loamy, Red, Sandy).
- `Crop Type`: Type of crop being grown (categorical: Barley, Cotton, Ground Nuts, Maize, Millets, Oil seeds, Paddy, Pulses, Sugarcane, Tobacco, Wheat).
- `Fertilizer Name`: The target variable, indicating the recommended fertilizer.

## Data Preprocessing
1.  **Loading Data**: The dataset is loaded into a Pandas DataFrame.
2.  **Encoding Categorical Features**: 
    - `Soil Type` and `Crop Type` are categorical features that need to be converted into numerical format for the machine learning model. This is achieved using `OneHotEncoder` from `sklearn.preprocessing`.
    - Separate `OneHotEncoder` instances are used for 'Soil Type' and 'Crop Type' to avoid conflicts.
    - The original categorical columns are dropped, and the new one-hot encoded columns are concatenated with the DataFrame.
3.  **Splitting Data**: The dataset is split into training and testing sets using `train_test_split` from `sklearn.model_selection` with an 80/20 ratio.

## Model Training
An XGBoost Classifier (`XGBClassifier`) is used for predicting the fertilizer. XGBoost is chosen for its performance and robustness in classification tasks.
1.  **Label Encoding Target Variable**: The `Fertilizer Name` (target variable) is label encoded using `LabelEncoder` from `sklearn.preprocessing`.
2.  **Model Initialization and Training**: An `XGBClassifier` is initialized and trained on the preprocessed training data (`x_train`, `y_train_encoded`).
3.  **Evaluation**: The model's performance is evaluated using `accuracy_score` and `confusion_matrix` on the test set (`x_test`, `y_test`). The model achieved an accuracy of 0.95.

## GUI Application
To provide an interactive way to get fertilizer recommendations, a web-based GUI is implemented using the `Gradio` library. This allows users to input the required parameters and receive a prediction directly in a web browser, which is ideal for environments like Google Colab where traditional desktop GUIs (like Tkinter) are not supported.

### GUI Input Features
-   **Numerical Features (Direct Input Fields)**:
    -   Temparature
    -   Humidity
    -   Moisture
    -   Nitrogen
    -   Potassium
    -   Phosphorous
-   **Categorical Features (Dropdowns)**:
    -   Soil Type (e.g., Black, Clayey, Loamy, Red, Sandy)
    -   Crop Type (e.g., Barley, Cotton, Ground Nuts, Maize, Millets, Oil seeds, Paddy, Pulses, Sugarcane, Tobacco, Wheat)

### Prediction Logic
The `predict_fertilizer_gradio` function takes the user inputs, processes them into the format expected by the trained XGBoost model (including one-hot encoding for categorical features), and then makes a prediction. It also includes a heuristic-based reasoning module that generates a human-readable explanation for the recommended fertilizer based on the input nutrient levels and the known properties of the fertilizer.

## How to Use the GUI
1.  **Run All Cells**: Ensure all preceding code cells in the Jupyter/Colab notebook have been executed successfully.
2.  **Launch Gradio Interface**: Execute the cell containing the Gradio interface definition and `iface.launch()`.
3.  **Access Public URL**: Once launched, Gradio will provide a public URL (e.g., `https://xxxx.gradio.live`). Open this URL in your web browser.
4.  **Enter Inputs**: In the GUI, fill in the numerical values for temperature, humidity, moisture, and nutrient levels. Select the soil type and crop type from the dropdown menus.
5.  **Get Prediction**: Click the "Predict Fertilizer" button. The GUI will display the predicted fertilizer name and a detailed reason for the recommendation.
