# ExpandedDataExp
Looking at more League of Legends!

# Problem Identification
The question that immediately jumped out to me was how we could could predict who won a game. Specifically, as a league player myself,  I wondered: Given a players data can we predict if they won or lost?

This question would involve binary classification in which we use given data to determine whether or not we believe the data belongs to one of two categories; in this case that would be a win or a loss. So the result of the game would be the response variable, since that is the varaible we are trying to learn to predict.

As far as what data I'll give myself to look at, I believe that all the information regarding a players in-game performance is fair game. Since our data's seeking to understand how their total performance affected the outcome of the game and if there's a useful correlation that can be used for prediction, it makes sense to look at any metric deemed useful to that end.

In order to measure my success I'll look at...
R^2 since it gives a good overview of how our predictions are doing.

# Baseline Model
My first model was fairly basic and only looked at a handful of metrics of performance to determine whether it predicted a win or a loss. These features included the side of the map they played on, the game length, number of kills, deaths, and assists. Side of the map was nominal and I simply one hot encoded the two sides into 1's and 0's and the rest of the features were quantitative so I simply passed them through my preprocessing straight to the Decision Tree Classifier.

To my shock the model ran surprisingly well. Unsurprisingly, it did well with the data it was trained on obtaining and R^2 of 0.9956, this is to be expected however since it's trained to match this data. The surprise came from it's performance on the test data it had not seen. It had an R^2 of 0.8304 on the R^2 which was much higher than I expected from such a simple model. While far from perfect, this meant that the statistics I looked at were pretty good indicators of how likely a player was to win.

# Final Model

Good Progress
