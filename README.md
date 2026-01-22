# 🛡️ Financial Crime Detection System (AI & Graph Theory)

A comprehensive, multi-layered financial security system designed to detect **Credit Card Fraud**, **Money Laundering (Smurfing)**, and verify user identity via **AI-powered OCR**. This system acts as a real-time "Digital Gatekeeper" for banking transactions.

## 🚀 Project Overview

Financial crime is evolving. Traditional rule-based systems cannot catch complex money laundering schemes or sophisticated credit card theft. This project solves that by combining three powerful technologies:
1.  **Machine Learning (Random Forest):** To detect anomalies in individual transactions (General Fraud).
2.  **Graph Theory (NetworkX):** To detect complex relationships and structuring patterns between accounts (Smurfing/Money Laundering).
3.  **Generative AI (Llama 3 Vision):** To perform intelligent OCR on ID cards for Know Your Customer (KYC) verification.

---

## ⚡ Key Features

### 1. 🧠 ML-Based Fraud Detection (The Brain)
* **Algorithm:** Random Forest Classifier with SMOTE (Synthetic Minority Over-sampling Technique) to handle imbalanced data.
* **Persistent Learning:** The model features an **Auto-Retrain** mechanism. It learns from new transactions and updates its internal logic automatically, evolving alongside criminal tactics.
* **Real-time Scoring:** Delivers a probability score (0-100%) for every transaction in milliseconds.

### 2. 🕸️ Smurfing & Structuring Detection (The Detective)
* **Graph Analysis:** Uses `NetworkX` to build a directed graph of all accounts and transactions.
* **Pattern Recognition:** Detects specific topologies indicative of money laundering:
    * **Fan-In/Fan-Out:** Rapid movement of funds from many sources to one (or vice versa).
    * **Structuring:** Splitting large sums into small, "safe" amounts to evade reporting limits.
* **Community Detection:** Identifies "Fraud Rings" acting in coordination.

### 3. 👁️ Intelligent KYC (The Eyes)
* **AI-Powered OCR:** Utilizes **Llama 3.3 (via Groq API)** to "see" and understand ID card images.
* **Context Aware:** Unlike standard OCR, it uses Generative AI to correct reading errors (e.g., fixing "19S0" to "1990") and extracts structured JSON data (Name, DOB).

### 4. 🌐 Flask API Architecture
* Fully RESTful API built with **Flask**.
* **Endpoints:**
    * `/analyze_transaction`: Unified risk assessment (ML + Graph + Rules).
    * `/detect_smurfing`: Deep-scan of the network for laundering patterns.
    * `/extract_id`: ID card text extraction.

---

## 🛠️ Tech Stack

* **Language:** Python 3.9+
* **Web Framework:** Flask
* **Machine Learning:** Scikit-Learn, Imbalanced-Learn (SMOTE), Joblib
* **Graph Theory:** NetworkX
* **Data Processing:** Pandas, NumPy
* **Generative AI:** LangChain, Groq API (Llama 3.3)
* **Utilities:** Geopy (Geolocation distance), Dotenv

---

## ⚙️ Installation & Setup

### Prerequisites
* Python installed on your machine.
* A Groq API Key (for the OCR feature).

### 1. Clone the Repository
```bash
git clone [https://github.com/yourusername/financial-crime-detection.git](https://github.com/yourusername/financial-crime-detection.git)
cd financial-crime-detection

### 2. Create a Virtual Environment
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

> **Note:** Create a `requirements.txt` with: `flask`, `pandas`, `scikit-learn`, `imbalanced-learn`, `networkx`, `langchain-groq`, `python-dotenv`, `geopy`, `joblib`

### 4. Configure Environment Variables
Create a `.env` file in the root directory:

```ini
GROQ_API_KEY=your_groq_api_key_here
```

### 5. Prepare Data
Ensure your transaction dataset (`filtered_data (1).csv`) and fraud community data (`fraud_community.json`) are in the root folder.

### 6. Run the Application
```bash
python main.py
```

The server will start at `http://127.0.0.1:5000`.

---

## 📡 API Usage Examples

### 1. Analyze a Transaction (Unified Check)
**Endpoint:** `POST /analyze_transaction`

**Request:**

```json
{
    "cardNum": "1234567890",
    "merchant": "Amazon",
    "amount": 1500.00,
    "trans_date_trans_time": "2023-10-27 14:30:00",
    "lat": 40.7128,
    "long": -74.0060,
    "merch_lat": 40.7300,
    "merch_long": -74.0100
}
```

**Response:**

```json
{
    "fraud_detection": {
        "result": "Fraud",
        "confidence": 0.85,
        "flags": ["high_amount", "geolocation_mismatch"]
    },
    "smurfing_detection": {
        "is_smurfing": false
    }
}
```

### 2. Extract ID Details (OCR)
**Endpoint:** `POST /extract_id`

**Body:** `form-data` with a key `image` containing the ID card file.

---

## 🧠 System Architecture

The system operates on a **"Two-Gate" Logic:**

**Gate 1 (ML Model):** Checks the attributes of the transaction. Is the amount too high? Is the location weird? Is the time suspicious?

**Gate 2 (Graph Detective):** Checks the relationships. Does this sender have a history of structuring payments? Is the receiver a known mule?

**Self-Healing Mechanism:** The `PersistentAutoRetrainFraudDetector` class monitors incoming data. When enough new transactions are processed, it triggers a background re-training process, ensuring the AI model never becomes obsolete.

---

## 🔮 Future Improvements

* Implement a real database (PostgreSQL/Neo4j) instead of CSV/JSON files for scalability.
* Add a frontend dashboard for bank analysts to visualize the fraud graph.
* Deploy to a cloud provider (AWS/GCP) using Docker.

---

## 🤝 Contribution

Contributions are welcome! Please fork the repo and submit a pull request.

---

## 📄 License

This project is licensed under the MIT License.

