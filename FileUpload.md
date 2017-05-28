[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|**File Upload**

## Uploading Files to Video Indexer

File Uploads can be accomplished using either binary transfer of data in in the body of a POST submission (multipart/form-data) or via a publicly accessible URL. For this How-To guide, we will use the latter. At the time of this writing, a good publicly accessible video is found at [https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4](https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4).

<video id="BigBuckBunny" class="video-js vjs-default-skin" controls preload="auto" width="640" height="360">
<source src="https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4" type='video/mp4'>
</video>


Using node.js and request, we can build a simple JavaScript application that submits the public URL to Azure for processing.

First we need to build a POST object. Most of the variables are submitted as if this were a GET request (in the URL); however, the Content Type and API Key fields must be submitted in the POST header.

```javascript
var uploadOptions = {
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=" + req.body.videoName + "&privacy=public&videoURL=" + req.body.videoURL + "&description=" + req.body.videoDesc,
            headers: {
                "Content-Type": "multipart/form-data",
                "Ocp-Apim-Subscription-Key": APIkey
            },
            method: "POST",
        };
```
