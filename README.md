# sentimental-birds

Sentimental analysis and visualization on Twitter streams.

# prerequisites

* Docker: https://www.docker.com/

# kafka connect twitter connector

- checkout kafka-connect-twitter plugin: https://github.com/jcustenborder/kafka-connect-twitter

```
cd kafka-connect-twitter
mvn clean package
cd target
tar -xvf kafka-connect-twitter-0.2-SNAPSHOT.tar.gz
```

- extract the kafka-connect-twitter-0.2-SNAPSHOT.tar.gz to ./lib/connectors/kafka-connect-twitter

* put the file 'twitter-connector.json' to the 'docker' folder: ./docker/twitter-connector.json

The content of twitter-connector.json should look like this (just replace all placeholders with your own Twitter credentials):
```
{
 "name": "twitter_source_json_01",
 "config": {
   "connector.class": "com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector",
   "twitter.oauth.accessToken": "YOUR-ACCESS-TOKEN",
   "twitter.oauth.consumerSecret": "YOUR-CONSUMER-SECRET",
   "twitter.oauth.consumerKey": "YOUR-CONSUMER-KEY",
   "twitter.oauth.accessTokenSecret": "YOUR-ACCESS-TOKEN-SECRET",
   "kafka.delete.topic": "twitter_deletes_json_01",
   "value.converter": "org.apache.kafka.connect.json.JsonConverter",
   "key.converter": "org.apache.kafka.connect.json.JsonConverter",
   "value.converter.schemas.enable": false,
   "key.converter.schemas.enable": false,
   "kafka.status.topic": "twitter_json_01",
   "process.deletes": true,
   "filter.keywords": "kafka,munich"
 }
}
```

# docker

Start confluent platform in docker:

```
docker-compose up
```

After all components are up and running, login into 'connect' image:

```
docker exec -it CONNECT_IMAGE_ID bash
```

Deploy and start kafka connect twitter plugin:

```
curl -X POST -H "Content-Type: application/json" localhost:8083/connectors --data-binary @/tmp/twitter/twitter-connector.json -v
```

Now take a look at the topic list:

```
kafka-topics --list --zookeeper zookeeper:2181
```

You should see the 'twitter_json_01' topic.

Look inside:

```
kafka-console-consumer --topic twitter_json_01 --bootstrap-server broker:9092
```

You should see records like this:

```
{
  "CreatedAt": 1527104606000,
  "Id": 999375270074306600,
  "Text": "RT @spectatorindex: Quality of living, 2018.\n\n1. Vienna\n2. Zurich\n3. Munich (tied)\n3. Auckland (tied)\n5. Vancouver\n6. Dusseldorf\n7. Frankfu…",
  "Source": "<a href=\"http://twitter.com/download/android\" rel=\"nofollow\">Twitter for Android</a>",
  "Truncated": false,
  "InReplyToStatusId": -1,
  "InReplyToUserId": -1,
  "InReplyToScreenName": null,
  "GeoLocation": null,
  "Place": null,
  "Favorited": false,
  "Retweeted": false,
  "FavoriteCount": 0,
  "User": {
    "Id": 76904782,
    "Name": "Burak GÜLLER",
    "ScreenName": "brkguller",
    "Location": "Kadirli, Türkiye",
    "Description": "Fundación Internacional de Derechos Humanos #BG #herşeykadirliçin #sunshine",
    "ContributorsEnabled": false,
    "ProfileImageURL": "http://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_normal.jpg",
    "BiggerProfileImageURL": "http://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_bigger.jpg",
    "MiniProfileImageURL": "http://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_mini.jpg",
    "OriginalProfileImageURL": "http://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48.jpg",
    "ProfileImageURLHttps": "https://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_normal.jpg",
    "BiggerProfileImageURLHttps": "https://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_bigger.jpg",
    "MiniProfileImageURLHttps": "https://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48_mini.jpg",
    "OriginalProfileImageURLHttps": "https://pbs.twimg.com/profile_images/982703661943017472/Aw8aiK48.jpg",
    "DefaultProfileImage": false,
    "URL": "https://fundacion.in/",
    "Protected": false,
    "FollowersCount": 1128,
    "ProfileBackgroundColor": "000000",
    "ProfileTextColor": "000000",
    "ProfileLinkColor": "1B95E0",
    "ProfileSidebarFillColor": "000000",
    "ProfileSidebarBorderColor": "000000",
    "ProfileUseBackgroundImage": false,
    "DefaultProfile": false,
    "ShowAllInlineMedia": false,
    "FriendsCount": 299,
    "CreatedAt": 1253786891000,
    "FavouritesCount": 53397,
    "UtcOffset": -18000,
    "TimeZone": "Quito",
    "ProfileBackgroundImageURL": "http://abs.twimg.com/images/themes/theme3/bg.gif",
    "ProfileBackgroundImageUrlHttps": "https://abs.twimg.com/images/themes/theme3/bg.gif",
    "ProfileBannerURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/web",
    "ProfileBannerRetinaURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/web_retina",
    "ProfileBannerIPadURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/ipad",
    "ProfileBannerIPadRetinaURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/ipad_retina",
    "ProfileBannerMobileURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/mobile",
    "ProfileBannerMobileRetinaURL": "https://pbs.twimg.com/profile_banners/76904782/1488607995/mobile_retina",
    "ProfileBackgroundTiled": false,
    "Lang": "tr",
    "StatusesCount": 2740,
    "GeoEnabled": true,
    "Verified": false,
    "Translator": false,
    "ListedCount": 16,
    "FollowRequestSent": false,
    "WithheldInCountries": []
  },
  "Retweet": true,
  "Contributors": [],
  "RetweetCount": 0,
  "RetweetedByMe": false,
  "CurrentUserRetweetId": -1,
  "PossiblySensitive": false,
  "Lang": "de",
  "WithheldInCountries": [],
  "HashtagEntities": [],
  "UserMentionEntities": [
    {
      "Name": "The Spectator Index",
      "Id": 1626294277,
      "Text": "spectatorindex",
      "ScreenName": "spectatorindex",
      "Start": 3,
      "End": 18
    }
  ],
  "MediaEntities": [],
  "SymbolEntities": [],
  "URLEntities": []
}
```

# third party software

* kafka-connect-twitter plugin: https://github.com/jcustenborder/kafka-connect-twitter
* confluent platform docker images: https://github.com/confluentinc/cp-docker-images
