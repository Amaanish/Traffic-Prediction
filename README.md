# Traffic Prediction System

A machine learning-powered traffic prediction system that analyzes and predicts travel times between 2 locations using real-time traffic data from TomTom API.

## Features

- **Real-time Traffic Monitoring**: Get current travel times with live traffic data
- **Intelligent Direction Selection**: Choose between going to or returning from destination
- **Dual Prediction Modes**: 
  - Find optimal travel times within time periods
  - Get specific time predictions for exact hours
- **Smart Time Categorization**: Morning, Afternoon, Evening, Night periods
- **Bidirectional Analysis**: Separate models for each travel direction
- **Visual Route Mapping**: Generate static maps showing your route with markers
- **Model Performance Metrics**: R-score and MAE displayed for transparency
- **Automatic Model Training**: Models train automatically when direction is selected

## Technologies Used

- **Python 3.11.6**
- **Machine Learning**: scikit-learn (KMeans, Random Forest)
- **Data Processing**: pandas, numpy
- **API Integration**: TomTom Routing API
- **Visualization**: matplotlib, staticmap
- **Data Storage**: Excel files via openpyxl

## Prerequisites

Before running this project, make sure you have:

1. Python 3.11.6 installed
2. A TomTom API key (get one [here](https://developer.tomtom.com/user/me/apps?type=free))
3. Required Python packages (see Installation section)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/Amaanish/Traffic-Prediction.git
cd Traffic-Prediction
```

2. Install required packages:
```bash
pip install pandas numpy scikit-learn requests matplotlib staticmap openpyxl
```

3. **Update the API key in the code**:
```python
API_KEY = "YOUR_TOMTOM_API_KEY_HERE"
```

4. **Prepare your data files**:
   - `traffic_data_to_go.xlsx`: Historical data for trips to destination
   - `traffic_data_return.xlsx`: Historical data for return trips
   
   These files should contain columns: `hour`, `day`, `travel_time_min`

## Data Collection

The system uses two main datasets:
- `traffic_data_to_go.xlsx`: Travel times from Centerpoint Metro to Muwaileh Park
- `traffic_data_return.xlsx`: Travel times from Muwaileh Park to Centerpoint Metro

### Automatic Data Collection
Uncomment the data collection section in the code to automatically collect traffic data every 10 minutes:

```python
# Uncomment the while loop in Phase 2 for automatic data collection
while True:
    # Collects traffic data every 10 minutes
    time.sleep(600)
```

## Usage

### Running the Application

```bash
python3 traffic_prediction.py
```

In this code, I have given 2 locations with their coordinates but you can change it to whatever location you want. The system will automatically train machine learning models and prompt you to choose between:
1. **Go to Centerpoint** - Predict travel times from Muwaileh Park to Centerpoint Metro
2. **Return from Centerpoint** - Predict travel times from Centerpoint Metro to Muwaileh Park

### Available Options

1. **Real-time Traffic**: Get current travel time with live traffic data
2. **Prediction Mode**: Choose from two prediction types:
   - **Best Time Finder**: Find optimal travel times for specific time periods
   - **Specific Time Prediction**: Get travel time prediction for exact hour

### Time Period Options
- **Morning**: 6:00 AM - 11:59 AM
- **Afternoon**: 12:00 PM - 4:59 PM  
- **Evening**: 5:00 PM - 8:59 PM
- **Night**: 9:00 PM - 5:59 AM

### Example Usage Flows

#### Flow 1: Find Best Travel Time
```
Do you want to go to Centerpoint or come back from Centerpoint? (Enter 'go' or 'return'): go
Would you like to see the traffic now or prediction?: Prediction
Enter the day you want to see the traffic: Monday
Do you want to know what time you'll reach if you leave at a certain time?: No
What time of day? (morning / afternoon / evening / night): morning

Output: Best hour to leave on Monday is 9:00 with predicted travel time 15.30 minutes.
```

#### Flow 2: Specific Time Prediction
```
Do you want to go to Centerpoint or come back from Centerpoint? (Enter 'go' or 'return'): return
Would you like to see the traffic now or prediction?: Prediction
Enter the day you want to see the traffic: Friday
Do you want to know what time you'll reach if you leave at a certain time?: Yes
Enter the hour (0-23): 14

Output: Predicted travel time on Friday at 14:00 is 18.45 minutes.
```

#### Flow 3: Real-time Traffic
```
Do you want to go to Centerpoint or come back from Centerpoint? (Enter 'go' or 'return'): go
Would you like to see the traffic now or prediction?: Now

Output: Estimated travel time (with traffic): 16.2 minutes
```

## Machine Learning Models

### K-Means Clustering
- Groups similar traffic patterns into 2 clusters for each direction
- Helps identify peak and off-peak traffic periods
- Separate clustering for "go" and "return" journeys

### Random Forest Regressor
- **Automatic Training**: Models are trained when you select direction
- **Dynamic Model Selection**: Uses appropriate model based on chosen direction
- **Feature Engineering**: Utilizes one-hot encoded day columns and hour data
- **Prediction Types**:
  - **Optimal Time Finding**: Scans time ranges to find best travel times
  - **Specific Time Prediction**: Predicts travel time for exact hour
- **Performance Metrics**: Displays R-score and MAE for model evaluation

### Model Training Process
1. Data is loaded and preprocessed with one-hot encoding
2. K-Means clustering identifies traffic patterns
3. Random Forest model trains on selected direction's data
4. Feature columns are automatically configured
5. Model is ready for predictions

## Model Performance

The system displays:
- **R-score**: Coefficient of determination (model accuracy)
- **MAE**: Mean Absolute Error (prediction accuracy in minutes)

## Route Visualization

The system generates static maps showing:
- **Green marker**: Starting point
- **Red marker**: Destination
- **Blue line**: Optimal route
- Maps are saved as PNG files for each direction

## Project Structure

```
traffic-prediction-system/
├── main.py          # Main application file
├── traffic_data_to_go.xlsx       # Historical data (to destination)
├── traffic_data_return.xlsx      # Historical data (from destination)
└── README.md                    # This file
```

## 🔧 Configuration

### Route Coordinates
Current setup uses:
- **Origin**: Centerpoint Metro (25.2302654, 55.3886272)
- **Destination**: Muwaileh Park (25.3061594, 55.4538881)

To change locations, update the coordinates in the code:
```python
destination = "LAT,LONG"  # Your destination coordinates
origin = "LAT,LONG"       # Your origin coordinates
```

## Data Schema

The system collects and uses the following data points:
- `timestamp`: Date and time of data collection
- `hour`: Hour of day (0-23)
- `day`: Day of week
- `travel_time_min`: Travel time in minutes


## Important Notes

- **API Key Security**: Never commit your actual API key to version control
- **Data Privacy**: Traffic data is collected for analysis purposes only
- **Rate Limits**: TomTom API has rate limits - the system includes 10-minute intervals for data collection
- **Local Use**: This current code is configured for UAE locations so you'll have to change the locations for your own use.


## Acknowledgments

- TomTom for providing the routing API
- scikit-learn community for machine learning tools
- Contributors and users of this project

---
