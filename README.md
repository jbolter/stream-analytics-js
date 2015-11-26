# GetStream JS Analytics Client

[![Build Status](https://travis-ci.org/GetStream/stream-analytics-js.svg?branch=master)](https://travis-ci.org/GetStream/stream-analytics-js)

## Get the library


### Async loading

The best way to add the client to your application is to load it asyncronously. You can paste the following snippet anywhere in the HTML and the client will load without interfering with the page load.

```html
<script type="text/javascript">
!function(a,b){a("StreamAnalytics","//d2j1fszo1axgmp.cloudfront.net/2.0.0/stream-analytics.min.js",b)}(function(a,b,c){var d,e,f;c["_"+a]={},c[a]=function(b){c["_"+a].clients=c["_"+a].clients||{},c["_"+a].clients[b.projectId]=this,this._config=b},d=["setUser","trackImpression","trackEngagement"];for(var g=0;g<d.length;g++){var h=d[g],i=function(a){return function(){return this["_"+a]=this["_"+a]||[],this["_"+a].push(arguments),this}};c[a].prototype[h]=i(h)}e=document.createElement("script"),e.async=!0,e.src=b,f=document.getElementsByTagName("script")[0],f.parentNode.insertBefore(e,f)},this);
</script>
```


### For Angular/Ember/... apps that use webpack or browserify

TODO: explain the steps to do that

## Configure an instance for your project

Once your application is enabled to use Stream's Analytics platform, you will get an `API_KEY` and a `token` to access the APIs. Reach support@getstream.io to set that up.

Analytics tracking is done using a client object, here's the code that initializes it.

```js
var client = new StreamAnalytics({
  apiKey: "API_KEY",
  token: "ANALYTICS_TOKEN"
});
```

## Set current user

Before you can track events, you need to set the current user. All analytics events are related to the users of your application.

```js
client.setUser('7b22b0b8-6bb0-11e5-9d70-feff819cdc9f');
```

## Track impressions

Everytime you display content to the user, you should track the impression. The `trackImpression` method allows you to track what content is displayed to the user. There are 2 required fields that you need to provide:

* foreign_ids: the list of IDs for the content you are showing to the user.
* feed_id: the feed_id (eg. "flat:123", "newsfeed:42") from where the content origins, if the content does not come from a Stream's powered feed you should use a clear and unique indentifier (eg. "user_search_page", "user42_profile_page", etc.)

```js
var impression = {
    foreign_ids: ['message:34349698'],
    feed_id: 'user:ChartMill'
};

// track impression for two messages presented to the user
client.trackImpression(impression);
```

## Engagement tracking

User interactions must be tracked with a label, optional extra information can be sent as well.

```js
engagement = {...}
// Sends one engagement event to the APIs
client.trackEngagement(engagement);

// Click on a message
var engagement = {
    label: 'click',
    foreign_id: 'message:34349698',
    feed_id: 'user:ChartMill'
};
client.trackEngagement(engagement, function(){console.log(arguments);});

// Share message
var engagement = {
    label: 'share',
    foreign_id: 'message:34349698',
    feed_id: 'user:ChartMill'
};
client.trackEngagement(engagement);
```

## Fields reference

### features 

If necessary, you can send additional information about the data involved or the specific event. The features field must be a list of feature objects. A feature object is made of two mandatory fields: "group" and "value"

eg.

```javascript
[{
    'group': 'topic',
    'value': 'coffee'
}]
```

### location 

A string representing the location of the content (eg. 'notification_feed')

### feed_id 

A string representing the origin feed for the content

### boost 

An integer that multiplies the score of the interaction (eg. 2 or -1)

### position 

When tracking interactions with content, it might be useful to provide the oridinal position (eg. 2)

### foreign_id

The ID for the content as you store that in your database (or/and on Stream's feeds) (eg. 'user:42') (engagement only)

### foreign_ids

An Array of foreign_ids (impressions only)

### label

The event identifier (eg. click, share, ...) this field is mandatory for every engagement event
