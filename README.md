# Bearing Fault Detection System

![Sri Madhura Engineering](https://img.shields.io/badge/Sri%20Madhura%20Engineering-Bearing%20Fault%20Detection-blue)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![Streamlit](https://img.shields.io/badge/Streamlit-1.x-red)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green)

A comprehensive web application for detecting faults in bearing components using deep learning models. Built with Streamlit and powered by LSTM and CNN-LSTM neural networks.

## 📋 Table of Contents
- [Features](#features)
- [Demo](#demo)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Data Collection Guide](#data-collection-guide)
- [Models](#models)
- [Database Setup](#database-setup)
- [Authentication](#authentication)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- **Dual Model Analysis**: Uses both LSTM and CNN-LSTM models for more reliable fault detection
- **Multiple Input Types**: Accepts both .txt vibration data files and .wav audio files
- **Real-time Visualization**: Displays vibration data and prediction confidences
- **Comprehensive Diagnostics**: Detects normal conditions and three fault types (inner race, outer race, ball)
- **Feature Extraction**: Calculates and displays 9 critical statistical features
- **Data Storage**: Saves all analyses in MongoDB Atlas for historical tracking
- **User Authentication**: Secure access to analysis history
- **Responsive UI**: Clean, intuitive interface with detailed information tabs
- **Export Functionality**: Download analysis history as Excel files

## 🎬 Demo

![Application Demo](https://example.com/demo.gif)

## 🚀 Installation

### Prerequisites
- Python 3.8+
- pip
- MongoDB Atlas account (or local MongoDB installation)

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/bearing-fault-detection.git
   cd bearing-fault-detection
   ```

2. **Create and activate a virtual environment (recommended)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download the pre-trained models**
   
   Download the following model files and place them in the project root directory:
   - `lstm_fault_detection_model.h5`
   - `cnn_lstm_fault_detection_model.h5`
   - `scaler.save`
   - `label_encoder.save`
   
   *Note: You can either download these from [our releases page](https://github.com/yourusername/bearing-fault-detection/releases) or train your own models using the scripts in the `model_training` directory.*

5. **Configure MongoDB**
   
   Update the MongoDB connection string in the application with your credentials:
   ```python
   connection_string = "mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority"
   ```
   
   For better security, you can use Streamlit secrets or environment variables:
   - Create a `.streamlit/secrets.toml` file with:
     ```toml
     [mongodb]
     connection_string = "mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority"
     ```
   - Or use a `.env` file with:
     ```
     MONGODB_URI="mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority"
     ```

6. **Run the application**
   ```bash
   streamlit run app.py
   ```

## 📊 Usage

### Analyzing Bearing Condition

1. Navigate to the "Prediction" tab
2. Select input type (Text File or Audio File)
3. Upload a vibration data file:
   - Text file should contain 20,480 space-separated acceleration values
   - Audio file should be a .wav recording of bearing vibration
4. Click "Analyze and Predict Fault"
5. Review the diagnosis results:
   - Diagnosis from both models
   - Statistical features extracted
   - Confidence levels for each fault type

### Viewing Analysis History

1. Navigate to the "History" tab
2. Log in with provided credentials (default: `madhura@123` / `madhura@123`)
3. Browse previous analyses
4. Download history as Excel file if needed

## 📂 Project Structure

```
bearing-fault-detection/
│
├── app.py                    # Main application file
├── requirements.txt          # Python dependencies
│
├── models/                   # Pre-trained models
│   ├── lstm_fault_detection_model.h5
│   ├── cnn_lstm_fault_detection_model.h5
│   ├── scaler.save
│   └── label_encoder.save
│
├── Uploads/                  # Directory for uploaded files (auto-created)
│
├── model_training/           # Scripts for training models
│   ├── train_lstm.py
│   ├── train_cnn_lstm.py
│   └── prepare_data.py
│
├── docs/                     # Documentation
│   ├── data_collection_guide.pdf
│   └── model_architecture.pdf
│
└── .streamlit/              # Streamlit configuration
    └── secrets.toml         # For storing sensitive information
```

## 📡 Data Collection Guide

### Hardware Requirements
- **Accelerometer**: MEMS or piezoelectric with at least 20 kHz bandwidth
- **DAQ/ADC**: 16-bit resolution or higher, minimum 20 kHz sampling rate

### Collection Process
1. **Sensor Placement**
   - Attach accelerometer to bearing housing
   - Use adhesive or magnetic mounts
   - Position near the load zone for best results

2. **Signal Acquisition**
   - Set sampling rate to at least 20 kHz
   - Apply anti-aliasing filters
   - Ensure proper grounding to reduce noise

3. **Recording**
   - Capture exactly 20,480 samples
   - Save as space-separated values in .txt format
   - Alternatively, record as .wav file

### Applicable Bearing Types
- Deep groove ball bearings
- Cylindrical/tapered roller bearings
- Angular contact bearings
- Thrust bearings

## 🧠 Models

### LSTM Model
- **Architecture**: Multiple LSTM layers with dropout for regularization
- **Input**: 9 statistical features extracted from vibration signals
- **Output**: 4 classes (Normal, Inner Race Fault, Outer Race Fault, Ball Fault)
- **Accuracy**: >92% on test dataset

### CNN-LSTM Model
- **Architecture**: Convolutional layers followed by LSTM layers
- **Input**: Same 9 statistical features as LSTM model
- **Advantage**: Better at capturing spatial and temporal patterns simultaneously
- **Accuracy**: >93% on test dataset

### Feature Extraction
Both models use these 9 statistical features:
1. Maximum Value
2. Minimum Value
3. Mean
4. Standard Deviation
5. RMS (Root Mean Square)
6. Skewness
7. Kurtosis
8. Crest Factor (Peak/RMS)
9. Form Factor (RMS/Mean Absolute Value)

## 🗄️ Database Setup

### MongoDB Atlas Configuration
1. Create a free account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Create a database named `bearing_fault_detection`
4. Create a collection named `uploads`
5. Create a database user with read/write privileges
6. Get your connection string from MongoDB Atlas
7. Update the application with your connection string

### Data Structure
Each analysis is stored with the following fields:
- `original_filename`: Name of the uploaded file
- `saved_filename`: Name of the file saved on server (with timestamp)
- `filepath`: Path where the file is stored
- `timestamp`: Date and time of analysis
- `output`: Prediction results from both models
- `input_type`: Type of input (Text File or Audio File)

## 🔒 Authentication

The history tab requires authentication to protect analysis data:

- Default username: `madhura@123` 
- Default password: `madhura@123`

*Note: For production use, modify the credentials in the code.*

## 🌐 Deployment

### Streamlit Cloud
1. Push your code to GitHub
2. Log in to [Streamlit Cloud](https://streamlit.io/cloud)
3. Deploy directly from your GitHub repository
4. Configure secrets for MongoDB credentials

### Docker
A `Dockerfile` is provided for containerized deployment:

```bash
# Build the Docker image
docker build -t bearing-fault-detection .

# Run the container
docker run -p 8501:8501 bearing-fault-detection
```

### Local Server
For a persistent local deployment:

```bash
nohup streamlit run app.py --server.port=8501 &
```

## 👥 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

---

Developed by Sri Madhura Engineering | For technical support contact: support@madhuraengineering.com