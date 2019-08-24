# Capstone

Guide to various notebooks

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















Problem Statement
- Andrew Yang is a 2020 democratic presidential canidate without much name recognition. Many of the ways to increase that name recognition such as cold-calling or going door-to-door do not have high conversion rates in getting people to listen / learn about one's canidate. A more effective solution would be to target groups of people who are more likely to be interested in a canidate's message and then having a conversation with those people.

- My project goal is to identify the topics/interests that are more likely to make one a supporter of Andrew Yang as opposed to other democratic presidential candiates. I plan on doing this with a large amount of reddit data and analysis. Specifically, I plan to identify the people who have most recently posted on the subreddits of various 2020 presidential candiates and then finding the other subreddits they're active in. I then plan to somehow group those subreddits together into a smaller set of features that I can run a classification model over. This should allow me to 1) obtain what groups of subreddits Yang supporters are more likely to be in and 2) attempt to predict what 2020 candiate a random reddit user would be mostly likely to support. I plan to evaluate my model by train-test-splitting my data and evaluating the accuracy of my model on the test dataset.

Additional ideas
- A flask app that predicts your presidential canidate based on your reddit user name
- Sentiment analysis of the tone of the various political subreddits

Risks / Known Things

-- I'm making an assumption that I can somehow group the large number of subreddits on reddit into a manageable set of features, I 'm thinking I may be able to apply a clustering algorithm of some sort by using NLP on their description but I'm not confident in that

-- Of the five subreddits I'm considering using, the ones for Kamela Harris and Joe Biden have very low numbers of subscribers and activity, this may affect my data. To account for this, I'm potentially thinking about doing a classification of Yang vs Not Yang and getting not Yang by combining the subscribers the other candiate subreddits with some users from r/politics (since it's a pretty left leaning sub where most people are likely democrats) or r/voteblue (where most people are presumably a democrat)

-- My methodology only works on active posters, a large chunk of reddit users are generally lurkers which my model won't be trained on

-- Reddit is a poor sample of the US voting demographic as a whole. I'm hoping I can extrapolate out useful data despite that but no this is definitly a potential concern

