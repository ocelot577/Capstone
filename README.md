# Capstone

## Executive Summary
- Andrew Yang is a 2020 democratic presidential candidate without much name recognition. Many of the ways to increase that name recognition such as cold-calling or going door-to-door do not have high conversion rates in getting people to listen / learn about one's canidate. A more effective solution would be to target groups of people who are more likely to be interested in a canidate's message and then having a conversation with those people. My project goal is to use data from Reddit to identify the topics/interests that are more likely to make one a supporter of Andrew Yang as opposed to other democratic presidential candiates.

Specifically, I plan to identify the people who have most recently posted on the subreddits of various 2020 presidential candiates  and then find the other subreddits they're active in. I then plan to group these subreddits together into a smaller set of features that I can run a classification model over. I also plan to use NLP on the comments that these users posted to see if there are any specific topics that Yang supporters talk about more than non-Yang supporters.

I will be using accuracy and sensitivity scores to evaluate my models and generally optimizing for sensitivity. My reasoning is that true positives are the most valuable for me since they're able to tell me what the interests of actual Yang supporters are. Also, false positives aren't as big of deal because it's possible that many of these users aren't Yang supporters because they haven't heard of him and not because they don't actually like him. These might actually be the people who are easiest to convert into Yang supporters.

The models I built were able to successfully identify the topics and interests of Yang supporters. In particular, I was able to achieve a sensitivity score of .5778 by using subreddits to predict Yang supporters and .7162 by using NLP and comments on the r/politics subreddit.

One of the big risks with using Reddit data for this analysis is the user demographic of Reddit which generally skews male and younger. This isn't a good representation of the US voting demographic as a whole. Despite this, I think I mitigate this risk somewhat since I'm identifying interests from my data which can then be extrapolated to groups of people who don't use Reddit. Another issue is I only end up catching users who post on Reddit which only captures a certain percentage of Reddit users. The remaining users who only browse and do not post can't be learned about using my methodologies.

## Explanation of Code and Notebooks

### Data gathering
To obtain data for my project, I used the PRAW library to scrape Reddit. I initially decided to identify users that frequented the subreddits of 2020 democratic canidates Andrew Yang, Pete Buttigieg, Kamala Harris, Joe Biden, and Bernie Sanders and for each user, grab the last 1,000 comments they had posted on Reddit and what subreddits those comments were on. The idea here being that these subreddits would indicate other interests that these users had. On my third scrape, I decided to add some additional subreddits indicative of the democratic party to the list to also gain insights on democrats who had not posted on the subreddits of one of the five canidates (presumably they either supported a different candidate, were undecided or weren't interested in posting on a candidate subreddit)

[First Scraping Notebook](First_Scraping_Notebook.ipynb)
- code for my 1st and 2nd scrapes
[Second Scraping Notebook](Second_Scraping_Notebook.ipynb)
- code for my 3rd scrape, added a couple subreddits to this scrape since I wanted posts from democrats who hadn't posted on one of the 5 candiate subreddits
[Combining Scrapes Notebook](Combine_Scrapes.ipynb)
- used to combine my scrapes into one dataframe

### EDA
One of the features I wanted to use for my model was subreddits or groups of similar subreddits that my users had posted in. To prevent data leakage, I wanted to make sure these features were created from only my training data set. Thus, I'm forced to define my target column at this stage and perform a train test split. I then identify the subreddits with 1,000 or more comments  from this training dataset and export that to a spreadsheet for evaluation.

[Create Target Column](Create_Target_Column.ipynb)
- Notebook used to create the target column
[Identify subreddits](Subreddit_Group)
- Notebook used ot identify the most popular subreddits
[Spreadsheet of subreddits](fin_subreddit_group.numbers)
- Spreadsheet where I analyzed which subreddits to use as features, which ones to group together, and which ones to discard

### Modeling - Subreddit Features
Once I identified the subreddits I wanted to use as my features, I created a dataframe of users and their comments only from the subreddits in question. Realizing, I had more subreddit features than desired, I then ran a logisitic regression classifier to identify the weakest features and then in a 2nd notebook recreated my dataframe with the weakest features removed. The 2nd logisitic regression classifier I ran achieved a score of .7695 which was a small improvement over the baseline of .7368.

Looking at the conusion matrix of my results, I see my true positives are 40 and my false negatives are 140. My goal is to identify features that predict the positive class so I'm really more interested in increasing my true positives even if that increases my false positives so thus I'm training for sensitivity (currently .2222). One way of doing this is to use an oversampling technique on the minority class. I used the Synthetic Minority Over-sampling Technique (SMOTE) to do this. Doing this caused my accuracy score to drop to .6715 but my sensitivity increased to .5778. From this model, I was able to identify a list of subreddit features that indicate if a Reddit user is more or less likely to be an Andrew Yang supporter.

[Create subreddit features dataframe & 1st model](Subreddit_Features_&_First_Model.ipynb)
[Remove some features & 2nd model](Remove_features_&_2nd_model.ipynb)

### Modeling - NLP Features
Next, I wanted to see if I could predict if a user was an Andrew Yang supporter or not based on the words they used on the r/politics subreddit. This could help with the predictive ability of my model and also identify the topics that Yang supporters talk about.

To do this, I created a dataframe of just the comments from the r/politics subreddit for only the users that I had analyzed in my previous steps. I then combined all of a user's comments on r/politics to a single string. Finally, I cleaned and parsed the data with RegEx to return only word tokens in a string. I did not apply any lemmatization or stop word removal in this step since the stop words that I wanted to remove would depend on what I wanted to do with my NLP data.

I removed stop words and did my lemmatization in my [Model with NLP notebook](NLP_model.ipynb). In addition to the standard stop words I wanted to remove, I also added the names of the various political candiadates. The thought here is the use of a candidates name is a very strong predictor of who a user supported and I instead wanted to identify if there were topics the users commented about could predict the candidate that they supported. 

My logisitical regression classifier with TF-IDF vectorization had an accuracy score of .8208 which was a small improvement over the baseline score of .7590. My sensitivity score was .3243. Using SMOTE to oversample my minority class, my sensitivity score increased to .7162.

[Create NLP feature](Create_NLP_features.ipynb)
[Process and clean NLP](Process_&_Clean_NLP.ipynb)
[Model with NLP](NLP_model.ipynb)

### Modeling - Combination of all features
In this notebook, I created a logistic regression classification model using both the subreddit and NLP features. Similar to my previous models, I ended up oversampling my minority class to maximize my sensitivity score at .6944 which is a decrease from my NLP only model. However, my NLP only model only works for users who had posted on r/politics whereas this model can be used for both users who have and have not posted on r/politics. In addition, I also generated an ROC curve for this model (ROC score of .8175) and identified that there would be a benefit on changing the probability threshold from the default probability threshold of .5 since it increases my model's sensitivity without decreasing the specificity too much.

[Model with both NLP and subreddit features](Model_subreddit_&_NLP.ipynb)

### Additional Side Explorations



