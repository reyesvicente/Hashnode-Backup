---
title: "Developing my local COVID-19 side project"
datePublished: Mon Apr 27 2020 02:57:43 GMT+0000 (Coordinated Universal Time)
cuid: ckgos4lqn05hencs14zbm4scz
slug: developing-my-local-covid-19-side-project-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682770347015/b80491cb-a7e4-46cc-a5be-092fae10ce58.jpeg
tags: digitalocean, docker, django, apis, python3

---

#### Introduction

The main plan was to develop a COVID-19 tracker in Django that shows local data in the Philippines - until the Department of Health hid and disabled page where the stats can be scraped.

I did some crowdsourcing on Facebook and everyone shared a photo of the daily stats per city - which wasn't what I expected - but it was my fault, my question wasn't clear.

#### Coding

I already have experience in playing with API's so I thought it would be easier now since I could just use my code as a reference.

I found [Chris Michael's](https://covid19-docs.chrismichael.now.sh/) from google and found it easy to use. The API returned:

```bash
{
"report":
    {
     "country":"philippines",
     "flag":"https://www.worldometers.info/img/flags/small/tn_rp-flag.gif",
     "cases":7579,
     "deaths":501,
     "recovered":862,
     "active_cases":[],
     "closed_cases":[]
    }
}
```

The data isn't complete but that was fine. So in calling and using the API, I ended up with:

`api.py`

```bash
import requests


def  get_data():

    url =  "https://covid19-server.chrismichael.now.sh/api/v1/ReportsByCountries/philippines"

    headers = {'Accept': 'application/json'}

    r = requests.get(url, headers=headers)

    stat = r.json()

    stat_data = {

    'stat': stat['report']

    }

    return stat_data
```

Which was pretty much the same as my first API experience:

{% link https://dev.to/highcenburg/a-python-program-which-tweets-from-the-icanhazdadjokecom-api-3197 %}

Although this was not my first time using an API in Python, this was, however, my first time integrating an API with Django. I ended up with:

`views.py`

```bash
...
from django.views.generic import ListView
from covid_virus_tracker.users import api
...

class  HomeView(ListView):

    def  get(self, request):  

        stat_data = api.get_data()

        return render(request, 'pages/home.html', stat_data)
```

However, the `api.py` was registering as a python module and I honestly had a hard time figuring out why. Until I peeked into the source code and figured out that when importing in `cookiecutter-django`, you've to start from the project, down to the app, then to the file, if coming from another app.

Ex: `from project_name.users.models import view`

Then on the `pages/home.html`, all I had to do was:

`{{ stat.recovered}} {{ stat.cases }} {{ stat.deaths }}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610227287/9nL4baPAG.png align="left")

However, I felt like the site needed something more. I felt like I had to let the users know what's happening all over the world, so I integrated an API that scrapes blogs with a COVID-19 keyword. I found RapidAPI, which I had an account already, and found the contextualsearch API. The API returned:

```bash
{
    "type": "news",
    "didUMean": "",
    "totalCount": 2922,
    "relatedSearch": [
        "coronavirus",
        "ist",
        "updated",
        "pm",
        "daily",
        "cases",
        "delhi",
        "india",
        "published",
        "times",
        "sign",
        "video online"
    ],
    "value": [
        {
            "title": "Grounded jazz saxophonist plans to 'dream big' after lockdown",
            "url": "https://www.smh.com.au/culture/music/grounded-jazz-saxophonist-plans-to-dream-big-after-lockdown-20200427-p54nis.html?ref=rss&utm_medium=rss&utm_source=rss_feed",
            "description": "The coronavirus Great Pause trashed Flora Carbo's plans for a world tour with a new album. Now she's fighting snails and making plans.",
            "body": "Very large text size\nARTIST IN THE TIME OF COVID-19: Flora Carbo\nJazz saxophonist Flora Carbo has recently released a new album, Voice. The next logical step? A launch then a tour  two things the Fairfield artist cant do thanks to the coronavirus.\nI had a six-week European trip cancelled; I was going to play at the Amersfoort Jazz Festival [Netherlands], attend the Jazzahead! conference in Germany and collaborate with some friends.\nJazz saxophonist Flora Carbo.\nCarbo was also going to capitalise on some serious buzz with gigs at the Melbourne International Jazz Festival (now cancelled, natch), where shes previously played with Maceo Parker and the Meltdown Big Band.\nAdvertisement\nShe was all set to front Flora Carbo Trio in the lunchtime series and I was going to be playing with [trumpeter] Niran Dasika as part of his group\".\nNow, Carbo is fighting snails out of her vegie garden, re-booking gigs, completing sudokus and 100 per cent subsisting on hot chocolate\".\nIm very lucky to have be",
            "keywords": "covid,time",
            "language": "en",
            "isSafe": true,
            "datePublished": "2020-04-27T01:35:36",
            "provider": {
                "name": "smh"
            },
            "image": {
                "url": "https://static.ffx.io/images/$zoom_1.32375%2C$multiply_0.7554%2C$ratio_1.776846%2C$width_1059%2C$x_0%2C$y_245/t_crop_custom/q_86%2Cf_auto/8a1b5ccca1398947c8147aae6400e5a7fecf2359",
                "height": 450,
                "width": 800,
                "thumbnail": "https://rapidapi.contextualwebsearch.com/api/thumbnail/get?value=5941184483138831277",
                "thumbnailHeight": 168,
                "thumbnailWidth": 298,
                "base64Encoding": null,
                "name": null,
                "title": null,
                "imageWebSearchUrl": null
            }
        }
    ]
}
```

Integrating was easier now since I've done it on the `api.py`. I ended up with:

```bash
import requests

import environ

env = environ.Env()


def get_data():
    url = "https://contextualwebsearch-websearch-v1.p.rapidapi.com/api/Search/NewsSearchAPI"
    querystring = {"autoCorrect": "false", "pageNumber": "1", "pageSize": "10", "q": "covid", "safeSearch": "true"}
    headers = {
      'x-rapidapi-host': "contextualwebsearch-websearch-v1.p.rapidapi.com",
      'x-rapidapi-key': env("x-rapidapi-key"),
    }
    r = requests.get(url, headers=headers, params=querystring)
    data = r.json()
    article_data = {
      'data': data['value']
    }
    return article_data
```

Notice that one difference is having to `import environ` and creating an `env = environ.ENV()` variable. I need this to hide my API keys and this is how `cookiecutter-django` hides it. All I had to do was place the keys in the `.envs/local/.django` & `.envs/production/.django` and I was good to go.

The `views.py` is:

```bash
...
from django.views.generic import ListView
from covid_virus_tracker.users import services
...

class  IndexData(ListView):

  

def  get(self, request):

    article_data = services.get_data()

    return render(request, 'pages/news.html', article_data)
```

On the `pages/news.html`, all I needed to do was loop through the data to show it. Like this:

```bash
{% for item in data %}
{{ item.title }}
{{ item.datePublished|slice:":10" }} # | naturaltime from humanize didn't work so I had to slice through the date
{{ item.description }}
{{ item.provider.name }}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1603610228557/1SAW5A96A.png align="left")

The site is live and can be seen at https://covid19ph-unofficial.icvn.tech/.