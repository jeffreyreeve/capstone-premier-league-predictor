# capstone-premier-league-predictor
A Python machine learning program that predicts final scores for Premier League matches

Motivation: I got the idea for this capstone project from something that has become a passion of mine in recent years, English Premier League Soccer. In conjunction with NBC Sports, they run a contest called "Premier League Pick 'Em." Each week, they pick 6 games and you have to try and call the exact, correct final scores of each game. The best I have done playing this contest is to predict 3 games correctly, predict the winner of 2 other games, and predict 1 game incorrectly. I figured that with enough data, I could create a machine learning model to predict final scores. 

Data: Data proved to be a major challenge in this project! I gathered data from three primary sources: Football-Data.co.uk, FBREF.com, and FIFA.

The Football-Data.co.uk data lists each match in the 2022-2023 Premier League season and provides match specific stats for each team in a single row. This data is active in that it was updated with each match played. The FBREF.com data was pulled manually from a mountain of data on their website. It provides team specific statistics for every squad in the league, but importantly it is static data. I was unable to pull dynamic data for these statistics. Finally, the FIFA data was taken from the data set that the international governing body of soccer publishes annually. It is also team specific and static. 

There were 380 rows and 215 columns. Each row represents a single match. There are also columns toward the end that are the same (same statistic) except some start with H and some start with A. This represents Home team stats and Away team stats.

Models: I used 3 different models: Ridge Regression, Neural Network, and Recurrent Neural Network. I started with Linear Regression but that was not a good match for what I'm trying to do. I actually got a negative figure for correct predictions  :)

I did a little better using a Ridge Regression to predict final scores. Correct predictions improved vs Linear Regression but was close to 0. MAE (mean absolute error) was about 1 goal. This is a recurring theme in my first 2 models. Also, for these models, I had to do 2 goal predictions. One for the home team and another for the away team. For some reason, the correct prediction percentage for the away team is always slightly worse, while MAE is slightly better.

I then moved on to running Neural Networks. I started with my full data set and found that results improved but were still not especially impressive. Tinkering with hidden layers and alpha produced 12.5% correct goal prediction percentage and mean absolute error remained around 1 for the home team predictions. Away team correct goal prediction was lower than home team but MAE dropped a bit.
I also used the team stats that were part of my full data set to see how they performed in the neural network. Interestingly, this produced some of my best results thus far. For the home team, correct prediction percentage came in at 14.4% with MAE of around 1. Away team predictions were not as good, coming in near zero and MAE again similar to using the full data set.

I had done some research and found lots of data science work has been done on the Premier League using Recurrent Neural Networks. One piece in Medium used an RNN to purportedly get 92% "accuracy" in predicting match winners. I don't really believe this claim. The article also mentioned a strategy that I decided to use. I call it the "5 previous games approach." The strategy is one where you look at the 5 games a team played previous to the game you're predicting.

I reformatted my data set so each game had 2 row, one for the home team and one for the away team. I added a numbered ‘GameID’ column and indexed on this to keep the games straight. I was also able to create a function that allows you to input a team name and Game ID and it will return data on the previous 5 games played.

I then ran an LSTM with 60 neurons in the initial layer, 30 neurons in a single dense layer, and 1 output layer. The model produced the same goal prediction for every match. Interestingly, adding more neurons and dense layers resolved the problem. In this case, sufficient complexity is necessary for the LSTM to perform.

I found that an LSTM Recurrent Neural Network with 384 neurons in the initial layer, 256 neurons each in 2 dense layers, and 1 output layer produced the best results. This produced 34% correct prediction percentages for final scores over the entire 2022-2023 Premier League season. It’s amazing to consider what the model is doing when you look at the model.summary() output! As a follow up, I took a look at how the model did when predicting match winner. I changed the architecture to 256 neurons in the initial layer, 256 neurons each in 2 dense layers, and 1 output layer. It produced good results predicting which team would score more goals, with nearly 72% correct prediction percentage.

Insights: Having completed this project, I can draw some insights from my labor. The first is that data is super important! It's not enough to simply gather lots of data like I did. More important is EDA and determining the relevance and predictive value of data. My sense is that having maybe one or two dozen good features would have been ideal. I believe having a mix of dynamic and static data did not help. Next time I would ensure all data used is updated every match, especially the team data. I believe I can get better results if I do this. Finally, I chose to only use data from 1 full season but I think having 3 seasons of data would produce better predictions. While things can change from one season to the next, it takes about 3 years to see the major differences in teams over time.

I found it very interesting that using 60 neurons with 2 dense layers of 30 neurons each was not enough to make predictions (it predicted the same thing for every match.) The model needed to have 384 neurons in the initial layer and 256 neurons in 2 dense layers to work well. Despite all the difficulty, you can get an LSTM to do some pretty incredible things. I was able to achieve 34% correct predictions for final match scores and 72% correct predictions for match winners over an entire season. Given my limited knowledge and experience, I'm actually pretty happy with my results.

For whatever reason, my models did better at predicting home team goals than away team goals, despite having all the same data. Both correct prediction percentage and error were lower for away team vs. home team.

There is a tradeoff between correct prediction percentage and error when making predictions. My correctly predicted goals scored went from 14% to 34% but error increased from around 1 goal to around 1.4 goals.

Linear and ridge regression models are not good for what I was trying to do. Even a regular neural network did not produce especially high correct prediction percentages for final scores or match winner

There seems to be some value in the betting house statistics for Premier League matches. That was the only dynamic data in my data set and it produced pretty good results.

In conclusion, I am struck by the complexity of soccer games and all the variables (features) that go into a final score or match result. Determining data relevance/predictiveness is truly an art. Setting up LSTM's that work well and produce results is a skill that requires practice. I would love to see the gambling outlet's models. Despite its challenges, I enjoyed this project and will continue it when our class ends.

