The JSON response is extremely extensive (as below). It is very unwieldy but contains all the salient information about the video. Luckily, there is a much more user-friendly way to handle this data that will be examined after we explore Closed Captions.[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/FileUpload)|[Processing State](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState)|[Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)|**Get Closed Captions**|[Get Widget URLs](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets)

## Retrieving Closed Captions

As before, we build a GET request with the ID. This is almost identical to the breakdown request, with the addition of "/VttUrl" to the end of the API request URL:
```javascript
// id would have been updated/set by the File Upload API call.
var id = "28d53fb324";

var captionOptions = {
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/" + id + "/VttUrl",
            headers: {
                "Ocp-Apim-Subscription-Key": APIkey
            },
            method: "GET",
        };
```

And a callback to handle the data:
```javascript
function VideoIndexerWebVTTCallback(error, response, body) {
        if (!error && response.statusCode < 400) {
            var vi = JSON.parse(body);
            console.log(vi);

            // Here, you can do whatever you wish with the returned data.
            var context = {};

            context.url = vi;
            res.render('captiondownloadlink', context);
        }
        else {
            var vi = JSON.parse(body);
            console.log(vi);
        }
```

Then send the request:
```javascript
request(captionOptions, VideoIndexerWebVTTCallback);
```

The response will be a single line string with the donwload URL contained:
```url
https://www.videoindexer.ai/Api/Widget/Breakdowns/28d53fb324/28d53fb324/Vtt
```

```
WEBVTT

00:00:00.000 --> 00:00:04.210


00:00:04.210 --> 00:00:05.520
Eimi.

00:00:05.520 --> 00:00:08.070


00:00:08.070 --> 00:00:12.220
Nangong.

00:00:12.220 --> 00:00:46.530


00:00:46.530 --> 00:00:47.020
Non.

00:00:47.020 --> 00:01:00.000
```

<form action="https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets">
    <input type="submit" value="Get Widget URLs >" />
</form>
