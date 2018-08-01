# Scraping an instagram hashtag - Working as of April 19th, 2017

**Basic knowledge of how a web browser works and how to use `Python 3` required**

Instagram's API no longer allows you to scrape data from a given hashtag. Thankfully, we can use its backend to get it automatically with the browser, and a simple python program

To get an instagram hashtag, visit this link:

[https://www.instagram.com/explore/tags/hashtag/](https://www.instagram.com/explore/tags/hashtag/)

Where hashtag is the name of your hashtag

Scroll to the bottom of the page and click the `Load More` button

Open your console (`Ctrl` + `Shift` + `i`) and paste in the following javascript code

```javascript
var pictureCount = 1000
```

Change pictureCount to the number of pictures you want to download, then press enter

Now paste the following javascript:

```javascript
var intervalID = window.setInterval(function() {

    if(document.getElementsByClassName("_8mlbc _vbtk2 _t5r8b").length < pictureCount){
        console.log("scrolled bottom")
        window.scrollTo(0,document.body.scrollHeight);

        setTimeout(function(){
                        console.log("scrolled top");
                        window.scrollTo(0,0);
                    },250);
    } else {
        clearInterval(intervalID);
        alert("Finished!")
        if (confirm("Export data")){
            var imgs = document.getElementsByClassName("_8mlbc _vbtk2 _t5r8b")
            var links = []
            for (var img in imgs){
                links.push(imgs[img].href);
            }
            window.open("data:text/json;charset=utf-8," + JSON.stringify(links))
        }
    }


}, 500);
```

Wait until the page has finished scrolling, accept the prompts and allow popups.

Leave that window open.

Create a new python program by copying the following code

```python
import requests
import json
from bs4 import BeautifulSoup
links = 

for link in links:
    req = requests.get(link).text
    soup = BeautifulSoup(req, "html.parser")
    scripts = soup.find_all("script")
    for script in scripts:
        if script.text[:18] == "window._sharedData":
            break

    data = json.loads(script.contents[0][21:-1])
    # print(json.dumps(data, indent=4))
    print("timestamp: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["taken_at_timestamp"]))
    print("caption: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["edge_media_to_caption"]["edges"][0]["node"]["text"]))
    print("user: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["owner"]["username"]))
    print("full name: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["owner"]["full_name"]))
    print("comments: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["edge_media_to_comment"]["count"]))
    print("likes: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["edge_media_preview_like"]["count"]))
    print("url: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["display_url"]))
    print("dimensions: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["dimensions"]))
    print("location: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["location"]))


    if(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["is_video"]):
        print("video: yes")
        print("video views: " + str(data["entry_data"]["PostPage"][0]["graphql"]["shortcode_media"]["video_view_count"]))

    else:
        print("video: no")

    print("################################################")
```


Paste the output of the javascript popup into the links variable. Be sure to remove any nulls that might be generated at the end of the list.

Make sure `BeautifulSoup`, `requests` and `json` are available on your operating system's version of Python.

The program will output like so:

```
date: 1475615708
caption: From @mr_dog_spa #cutepetclub
user: cutepetclub
full name: Cute Pet Club
comments: 597
likes: 21712
url: https://scontent-syd1-1.cdninstagram.com/t51.2885-15/e15/14488289_188979234844412_7114537064384692224_n.jpg?ig_cache_key=MTM1Mzg4NTE1MzQ0MTQyMjYxMg%3D%3D.2
video: yes
video views: 70070
################################################
date: 1475613401
caption: 😍💞😍 TWO PaRTS OF ToP CLiPS iN THe PaST 😍💞😍 ViDeO : @marianne.holmli 
#animals #animal #Pets #pet #dogsofinstagram #dog #puppy #hound #cutepuppy #instapuppy #puppies #cachorrinho #woof #fluffy #paws #cachorro #perro #baby #собака #щенок #babyanimals #funny #chowchow #love #anjing #chowchowpuppy #vine #강아지 #犬 #개 
MY SPESIAL CHOW FRIENDS : 
@SDSTaSiuK @DIGSBY_N_CiNDeReLLa_THe_CHoWS 
@KHePeLKHaN.CHoWCHoW

TaG YouR FRieNDs :👇👥👇
user: chowchow.gallery
full name: CHOWSTAGRAM CHoW CHoW PuPPieS
comments: 363
likes: 4297
url: https://scontent-syd1-1.cdninstagram.com/t51.2885-15/e15/14606962_1323728720995088_3535359629137543168_n.jpg?ig_cache_key=MTM1Mzg2NTc5MzYwNzMwMjI4NQ%3D%3D.2
video: yes
video views: 10639
################################################
date: 1475606702
caption: So precious 💕
@mikelefebvre1
user: ilovegolden_retrievers
full name: I Love Golden Retrievers
comments: 268
likes: 15396
url: https://scontent-syd1-1.cdninstagram.com/t51.2885-15/s640x640/sh0.08/e35/14473915_1105621309485529_7287598845976903680_n.jpg?ig_cache_key=MTM1MzgwOTYwMTA5MDQzMjgxNg%3D%3D.2
video: no
################################################
date: 1475618794
caption: Photo by @lylathebernie
user: babyanimalstagram
full name: Baby Animal Instagram
comments: 212
likes: 11574
url: https://scontent-syd1-1.cdninstagram.com/t51.2885-15/e35/14449346_391509197639414_4618319290473381888_n.jpg?ig_cache_key=MTM1MzkxMTAzNTI4MzQwMjAyNg%3D%3D.2
video: no
################################################
date: 1475607841
caption: pls don't talk to me ( @cabbagecatmemes )
user: chaos.reigns_
full name: chaos reigns
comments: 337
likes: 4498
url: https://scontent-syd1-1.cdninstagram.com/t51.2885-15/e35/14515597_1413720808929307_990947391542657024_n.jpg?ig_cache_key=MTM1MzgxOTE1OTQ0MDM3OTAwNQ%3D%3D.2
video: no
################################################
```

You can modify the code to do more analysis


## Disclaimer
This was written over a 1 hour period; it might be wrong and it will probably break.
