[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/FileUpload)|[Processing State](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState)|[Get Breakdown](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetBreakdown)|**Get Closed Captions**|[Get Widget URLs](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets)

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

This file, if downloaded, contained timed text closed captions in the WebVTT format, primarily useful for accessibility. Each caption has a start (on) and end (off) time, during which any associated text should be displayed. Read more on WebVTT captioning at [http://www.w3.org/TR/webvtt1/](http://www.w3.org/TR/webvtt1/)

The contents of the WebVTT for our Big Blue Bunny video are limited, because the 1 minute clip has no significant talking.
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

Much more interesting captions are available from a different video (id=2fe32f6540):
```tap
WEBVTT

00:00:00.000 --> 00:00:03.790
April is a month or the military child and all

00:00:03.873 --> 00:00:05.850
of us who have children.

00:00:05.850 --> 00:00:09.490
That are in the military know that their sacrifices.

00:00:09.490 --> 00:00:13.337
Those children pay a tremendous price for being in a

00:00:13.411 --> 00:00:18.220
military family and we as military service member Pate tremendous

00:00:18.294 --> 00:00:19.330
price as well.

00:00:19.330 --> 00:00:23.570
We miss the school plays we miss the school games

00:00:23.656 --> 00:00:25.560
we missed tournaments.

00:00:25.560 --> 00:00:31.140
We may miss graduations. The parent-teachers conferences that we miss.

00:00:31.140 --> 00:00:34.456
It is truly a sacrificed that way is military members

00:00:34.518 --> 00:00:37.959
and military children pay suit so to all those military

00:00:38.021 --> 00:00:39.210
children out there.

00:00:39.210 --> 00:00:42.910
Thank you. Thank you very much for allowing your parents

00:00:42.976 --> 00:00:45.685
to do what they love to do and that serve

00:00:45.751 --> 00:00:48.790
in America's military from all of us to all of

00:00:48.790 --> 00:00:51.520
you. We know it's tough,

00:00:51.520 --> 00:00:55.639
but military kids are strong army strong airborne all the

00:00:55.711 --> 00:00:56.000
way.
```


<form action="https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets">
    <input type="submit" value="Get Widget URLs >" />
</form>
