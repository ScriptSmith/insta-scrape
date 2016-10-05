# Scraping an instagram hashtag - Working as of October 5th  2016

**Basic knowledge of how a web browser works and how to use `Python 3` required**

To get an instagram hashtag, visit this link:

[https://www.instagram.com/explore/tags/hashtag/](https://www.instagram.com/explore/tags/hashtag/)

Where hashtag is the name of your hashtag

Scroll to the bottom of the page and click the `Load More` button

Open your console (`Ctrl` + `Shift` + `i`) and paste in the following javascript code

```javascript
var pictureCount = 1000
```

Change pictureCount to the number of pictures you want to download, then press enter

Now past the following code

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
    data = json.loads(soup.find_all("script")[6].contents[0][21:-1])
    # print(json.dumps(data, indent=4))
    print("date: " + str(data["entry_data"]["PostPage"][0]["media"]["date"]))
    print("caption: " + str(data["entry_data"]["PostPage"][0]["media"]["caption"]))
    print("user: " + str(data["entry_data"]["PostPage"][0]["media"]["owner"]["username"]))
    print("full name: " + str(data["entry_data"]["PostPage"][0]["media"]["owner"]["full_name"]))
    print("comments: " + str(data["entry_data"]["PostPage"][0]["media"]["comments"]["count"]))
    print("likes: " + str(data["entry_data"]["PostPage"][0]["media"]["likes"]["count"]))
    print("url: " + str(data["entry_data"]["PostPage"][0]["media"]["display_src"]))


    if(data["entry_data"]["PostPage"][0]["media"]["is_video"]):
        print("video: yes")
        print("video views: " + str(data["entry_data"]["PostPage"][0]["media"]["video_views"]))

    else:
        print("video: no")

    print("################################################")
```

You can modify the code to export in other formats, or get additional data.

## Disclaimer
This was written over a 1 hour period; it might be wrong and it will probaly break.
