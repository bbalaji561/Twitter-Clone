# Twitter-Clone
Implemented a Twitter server engine using suave framework which provides support for both Websockets and Http and performed REST operations like GET and POST. The server engine uses an actor to push the live feeds to the online users through websockets. Implemented a simple client - UI in the browser, using HTML and JavaScript which invokes connections with the server using websockets and http.

## Commands to Run ##
First have to start the server and then client/clients as required. Server always runs on port 8080 by default. 
NOTE: ​It is best to use incognito browser to avoid browser alerts for unsafe password use.
packages might be needed: dotnet add package [ Suave --version 2.5.6 or Akka.FSharp or FSharp.Json or Newtonsoft.Json ]

#### Server ####
dotnet run project_4_2.fsproj
#### Client/s ####
Client runs on the browser at localhost:8080. Server instantiates the Login.html page once it is started. For multiple clients, multiple connections(new tabs) needed to be opened.

## Technologies: ## 
1. ​Server: ​Developed the code in F# and used suave framework to provide a ​websocket and ​http interface. Implemented both REST APIs and websockets for different services. Also implemented an actor model using AKKA framework for handling the Live feed operations as and when a user posts a new tweet. Used Map/Set/List data structures provided by F# for storing the data and to perform operations on them.
2. ​Client: The client is written in HTML and JavaScript to perform ​REST operations and websocket connections with the server.
3. Used ​JSON​ objects for communication between server and client.
4. For ​successful responses respective actions were performed on the client and for ​error responses corresponding alert message boxes were popped at the client.

## Implementation ## 
#### TwitterServer #### 
TwitterServer is our server engine and it supports the following services: ​Client Registration, Tweet, Re-tweet, Subscribe/Follow, Query HashTags or MentionUsers.​ Built in-memory tables by using the map data structure in F# to store the data. We used actors to push the live feeds and notifications to the online users through websockets as this service is independent of the current user request and can be done concurrently.
#### Where did we use Websockets? #### 
Services where the data need to be dynamically reflected in real time on the client from the server are implemented using websockets.
#### Live Feed: #### 
- When the user just comes online, his/her latest feed is loaded into the UI from the server using websockets.
If he is a newly registered user, he will not have any feeds and the same message is displayed in the UI. If he is a returning user, he gets his old feeds.
- When a user posts a new tweet, the request hits the server and the same tweet is posted on its active follower’s feed section using websockets.
#### Follow: #### 
When the user is following another user, both of them get their respective notifications using websockets from the server for their actions.

## Where did we use HTTP and REST? ## 
Services where it needs an immediate response for their requests or where there are no further actions delegated to be performed on the server.
Register (/register) - Registers a new user and stores the record.
Login (/login) - Logins the registered user and if the user is not registered then responds with an error.
Query (/search) - Maximum upto Top 10 tweets having the respective hashTag or mentionedUser is responded if present.
Follow (/follow) - User sends a post request to follow another user.
Tweet (/tweet) - User posts a tweet.
The below image highlights all the API endpoints used in the code. All the endpoints are of type POST except the initial Login page and search service which are of GET types. Finally, /livefeed represents the websocket point.
 
### JSON objects: ### 
For modularization purposes, we used only 2 kinds of objects one for request and another for response.
​Request​ from client:
{
  ‘userID’: string,
  ‘value’: string 
}

Description of the keys and its purpose:
userID: User ID of the logged in user
value: Varies according to the service, the value field has the following:
for registration: password
for login: password
for follow: follower ID
for tweet: Tweet message
for retweet: Re-Tweet message
for query: either the hashtag or mentioned user
  
​Response​ from server: 
{
  ‘userID’: string, 
  ‘service’: string, 
  ‘message’: string, 
  ‘code’: string
}

Description of the keys and its purpose:
userID: user ID of the user performing the service
service: One of the following services - Register, Login, Follow, Tweet, ReTweet, Query, LiveFeed. 
message: Response messages for the actions performed, the value of the field will be the following:
  for registration: Either user is registered successfully or not
  for login: Either user is logged in successfully or has failed because of incorrect password or user itself is not registered earlier.
  for follow: If the user ID and follower ID are registered users and if the user ID is added to the follower ID’s set properly or not.
  for tweet: If the tweet is posted by a registered user or not and is parsed correctly for hashtags or mentioned users.
  for retweet: If the tweet is posted by a registered user or not.
  for query: if the hashtag or mentioned user is present then maximum upto their top 10 tweets, if not error response.
code: Either ‘OK’ for successful response or ‘FAIL’ for error responses, mainly to distinguish at the client side.

### Snapshots of request/response types: ### 
#### GET: ####
   
#### POST: ####

#### Websocket: ####
 
