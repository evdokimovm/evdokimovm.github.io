---
layout: post
title: Scrape images from Craigslist search results with Python and BeautifulSoup
categories: [Python, WebScraping, BeautifulSoup, bs4]
tags: [Python, WebScraping, BeautifulSoup, bs4]
fullview: false
comments: true
description: Scrape images from Craigslist search results with Python and BeautifulSoup. This task is a lil-bit tricky. The thing about it is that the images for item cards in search results sets dynamically with JavaSript by some kind templating script or engine.
---

This task is a lil-bit tricky. The thing about it is that the images for item cards in search results sets dynamically with JavaSript by some kind templating script or engine with template which is looks as this one:

![TEMPLATE](https://i.imgur.com/DQCjwR6.jpg)

Thus when you trying to get the whole HTML of page with the `requests` library or something it delivers without links to images. Moreover it delivers without a single `<img>` tag.

You can check it by yourself. Run the following python code in your [terminal](https://aka.ms/terminal-preview):

{% highlight python %}
import requests

URL = "https://sandiego.craigslist.org/search/sss?query=python"

requests.get(URL).content
{% endhighlight %}

then with the `[CTRL+SHIFT+F]` shortcut try to find any `img` tag related to product cards.

I wouldn't want to use a whole Selenium browser emulator for such a simple task. There must be some additional data here to generate image links for the above templates, right? Of course. If you try to inspect the code of any item card then you will see that the certain part of the link is the same as the string contains in the `data-id` attribute in parent `<a>` tag.

The image IDs separated by comma as you can see on the following image, so each id in the string corresponds to each image in the gallery respectively. The item images on the search results page has same IDs as the images on the actual items page. The only resolution is actually changes, it's `300x300` for search results and `600x450` for full size. I'll use `300x300` in code examples.

![IDS](https://i.imgur.com/lkAr8GA.png)

So now let's get the IDs for each item iteratively and generate some `*.json` file contains the list of dictionaries that include title and list of all image links:

{% highlight python %}
import json
import requests
from bs4 import BeautifulSoup as bs

image_url = "https://images.craigslist.org/{}_300x300.jpg"
URL = "https://sandiego.craigslist.org/search/sss?query=python"
html_content = requests.get(URL).content
soup = bs(html_content, 'html.parser')

res = []

for li in soup.find_all('li', class_='result-row'):
    lia = li.find("a", class_="result-image")
    desc = li.div.h3.a.text
    if 'empty' not in lia['class']:
        ids = lia['data-ids'].replace('3:', '').split(',')
    else:
        ids = ''
    res.append({ 'title': desc, 'photos': [image_url.format(i) for i in ids] })

jsonString = json.dumps(res)
jsonFile = open("res.json", "w")
jsonFile.write(jsonString)
jsonFile.close()
{% endhighlight %}

As a result of executing the code you will receive a `res.json` file with contents of such form:

{% highlight json %}
[
    {
        "title": "Framed art Print of a Black-Headed Python Capturing a Bearded Dragon",
        "photos": [
            "https://images.craigslist.org/00u0u_isZIO2OYkzdz_07K05j_300x300.jpg"
        ]
    },
    {
        "title": "Burmese python albino",
        "photos": [
            "https://images.craigslist.org/00p0p_fvH8fJ1iO1xz_0bC0kE_300x300.jpg",
            "https://images.craigslist.org/00N0N_5HSS6jLkk89z_0bC0kE_300x300.jpg",
            "https://images.craigslist.org/00S0S_hkcoPee2GHLz_0bC0kE_300x300.jpg"
        ]
    },
    {
        "title": "Ball python and home",
        "photos": [
            "https://images.craigslist.org/00X0X_7To7SQFX1Njz_0CI0cA_300x300.jpg",
            "https://images.craigslist.org/00000_lmKzCJaOom9z_0t20CI_300x300.jpg"
        ]
    }
]
{% endhighlight %}
