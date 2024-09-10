
# Brainstation Capstone - Formula 1 Predictive Modeling

![Formula 1 Predictive Modeling](https://github.com/Manny-Brar/Brainstation_Capstone/blob/a5ebdf5364a125699ed359491746b3d4646fd716/F1%20Predictive%20Analytics.jpg)

## Project Summary:
This project aims to explore and develop predictive analytics models that utilizes historical race data, weather conditions, and car performance metrics to forecast race outcomes in Formula 1. 

This project will utilize machine learning algorithms to make predictions on AVG Race Lap Times and will explore an extensive dataset that includes lap times from past races and qualification lap results, tire usage, pit stop strategies, weather conditions, and additional pertinent variables from historical Formula 1 races. The predictive model developed from this analysis will undergo thorough training, testing, and validation phases using these datasets, to confirm its precision and dependability in predicting outcomes.

![SWOT Analysis](https://github.com/Manny-Brar/Brainstation_Capstone/blob/a5ebdf5364a125699ed359491746b3d4646fd716/F1%20Predictive%20Analytics%20(1).jpg)



## Data Sources:
[Formula 1](https://www.formula1.com/en/results.html/2024/races.html) | 
[Ergast API](https://ergast.com/mrd/) | 
[FastAPI](https://theoehrly.github.io/Fast-F1-Pre-Release-Documentation/api.html#module-fastf1.api) | 


## Data Dictionary(Updated:Apr 1, 2024)    

GP Winners Dataframe:

| Column                  | Description |
| :---                    | --- |
| Date                    | Date of Grand Prix Race | 
| Grand_Prix              | Location of Grand Prix | 
| Winner                  | Winning Driver of Grand Prix |
| Team                    | Team of Winning Driver |
| Laps                    | Total Laps Completed | 
| RaceTime_sec            | Total Race Duration in Seconds | 
| AVG_Lap_Time_sec        | Average Lap Time For Winning Driver | 


Quali Results Dataframe:

| Column                  | Description |
| :---                    | --- |
| Race_Winner             | GP Winner |
| Race_AVG_Lap_Time_sec   | GP Winners AVG Lap Time (Race) |
| Time                    | Time of Event
| Driver                  | Quali Driver |
| DriverNumber            | Quali Driver # |
| LapTime                 | Quali Lap Time |
| LapNumber               | Quali Lap Number |
| Stint                   | Quali Stint Number |
| PitOutTime              | Pit Stop Exit Time |
| PitInTime               | Pit Stop Entry Time |
| Sector1Time             | Sector 1 Time for Lap |
| Sector2Time             | Sector 2 Time for Lap |
| Sector3Time             | Sector 3 Time for Lap |
| SpeedI1                 | Speed Trap 1 (km/h) |
| SpeedI2                 | Speed Trap 2 (km/h) |
| IsPersonalBest          | Is Lap Personal Best? |
| Compound                | Tyre Compound |
| TyreLife                | Tyre Life in Laps Completed |
| FreshTyre               | Is Tyre Fresh |
| Team                    | Drivers Team |
| TrackStatus             | Track Status |
| Position                | Quali Position |
| Deleted                 | Is Lap Deleted? |
| Year                    | GP Year |
| Grand Prix              | GP Location |
| Event_id                | Event ID |


Weather Dataframe:

| Column                  | Description |
| :---                    | --- |
| Time                    | Time of Weather Snapshot |
| AirTemp                 | Air Temp |
| Humidity                | Humidity |
| Pressure                | Air Pressure |
| Rainfall                | Rainfall on Track |
| TrackTemp               | Track Level Temp |
| WindDirection           | Wind Direction |
| WindSpeed               | Wind Speed on Track |
| Year                    | Year of GP |
| Grand Prix              | Grand Prix Location |
| Event_id                | Event ID |


## Data Pipeline & Infrastructure

![Data Infrastructure](https://github.com/Manny-Brar/Brainstation_Capstone/blob/a5ebdf5364a125699ed359491746b3d4646fd716/GCP%20horizontal%20framework%20(3).png)


## Code

      
    def get_urls():
        global YEARS
        global all_race_url
        global url
        
        YEARS=list(range(1950,2024)) 
        all_race_url=[]
        for year in YEARS:
            url=f'https://www.formula1.com/en/results.html/{year}/races.html'
            all_race_url.append(url)
        return all_race_url
    
    get_urls()
    
    #^^^ We know F1 has been going since 1950, so 'YEARS', is a list of numerical values from 1950-2023 ^^^
    #^^^ Next I wrote a simple loop, utilizing f string to iterate through the list of YEARS 
    # and input year into the url and appended to an empty list as 'all_race_url'

Creating a list of URL's to scrape from formula1.com


    """Function to check status_code for url requests.
      Will iterate through list of url's"""

    def status_code():
        global result
        global soup
        for url in all_race_url:
            result=requests.get(url)
            soup=BeautifulSoup(result.content)
            result.status_code
            if result.status_code != 200:
                print(url,'STATUS_CODE ERROR')
                break
            else:
                None
        print('SUCCESS: ALL STATUS CODES=200')
        
    status_code()


## ML Model Exploration

      # Defining the model architecture
      model = Sequential([
          Dense(93, input_shape=(X_train_preprocessed.shape[1],), activation='relu'),
          Dropout(0.25),
          Dense(36, activation='relu'),
          Dropout(0.25),
          Dense(63, activation='relu'),
          Dropout(0.25),
          Dense(96, activation='relu'),
          Dropout(0.25),
          Dense(93, activation='relu'),
          Dropout(0.25),
          Dense(96, activation='relu'),
          Dropout(0.25),

          Dense(1, activation='linear')  # Change to linear activation for regression
          #Dense(1, activation='softmax')
          #Dense(1, activation="sigmoid")
      ])

      # Compiling the model
      model.compile(optimizer='adam', loss='mean_squared_error', metrics=['mean_absolute_error'])
      #model.compile(optimizer='adam', loss=keras.losses.BinaryCrossentropy(), metrics=[keras.metrics.BinaryAccuracy()])


      # Model summary 
      model.summary()


      ########################

      # Training the model
      history = model.fit(X_train_preprocessed, y_train, epochs=300, batch_size=6, validation_split=0.2)

      # Evaluating the model
      loss, mae = model.evaluate(X_test_preprocessed, y_test)
      print(f'Test loss: {loss:.4f}')
      print(f'Test mean absolute error: {mae:.4f}')


![ML Model Exploration](https://github.com/Manny-Brar/Brainstation_Capstone/blob/d9f043402e7722362ca901d8f833e02c176a2609/F1%20Predictive%20Analytics%20(2).png)

![Neural Network Result Dash]([https://github.com/Manny-Brar/F1_Winner_Prediction_NeuralNetwork/blob/0d1649cb02f8bce204be11fa83177d73bfcc044f/1.jpg])
