# Twitter System Design :

# Functional Requirements :

1. User should Tweet and send to millions of users followed
2. Time line
   - User Time Line
   - Home Time Line
3. Search
4. HashTags
5. User should follow some one

# Non Fucntional :

1. Twitter is read Heavy
2. Consistancy can be compramised - Evntual consistancy
3. Available
4. Scalable
5. No SPF
6. Low Latancy

# Capacity Estimation:

1. Toatal User 300M
2. Daily New users 100k
3. Tweets per second - 600 Tweets
4. Reads per second - 6,00,000 Tweets

Memory Required :
Tweets size : Message - 200 bytes
Images - 100 bytes
Video - 200 bytes
====================
500 bytes
====================

Write Data = 600 _ 500 bytes _ 24 _ 3600 _ 30 = X bytes of memory required for month

Bandwidth : 600 \* 500 bytes of data will be sent over the network per second

# High Level Design :

- We can have entual consistancy

# Rest API & Data base

- User Register Using Email

  - Rest API :
    POST - /create/user
    Payload - {username - "" , Password - "" ,Email - "" ,Mobile - ""}

  Data Base : Users Table,User Tweets,User Followers

- Tweet :

  Lets think we need to go with Rest API or websockets

  We will go with websockets
  Create a web socket Manager which will have the web sockets to the user
  Send Tweet Using the web socket

  post a Tweet : { Tweet Message :"" ,Tweets Image/Video : ""}
  One of the System holding the sockets read the message and

  - call Content storage service to save data
  - Save the data in user database ( Lets think about the scalling later)
  - Update the user and the tweet details in cache ( set cache eviction time 1 Day)

  Notify the users Home page :

  - Once the tweet is saved post a message on kafka ( message shoul have the posted UserId,message)
  - All the Socket servers will have a reciver to read the message from kafka
  - Sokcet machines also maintain the user ids they are holding
  - From redis get the Follower Ids
  - If the message recived from kafka is from follower then push the message on to the respective User Websoket.

  Retweets:

  - Send message on to the websocket {ReplyToTweetId : ,message:"",time:""}
  - Save it in user tweets table and update the self referential key.

  ## Time Line ( IMP ):

  - Twitter will have Two time lines
    - User ( This will have all the user posted tweets)
    - Home ( This will have all the user follower tweets)
  - Deplaying time line Evry time
    - Getting the user Tweets
    - Getting the user followers
    - Getting the followers
    - Merge them to generate Home Time line and User Time line is difficult
  - Twitter follows **Fanout** Mechnism to solve this problem
  - Maintain Two caches fo every users
    - UT
    - HT
  - When user post a tweet
    - update the UT cache with the tweet
    - Also update the followers HT Cache with the Tweet

  When user calls the following to display HT & UT

  - Rest API : /user/timeline/{userId}
  - Get the Tweets Service -

    - Get the Details from UT and HT cahces
    - Cache missing case - From User Tweets sort by time stamp created

  - When the celebrities tweet ?
    - Difficult to update the HT of all the followers
  - Maintain the celebrities list for the user in a seperate table
    - Get the data from DB of the followers and show it on time line

  Search Tweets :

  - All the tweet messages should be indexed
  - Call the Tweet Service to get the tweets from solar indexed
  - Get the Tweets from tweet ids.

  # Deep Dive on to Design :

  - Read heavy operation
  - Cache the Users and user tweets and User followers
  - Index the Tweets for dearch operation
  - User Active - ACtive load balancer
  - We will use
    Partioning based on the user Id and range based partioning as more no of tweets are avialble.
