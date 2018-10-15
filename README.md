
# Temporality of POTUS' tweets:

I am currently enrolled in this course at Stanford continuing studies school titled: _POL 35 â€” Journalism Under Siege? Truth and Trust in a Time of Turmoil_ (Course link: https://continuingstudies.stanford.edu/courses/liberal-arts-and-sciences/journalism-under-siege-truth-and-trust-in-a-time-of-turmoil/20181_POL-35) 

Week-2 of the course covered the topic "Power to the People: Holding the Powerful Accountable". During the second half of the class, two journalists Sally Buzbee (Executive Editor, The Associated Press) and  Daniel Dale (Correspondent, The Toronto Star) referred to some of the new challenges that have emerged during President-45's administration. They spoke of three aspects of the tweet-storm that emanated from the POTUS himself: _Velocity_, _Source_ and _Timing_

By source, I mean the person and the device from which the tweets were literally typed.
With regards to the temporal aspects, I was exposed to these interesting claims that a substantial chunk of the tweets were being tweeted in the wee hours of the morn and that this timing coincided with the TV show titled 'Fox and Friends'.

Below, I study and quantify this claim in terms of numbers.

# Step uno: Install tweepy if you haven't:


```python
!pip install tweepy
```

    Requirement already satisfied: tweepy in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (3.6.0)
    Requirement already satisfied: PySocks>=1.5.7 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from tweepy) (1.6.7)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from tweepy) (1.0.0)
    Requirement already satisfied: requests>=2.11.1 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from tweepy) (2.18.4)
    Requirement already satisfied: six>=1.10.0 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from tweepy) (1.11.0)
    Requirement already satisfied: oauthlib>=0.6.2 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from requests-oauthlib>=0.7.0->tweepy) (2.1.0)
    Requirement already satisfied: idna<2.7,>=2.5 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from requests>=2.11.1->tweepy) (2.6)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from requests>=2.11.1->tweepy) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from requests>=2.11.1->tweepy) (2018.1.18)
    Requirement already satisfied: urllib3<1.23,>=1.21.1 in c:\toolkits.win\anaconda3-4.4.0\lib\site-packages (from requests>=2.11.1->tweepy) (1.22)
    


```python
# General:
import tweepy           # To consume Twitter's API
import pandas as pd     # To handle data
import numpy as np      # For number computing
from tqdm import tqdm
# For plotting and visualization:
from IPython.display import display
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

# Authentication credentials for using the twitter API:


```python
from credentials import CONSUMER_KEY, CONSUMER_SECRET,ACCESS_TOKEN,ACCESS_SECRET 
```

# Actually tweet scraping:

## Help source: https://gist.github.com/yanofsky/5436496

_Note that Twitter only allows access to a users most recent 3240 tweets with this method_


```python

#authorize twitter, initialize tweepy
auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
auth.set_access_token(ACCESS_TOKEN,ACCESS_SECRET)
api = tweepy.API(auth)

screen_name="realDonaldTrump"

#initialize a list to hold all the tweepy Tweets
alltweets = []	

#make initial request for most recent tweets (200 is the maximum allowed count)
new_tweets = api.user_timeline(screen_name = screen_name,count=200)


```

Neat. Now. let's explore the object new_tweets object that is returned.

```
dir(new_tweets[0])

['__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getstate__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 '_api',
 '_json',
 'author',
 'contributors',
 'coordinates',
 'created_at',
 'destroy',
 'entities',
 'favorite',
 'favorite_count',
 'favorited',
 'geo',
 'id',
 'id_str',
 'in_reply_to_screen_name',
 'in_reply_to_status_id',
 'in_reply_to_status_id_str',
 'in_reply_to_user_id',
 'in_reply_to_user_id_str',
 'is_quote_status',
 'lang',
 'parse',
 'parse_list',
 'place',
 'possibly_sensitive',
 'quoted_status',
 'quoted_status_id',
 'quoted_status_id_str',
 'retweet',
 'retweet_count',
 'retweeted',
 'retweets',
 'source',
 'source_url',
 'text',
 'truncated',
 'user']
 
 ```
 
 Should you feel goaded to explore further, here are the resources. I will revisit these in the forthcoming notebooks:

Resource page:

<img src="tweet_objects.png",width=600,height=600>

Links:

https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object.html


https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/user-object.html



Note to self: Interesting attributes from the twitter API documentation.

```
list_components=[ 'coordinates',
 'created_at',
 'destroy',
 'entities',
 'favorite',
 'favorite_count',
 'favorited',
 'geo',
 'id',
 'id_str',
 'in_reply_to_screen_name',
 'in_reply_to_status_id',
 'in_reply_to_user_id',
 'is_quote_status',
 'lang',
 'parse',
 'parse_list',
 'place',
 'possibly_sensitive',
 'retweet',
 'retweet_count',
 'retweeted',
 'retweets',
 'source',
 'source_url',
 'text',
 'truncated',
 'user']
for comp in list_components:
    
    print('\n ___________')
    print(comp)
    print(new_tweets[0].__getattribute__(comp))
    
```

# Let's take a small detour to study the author field itself and save it as a dataframe:


```python
author_attrib_list=dir(new_tweets[0].author)
df_useful_attribs=pd.DataFrame(columns=['field','Author_attribute'])
ind=0
for attrib in author_attrib_list:
    
    if((attrib[0]!='_')&(attrib!='entities')):
        df_useful_attribs.loc[ind,'field']=attrib
        df_useful_attribs.loc[ind,'Author_attribute']=new_tweets[0].author.__getattribute__(attrib)
        ind+=1
df_useful_attribs.to_csv('df_aattribs.csv',index=False)
df_useful_attribs
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>field</th>
      <th>Author_attribute</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>contributors_enabled</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>created_at</td>
      <td>2009-03-18 13:46:38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>default_profile</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>default_profile_image</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>description</td>
      <td>45th President of the United States of AmericaðŸ‡ºðŸ‡¸</td>
    </tr>
    <tr>
      <th>5</th>
      <td>favourites_count</td>
      <td>25</td>
    </tr>
    <tr>
      <th>6</th>
      <td>follow</td>
      <td>&lt;bound method User.follow of User(_api=&lt;tweepy...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>follow_request_sent</td>
      <td>False</td>
    </tr>
    <tr>
      <th>8</th>
      <td>followers</td>
      <td>&lt;bound method User.followers of User(_api=&lt;twe...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>followers_count</td>
      <td>55128105</td>
    </tr>
    <tr>
      <th>10</th>
      <td>followers_ids</td>
      <td>&lt;bound method User.followers_ids of User(_api=...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>following</td>
      <td>False</td>
    </tr>
    <tr>
      <th>12</th>
      <td>friends</td>
      <td>&lt;bound method User.friends of User(_api=&lt;tweep...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>friends_count</td>
      <td>47</td>
    </tr>
    <tr>
      <th>14</th>
      <td>geo_enabled</td>
      <td>True</td>
    </tr>
    <tr>
      <th>15</th>
      <td>has_extended_profile</td>
      <td>False</td>
    </tr>
    <tr>
      <th>16</th>
      <td>id</td>
      <td>25073877</td>
    </tr>
    <tr>
      <th>17</th>
      <td>id_str</td>
      <td>25073877</td>
    </tr>
    <tr>
      <th>18</th>
      <td>is_translation_enabled</td>
      <td>True</td>
    </tr>
    <tr>
      <th>19</th>
      <td>is_translator</td>
      <td>False</td>
    </tr>
    <tr>
      <th>20</th>
      <td>lang</td>
      <td>en</td>
    </tr>
    <tr>
      <th>21</th>
      <td>listed_count</td>
      <td>94784</td>
    </tr>
    <tr>
      <th>22</th>
      <td>lists</td>
      <td>&lt;bound method User.lists of User(_api=&lt;tweepy....</td>
    </tr>
    <tr>
      <th>23</th>
      <td>lists_memberships</td>
      <td>&lt;bound method User.lists_memberships of User(_...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>lists_subscriptions</td>
      <td>&lt;bound method User.lists_subscriptions of User...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>location</td>
      <td>Washington, DC</td>
    </tr>
    <tr>
      <th>26</th>
      <td>name</td>
      <td>Donald J. Trump</td>
    </tr>
    <tr>
      <th>27</th>
      <td>notifications</td>
      <td>False</td>
    </tr>
    <tr>
      <th>28</th>
      <td>parse</td>
      <td>&lt;bound method User.parse of &lt;class 'tweepy.mod...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>parse_list</td>
      <td>&lt;bound method User.parse_list of &lt;class 'tweep...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>profile_background_color</td>
      <td>6D5C18</td>
    </tr>
    <tr>
      <th>31</th>
      <td>profile_background_image_url</td>
      <td>http://abs.twimg.com/images/themes/theme1/bg.png</td>
    </tr>
    <tr>
      <th>32</th>
      <td>profile_background_image_url_https</td>
      <td>https://abs.twimg.com/images/themes/theme1/bg.png</td>
    </tr>
    <tr>
      <th>33</th>
      <td>profile_background_tile</td>
      <td>True</td>
    </tr>
    <tr>
      <th>34</th>
      <td>profile_banner_url</td>
      <td>https://pbs.twimg.com/profile_banners/25073877...</td>
    </tr>
    <tr>
      <th>35</th>
      <td>profile_image_url</td>
      <td>http://pbs.twimg.com/profile_images/8742761973...</td>
    </tr>
    <tr>
      <th>36</th>
      <td>profile_image_url_https</td>
      <td>https://pbs.twimg.com/profile_images/874276197...</td>
    </tr>
    <tr>
      <th>37</th>
      <td>profile_link_color</td>
      <td>1B95E0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>profile_sidebar_border_color</td>
      <td>BDDCAD</td>
    </tr>
    <tr>
      <th>39</th>
      <td>profile_sidebar_fill_color</td>
      <td>C5CEC0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>profile_text_color</td>
      <td>333333</td>
    </tr>
    <tr>
      <th>41</th>
      <td>profile_use_background_image</td>
      <td>True</td>
    </tr>
    <tr>
      <th>42</th>
      <td>protected</td>
      <td>False</td>
    </tr>
    <tr>
      <th>43</th>
      <td>screen_name</td>
      <td>realDonaldTrump</td>
    </tr>
    <tr>
      <th>44</th>
      <td>statuses_count</td>
      <td>39274</td>
    </tr>
    <tr>
      <th>45</th>
      <td>time_zone</td>
      <td>None</td>
    </tr>
    <tr>
      <th>46</th>
      <td>timeline</td>
      <td>&lt;bound method User.timeline of User(_api=&lt;twee...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>translator_type</td>
      <td>regular</td>
    </tr>
    <tr>
      <th>48</th>
      <td>unfollow</td>
      <td>&lt;bound method User.unfollow of User(_api=&lt;twee...</td>
    </tr>
    <tr>
      <th>49</th>
      <td>url</td>
      <td>https://t.co/OMxB0x7xC5</td>
    </tr>
    <tr>
      <th>50</th>
      <td>utc_offset</td>
      <td>None</td>
    </tr>
    <tr>
      <th>51</th>
      <td>verified</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



# Neat! Now, let's resume and mined the 3200+ most recent tweets:


```python
#save most recent tweets
alltweets.extend(new_tweets)

#save the id of the oldest tweet less one
oldest = alltweets[-1].id - 1

#keep grabbing tweets until there are no tweets left to grab
while len(new_tweets) > 0:
    print("getting tweets before %s" % (oldest))

    #all subsiquent requests use the max_id param to prevent duplicates
    new_tweets = api.user_timeline(screen_name = screen_name,count=200,max_id=oldest)

    #save most recent tweets
    alltweets.extend(new_tweets)

    #update the id of the oldest tweet less one
    oldest = alltweets[-1].id - 1

    print("...%s tweets downloaded so far" % (len(alltweets)))

# #transform the tweepy tweets into a 2D array that will populate the csv	
# outtweets = [[tweet.id_str, tweet.created_at, tweet.text.encode("utf-8")] for tweet in alltweets]

# #write the csv	
# with open('%s_tweets.csv' % screen_name, 'wb') as f:
#     writer = csv.writer(f)
#     writer.writerow(["id","created_at","text"])
#     writer.writerows(outtweets)

# pass
```

    getting tweets before 1044269053274193919
    ...400 tweets downloaded so far
    getting tweets before 1039292858715578373
    ...600 tweets downloaded so far
    getting tweets before 1033500723709992959
    ...800 tweets downloaded so far
    getting tweets before 1027200569096765440
    ...1000 tweets downloaded so far
    getting tweets before 1021711954589696000
    ...1200 tweets downloaded so far
    getting tweets before 1014146835135516671
    ...1400 tweets downloaded so far
    getting tweets before 1008357782410711040
    ...1600 tweets downloaded so far
    getting tweets before 1002027245131661311
    ...1800 tweets downloaded so far
    getting tweets before 993475610377932800
    ...2000 tweets downloaded so far
    getting tweets before 984631073865953279
    ...2200 tweets downloaded so far
    getting tweets before 972271520847466497
    ...2400 tweets downloaded so far
    getting tweets before 960906105982463999
    ...2600 tweets downloaded so far
    getting tweets before 949066181381632000
    ...2800 tweets downloaded so far
    getting tweets before 937309279257792511
    ...2997 tweets downloaded so far
    getting tweets before 928074747316928512
    ...3196 tweets downloaded so far
    getting tweets before 920794597265088511
    ...3243 tweets downloaded so far
    getting tweets before 919009334016856064
    ...3243 tweets downloaded so far
    


```python
n_tweets=len(alltweets)
n_tweets
```




    3243



# I have hand-picked some of the useful tweet attributes that I deem are worthy of analysis besides the timestamps that form the original object of study:


```python
useful_cols=['coordinates',
 'created_at',
 'favorite_count',
 'favorited',
 'geo',
 'id',
 'in_reply_to_screen_name',
 'in_reply_to_status_id',
 'is_quote_status',
 'lang',
 'place',
 'possibly_sensitive',
 'retweet_count',
 'retweeted',
 'source',
 'source_url',
 'text',
 'truncated']
df_tweets=pd.DataFrame(columns=useful_cols)

```


```python
for i in tqdm(range(n_tweets)):
    for col in useful_cols:
        try:
            df_tweets.loc[i,col]=alltweets[i].__getattribute__(col)
        except:
            df_tweets.loc[i,col]='Unavailable'
```

    100%|â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ| 3243/3243 [00:22<00:00, 144.90it/s]
    


```python
df_tweets.to_csv('df_tweets.csv',index=False)
```


```python
df_tweets.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>coordinates</th>
      <th>created_at</th>
      <th>favorite_count</th>
      <th>favorited</th>
      <th>geo</th>
      <th>id</th>
      <th>in_reply_to_screen_name</th>
      <th>in_reply_to_status_id</th>
      <th>is_quote_status</th>
      <th>lang</th>
      <th>place</th>
      <th>possibly_sensitive</th>
      <th>retweet_count</th>
      <th>retweeted</th>
      <th>source</th>
      <th>source_url</th>
      <th>text</th>
      <th>truncated</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>None</td>
      <td>2018-10-14 22:44:59</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1051604811530072066</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
      <td>en</td>
      <td>None</td>
      <td>Unavailable</td>
      <td>9731</td>
      <td>False</td>
      <td>Twitter for iPhone</td>
      <td>http://twitter.com/download/iphone</td>
      <td>RT @realDonaldTrump: I will be interviewed on ...</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>None</td>
      <td>2018-10-14 19:54:20</td>
      <td>51810</td>
      <td>False</td>
      <td>None</td>
      <td>1051561865061502977</td>
      <td>None</td>
      <td>None</td>
      <td>True</td>
      <td>en</td>
      <td>None</td>
      <td>False</td>
      <td>13051</td>
      <td>False</td>
      <td>Twitter for iPhone</td>
      <td>http://twitter.com/download/iphone</td>
      <td>Thank you to NBC for the correction! https://t...</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>None</td>
      <td>2018-10-14 19:45:05</td>
      <td>45492</td>
      <td>False</td>
      <td>None</td>
      <td>1051559539605164034</td>
      <td>None</td>
      <td>None</td>
      <td>True</td>
      <td>en</td>
      <td>None</td>
      <td>False</td>
      <td>9186</td>
      <td>False</td>
      <td>Twitter for iPhone</td>
      <td>http://twitter.com/download/iphone</td>
      <td>Thank you! https://t.co/CrExOML9Mi</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>None</td>
      <td>2018-10-14 19:44:20</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1051559350047821826</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
      <td>und</td>
      <td>None</td>
      <td>False</td>
      <td>4885</td>
      <td>False</td>
      <td>Twitter for iPhone</td>
      <td>http://twitter.com/download/iphone</td>
      <td>RT @IVT2020: @realDonaldTrump https://t.co/cvY...</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>None</td>
      <td>2018-10-14 19:39:16</td>
      <td>48219</td>
      <td>False</td>
      <td>None</td>
      <td>1051558072433463297</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
      <td>en</td>
      <td>None</td>
      <td>Unavailable</td>
      <td>9731</td>
      <td>False</td>
      <td>Twitter for iPhone</td>
      <td>http://twitter.com/download/iphone</td>
      <td>I will be interviewed on â€œ60 Minutesâ€ tonight ...</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



# ANALYSIS

## OK. Now that the data has been collected, let's look at Question-1: What was the literal device source from which the tweets were typed?
The twitter API provides for a 'source' field. [Reference link: 
https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object ]

source	: String	: *Utility used to post the Tweet, as an HTML-formatted string. Tweets from the Twitter website have a source value of web.
Example: "source":"Twitter for Mac"*

So, here are the results:


```python
df_tweets.source.value_counts().plot(kind='barh');
plt.title(df_tweets.source.value_counts());
```


![png](output_21_0.png)



```python
df_tweets.source.value_counts()
```




    Twitter for iPhone    3103
    Media Studio            67
    Twitter for iPad        37
    Twitter Web Client      36
    Name: source, dtype: int64



If you are wondering what 'Media studio' is from where 67 tweets seem to have emanated, here is twitter's official description:
*"Media Studio, which replaces video.twitter.com, is an even more comprehensive desktop destination where you can access all of our video publishing tools and resources in one place."*


https://blog.twitter.com/official/en_au/a/2016/au-launching-twitter-media-studio.html

Also, as far the iPhone usage is concerned, politico had a story on this that you can access here:

https://www.politico.com/story/2018/05/21/trump-phone-security-risk-hackers-601903

# Question-2 - Time aspect of it all:

Before we answer that question, we need t pay a lip service to careful timestamp conversion. According to the API documentation:

Field: 'created_at' :String	: UTC time when this Tweet was created. 

Example: "created_at":"Wed Aug 27 13:08:45 +0000 2008"

OK, so we need to convert all the timestamps from UTC to Wasington DC local time, which would be US/Eastern.  Before, we use the *pytz* solution, let's build some ground-truth for verification. Howevering over the tweet gives you the local time of posting of the tweet. Also, inspecting the source (By either Right- Click + 'inspect source' or Ctrl+Shift+I in chrome) gives you the Unix epoch time stamp that can then be converted to the local time. So, let's check the latest tweet in the image below:


<img src="sanity_check.png",width=600,height=600>

OK, so both the methods point to the fact that the tweet was posted at 15:54 pm Eastern time or 12:54 Pacific time. 


```python
import pytz
from datetime import datetime

est = pytz.timezone('US/Eastern')
utc = pytz.utc

print(df_tweets.text[0], utc.localize(df_tweets.created_at[0]).astimezone(est))
```

    RT @realDonaldTrump: I will be interviewed on â€œ60 Minutesâ€ tonight at 7:00 P.M., after NFL game. Enjoy! 2018-10-14 18:44:59-04:00
    


```python
# SO, LET'S VEREIFY IT!
utc.localize(df_tweets.created_at[0]).astimezone(est).hour
```




    18



 ##  BOOM! 
 Cool. So, let's add a column of *hour_DC* to the dataframe


```python
def convert_ET(ts):
    return utc.localize(ts).astimezone(est)
def convert_ET_hour(ts):
    return utc.localize(ts).astimezone(est).hour
def convert_ET_weekday(ts):
    return utc.localize(ts).astimezone(est).weekday()

```


```python
df_tweets['time_EST']=df_tweets.created_at.apply(convert_ET)
df_tweets['time_hr']=df_tweets.created_at.apply(convert_ET_hour)
df_tweets['time_weekday']=df_tweets.created_at.apply(convert_ET_weekday)
```


```python
plt.figure(figsize=(15,5))
plt.subplot(121)
count=df_tweets.time_weekday.value_counts().sort_index().values
x_vec=df_tweets.time_weekday.value_counts().sort_index().index.values
df_tweets.time_weekday.value_counts().sort_index().plot(kind='bar');
for i in range(len(x_vec)):
    plt.text(x = x_vec[i]-0.25 , y = count[i]+0.2, s = count[i], size = 10)

plt.title('Monday is 0 and Sunday is 6')
plt.ylabel('Number of tweets');
plt.xlabel('Hour of the day');

###############################


plt.subplot(122)
count=df_tweets.time_hr.value_counts().sort_index().values
x_vec=df_tweets.time_hr.value_counts().sort_index().index.values
df_tweets.time_hr.value_counts().sort_index().plot(kind='bar');
for i in range(len(x_vec)):
    plt.text(x = x_vec[i]-0.25 , y = count[i]+0.2, s = count[i], size = 10)

plt.title(' Fox and friends airs from 6 AM to 9 AM')
plt.ylabel('Number of tweets');
plt.xlabel('Hour of the day');
######################

```


![png](output_30_0.png)


So yeah, the peaks between 6 AM and 9 AM is what the press has labelled as 'hyperaggressive early morning tweetstorms' (Link: https://www.politico.com/magazine/story/2018/01/05/trump-media-feedback-loop-216248 ) also widely hypothesized to be fuelled by what seems like a 3 hour show titled 'Fox & Friends' (Never watched it. Don't wake up that early)

Additional link: https://www.mediamatters.org/blog/2017/03/20/bigotry-and-idiocy-donald-trumps-favorite-news-show/215759


```python
ct_day_hr=pd.crosstab(df_tweets.time_weekday,df_tweets.time_hr)
sns.heatmap(ct_day_hr)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2074b6d73c8>




![png](output_32_1.png)

