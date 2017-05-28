 Unfortunately, this can dnot be done client-side because the AP{I[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|**File Upload**|[Processing State](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState)|[Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)|[Get Closed Captions](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWebVTT)|[Get Widget URLs](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets)

## Uploading Files to Video Indexer

File Uploads can be accomplished using either binary transfer of data in in the body of a POST submission (multipart/form-data) or via a publicly accessible URL. For this How-To guide, we will use the latter. At the time of this writing, a good publicly accessible video is found at [https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4](https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4).

<video id="BigBuckBunny" class="video-js vjs-default-skin" controls preload="auto" width="640" height="360">
<source src="https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4" type='video/mp4'>
</video>


Using node.js and request, we can build a simple JavaScript application that submits the public URL to Azure for processing. 
NOTE: Unfortunately, this cannot be done client-side because the PAI does not return a CORS header and the browser will error with
```
Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

First we need to build a POST object. Most of the variables are submitted as if this were a GET request (in the URL); however, the Content Type and API Key fields must be submitted in the POST header.

```javascript 
// The below variables could be collected from a form, or hardcoded as seen here 
var APIkey = 'YourAPIKeyHere'; 
var videoName = "Big Buck Bunny"; 
var videoURL = "https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4"; 
var videoDesc = "Submitted via POST request to REST API"; 
 
// This uploadOptions object will be submitted in the request. Effectively, it represents the HTTP request header. 
var uploadOptions = { 
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=" + videoName + "&privacy=public&videoURL=" + videoURL + "&description=" + videoDesc, 
            headers: { 
                "Content-Type": "multipart/form-data", 
                "Ocp-Apim-Subscription-Key": APIkey 
            }, 
            method: "POST", 
        }; 
``` 

Next, we need a request callback function. This function can be fairly simple, as the File Upload API will only return an identifying hash code like ```28d53fb324```.

```javascript
var id;     // Will be like 28d53fb324

function VideoIndexerUploadCallback(error, response, body) {
            if (!error && response.statusCode < 400) {
                id = JSON.parse(body);
                console.log(id);
            }
        }
```

Then we can send the request:
```javascript
request(uploadOptions, VideoIndexerUploadCallback);
```

When the callback is called, the API will respond with a single value like ```28d53fb324```. Store that value for later (as in the callback).

<form action="https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState">
    <input type="submit" value="Processing State >" />
</form>
