# Did I Win My Game?
Looking at more League of Legends!

# Problem Identification
The question that immediately jumped out to me was how we could could predict who won a game. Specifically, as a league player myself, I wondered: Given a players data can we predict if they won or lost?

This question would involve binary classification in which we use given data to determine whether or not we believe the data belongs to one of two categories; in this case that would be a win or a loss. So the result of the game would be the response variable, as that is the varaible we are trying to learn to predict.

As far as what data I'll give myself to look at, I believe that all the information regarding a players in-game performance is fair game. Since our data's seeking to understand how their total performance affected the outcome of the game and if there's a useful correlation that can be used for prediction, it makes sense to look at any metric deemed useful to that end. The only metric the model will not look at is the actual result, because then it wouldn't be much of a prediction!

For simplicity's sake my baseline model will simply be looking at the R^2 values to measure how well my model is performing on the training and test data, since that alone will likely be enough to show me how over or underfit my model is to the data. 

That being said for my final model I'll want a more robust understanding of how my model is performing. After finding the R^2, I'll also go and look at the accuracy, precision, recall, and f1-score of the model to see where any short comings may lie in the model. Furthermore, using that information I'll also construct a confusion matrix for easier visualization. Lets get started!

# Baseline Model
My first model was fairly basic and only looked at a handful of metrics of performance to determine whether it predicted a win or a loss. These features included the side of the map they played on, the game length, number of kills, deaths, and assists. Side of the map was nominal and I simply one hot encoded the two sides into 1's and 0's and the rest of the features were quantitative so I simply passed them through my preprocessing straight to the Decision Tree Classifier.

To my shock the model ran surprisingly well. It did well with the data it was trained on, obtaining an R^2 of 0.9956, this is to be expected however since it's trained to match this data. The surprise came from it's performance on the test data it had not seen. It had an R^2 of 0.8304 which was much higher than I expected from such a simple model. While far from perfect, this meant that the statistics I looked at were pretty good indicators of how likely a player was to win. 

One problem I saw from this result was that the R^2 between the training and testing set were quite different and indicative of overfitting. I'll want to address that problem as I approach my final model.

# Final Model
It was clear to me that the features I already had were good indicators. I felt that my model would only require a a few additions and tweaks to become great. With that in mind, I added only two new features to my final model. I thought these features would cover the gaps left out by my current model. 

The first feature I added was 'earned gpm' or gold per minute. In a low kill game of league, many players might have little to no kills, but big differences can arise based on how many smaller minions the players are killing, which can lead to big gold discrepancies. These gold differences can be game winning or losing and I thought that this would be an important inclusion to my model.

Next, I'd tried to stay focused on the individual player with my other stats, but it's impossible to ignore that league is a team game. One player alone cannot tell the whole story and while I still wanted to keep my focus on an individual players performance, I allowed my model one indulgence 'teamkills' so that it could get a rough estimate as to how the rest of the team was playing and how the game was going.

Besides adding these two features I also made some changes to them and my previous features to hopefully make them more meaningful. Firstly, I believed that gamelength by itself could tell the model nothing about a win or a loss and was only useful to put the other features into perspective. So, I used gamelength to transform kills, deaths, assists, and teamkills into those stats per second. Let me explaing why this is more indicative of a win of a loss. If I told someone that a player had 15 kills, they might say that's not many for a game that lasted thirty minutes. But, if the game was only 15 minutes then that meant it was a lot of kills. Thus, when time is accounted for these statistics are more meaningful.

Finally, these transformed columns were hard to follow since now they were pretty small decimals since it was kills per second and so on, to make it more understandable I converted all these quanitiative columns to their z-scores. This would make it easier for a viewer to understand how these numbers compare to the other values in that feature.

Now I had new and improved features, but still had to address the overfitting that occured. It was time to find what hyperparameter would work best, so that my Decision Tree Classifier wasn't going to deep and overfitting to my training data.

I ran the model with different depths (ranging from 1 to 20) to test how they performed, and discovered that a depth of 7 minimized my testing error. It resulted in an R^2 of 0.9118 for my training data and 0.9083 for my testing data, which I was really happy with.

I also wanted to make sure that there wasn't too much bias towards false positives or negatives so I decided to look at the confusion matrix and gather some statistics to measure the models performance. I found:

Accuracy was: 0.9082502412351239

Precision was: 0.9104106512672442

Recall was: 0.9069191434963247

F1 score was: 0.9086615433877682

<img src="assets/conf_matrix.png">

These scores all looked pretty good. This indicated a healthy, successful model.

# Fairness Analysis
I wanted to understand where some of my models pitfalls might still lie. Personally, I was interested in how our model was able to handle players with high numbers of kills and if the model had the same accuracy when dealing with players that had less kills. Since my kills data was already broken down into z-score I decided to split up these two sets of players right down the middle on 0, the average of my z-scores. Players with more than a z-score of 0 would be my "high kill" players, while players with less than or equal to a z-score of 0, would be my "low/avg kill" players. I would evaluate the model based on its accuracy.

My null hypothesis for the permutation test was that the model is fair and that the accuracy between high kill and low/avg kill players would be the same. The alternate hypothesis was that the model is more accurate for players with low/avg kill values. The test statistic would be the diffurence in accuracy.

Here were the results:

<iframe src="assets/High_Kills.html" width=600 height=600 frameBorder=0></iframe>

The resulting p-value was 0, and thus less than the significance level of 0.05. This indicates our data supports the alternative hypothesis that the model is better for players with low/avg levels of kills.

# Conclusion
While my final model might still work better for some data than others, I'm very pleased with its results. It performs highly on almost every metric I used to gauge its performance and it generalizes almost as well as it trains!

