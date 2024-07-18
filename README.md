# Marathon-Runners-Exploratory-Analysis


## Problem Statement

The dataset contains the information about the marathon race and the participants.Based upon this i have gained insights to some interesting information regarding the race.


## Data used

https://www.kaggle.com/datasets/aiaiaidavid/the-big-dataset-of-ultra-marathon-running



** The data was cleaned and trimmed down for ease of purpose.
   
   The location was limited to Usa
   The maximum year was fixed as 2020.
   The race length were only 50mi and 50km
   Few unused columns were trimmed and few were recreated


## Insights

    1. Participants in 50 km and 50 mi race based on gender

        Plotting function:
        sns.histplot(ds3,x='race_length',hue="athlete_gender",element="step")
   ![Screenshot 2024-07-18 130004](https://github.com/user-attachments/assets/2bedfbaa-5ff0-4c1d-8b9d-6631cf480c21)
 
    
    2. Average speed of runners according to participant count

        Plotting function:
        sns.displot(ds3[ds3["race_length"]=="50mi"]["athlete_average_speed"])

   ![Screenshot 2024-07-18 130329](https://github.com/user-attachments/assets/ab4ad826-cf87-43a0-a877-ffe01e5a830e)

 
    
    3. Athletes average speed distribution and comparison with gender

        Plotting function:
        sns.violinplot(data=ds3,x="race_length",y="athlete_average_speed",hue="athlete_gender",split= True,inner="quarts",linewidth=1)
   ![Screenshot 2024-07-18 130811](https://github.com/user-attachments/assets/f6d5c530-d8c0-431a-b352-a16c187fccf5)

 
    
    4. Relation between the average speed and the age

        Plotting function:
        sns.lmplot(data=ds3,x="athlete_age",y="athlete_average_speed",hue="athlete_gender")
   ![Screenshot 2024-07-18 130956](https://github.com/user-attachments/assets/019504bc-cf98-40b2-8158-1b9f4d346bed)

 
    



## Key questions answered:

    1.How does the average speed of the participants in the 50km and 50mi races differ based upon their gender?

        Query used:
        ds3.groupby(["race_length","athlete_gender"])["athlete_average_speed"].mean()

![Screenshot 2024-07-18 123432](https://github.com/user-attachments/assets/a5acd82e-69fc-4b1e-b3ca-4d35875cddbf)
    
    2.What age Groups that perform best in the 50mi?

        Query used:           
        ds3.query('race_length=="50mi"').groupby('athlete_age')["athlete_average_speed"].agg(["mean","count"]).sort_values("mean",ascending=False).query('count>19')

![Screenshot 2024-07-18 124546](https://github.com/user-attachments/assets/bef85a5f-d5fc-4b2d-ba9d-320840fb7b4c)

    3. How does a particular season affect the performance of the participants?
        
        Creation of new columns for querying;

        ds3["race_month"]=ds3["race_day"].str.split(".").str.get(1).astype(int)

        
        ds3["race_season"]=ds3["race_month"].apply(lambda x: 'winter' if (x>11 or x<3) else "spring" if (x>2 and x<6) else "summer" if(x>5 and x<9) else "fall" )


        Query used:
        ds3.groupby("race_season")["athlete_average_speed"].mean().sort_values(ascending=False)

![Screenshot 2024-07-18 125124](https://github.com/user-attachments/assets/3bf1a072-8b4e-4381-b911-6ce0a3347687)
    

## Python funciton used

query(), sort_values(), groupby(), agg(), split(), get(), head(), histplot(), lmplot(), violinplot(), displot(), astype(), dropna(), dtypes, isna(), .shape
