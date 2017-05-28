[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/FileUpload)|**Processing State**|[Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)

## Obtaining the Processing State

Once a video breakdown id has been obtained from the Video Indexer API (like ```28d53fb324```), the processing is not instantaneous. We can obtain the state of the processing by building a GET request. This is constructed similarly to the POST request used for File Upload, but with a different method. The primary reason to do the request this way is because the header must still contain the API key for obfuscation purposes.

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

As with the File Upload, we need a callback to handle the response to the API call.

```javascript
function VideoIndexerProgressCallback(error, response, body) {
            if (!error && response.statusCode < 400) {
                var vi = JSON.parse(body);
                console.log(vi);

                var context = {};

                context.id - id;
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

The API will eventually return a JSON object similar to:
```json
{ "state": "Uploaded", "progress": "" }
```
or
```json
{ state: 'Processing', progress: '15%' }
```

Move on to [Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)
