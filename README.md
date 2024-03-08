
# Brainstation Capstone - Formula 1 Predictive Modeling

![Formula 1 Predictive Modeling](https://github.com/Manny-Brar/Brainstation_Capstone/blob/578a5104f1d958f25d2cb1f0a252ac0728296194/F1%20Predictive%20Analytics.png)

## Project Summary:
This project aims to explore and develop predictive analytics models that utilizes historical race data, weather conditions, and car performance metrics to forecast race outcomes in Formula 1. 

This project will utilize machine learning algorithms to make predictions on top 5 race placements and will explore an extensive dataset that includes lap times from past races and qualification lap results, tire usage, pit stop strategies, weather conditions, and additional pertinent variables from historical Formula 1 races. The predictive model developed from this analysis will undergo thorough training, testing, and validation phases using these datasets, to confirm its precision and dependability in predicting outcomes.

![SWOT Analysis](https://github.com/Manny-Brar/Brainstation_Capstone/blob/b0dd0aa205abb4732840e4bcee3a96e67ac26cdf/F1%20Predictive%20Analytics%20(1).png)



## Data Sources:
[Formula 1](https://www.formula1.com/en/results.html/2024/races.html) | 
[Ergast API](https://ergast.com/mrd/) | 
[FastAPI](https://theoehrly.github.io/Fast-F1-Pre-Release-Documentation/api.html#module-fastf1.api) | 


## Data Dictionary(Updated:Mar 7, 2024)    

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

![ML Model Exploration](https://github.com/Manny-Brar/Brainstation_Capstone/blob/8e38791c571d972fe2b3b4f94c01fcfa41ef860c/F1%20Predictive%20Analytics%20(2).png)
