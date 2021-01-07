# Liverooms

## Get a Liveroom Websocket connection

```shell
curl "/liveRoom/getData/{liveRoomId}" \
  -H "Authorization: meowmeowmeow"
```

This endpoint returns a Websocket connection for the requested liveroom.

### WSS Request

`GET /liveRoom/getData/{liveRoomId}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
liveRoomId | none | The ID of the liveroom

<aside class="success">Example — <code>https://apidev.gamelive.gg/liveRoom/getData/5fd39cbfa54d753101b443d1</code></aside>
<aside class="notice">Requires auth token</aside>
<aside class="warning">Rename this endpoint</aside>


## Get a Specific Liveroom

```shell
curl "/liveRoom" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
    "id": "5fd39cbfa54d753101b443d1",
    "name": "Joujou",
    "imgUrl": "/images/logo_transparent.png",
    "admins": [
        {
            "userId": "5fd386c0ebbd377cf944bda0"
        }
    ],
    "sessions": {
        "5fd39cbfa54d753101b443d3": {
            "id": "5fd39cbfa54d753101b443d3",
            "isCurrent": true,
            "dateBegin": "2020-12-11T16:22:23.51Z",
            "isSuper": false,
            "isHidden": false,
            "roomId": "5fd39cbfa54d753101b443d1",
            "questions": [
                {
                    "id": "5fd9e4c0add7ea00019a0b66",
                    "roomId": "5fd39cbfa54d753101b443d1",
                    "sessionId": "5fd39cbfa54d753101b443d3",
                    "status": "closed",
                    "answersCount": {
                        "A": 0
                    },
                    "answersCountPercentage": {
                        "A": 0
                    },
                    "bet": {
                        "id": "5cb5d528b118043ef145e67d",
                        "matchId": "",
                        "gameNumber": 0,
                        "ref": "LOL-PRONO",
                        "matchDateTime": "0001-01-01T00:00:00Z",
                        "accurate": false,
                        "aknowledged": false,
                        "timestamp": "2020-08-31T13:52:05.319Z",
                        "valid": false,
                        "basePoints": 0,
                        "multipliers": {
                            "odd": 0,
                            "perfect": 0
                        },
                        "liveBetDuration": "0001-01-01T00:00:00Z",
                        "score": {
                            "blueTeamScore": 0,
                            "redTeamScore": 0
                        },
                        "rosters": [],
                        "seasons": [],
                        "sessionId": "",
                        "questionId": "",
                        "creatorId": "587ff85ff2c639e86624c040",
                        "roomId": "5dfb99d3c685720001dfec83",
                        "question": "Where will the next kill happen ?",
                        "answers": {
                            "A": "Top Lane",
                            "B": "Mid Lane",
                            "C": "Jungle",
                            "D": "Bot Lane"
                        },
                        "imgUrl": "https://static.gamelive.gg/share/rooms/5fca126c31795f5ed38259fd.jpg",
                        "language": "en",
                        "isReusable": true,
                        "isGlobal": true
                    },
                    "questionType": "LOL-PRONO",
                    "questionDuration": 30,
                    "timeEnd": "2020-12-16T10:43:44.622Z",
                    "displayOptions": {}
                }
            ]
        }
    },
    "game": "lol",
    "language": "en",
    "isDeleted": false,
    "isPrivate": false,
    "isTest": false,
    "isPromoted": false,
    "qrCodeImgUrl": "https://static.gamelive.gg/share/QRCodes/5fd39cbfa54d753101b443d2.jpg",
    "shortId": "iwwx",
    "backgroundImage": "",
    "streamedModules": {
        "leaderboard": false,
        "predictions": false,
        "smartDisplaying": false,
        "showPromoImages": false,
        "showQRCodeImage": false,
        "showCustomUrl": false
    },
    "twitchCustom": {
        "headerImage": "",
        "headerImageUrl": "",
        "twitterUrl": "",
        "facebookUrl": "",
        "instagramUrl": ""
    }
}
```

This endpoint retrieves a specific Liveroom.

<aside class="success">Example — <code>https://apidev.gamelive.gg/liveRoom/5fd39cbfa54d753101b443d1</code></aside>

### HTTP Request

`GET /liveRoom`

### URL Parameters

Parameter | Description
--------- | -----------
liveRoomId | The ID (ObjectId string) of the Liveroom to retrieve

## Join a Specific Liveroom

```shell
curl "/liveRoom/join" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
}
```

This endpoint allows a user to join a specific Liveroom.

### HTTP Request

`GET /liveRoom/join`

### URL Parameters

Parameter | Description
--------- | -----------
liveRoomId | The ID of the liveroom to join

## Get a summary of a Liveroom

```shell
curl "/liveRoom/summary" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": "5fd39cbfa54d753101b443d1",
        "name": "Joujou",
        "imgUrl": "/images/logo_transparent.png",
        "admins": [
            {
                "userId": "5fd386c0ebbd377cf944bda0"
            }
        ],
        "language": "en"
    }
]
```

This endpoint return the summaries of Liverooms which ids are passed in the body of the request

### HTTP Request

`GET /liveRoom/join`

### URL Parameters

Parameter | Description
--------- | -----------
liveRoomIds | (optional) The ID of the liverooms
