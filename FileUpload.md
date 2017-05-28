[Home](https://jaegermeiste.github.io/VideoIndexerHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/VideoIndexerHowToGuide/AccountCreation)|**File Upload**|[Processing State](https://jaegermeiste.github.io/VideoIndexerHowToGuide/ProcessingState)
[Get Breakdown](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetBreakdown)|[Get Closed Captions](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetWebVTT)|[Get Widgets](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetWidgets)|[Account Overview](https://jaegermeiste.github.io/VideoIndexerHowToGuide/AccountOverview)

## Uploading Files to Video Indexer

File Uploads can be accomplished using either binary transfer of data in in the body of a POST submission (multipart/form-data) or via a publicly accessible URL. For this How-To guide, we will use the latter. At the time of this writing, a good publicly accessible video is found at [https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4](https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4).

> NOTE: The Azure API documentation suggests that uploading a file to OneDrive (shared publicly) is a viable option; however, as of this writing, attempting to do so results in a JSON error suggesting that "Partner Uploads" are not working.

<video id="BigBuckBunny" class="video-js vjs-default-skin" controls preload="auto" width="800" height="450">
<source src="https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4" type='video/mp4'>
</video>


Using node.js and request, we can build a simple JavaScript application that submits the public URL to Azure for processing. 
> NOTE: Unfortunately, this cannot be done client-side (or in a JSFiddle) because the API does not return a CORS header and the browser will error with 
> ``` Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. ```

Node.js and request are better explained elsewhere, but in a nutshell, [Node.js](https://nodejs.org/en/) is a JavaScript runtime primarily intended for server-side applications and [request](https://github.com/request/request) is a package built on that runtime that allows the construction of [POST and GET HTTP requests](https://www.w3schools.com/tags/ref_httpmethods.asp).

First we need to build a POST object. Most of the variables are submitted as if this were a GET request (appended to the URL); however, the Content Type and API Key fields must be submitted in the header. The latter requirement is for security through obfuscation and applies to every request we make of the API, whether it is a POST or GET request.

First, we will set some variables. These will be used to build our POST request. Each variable is a string, and naturally you would replace ```YourAPIKeyHere``` with the actual API key from the profile page of your account.
```javascript 
// The below variables could be collected from a form, or hardcoded as seen here 
var APIkey = 'YourAPIKeyHere'; 
var videoName = "Big Buck Bunny"; 
var videoURL = "https://www.quirksmode.org/html5/videos/big_buck_bunny.mp4"; 
var videoDesc = "Submitted via POST request to REST API";
```

Next, we will build an options object. request has the ability to make simple requests by calling the URL directly, but it is easier to set up an options object for more complex requests like this API requires. First we set the ```url:``` parameter to the file upload URL specified by the API, concatenating on the variables we set above. We then set header parameters. ```"Content-Type"``` is hard-coded to ```"multipart/form-data"``` - hardcoded because it cannot be any other type per the API requirements. ```"Ocp-Apim-Subscription-Key"``` is set to the APIkey variable set above. Finally, the ```method:``` is set to ```"POST"```. File uploads are the only HTTP request that requires a POST object. All future API calls will be GET.
```javascript
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

Next, we need a request callback function. The request call will complete almost instantly, as it sends the POST request off to Azure. When Azure does its thing and responds, the callback function will be called by the runtime. In this case, the function can be fairly simple, as the File Upload API will only return an identifying hash code like ```28d53fb324```, which we need to store.

```javascript
var id;     // Global, will eventually be set to a value like 28d53fb324

function VideoIndexerUploadCallback(error, response, body) {
            if (!error && response.statusCode < 400) {
                id = JSON.parse(body);
                console.log(id);
            }
        }
```

Then we can send the request, which takes the options object and callback function name as parameters:
```javascript
request(uploadOptions, VideoIndexerUploadCallback);
```

The request will complete (effectively) instantaneously. Once Azure processes the upload (or errors out), a response will be sent to the server and the callback function will be called to handle the response. When the callback is called, and assuming the upload request was successful, the API will respond with a single value like ```28d53fb324```. That value is a GUID for the uploaded video breakdown. Store the value for later (done in the callback).

<form action="https://jaegermeiste.github.io/VideoIndexerHowToGuide/ProcessingState">
    <input type="submit" value="Processing State >" />
</form>
