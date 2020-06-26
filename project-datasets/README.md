# Project description

This will be an open-ended project in which each group will be assigned a dataset, described below, and they have to turn data into information. It is expected that students apply the material exposed during the week. Specifically, materials presented in __Visualizations with R__ and __Regression Models - Statistical Learning__. For the presentation, each group should follow this structure:

1. __Exploratory data analysis__: Here we expect the students to apply the visualization techniques presented during the week to gain insight on the characteristics of the variables and the relationships between them. 
2. __Formulate a question of interest__: Here we expect the students to formulate a question to be answered with data, a hypothesis. 
3. __Findings and future work__: Here we expect the students to present their findings. This should be a combination of the insights gained from visual inspection and exploratory data analysis plus some form of formal analysis with techniques learned in the regression workshop. 

# Datasets

Four datasets are included here:

1. ___kobe-bryant.csv___
2. ___lebron-james.csv___
3. ___michael-jordan.csv___
4. ___us-mass-shootings.csv___, 

The first three datasets contain information on three of the best players in NBA history: [Kobe Bryant](https://en.wikipedia.org/wiki/Kobe_Bryant), [Lebron James](https://en.wikipedia.org/wiki/LeBron_James), and [Michael Jordan](https://en.wikipedia.org/wiki/Michael_Jordan). Specifically, each file has the player statistics for each game played. The data comes from the [Basketball-Reference website](https://www.basketball-reference.com). Below you can find a description of the variables.

The last dataset contains information on US Mass shootings of recent history. For each event, the dataset contains demographic information on the shooter, location, fatalities, and others. These data were obtained from the [Mother Jones website](https://www.motherjones.com/politics/2012/12/mass-shootings-mother-jones-full-data/). Like the other datasets, you can find a description of the variables below. 

### Variables for NBA data
- Rk: game of the season
- G: same as Rk
- Month: self explanatory
- Day: self explanatory
- Age.Year: age of the player
- Age.Day: number of days after the birthday
- Tm: players team
- X: indicator variable of home game
- Opp: opposing team
- X.1: indicator variable representing win or lose (also includes point differential)
- GS: indicator of wheter the player started the game
- MP: minutes played
- FG: baskets made
- FGA: baskets attempted
- FG: field goal percentage
- X3P: three pointers made
- X3PA: three pointers attempted
- X3P.: three point percentage
- FT: free throws made
- FTA: free throws attempted
- FT.: free throws percentage
- ORB: offensive rebounds
- DRB: deffensive rebounds
- TRB: total rebounds
- AST: assists
- STL: steals
- BLK: blocks
- TOV: turn overs
- PF: personal fouls
- PTS: points
- GmSc: a metric intended to give a total perspective on a player’s statistical performance in a basketball game (more [here](https://captaincalculator.com/sports/basketball/game-score-calculator/))
- PM: a metric intented to measure a players contribution to the game (more [here](https://www.basketball-reference.com/about/bpm2.html))
- Season: self explanatory
- Date: self explanatory
- Status: either regular season or playoff
- Name: name of the player
- LName: last name of the player

### Variables for US Mass Shootings data
- case: event identifier
- location: location in the US
- date: self explanatory
- summary: summary of the event
- fatalities: number of people killed
- injured: number of people injured
- total_victims: number of afflicted victims
- location_1: general description of where the shooting happened
- age_of_shooter: self explanatory
- prior_signs_mental_health_issues: indicator variable with information of mental health of shooter
- mental_health_details: information on mental healt status
- weapons_obtained_legally: indicator of whether the weapon was obtained legally 
- where_obtained: where the weapon was obtained
- weapon_type: weapon classification
- weapon_details: information on the weapon
- race: race of the shooter
- gender: gender of the shooter
- sources: references
- mental_health_sources: references
- sources_additional_age: references
- lattitude: self explanatory
- longitude: self explanatory
- type: type of shooting
- year: self explanatory




