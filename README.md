
## Getting Started

### Prerequisites

*   **Python 3.8+:**  This project is built with Python 3.8 or later.  It's highly recommended to use a virtual environment to manage dependencies.
*   **Required Libraries:** The necessary Python libraries are listed in `requirements.txt`.

### Installation and Setup

1.  **Clone the Repository:**

    ```bash
    git clone <repository_url>  # Replace with the actual URL
    cd Sentiment-Analysis
    ```

2.  **Create a Virtual Environment (Recommended):**

    ```bash
    python3 -m venv .venv
    source .venv/bin/activate  # On Linux/macOS
    .venv\\Scripts\\activate  # On Windows
    ```

3.  **Install Dependencies:**

    ```bash
    pip install -r requirements.txt
    ```
    *If you encounter version conflicts, ensure you are using the SAME Python, scikit-learn, and XGBoost versions as those used during model training (see the Jupyter Notebook for details).  Creating a new virtual environment with these specific versions is strongly recommended.*  The `requirements.txt` file provided is:
        ```
        flask
        flask-cors
        nltk
        matplotlib
        pandas
        scikit-learn==1.6.1
        xgboost
        wordcloud
        ```
    *If you do not have the exact versions used to train the models, you MUST retrain the models using the provided Jupyter notebook within your new environment and save the resulting `.pkl` files, overwriting the existing ones.*

4. **Download NLTK Resources (One-Time):**

    If you get an error about missing NLTK resources after installation. Run the following in a Python shell or a script *once*:

      ```python
      import nltk
      nltk.download('stopwords')
      ```

### Running the Application

1.  **Start the Flask Server:**

    ```bash
    python api.py
    ```

    This will start the Flask development server, typically on `http://127.0.0.1:5000`.  You'll see output in your console indicating that the server is running.  *Crucially, ensure the server is running in the same virtual environment where you installed the dependencies.*

2.  **Access the Web Interface:**

    *   Open `landing.html` in your web browser for the landing page with integrated prediction functionality.
    *   Open `index.html` for a dedicated, single-page application.

3.  **Make Predictions:**

    *   **Single Sentence:** Type text into the textarea and click "Predict Sentiment".
    *   **Bulk Prediction (CSV):**
        *   Click the "Upload CSV File" area (or drag and drop a file).
        *   Select a CSV file.  *The CSV file must have a column named `verified_reviews` containing the text to be analyzed.*
        *   Click "Predict Sentiment".
        *   The sentiment distribution graph will appear.
        *   A "Download Predictions" button will appear; click it to download the results as a CSV file.

## Data and Model

*   **Dataset:** The project uses the Amazon Alexa Reviews dataset (`amazon_alexa.tsv`), a tab-separated file containing user reviews and their corresponding feedback (positive or negative).  The `verified_reviews` column is used for sentiment analysis.
*   **Model:** An XGBoost classifier (`model_xgb.pkl`) is pre-trained on the dataset and included in the `Models2` (or `Models`) directory.  This model is loaded by the Flask app (`api.py`).
*   **Preprocessing:**  The `Data Exploration & Modelling.ipynb` notebook demonstrates the data preprocessing steps, including:
    *   Text cleaning (removing non-alphabetic characters).
    *   Lowercasing.
    *   Tokenization (splitting text into words).
    *   Stemming (reducing words to their root form using `PorterStemmer`).
    *   Stop word removal (removing common English words like "the", "a", "is").
    *   Feature extraction using `CountVectorizer` (creating a bag-of-words representation).
    *   Feature scaling using `MinMaxScaler` (scaling numerical features to the range [0, 1]).
*   **CountVectorizer:**  The trained `CountVectorizer` (`countVectorizer.pkl`) is crucial for ensuring that the input text is transformed into the same numerical format that the model expects.
*   **Scaler:**  The `MinMaxScaler` (`scaler.pkl`) is used to scale the numerical features to the range [0, 1]. This is important for the performance of many machine learning models, including XGBoost.

## API Endpoints

The Flask app exposes the following endpoints:

*   `/`: (GET, POST) - Serves the `landing.html` page.  Handles both single-sentence prediction (POST with JSON body) and bulk CSV prediction (POST with file upload).
*   `/predict`: (POST) - The main prediction endpoint.  Handles both single-sentence prediction (POST with JSON body) and bulk CSV prediction (POST with file upload).
    *    **Request (Single Sentence):**
         ```json
         {
             "text": "Your text here..."
         }
         ```
     *   **Response (Single Sentence):**
         ```json
         {
             "prediction": "Positive"  // Or "Negative"
         }
         ```
    *  **Request (CSV):**  A `multipart/form-data` request with a file named `file`.

    *   **Response (CSV):**  A CSV file download named `Predictions.csv`, containing the original data and a new "Predicted sentiment" column.  The response also includes custom headers:
        *   `X-Graph-Exists`:  `"true"` (indicates that a graph is available).
        *   `X-Graph-Data`:  A base64-encoded PNG image of the sentiment distribution pie chart.

*  `/test`: (GET) - A simple test endpoint to verify that the server is running. Returns a success message.

## Troubleshooting

*   **Version Conflicts:** If you encounter errors, *double-check* that you are using the correct versions of Python, scikit-learn, and XGBoost, as specified in the "Getting Started" section and in the Jupyter Notebook.  Using a virtual environment is *highly* recommended.  If you have problems, completely remove your `.venv` directory and recreate it, ensuring you install the *exact* versions.

*   **Missing NLTK Resources:** If you see an error related to missing NLTK resources (like stopwords), run the NLTK downloader as described in the "Installation and Setup" section.

*   **CSV File Format:**  Ensure your CSV file for bulk prediction has a column named *exactly* `verified_reviews`. The file should be a tab-separated value file (.tsv)
* **Server Not Restarting:** After making changes to api.py, ensure you save the file and restart the Flask server

*  **"Positive" Predictions Only:** If you get only "Positive" predictions for text input, even for clearly negative sentences, *re-train and re-save the models* using the Jupyter Notebook, *within the correct, consistent Python environment*. The provided code loads the pre-trained models from the `Models2` directory. Incorrect preprocessing or version mismatches are the most likely causes.

## License

This project is licensed under the [MIT License](LICENSE) - see the `LICENSE` file for details.  (You should create a `LICENSE` file and choose an appropriate open-source license if you intend to share your code publicly.)

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Acknowledgements

*   This project uses the [Amazon Alexa Reviews](https://www.kaggle.com/sid321axn/amazon-alexa-reviews) dataset.
*   The machine learning models are built using [scikit-learn](https://scikit-learn.org/stable/) and [XGBoost](https://xgboost.readthedocs.io/en/stable/).
*   The web interface is built with [Flask](https://flask.palletsprojects.com/) and [Tailwind CSS](https://tailwindcss.com/).
*   Data visualization uses [Matplotlib](https://matplotlib.org/).

This README provides a comprehensive overview of the project, making it easy for others (and your future self) to understand, use, and contribute to your sentiment prediction application.  It includes clear instructions, addresses potential issues, and acknowledges the data and tools used.  Good luck with your project!
