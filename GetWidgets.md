[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/FileUpload)|[Processing State](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState)|[Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)|[Get Closed Captions](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWebVTT)|**Get Widgets**|[Account Overview](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountOverview)

## Obtaining Interface Widgets

Thus far, we have performed several API requests and retrieved JSON in return. The Breakdown JSON in particular, while detailed, is extremely ungainly. While an enterprising programmer could make use of the JSON data and build an interesting analytics dashboard, wouldn't it be great if the work had already been done for you?

The Video Indexer API provides two major widgets to allow end users to interact with the video data. The first we will examine is the player widget.

Yet again, we build a GET request with the ID. This is almost identical to the breakdown and WebVTT requests, however, this time we add "/PlayerWidgetUrl" to the end of the API request URL:
```javascript
// id would have been updated/set by the File Upload API call.
var id = "28d53fb324";

var playerOptions = {
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/" + id + "/PlayerWidgetUrl",
            headers: {
                "Ocp-Apim-Subscription-Key": APIkey
            },
            method: "GET",
        };
```

And a callback to handle the data:
```javascript
function VideoIndexerPlayerCallback(error, response, body) {
        if (!error && response.statusCode < 400) {
            var vi = JSON.parse(body);
            console.log(vi);

            // Here, you can do whatever you wish with the returned data.
            var context = {};

            context.url = vi;
            res.render('playerembedlink', context);
        }
        else {
            var vi = JSON.parse(body);
            console.log(vi);
        }
```

Then send the request:
```javascript
request(playerOptions, VideoIndexerPlayerCallback);
```

The response will be a single line string with the player embed URL contained:
```url
https://www.videoindexer.ai/embed/player/28d53fb324
```
Embedding the widget results in this:
<iframe src="https://www.videoindexer.ai/embed/player/28d53fb324" width="800" height="450"></iframe>

The second widget is more interesting. It provides access to (some of) the actual analysis results in a user firendly manner.

For a final time, we build a GET request with the ID. This is almost identical to the previous API requests, however, this time we add "/InsightsWidgetUrl" to the end of the API request URL:
```javascript
// id would have been updated/set by the File Upload API call.
var id = "28d53fb324";

var insightOptions = {
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/" + id + "/InsightsWidgetUrl",
            headers: {
                "Ocp-Apim-Subscription-Key": APIkey
            },
            method: "GET",
        };
```

And a callback to handle the data:
```javascript
function VideoIndexerInsightCallback(error, response, body) {
        if (!error && response.statusCode < 400) {
            var vi = JSON.parse(body);
            console.log(vi);

            // Here, you can do whatever you wish with the returned data.
            var context = {};

            context.url = vi;
            res.render('insightembedlink', context);
        }
        else {
            var vi = JSON.parse(body);
            console.log(vi);
        }
```

Then send the request:
```javascript
request(insightOptions, VideoIndexerInsightCallback);
```

The response will be a single line string with the insights embed URL contained:
```url
https://www.videoindexer.ai/embed/insights/28d53fb324
```

Embedding the widget results in this:
<iframe src="https://www.videoindexer.ai/embed/insights/28d53fb324" width="800" height="450"></iframe>

<form action="https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountOverview">
    <input type="submit" value="Account Overview >" />
</form>
