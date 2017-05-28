[Home](https://jaegermeiste.github.io/VideoIndexerHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/VideoIndexerHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/VideoIndexerHowToGuide/FileUpload)|**Processing State**
[Get Breakdown](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetBreakdown)|[Get Closed Captions](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetWebVTT)|[Get Widgets](https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetWidgets)|[Account Overview](https://jaegermeiste.github.io/VideoIndexerHowToGuide/AccountOverview)

## Obtaining the Processing State

Once a video breakdown id has been obtained from the Video Indexer API (like ```28d53fb324```), the actual video processing is not instantaneous. We can obtain the state of the processing by building a GET request. This is constructed similarly to the POST request used for File Upload, but with a different method. The primary reason to do the request this way is because the header must still contain the API key for obfuscation purposes. Another change is that although this is a get request, we do not pass variables in the URL per se, rather, from this point forward, the video ID becomes part of the path, and the API call is determined by the subfolder appended to that video ID path. In this case, we append ```"/State"``` to get the processing status.

```javascript
// id would have been updated by the File Upload API call.
var id = "28d53fb324";

var progressOptions = {
                    url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/" + id + "/State",
                    headers: {
                        "Ocp-Apim-Subscription-Key": APIkey
                    },
                    method: "GET"
                };
```

As with the File Upload, we need a callback to handle the response to the API call. The below callback adds some lines not seen in the previous callback. ```context``` is just an object containing an assortment of variables, which we extract from the Azure JSON response. ```res.render('upload', context);``` utlizes a Node.js module called [express-handlebars](https://github.com/ericf/express-handlebars) to display a templated web page to the user containing the result of the progress response. The details of express-handlebars are not germane to this guide, but it is important to show that you would normally provide visible feedback to a user to show that things are moving in the background. 

```javascript
function VideoIndexerProgressCallback(error, response, body) {
            if (!error && response.statusCode < 400) {
                var vi = JSON.parse(body);
                console.log(vi);

                var context = {};

                context.id = id;
                context.state = vi.state;
                context.progress = vi.progress;

                res.render('upload', context);  // This call might build a templated page displaying the values.
            }
        }
```

Finally, we can call the API.

```javascript
request(progressOptions, VideoIndexerProgressCallback);
```

The API will return a JSON object initially similar to:
```json
{ 
  "state": "Uploaded", 
  "progress": "" 
}
```
A later call would return a JSON object like this:
```json
{ 
  "state": "Processing", 
  "progress": "15%" 
}
```
Eventually, when processing is complete, the API will return this:
```json
{
  "state": "Processed",
  "progress": ""
}
```

<form action="https://jaegermeiste.github.io/VideoIndexerHowToGuide/GetBreakdown">
    <input type="submit" value="Get Breakdown >" />
</form>

(C)2017 Jason George - Email: georgeja at oregonstate.edu
