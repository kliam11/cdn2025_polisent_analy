# CDN 2024-25 Federal Political Sentiment Analysis

Our project responds to this challenge by using sentiment analysis to track and evaluate public perceptions of Canadian federal party leaders. While our original goal was to build a recommendation model using sentiment and polling data from competitive ridings in the Greater Toronto Area, data scarcity and time constraints led us to refine our scope.
We are now focusing on Reddit, a platform with rich political discourse and accessible data, to develop a sentiment analysis model that tracks how federal leaders are viewed over time. This project aims to visualize sentiment trends across monthly intervals, with future potential for near real-time updates. 
We were interested in doing full poll predictions though we concluded that it would be too much to focus on for this project. Our focus for now is on building a strong sentiment analysis pipeline that can offer insights into the evolving landscape of Canadian voter attitudes and potentially complement traditional polling methods.

Our research question is: “How can we use Reddit to identify topic trends, and sentiments towards Canadian parties and leaders to improve campaign messaging?”

## Methodology

### Data collection:

We retrieve 4876 posts in relation to the LPC, CPC and NDP from r/canada r/onguardforthee and r/canadianpolitics, r/ontario and r/alberta using PullPushAPI under the following criteria:
The post contains one of the keywords: ['liberals', 'conservatives', 'ndp', 'poilievre', 'trudeau', 'carney', 'singh']
The post has a minimum score of 100 to indicate reasonable engagement 
Posts are collected within monthly timeframes from March 2024 to February 2025.
They are stored in a single TSV file.
We choose subreddits that appear to have high activity
We then use PRAW to extract all comments or the top 1000 comments per post, whichever comes first. Comments are stored in a monthly-accumulated TSV file. We collect the text, comment ID, associated_post ID and the comment’s score. We have gathered 487658 comments in that time frame.

### Preprocessing:

We remove duplicate IDs from both posts and comments. 
Posts left: 2577
For comments, we apply a loose selection criteria via vectorized operation  to the comments to ensure some relation to federal politics. 
Comments left: 331809
Sanitization:
We remove stopwords, handle special characters, and apply lemmatization.
Comments are tokenized. A max of 512 tokens is required for BERT models. We pad for shorter messages and truncate for larger messages. 

### Analysis:

We conduct various analyses: 
Topic modelling with a BERTTopic of the top 4 topics per month over time ✅
Mentions of candidates over time						  ✅
Mentions of party over time						  	  ✅
Total sentiment numbers within the last year
Sentiment towards each party over time
Topic analysis in correlation to mentioned candidates and underlying sentiment

For topic-candidate sentiment correlation we do the following:
We analyze the comment for a) its sentiment (1 being most positive and -1 being most negative) and its corresponding probability and b) who it is referring to via Named Entity Recognition. 
We assign the comment its target, and a sentiment probability (leaning)
We use the weighting of the sentiment and the adjusted score. 
We then accumulate references and their score X sentiment to assign a popular opinion for the given party
Where negative sentiment reduces the impact and positive increases it 
We correlate over each candidate and topic
