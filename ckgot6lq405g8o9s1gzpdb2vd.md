---
title: "A Python Program that Tweets Fetched Data from the icanhazdadjoke.com"
datePublished: Wed Mar 06 2019 07:17:07 GMT+0000 (Coordinated Universal Time)
cuid: ckgot6lq405g8o9s1gzpdb2vd
slug: a-python-program-that-tweets-fetched-data-from-the-icanhazdadjokecom
canonical: https://dev.to/highcenburg/a-python-program-which-tweets-from-the-icanhazdadjokecom-api-3197
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603612002217/joaYZNgQd.png
tags: twitter, apis, python3

---

*Cover photo by https://icons8.com/*

%[https://soundcloud.com/mrmindful/mindful-vibes-episode-10-jazz-hop-mix]

*The purpose of this program was to automate the jokes I have been posting on my facebook account thru twitter. Although I haven't figured out AWS Lambda yet, I believe I'll get there in no time.* 

This program is run thru IDLE, which, according to [Wikipedia](https://g.co/kgs/iQ9hDM), is bundled with the Mac OS X Python since 1.5.2b1.

We start learning with "[Getting started with the Twitter API](https://projects.raspberrypi.org/en/projects/getting-started-with-the-twitter-api)", then we progress to calling the [icanhazdadjoke api](https://icanhazdadjoke.com) throughout the article.

In this program, I learned 4 things: 

* How to apply & create the Twitter API keys used in this application

* How to use Twython to send tweets using Python

* How to call an API &

* How to search for the right questions on a search engine to get the right answer

#### First, we apply for a Twitter Developer account with the help of [this](https://projects.raspberrypi.org/en/projects/getting-started-with-the-twitter-api/3). Upon completing the application, we will receive an email confirmation from Twitter to confirm our email address. Then we wait for our application to be approved. We can check the status at [developer.twitter.com](https://developer.twitter.com).

#### After getting approved for the developer account, we login to [developer.twitter.com](https://developer.twitter.com) to get our application registered to get our API Keys.
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611975849/x5bYQnP-9.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611978085/_qnraKNFi.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611979983/ocZo0xkGs.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611981999/ZEimSdSEk.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611983680/ZPoxBUFr_.png)

##### Review the developer terms and click create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611985651/ea_Jljklh.png)

#### Click on keys and tokens to view your keys and access tokens then click create under Access token & access token secret.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611987342/bvoW9Gfjh.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611989020/tj65DaKX_.png)


### **Coding the program and sending our first tweet**

#### Create an auth.py file on IDLE and paste your keys inside then hit save

<pre>consumer_key = 'ENTER_YOUR_CONSUMER_KEY'
consumer_secret = 'ENTER_YOUR_CONSUMER_SECRET'
access_token = 'ENTER_YOUR_ACCESS_TOKEN'
access_token_secret = 'ENTER_YOUR_ACCESS_TOKEN'</pre>

#### Create another file and import twython from Twython and the requests module

<pre>from twython import Twython
import requests</pre>

#### Also import the variables from auth.py

<pre>from auth import (
    consumer_key,
    consumer_secret,
    access_token,
    access_token_secret
)</pre>

save this as twitter.py

#### Create a connection with the Twitter API using Twython 

<pre>twitter = Twython(
    consumer_key,
    consumer_secret,
    access_token,
    access_token_secret
)</pre>

#### We'll use the <pre>update_status()</pre> function of Twitter's API to send a tweet

<pre>message = "Hello world!"
twitter.update_status(status=message)
print('Tweeted: ' % message)</pre>

#### Now we run the module by using F5 or from tweet.py -> Run -> Run Module. 

View from the mobile : 
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611990830/ffPlL6FI4.jpeg)

View from the CLI : 
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611992616/WIxgoXD0p.png)

View from the browser :
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611994195/kTJiSol6E.png)


### **Calling the icanhazdadjoke API**

Fortunately, *icanhazdadjoke* can be called without an authentication by the **GET** method. Read through their docs [here](https://icanhazdadjoke.com/api) if you want to learn more

<pre>url = 'https://icanhazdadjoke.com/'
headers = {'Accept': 'application/json'}
joke_msg = requests.get(url, headers=headers).json().get('joke')
print(joke_msg)</pre>

The response is : :raised_hands::raised_hands::raised_hands:
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611995717/qGqv4jCgr.png)

### Now, we play around the codes.

#### Designing the Code Structure

We want the output of the *icanhazdadjoke* tweeted on our twitter account so we use the **url** variable to call their API. I start with the output, going back to calling the API.

**We end up with this code :**

<pre>url = 'https://icanhazdadjoke.com/'
headers = {'Accept': 'application/json'}
joke_msg = requests.get(url, headers=headers).json().get('joke')
twitter.update_status(status=joke_msg)
print("Tweeted: " + joke_msg)</pre>

The output is : :scream::scream::scream:

CLI : 
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611997284/AgtFSZkmy.png)

Browser : 
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603611999031/-sjvPNujs.png)

Mobile : 
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603612000667/llomQGXR_.jpeg)

:raised_hands::scream::raised_hands::scream::raised_hands:

**Next step here would be to figure out AWS Lambda to automate the tweets twice a day and forget about it.**