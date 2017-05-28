[Home](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/)|[Account Creation](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/AccountCreation)|[File Upload](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/FileUpload)|[Processing State](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/ProcessingState)|**Get Breakdown**|[Get Closed Captions](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWebVTT)|[Get Widget URLs](https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWidgets)

## Obtaining the Breakdown
The breakdown is the interesting data available after the processing is complete. A breakdown consists of various elements, all of which represent potentially interesting pieces of data about the uploaded video.

As before, we build a GET request with the ID:
```javascript
// id would have been updated/set by the File Upload API call.
var id = "28d53fb324";

var breakdownOptions = {
            url: "https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/" + id,
            headers: {
                "Ocp-Apim-Subscription-Key": APIkey
            },
            method: "GET",
        };
```

And a callback to handle the data:
```javascript
function VideoIndexerStatusCallback(error, response, body) {
        if (!error && response.statusCode < 400) {
            var vi = JSON.parse(body);
            console.log(vi);

            // Here, you can do whatever you wish with the returned data.
            var context = {};

            context.name = vi.name;
            context.id = vi.id;
            context.createTime = vi.createTime;
            context.privacyMode = vi.privacyMode;
            context.duration = vi.durationInSeconds;
            context.thumbnailURL = vi.breakdowns[0].thumbnailUrl;
            context.breakdown = vi.breakdowns[0];

            res.render('statusresult', context);
        }
        else {
            var vi = JSON.parse(body);
            console.log(vi);
        }
```

The JSON response is extremely extensive:
```json
{
  "accountId": "695a52c9-3940-44b4-a3eb-9ffe6b636276",
  "id": "28d53fb324",
  "partition": null,
  "name": "Big Buck Bunny",
  "description": "From Quirks Mode",
  "userName": "Azure Cognitive Services How-To Guide",
  "createTime": "2017-05-28T00:51:18.0367165+00:00",
  "organization": "",
  "privacyMode": "Public",
  "state": "Processed",
  "isOwned": true,
  "isBase": true,
  "durationInSeconds": 60,
  "summarizedInsights": {
    "name": "Big Buck Bunny",
    "shortId": "28d53fb324",
    "privacyMode": 2,
    "duration": {
      "time": "00:01:00",
      "seconds": 60.0
    },
    "thumbnailUrl": "https://www.videoindexer.ai/api/Thumbnail/28d53fb324/5d3f131d-a569-45bb-a08c-7ff086d43403",
    "faces": [
      {
        "id": 1168,
        "shortId": "28d53fb324",
        "name": "Unknown #1",
        "description": null,
        "title": null,
        "thumbnailUrl": "/api/Thumbnail/28d53fb324/521a8c78-25f7-4c89-be26-53760729cb7e",
        "thumbnailFullUrl": "https://www.videoindexer.ai/api/Thumbnail/28d53fb324/521a8c78-25f7-4c89-be26-53760729cb7e",
        "appearances": [
          {
            "startTime": "00:00:37.9350000",
            "endTime": "00:01:00",
            "startSeconds": 37.9,
            "endSeconds": 60.0
          }
        ],
        "seenDuration": 22.1,
        "seenDurationRatio": 0.37457627118644071
      }
    ],
    "topics": [],
    "sentiments": [
      {
        "sentimentKey": "Neutral",
        "appearances": [
          {
            "startTime": "00:00:00",
            "endTime": "00:00:04.2100000",
            "startSeconds": 0.0,
            "endSeconds": 4.2
          },
          {
            "startTime": "00:00:04.2100000",
            "endTime": "00:00:05.5200000",
            "startSeconds": 4.2,
            "endSeconds": 5.5
          },
          {
            "startTime": "00:00:05.5200000",
            "endTime": "00:00:08.0700000",
            "startSeconds": 5.5,
            "endSeconds": 8.1
          },
          {
            "startTime": "00:00:08.0700000",
            "endTime": "00:00:12.2200000",
            "startSeconds": 8.1,
            "endSeconds": 12.2
          },
          {
            "startTime": "00:00:12.2200000",
            "endTime": "00:00:46.5300000",
            "startSeconds": 12.2,
            "endSeconds": 46.5
          },
          {
            "startTime": "00:00:46.5300000",
            "endTime": "00:01:00",
            "startSeconds": 46.5,
            "endSeconds": 60.0
          }
        ],
        "seenDurationRatio": 1.0
      }
    ],
    "audioEffects": [
      {
        "audioEffectKey": "Silence",
        "appearances": [
          {
            "startTime": "00:00:00",
            "endTime": "00:01:00",
            "startSeconds": 0.0,
            "endSeconds": 60.0
          }
        ],
        "seenDurationRatio": 1.0,
        "seenDuration": 60.0
      },
      {
        "audioEffectKey": "Speech",
        "appearances": [
          {
            "startTime": "00:00:04.2100000",
            "endTime": "00:00:12.2200000",
            "startSeconds": 4.2,
            "endSeconds": 12.2
          },
          {
            "startTime": "00:00:46.5300000",
            "endTime": "00:00:47.0200000",
            "startSeconds": 46.5,
            "endSeconds": 47.0
          }
        ],
        "seenDurationRatio": 0.1440677966101695,
        "seenDuration": 8.5
      },
      {
        "audioEffectKey": "HandClaps",
        "appearances": [
          {
            "startTime": "00:00:02.5000000",
            "endTime": "00:00:05.5000000",
            "startSeconds": 2.5,
            "endSeconds": 5.5
          }
        ],
        "seenDurationRatio": 0.050847457627118647,
        "seenDuration": 3.0
      }
    ]
  },
  "breakdowns": [
    {
      "accountId": "695a52c9-3940-44b4-a3eb-9ffe6b636276",
      "id": "28d53fb324",
      "state": "Processed",
      "processingProgress": "10%",
      "externalId": null,
      "externalUrl": null,
      "metadata": null,
      "insights": {
        "transcriptBlocks": [
          {
            "id": 0,
            "lines": [
              {
                "id": 0,
                "timeRange": {
                  "start": "00:00:00",
                  "end": "00:00:04.2100000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:00",
                  "end": "00:00:04.2100000"
                },
                "participantId": -2,
                "text": "",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.0,
            "faces": [],
            "ocrs": [],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": [
                  {
                    "type": 2,
                    "timeRange": {
                      "start": "00:00:02.5000000",
                      "end": "00:00:04.2100000"
                    }
                  }
                ]
              },
              {
                "type": 4,
                "ranges": []
              },
              {
                "type": 5,
                "ranges": [
                  {
                    "type": 5,
                    "timeRange": {
                      "start": "00:00:00",
                      "end": "00:00:04.2100000"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 0,
                "timeRange": {
                  "start": "00:00:00",
                  "end": "00:00:01.5020000"
                },
                "keyFrame": "00:00:01.0430000",
                "shots": [
                  {
                    "id": 0,
                    "timeRange": {
                      "start": "00:00:00",
                      "end": "00:00:01.5020000"
                    },
                    "keyFrame": "00:00:01.0430000"
                  }
                ]
              },
              {
                "id": 1,
                "timeRange": {
                  "start": "00:00:01.5020000",
                  "end": "00:00:04.2100000"
                },
                "keyFrame": "00:00:06.4270000",
                "shots": [
                  {
                    "id": 1,
                    "timeRange": {
                      "start": "00:00:01.5020000",
                      "end": "00:00:04.2100000"
                    },
                    "keyFrame": "00:00:06.4270000"
                  }
                ]
              }
            ],
            "annotations": [
              {
                "name": "tree",
                "timeRanges": [
                  {
                    "start": "00:00:00.0830000",
                    "end": "00:00:01.4180000"
                  }
                ]
              },
              {
                "name": "grass",
                "timeRanges": [
                  {
                    "start": "00:00:00.0830000",
                    "end": "00:00:01.4180000"
                  }
                ]
              },
              {
                "name": "outdoor",
                "timeRanges": [
                  {
                    "start": "00:00:00.0830000",
                    "end": "00:00:01.4180000"
                  }
                ]
              }
            ]
          },
          {
            "id": 1,
            "lines": [
              {
                "id": 1,
                "timeRange": {
                  "start": "00:00:04.2100000",
                  "end": "00:00:05.5200000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:04.2100000",
                  "end": "00:00:05.5200000"
                },
                "participantId": 1,
                "text": "Eimi.",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.5,
            "faces": [],
            "ocrs": [],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": [
                  {
                    "type": 2,
                    "timeRange": {
                      "start": "00:00:04.2100000",
                      "end": "00:00:05.5000000"
                    }
                  }
                ]
              },
              {
                "type": 4,
                "ranges": [
                  {
                    "type": 4,
                    "timeRange": {
                      "start": "00:00:04.2100000",
                      "end": "00:00:05.5200000"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 1,
                "timeRange": {
                  "start": "00:00:04.2100000",
                  "end": "00:00:05.5200000"
                },
                "keyFrame": "00:00:06.4270000",
                "shots": [
                  {
                    "id": 1,
                    "timeRange": {
                      "start": "00:00:04.2100000",
                      "end": "00:00:05.5200000"
                    },
                    "keyFrame": "00:00:06.4270000"
                  }
                ]
              }
            ],
            "annotations": []
          },
          {
            "id": 2,
            "lines": [
              {
                "id": 2,
                "timeRange": {
                  "start": "00:00:05.5200000",
                  "end": "00:00:08.0700000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:05.5200000",
                  "end": "00:00:08.0700000"
                },
                "participantId": -2,
                "text": "",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.0,
            "faces": [],
            "ocrs": [],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": []
              },
              {
                "type": 4,
                "ranges": [
                  {
                    "type": 4,
                    "timeRange": {
                      "start": "00:00:05.5200000",
                      "end": "00:00:05.5200000"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 1,
                "timeRange": {
                  "start": "00:00:05.5200000",
                  "end": "00:00:08.0700000"
                },
                "keyFrame": "00:00:06.4270000",
                "shots": [
                  {
                    "id": 1,
                    "timeRange": {
                      "start": "00:00:05.5200000",
                      "end": "00:00:08.0700000"
                    },
                    "keyFrame": "00:00:06.4270000"
                  }
                ]
              }
            ],
            "annotations": []
          },
          {
            "id": 3,
            "lines": [
              {
                "id": 3,
                "timeRange": {
                  "start": "00:00:08.0700000",
                  "end": "00:00:12.2200000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:08.0700000",
                  "end": "00:00:12.2200000"
                },
                "participantId": 2,
                "text": "Nangong.",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.5,
            "faces": [],
            "ocrs": [],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": []
              },
              {
                "type": 4,
                "ranges": [
                  {
                    "type": 4,
                    "timeRange": {
                      "start": "00:00:08.0700000",
                      "end": "00:00:12.2200000"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 1,
                "timeRange": {
                  "start": "00:00:08.0700000",
                  "end": "00:00:11.9770000"
                },
                "keyFrame": "00:00:06.4270000",
                "shots": [
                  {
                    "id": 1,
                    "timeRange": {
                      "start": "00:00:08.0700000",
                      "end": "00:00:11.9770000"
                    },
                    "keyFrame": "00:00:06.4270000"
                  }
                ]
              },
              {
                "id": 2,
                "timeRange": {
                  "start": "00:00:11.9770000",
                  "end": "00:00:12.2200000"
                },
                "keyFrame": "00:00:11.9360000",
                "shots": [
                  {
                    "id": 2,
                    "timeRange": {
                      "start": "00:00:11.9770000",
                      "end": "00:00:12.2200000"
                    },
                    "keyFrame": "00:00:11.9360000"
                  }
                ]
              }
            ],
            "annotations": [
              {
                "name": "tree",
                "timeRanges": [
                  {
                    "start": "00:00:08.0960000",
                    "end": "00:00:09.4310000"
                  }
                ]
              },
              {
                "name": "grass",
                "timeRanges": [
                  {
                    "start": "00:00:08.0960000",
                    "end": "00:00:09.4310000"
                  },
                  {
                    "start": "00:00:10.7670000",
                    "end": "00:00:12.1020000"
                  }
                ]
              },
              {
                "name": "plant",
                "timeRanges": [
                  {
                    "start": "00:00:12.1020000",
                    "end": "00:00:12.2200000"
                  }
                ]
              }
            ]
          },
          {
            "id": 4,
            "lines": [
              {
                "id": 4,
                "timeRange": {
                  "start": "00:00:12.2200000",
                  "end": "00:00:46.5300000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:12.2200000",
                  "end": "00:00:46.5300000"
                },
                "participantId": -2,
                "text": "",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.0,
            "faces": [
              {
                "id": 1168,
                "thumbnailId": "54803653-d323-451a-bf39-be652dfee01c",
                "ranges": [
                  {
                    "timeRange": {
                      "start": "00:00:37.9350000",
                      "end": "00:00:46.5300000"
                    },
                    "adjustedTimeRange": {
                      "start": "00:00:37.9350000",
                      "end": "00:00:46.5300000"
                    }
                  }
                ]
              }
            ],
            "ocrs": [
              {
                "timeRange": {
                  "start": "00:00:21.5760000",
                  "end": "00:01:00.0950000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:21.5760000",
                  "end": "00:00:46.5300000"
                },
                "lines": [
                  {
                    "id": 0,
                    "width": 0,
                    "height": 0,
                    "language": "English",
                    "textData": "THE PEACH OPEN MOVIE PROJECT",
                    "confidence": 541.54166666666663
                  },
                  {
                    "id": 1,
                    "width": 0,
                    "height": 0,
                    "language": "English",
                    "textData": "PRESENTS",
                    "confidence": 540.0
                  }
                ]
              }
            ],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": []
              },
              {
                "type": 4,
                "ranges": [
                  {
                    "type": 4,
                    "timeRange": {
                      "start": "00:00:12.2200000",
                      "end": "00:00:12.2200000"
                    }
                  }
                ]
              },
              {
                "type": 5,
                "ranges": [
                  {
                    "type": 5,
                    "timeRange": {
                      "start": "00:00:12.2200000",
                      "end": "00:00:46.5300000"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 2,
                "timeRange": {
                  "start": "00:00:12.2200000",
                  "end": "00:00:15.8580000"
                },
                "keyFrame": "00:00:11.9360000",
                "shots": [
                  {
                    "id": 2,
                    "timeRange": {
                      "start": "00:00:12.2200000",
                      "end": "00:00:15.8580000"
                    },
                    "keyFrame": "00:00:11.9360000"
                  }
                ]
              },
              {
                "id": 3,
                "timeRange": {
                  "start": "00:00:15.8580000",
                  "end": "00:00:23.1610000"
                },
                "keyFrame": "00:00:16.0670000",
                "shots": [
                  {
                    "id": 3,
                    "timeRange": {
                      "start": "00:00:15.8580000",
                      "end": "00:00:23.1610000"
                    },
                    "keyFrame": "00:00:16.0670000"
                  }
                ]
              },
              {
                "id": 4,
                "timeRange": {
                  "start": "00:00:23.1610000",
                  "end": "00:00:46.5300000"
                },
                "keyFrame": "00:00:34.7220000",
                "shots": [
                  {
                    "id": 4,
                    "timeRange": {
                      "start": "00:00:23.1610000",
                      "end": "00:00:46.5300000"
                    },
                    "keyFrame": "00:00:34.7220000"
                  }
                ]
              }
            ],
            "annotations": [
              {
                "name": "tree",
                "timeRanges": [
                  {
                    "start": "00:00:24.1210000",
                    "end": "00:00:26.7910000"
                  },
                  {
                    "start": "00:00:26.7920000",
                    "end": "00:00:29.4620000"
                  },
                  {
                    "start": "00:00:29.4630000",
                    "end": "00:00:30.7980000"
                  }
                ]
              },
              {
                "name": "grass",
                "timeRanges": [
                  {
                    "start": "00:00:14.7730000",
                    "end": "00:00:16.1080000"
                  },
                  {
                    "start": "00:00:24.1210000",
                    "end": "00:00:26.7910000"
                  },
                  {
                    "start": "00:00:26.7920000",
                    "end": "00:00:29.4620000"
                  },
                  {
                    "start": "00:00:29.4630000",
                    "end": "00:00:32.1330000"
                  },
                  {
                    "start": "00:00:32.1340000",
                    "end": "00:00:34.8040000"
                  }
                ]
              },
              {
                "name": "outdoor",
                "timeRanges": [
                  {
                    "start": "00:00:17.4440000",
                    "end": "00:00:18.7790000"
                  },
                  {
                    "start": "00:00:21.4500000",
                    "end": "00:00:22.7850000"
                  },
                  {
                    "start": "00:00:22.7860000",
                    "end": "00:00:24.1210000"
                  },
                  {
                    "start": "00:00:28.1270000",
                    "end": "00:00:29.4620000"
                  }
                ]
              },
              {
                "name": "plant",
                "timeRanges": [
                  {
                    "start": "00:00:12.2200000",
                    "end": "00:00:14.7720000"
                  },
                  {
                    "start": "00:00:14.7730000",
                    "end": "00:00:16.1080000"
                  }
                ]
              },
              {
                "name": "flower",
                "timeRanges": [
                  {
                    "start": "00:00:13.4370000",
                    "end": "00:00:14.7720000"
                  },
                  {
                    "start": "00:00:14.7730000",
                    "end": "00:00:16.1080000"
                  }
                ]
              },
              {
                "name": "animal",
                "timeRanges": [
                  {
                    "start": "00:00:16.1080000",
                    "end": "00:00:17.4430000"
                  },
                  {
                    "start": "00:00:17.4440000",
                    "end": "00:00:18.7790000"
                  },
                  {
                    "start": "00:00:20.1150000",
                    "end": "00:00:21.4500000"
                  }
                ]
              },
              {
                "name": "bird",
                "timeRanges": [
                  {
                    "start": "00:00:16.1080000",
                    "end": "00:00:17.4430000"
                  },
                  {
                    "start": "00:00:17.4440000",
                    "end": "00:00:18.7790000"
                  }
                ]
              },
              {
                "name": "bird of prey",
                "timeRanges": [
                  {
                    "start": "00:00:20.1150000",
                    "end": "00:00:21.4500000"
                  }
                ]
              },
              {
                "name": "laying",
                "timeRanges": [
                  {
                    "start": "00:00:45.4880000",
                    "end": "00:00:46.5300000"
                  }
                ]
              }
            ]
          },
          {
            "id": 5,
            "lines": [
              {
                "id": 5,
                "timeRange": {
                  "start": "00:00:46.5300000",
                  "end": "00:00:47.0200000"
                },
                "adjustedTimeRange": {
                  "start": "00:00:46.5300000",
                  "end": "00:00:47.0200000"
                },
                "participantId": 3,
                "text": "Non.",
                "isIncluded": true,
                "confidence": 1.0
              },
              {
                "id": 6,
                "timeRange": {
                  "start": "00:00:47.0200000",
                  "end": "00:01:00"
                },
                "adjustedTimeRange": {
                  "start": "00:00:47.0200000",
                  "end": "00:01:00"
                },
                "participantId": 3,
                "text": "",
                "isIncluded": true,
                "confidence": 1.0
              }
            ],
            "sentimentIds": [],
            "thumbnailsIds": [],
            "sentiment": 0.5,
            "faces": [
              {
                "id": 1168,
                "thumbnailId": "521a8c78-25f7-4c89-be26-53760729cb7e",
                "ranges": [
                  {
                    "timeRange": {
                      "start": "00:00:46.5300000",
                      "end": "00:00:56.2130000"
                    },
                    "adjustedTimeRange": {
                      "start": "00:00:46.5300000",
                      "end": "00:00:56.2130000"
                    }
                  },
                  {
                    "timeRange": {
                      "start": "00:00:56.8390000",
                      "end": "00:01:00"
                    },
                    "adjustedTimeRange": {
                      "start": "00:00:56.8390000",
                      "end": "00:01:00"
                    }
                  }
                ]
              }
            ],
            "ocrs": [],
            "audioEffectInstances": [
              {
                "type": 2,
                "ranges": []
              },
              {
                "type": 4,
                "ranges": [
                  {
                    "type": 4,
                    "timeRange": {
                      "start": "00:00:46.5300000",
                      "end": "00:00:47.0200000"
                    }
                  }
                ]
              },
              {
                "type": 5,
                "ranges": [
                  {
                    "type": 5,
                    "timeRange": {
                      "start": "00:00:47.0200000",
                      "end": "00:01:00"
                    }
                  }
                ]
              }
            ],
            "scenes": [
              {
                "id": 4,
                "timeRange": {
                  "start": "00:00:46.5300000",
                  "end": "00:00:56.2130000"
                },
                "keyFrame": "00:00:34.7220000",
                "shots": [
                  {
                    "id": 4,
                    "timeRange": {
                      "start": "00:00:46.5300000",
                      "end": "00:00:47.8670000"
                    },
                    "keyFrame": "00:00:34.7220000"
                  },
                  {
                    "id": 5,
                    "timeRange": {
                      "start": "00:00:47.8670000",
                      "end": "00:00:56.2130000"
                    },
                    "keyFrame": "00:00:54.8780000"
                  }
                ]
              },
              {
                "id": 5,
                "timeRange": {
                  "start": "00:00:56.2130000",
                  "end": "00:01:00"
                },
                "keyFrame": "00:00:58.3000000",
                "shots": [
                  {
                    "id": 6,
                    "timeRange": {
                      "start": "00:00:56.2130000",
                      "end": "00:01:00"
                    },
                    "keyFrame": "00:00:58.3000000"
                  }
                ]
              }
            ],
            "annotations": [
              {
                "name": "tree",
                "timeRanges": [
                  {
                    "start": "00:00:48.1590000",
                    "end": "00:00:49.4940000"
                  },
                  {
                    "start": "00:00:50.8300000",
                    "end": "00:00:53.5000000"
                  },
                  {
                    "start": "00:00:53.5010000",
                    "end": "00:00:54.8360000"
                  },
                  {
                    "start": "00:00:57.5070000",
                    "end": "00:00:58.8420000"
                  }
                ]
              },
              {
                "name": "grass",
                "timeRanges": [
                  {
                    "start": "00:00:48.1590000",
                    "end": "00:00:50.8290000"
                  },
                  {
                    "start": "00:00:50.8300000",
                    "end": "00:00:53.5000000"
                  },
                  {
                    "start": "00:00:53.5010000",
                    "end": "00:00:56.1710000"
                  }
                ]
              },
              {
                "name": "outdoor",
                "timeRanges": [
                  {
                    "start": "00:00:48.1590000",
                    "end": "00:00:50.8290000"
                  },
                  {
                    "start": "00:00:50.8300000",
                    "end": "00:00:53.5000000"
                  },
                  {
                    "start": "00:00:53.5010000",
                    "end": "00:00:54.8360000"
                  },
                  {
                    "start": "00:00:57.5070000",
                    "end": "00:00:58.8420000"
                  },
                  {
                    "start": "00:00:58.8430000",
                    "end": "00:01:00"
                  }
                ]
              },
              {
                "name": "laying",
                "timeRanges": [
                  {
                    "start": "00:00:46.5300000",
                    "end": "00:00:46.8230000"
                  }
                ]
              },
              {
                "name": "sky",
                "timeRanges": [
                  {
                    "start": "00:00:57.5070000",
                    "end": "00:00:58.8420000"
                  }
                ]
              },
              {
                "name": "person",
                "timeRanges": [
                  {
                    "start": "00:00:58.8430000",
                    "end": "00:01:00"
                  }
                ]
              }
            ]
          }
        ],
        "topics": [],
        "faces": [
          {
            "id": 1168,
            "bingId": null,
            "name": "Unknown #1",
            "thumbnailId": "521a8c78-25f7-4c89-be26-53760729cb7e",
            "description": null,
            "title": null,
            "imageUrl": null,
            "confidence": 0.0,
            "knownPersonId": "00000000-0000-0000-0000-000000000000"
          }
        ],
        "participants": [
          {
            "id": 1,
            "name": "Speaker #1",
            "pictureUrl": null
          },
          {
            "id": 2,
            "name": "Speaker #2",
            "pictureUrl": null
          },
          {
            "id": 3,
            "name": "Speaker #3",
            "pictureUrl": null
          }
        ],
        "contentModeration": {
          "adultClassifierValue": 0.78258,
          "bannedWordsCount": 0,
          "bannedWordsRatio": 0.0,
          "isSuspectedAsAdult": false,
          "isAdult": false
        },
        "audioEffectsCategories": [
          {
            "type": 2,
            "key": "HandClaps"
          },
          {
            "type": 5,
            "key": "Silence"
          },
          {
            "type": 4,
            "key": "Speech"
          }
        ]
      },
      "thumbnailUrl": "https://www.videoindexer.ai/api/Thumbnail/28d53fb324/5d3f131d-a569-45bb-a08c-7ff086d43403",
      "publishedUrl": "https://BreakdownMedia.azureedge.net:443/c53a8b87-5f24-48c7-b070-2e708ceebed9/Big%20Buck%20Bunny.ism/manifest",
      "viewToken": "Bearer=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1cm46bWljcm9zb2Z0OmF6dXJlOm1lZGlhc2VydmljZXM6Y29udGVudGtleWlkZW50aWZpZXIiOiIzZDhmNTI4YS1jNWM4LTRiNTItYjIyZi1kM2M5MmZlNzdmOWIiLCJpc3MiOiJodHRwczovL2JyZWFrZG93bi5tZSIsImF1ZCI6IkJyZWFrZG93blVzZXIiLCJleHAiOjE0OTYwMjk1OTcsIm5iZiI6MTQ5NTk0MzEzN30.1808FiWXpjqX5nz46KzGu9KHu2V4U3g0Nf6HOQbO81g",
      "sourceLanguage": "English",
      "language": "English"
    }
  ],
  "social": {
    "likedByUser": false,
    "likes": 0,
    "views": 0
  }
}
```

<form action="https://jaegermeiste.github.io/MSCognitiveServicesHowToGuide/GetWebVTT">
    <input type="submit" value="Get Closed Captions >" />
</form>
