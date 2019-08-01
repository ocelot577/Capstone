# Capstone

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

