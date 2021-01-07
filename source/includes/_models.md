# Models

## Liveroom

This is the main object to create activations.
An user is admin of the the rooms he created.
A room a session, each sessions corresponding to a leaderboard and containing questions.

> Model:

```golang
type Room struct {
	Id          bson.ObjectId   `json:"id" bson:"_id"`
	TwChannelId string          `json:"twChannelId,omitempty" bson:"twChannelId,omitempty"`
	Name        string          `json:"name" bson:"name"`
	ImgUrl      string          `json:"imgUrl,omitempty" bson:"imgUrl,omitempty"`
	Description string          `json:"description,omitempty" bson:"description,omitempty"`
	SubTitle    string          `json:"subTitle,omitempty" bson:"subTitle,omitempty"`
	BannedIds   []bson.ObjectId `json:"bannedIds,omitempty" bson:"bannedIds,omitempty"`
	Admins      []AdminInfos    `json:"admins,omitempty" bson:"admins,omitempty"`
	//Contains all the users that joined the room at one point
	JoinedUsers []users.UserInfo `json:"joinedUsers,omitempty" bson:"joinedUsers,omitempty"`

	// Sessions are indexed with their ID
	Sessions map[string]seasons.Season `json:"sessions,omitempty" bson:"sessions,omitempty"`
	// TEMP OBJECTs
	//OldSessions []seasons.Season `json:"oldSessions,omitempty" bson:"oldSessions,omitempty"`
	//DEPRECATED, included into the Session object
	Questions []questions.Question `json:"questions,omitempty" bson:"questions,omitempty"`

	Game     string   `json:"game,omitempty" bson:"game,omitempty"`
	Language string   `json:"language,omitempty" bson:"language,omitempty"`
	Hashtags []string `json:"hashtags,omitempty" bson:"hashtags,omitempty"`
	// a room can't be edited if it is deleted
	IsDeleted bool   `json:"isDeleted" bson:"isDeleted"`
	Color     string `json:"color,omitempty" bson:"color,omitempty"`
	IsPrivate bool   `json:"isPrivate" bson:"isPrivate"`
	// a room can't be edited if it is test
	// a test room has automatics bets that are run every 90 sec
	IsTest       bool   `json:"isTest" bson:"isTest"`
	IsPromoted   bool   `json:"isPromoted" bson:"isPromoted"`
	StreamName   string `json:"streamName,omitempty" bson:"streamName,omitempty"`
	ChatChannel  string `json:"chatChannel,omitempty" bson:"chatChannel,omitempty"`
	QRCodeImgUrl string `json:"qrCodeImgUrl,omitempty" bson:"qrCodeImgUrl,omitempty"`
	RoomUrl      string `json:"roomUrl,omitempty" bson:"roomUrl,omitempty"`
	ShortId      string `json:"shortId,omitempty" bson:"shortId,omitempty"` // Unique code of 6 digits to join the room easily
	//ShowQRCodeImage bool            `json:"showQRCodeImage" bson:"showQRCodeImage"`
	BackgroundImage string          `json:"backgroundImage" bson:"backgroundImage"`
	StreamedModules StreamedModules `json:"streamedModules,omitempty" bson:"streamedModules,omitempty"`
	//TODO : to change to array
	PromotionalImagesUrl string             `json:"promotionalImagesUrl,omitempty" bson:"promotionalImagesUrl,omitempty"`
	TwitchCustom         TwitchCustomStruct `json:"twitchCustom,omitempty" bson:"twitchCustom,omitempty"`
}

type AdminInfos struct {
	UserId bson.ObjectId `json:"userId,omitempty" bson:"userId,omitempty"`
	Name   string        `json:"name,omitempty" bson:"name,omitempty"`
}

// An edulcored version of the Room struct to avoir too much traffic
type RoomSummary struct {
	Id          bson.ObjectId `json:"id,omitempty" bson:"_id,omitempty"`
	Name        string        `json:"name,omitempty" bson:"name,omitempty"`
	ImgUrl      string        `json:"imgUrl,omitempty" bson:"imgUrl,omitempty"`
	Description string        `json:"description,omitempty" bson:"description,omitempty"`
	SubTitle    string        `json:"subTitle,omitempty" bson:"subTitle,omitempty"`
	Admins      []AdminInfos  `json:"admins,omitempty" bson:"admins,omitempty"`
	Language    string        `json:"language,omitempty" bson:"language,omitempty"`
}

// liveRoom is the live pendant of the Room ; this object is sent to the clients through the websocket
type liveRoom struct {
	Room *Room `json:"room,omitempty" bson:"room,omitempty"`
	// HasActiveQuestion is set to true if a question is in the "OPEN" state
	// If HasActiveQuestion is true, a routine updating the room is running
	HasActiveQuestion bool `json:"hasActiveQuestion"`

	// ConnectedClients is the map containing all the webscocket connections of the clients connected to the room
	ConnectedClients []*customWs `json:"-" bson:"-"`
	// OperationExecuted: Can either be ["QuestionsManipulation", "ResetPoints", "AdminUpdate", "Ping"]
	OperationExecuted string `json:"operationExecuted"`
	// UpdateTickerChannel is a chan used to keep track of the ticker goroutine that sends updates to all the clients regularily
	// and to end it eventually
	UpdateTickerChannel chan bool `json:"-" bson:"-"`
	// PingTickerChannel is a chan used for the ticker implementing the ping/pong control messages of the websocket
	PingTickerChannel chan bool `json:"-" bson:"-"`
}

// StreamedModules defines the list of modules that are streamed live
type StreamedModules struct {
	Leaderboard     bool `json:"leaderboard"`
	Predictions     bool `json:"predictions"`
	SmartDisplaying bool `json:"smartDisplaying"`
	ShowPromoImages bool `json:"showPromoImages"`
	ShowQRCodeImage bool `json:"showQRCodeImage"`
	ShowCustomUrl   bool `json:"showCustomUrl"`
}

// TwitchCustom contains all the customizable things in the twitch extension
type TwitchCustomStruct struct {
	HeaderImage    string `json:"headerImage"`
	HeaderImageUrl string `json:"headerImageUrl"`
	TwitterUrl     string `json:"twitterUrl"`
	FacebookUrl    string `json:"facebookUrl"`
	InstagramUrl   string `json:"instagramUrl"`
}

```

## Questions

A question is used in a Room and provides contextual information on a BetUnit: corresponding match Id, answers given by the users...

```shell
curl "/liveRoom/join" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```golang
type Question struct {
	Id      bson.ObjectId `json:"id" bson:"_id"`
	RoomId  bson.ObjectId `json:"roomId,omitempty" bson:"roomId,omitempty"`
	MatchId bson.ObjectId `json:"matchId,omitempty" bson:"matchId,omitempty"`
	// SessionId is used to avoid erasing multiple similar bets for a user
	SessionId bson.ObjectId `json:"sessionId,omitempty" bson:"sessionId,omitempty"`

	Status                 string         `json:"status,omitempty" bson:"status,omitempty"`
	AnswersCount           map[string]int `json:"answersCount,omitempty" bson:"answersCount,omitempty"`
	AnswersCountPercentage map[string]int `json:"answersCountPercentage,omitempty" bson:"answersCountPercentage,omitempty"`
	Bet                    bets.BetUnit   `json:"bet" bson:"bet"`
	// Can either be [{game}-TRIVIA, {game}-PRONO]
	QuestionType     string    `json:"questionType" bson:"questionType"`
	QuestionDuration int       `json:"questionDuration" bson:"questionDuration"`
	TimeEnd          time.Time `json:"timeEnd" bson:"timeEnd"`
	// Display options used individually by the liveRooms
	DisplayOptions DisplayOptions `json:"displayOptions" bson:"displayOptions"`
	//used only for evry2
	// BreakDuration       int            `json:"breakDuration" bson:"breakDuration"`

}

type DisplayOptions struct {
	ShowAnswerCount bool `json:"showAnswerCount,omitempty" bson:"showAnswerCount,omitempty"`
	ShowPercentages bool `json:"showPercentages,omitempty" bson:"showPercentages,omitempty"`
}
```

This endpoint allows a user to join a specific Liveroom.

### HTTP Request

`GET /liveRoom/join`

### URL Parameters

Parameter | Description
--------- | -----------
liveRoomId | The ID of the liveroom to join

## Bets

```shell
curl "/liveRoom/summary" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```golang
/**
*	Bet Ref typo :
*	- bo5	:	Score for both team for a Bo5 match
*	- tow1	:	1rst tower
*	- fbl	:	1rst blood
*	- mid15	:	Best midlaner at 15'
*	- inh1	:	1rst inhibitor
**/
//swagger:model
type BetUnit struct {
	Id             bson.ObjectId `json:"id" bson:"_id,omitempty"`
	MatchId        bson.ObjectId `json:"matchId" bson:"matchId,omitempty"`
	GameNumber     int           `json:"gameNumber" bson:"gameNumber"`
	Ref            string        `json:"ref" bson:"ref"`
	MatchDateTime  time.Time     `json:"matchDateTime" bson:"matchDateTime"`
	Accurate       bool          `json:"accurate" bson:"accurate"`
	Aknowledged    bool          `json:"aknowledged" bson:"aknowledged"`                 // true if the points for this bet have been counted
	Timestamp      time.Time     `json:"timestamp,omitempty" bson:"timestamp,omitempty"` // Time when user's bet is saved in Db / IN gamelive : time where the bet was created
	EventTimestamp int64         `json:"-" bson:"eventTimestamp,omitempty"`              // Time when event happend (firstblood for a FBL bet), in millisecond after the match begun
	Valid          bool          `json:"valid" bson:"valid"`
	InvalidReason  string        `json:"invalidReason,omitempty" bson:"invalidReason,omitempty"`
	BasePoints     int           `json:"basePoints" bson:"basePoints"`
	PointsGains    int           `json:"pointsGains,omitempty" bson:"pointsGains,omitempty"`
	Multipliers    struct {
		Odd     float32 `json:"odd" bson:"odd"`
		Perfect float32 `json:"perfect" bson:"perfect"`
	} `json:"multipliers" bson:"multipliers"`
	LiveBetExpire time.Time `json:"liveBetDuration" bson:"liveBetDuration"`
	//in:body
	Score struct {
		BlueTeamId    string `json:"blueTeamId,omitempty" bson:"blueTeamId,omitempty"`
		BlueTeamScore int    `json:"blueTeamScore" bson:"blueTeamScore"`
		RedTeamId     string `json:"redTeamId,omitempty" bson:"redTeamId,omitempty"`
		RedTeamScore  int    `json:"redTeamScore" bson:"redTeamScore"`
		Roster        string `json:"roster,omitempty" bson:"roster,omitempty"`
		CustomAnswer  string `json:"customAnswer,omitempty" bson:"customAnswer,omitempty"`
	} `json:"score,omitempty" bson:"score"`
	AchievementsList map[string][]string `json:"-" bson:"achievementsList"`
	Rosters          []BetRosters        `json:"rosters" bson:"rosters"`
	Seasons          []bson.ObjectId     `json:"seasons" bson:"seasons"`

	//Gamelive specific fields
	SessionId     bson.ObjectId     `json:"sessionId" bson:"sessionId,omitempty"`
	QuestionId    bson.ObjectId     `json:"questionId" bson:"questionId,omitempty"`
	CreatorId     bson.ObjectId     `json:"creatorId" bson:"creatorId,omitempty"`
	RoomId        bson.ObjectId     `json:"roomId" bson:"roomId,omitempty"` // Is used for bets that are dedicated to only one room
	Question      string            `json:"question,omitempty" bson:"question,omitempty"`
	Answers       map[string]string `json:"answers,omitempty" bson:"answers,omitempty"`
	CorrectAnswer string            `json:"correctAnswer,omitempty" bson:"correctAnswer,omitempty"`
	ImgUrl        string            `json:"imgUrl,omitempty" bson:"imgUrl,omitempty"`
	Language      string            `json:"language,omitempty" bson:"language,omitempty"`
	IsReusable    bool              `json:"isReusable" bson:"isReusable"` // Set to true if can be reused again in this particular room
	IsGlobal      bool              `json:"isGlobal" bson:"isGlobal"`     // Set to true if it's available to all rooms

}

type BetRosters struct {
	Id        bson.ObjectId `json:"id" bson:"id,omitempty"`
	Imageurl  string        `json:"imageUrl" bson:"imageUrl"`
	Shortname string        `json:"shortName" bson:"shortName"`
}

```

This endpoint return the summaries of Liverooms which ids are passed in the body of the request

### HTTP Request

`GET /liveRoom/join`

### URL Parameters

Parameter | Description
--------- | -----------
liveRoomIds | (optional) The ID of the liverooms
