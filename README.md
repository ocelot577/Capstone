# Capstone

## Executive Summary
Andrew Yang is a 2020 democratic presidential candidate without much name recognition. Many of the ways to increase that name recognition such as cold-calling or going door-to-door do not have high conversion rates in getting people to listen / learn about one's candidate. A more effective solution would be to target groups of people who are more likely to be interested in a candidate's message and then having a conversation with those people. My project goal is to use data from Reddit to identify the topics/interests that are more likely to make one a supporter of Andrew Yang as opposed to other democratic presidential candidates.

I was able to do this by identifying users that posted on the Andrew Yang subreddit, the subreddits of a number of other 2020 presidential candidates, and/or a number of subreddits relating to the democratic party. I then identified other subreddits these users had posted to and grouped the more popular of those subreddits together to form a list of "interests". I also gathered the text of the comments that were posted to the r/politics subreddit to determine if there were specific words/phrases that Yang supporters used more than non-Yang supporters. Finally, I ran a number of classification models of these features to identify the interests/words that are more indicative of a user being a Yang supporter.

I used accuracy and sensitivity scores to evaluate my models and generally optimized for sensitivity. My reasoning is that the true positives (Yang supporters that are predicted to be Yang supporters by my model) are the most valuable for this project since they're able to tell me what the interests of actual Yang supporters are. Also, false positives aren't as big of a deal because it's possible that many of these users aren't Yang supporters because they haven't heard of him and not because they don't actually like him. These might actually be the people who are easiest to convert into Yang supporters.

The models I built were able to successfully identify the interests of Yang supporters and common words they use. In particular, I was able to achieve a sensitivity score of .5778 by using subreddits to predict Yang supporters and .7162 by using NLP and comments on the r/politics subreddit. In addition to the expected interests of Yang supporters (Ex: universal basic income and Asian-American identity), my model also identified some less-expected areas of interest (crypto-currency users, hip hop fans, and marijuana enthusiasts) which could potentially be areas where current Yang supporters who are interested in these fields could perhaps convert others who share those interests to become Yang supporters.

One of the risks with using Reddit data for this analysis is the user demographic of Reddit which generally skews male and younger. This isn't a good representation of the US voting demographic as a whole. I think I was able to mitigate this somewhat since I'm identifying interests from my data which can then be extrapolated to groups of people who don't use Reddit. Another issue is my model only gets data from users who post on Reddit and thus doesn't learn anything about users who browse Reddit but never post, a subset that could actually be the majority of Reddit users

## Explanation of Code and Notebooks

### Data gathering
To obtain data for my project, I used the PRAW library to scrape Reddit. I initially decided to identify users that frequented the subreddits of 2020 democratic canidates Andrew Yang, Pete Buttigieg, Kamala Harris, Joe Biden, and Bernie Sanders and for each user, grab the last 1,000 comments they had posted on Reddit and what subreddits those comments were on. The idea here being that these subreddits would indicate other interests that these users had. On my third scrape, I decided to add some additional subreddits indicative of the democratic party to the list to also gain insights on democrats who had not posted on the subreddits of one of the five canidates (presumably they either supported a different candidate, were undecided or weren't interested in posting on a candidate subreddit)

* [First Scraping Notebook](01_First_Scraping_Notebook.ipynb) - code for my 1st and 2nd scrapes
* [Second Scraping Notebook](02_Second_Scraping_Notebook.ipynb) - code for my 3rd scrape, added a couple subreddits to this scrape since I wanted posts from democrats who hadn't posted on one of the 5 candiate subreddits
* [Combining Scrapes Notebook](03_Combine_Scrapes.ipynb) - used to combine my scrapes into one dataframe

### EDA
One of the features I wanted to use for my model was subreddits or groups of similar subreddits that my users had posted in. To prevent data leakage, I wanted to make sure these features were created from only my training data set. Thus, I'm forced to define my target column at this stage and perform a train test split. I then identify the subreddits with 1,000 or more comments  from this training dataset and export that to a spreadsheet for evaluation.

* [Create Target Column](04_Create_Target_Column.ipynb) - Notebook used to create the target column
* [Identify subreddits](05_Subreddit_Group) - Notebook used ot identify the most popular subreddits
* [Spreadsheet of subreddits](fin_subreddit_group.numbers) - Spreadsheet where I analyzed which subreddits to use as features, which ones to group together, and which ones to discard

### Modeling - Subreddit Features
Once I identified the subreddits I wanted to use as my features, I created a dataframe of users and their comments only from the subreddits in question. Realizing, I had more subreddit features than desired, I then ran a logisitic regression classifier to identify the weakest features and then in a 2nd notebook recreated my dataframe with the weakest features removed. The 2nd logisitic regression classifier I ran achieved a score of .7695 which was a small improvement over the baseline of .7368.

Looking at the conusion matrix of my results, I see my true positives are 40 and my false negatives are 140. My goal is to identify features that predict the positive class so I'm really more interested in increasing my true positives even if that increases my false positives so thus I'm training for sensitivity (currently .2222). One way of doing this is to use an oversampling technique on the minority class. I used the Synthetic Minority Over-sampling Technique (SMOTE) to do this. Doing this caused my accuracy score to drop to .6715 but my sensitivity increased to .5778. From this model, I was able to identify a list of subreddit features that indicate if a Reddit user is more or less likely to be an Andrew Yang supporter.

* [Create subreddit features dataframe & 1st model](06_Subreddit_Features_&_First_Model.ipynb)
* [Remove some features & 2nd model](07_Remove_features_&_2nd_model.ipynb)

### Modeling - NLP Features
Next, I wanted to see if I could predict if a user was an Andrew Yang supporter or not based on the words they used on the r/politics subreddit. This could help with the predictive ability of my model and also identify the topics that Yang supporters talk about.

To do this, I created a dataframe of just the comments from the r/politics subreddit for only the users that I had analyzed in my previous steps. I then combined all of a user's comments on r/politics to a single string. Finally, I cleaned and parsed the data with RegEx to return only word tokens in a string. I did not apply any lemmatization or stop word removal in this step since the stop words that I wanted to remove would depend on what I wanted to do with my NLP data.

I removed stop words and did my lemmatization in my [Model with NLP notebook](10_NLP_model.ipynb). In addition to the standard stop words I wanted to remove, I also added the names of the various political candiadates. The thought here is the use of a candidates name is a very strong predictor of who a user supported and I instead wanted to identify if there were topics the users commented about could predict the candidate that they supported. 

My logisitical regression classifier with TF-IDF vectorization had an accuracy score of .8208 which was a small improvement over the baseline score of .7590. My sensitivity score was .3243. Using SMOTE to oversample my minority class, my sensitivity score increased to .7162.

* [Create NLP feature](08_Create_NLP_features.ipynb)
* [Process and clean NLP](09_Process_&_Clean_NLP.ipynb)
* [Model with NLP](10_NLP_model.ipynb)

### Modeling - Combination of all features
In this notebook, I created a logistic regression classification model using both the subreddit and NLP features. Similar to my previous models, I ended up oversampling my minority class to maximize my sensitivity score at .6944 which is a decrease from my NLP only model. However, my NLP only model only works for users who had posted on r/politics whereas this model can be used for both users who have and have not posted on r/politics. In addition, I also generated an ROC curve for this model (ROC score of .8175) and identified that there would be a benefit on changing the probability threshold from the default probability threshold of .5 since it increases my model's sensitivity without decreasing the specificity too much.

* [Model with both NLP and subreddit features](11_Model_subreddit_&_NLP.ipynb)

### Additional Side Explorations



