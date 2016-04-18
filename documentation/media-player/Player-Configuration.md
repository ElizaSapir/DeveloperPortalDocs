---
layout: page
title: Kaltura Media Player Configuration and Parameters
---

The Kaltura player configuration is determine by a JSON structured configuration object also called Flashvars.  
The Flashvars config params object defines key / values pairs for player properties as well as a plugins configuration list.   

You can customize the Kaltura player using one of 2 options:

1. Using Kaltura Universal Studio: The Universal Studio an a part of Kaltura KMC and allows visual configuration of player parameters and plugins.  
For more info and a full user guide please refer to the Universal Studio Information Guide.  
2. The Universal Studio saves the Flashvars object for you as part of the player's configuration in the Kaltura database. When using Studio, you do not have to manually define the Flashvars object.  
Define a Flashvars object as part of your Embed code:  
Using this method, you define your Flashvars object as a JSON object in your player embed code statement. You can define values for player properties and plugins configuration.  
**NOTE:** Values defined in the Embed code Flashvars objects override any values previously saved using Studio for the same keys.   

## Considerations for Configuring Players

How should you configure the player, and what to consider when specifying configuration parameters?  
As a general rule of thumb, you should always prefer to configure flashvars via the Player Studio menus and the "UI Variables" section:  

* Configuration defined in Studio applies to all player instances across web pages and embed codes whereas embed code Flashvars applies only to a specific player instance.
* Visual and self explanatory interface for the most commonly used player properties and plugins.

### Use embed code Flashvars in these scenarios:   

> Note that any changes made via flashvars on the embedding page, will require editing these pages to make future changes. 

1. You need to set the Flashvars values dynamically based on you website / app logic.  
For example, you may need to pass metadata fields that are derived from the specific view or page at runtime and thus will have to set these values only when rendering the player.
2. You want to inherit all of the player properties defined in Studio but you need to override specific properties for a specific player instance - note that if used across many pages or sites, it will be better practice to create a new player and configure it via the player studio.    

### Passing config params in embed methods

The Kaltura player can be embedded into webpages in few ways. The embed code Flashvars object is passed in a different way for different embed types. Learn more about embed types.  

#### Config params in JavaScript dynamic embed

For dynamic embeds: Use a JSON object as part of your embed code:  

```javascript
kWidget.embed({
    'targetId': 'kaltura_player',
    'wid': '_243342',
    'uiconf_id' : '12905712',
    'entry_id' : '0_uka1msg4',
    'flashvars': {
        'autoPlay': true,
        'watermark': {
            'plugin' : "true",
            'img' : "http://www.kaltura.com/content/uiconf/kaltura/kmc/appstudio/kdp3/exampleWatermark.png",
            'href' : "http://www.kaltura.com/",
            'cssClass' : "topRight"
        }
    }
});
```

#### Config params in iFrame embed

For auto / iFrame embeds, pass the key / value pairs on the iFrame URL query string, using the flashvars with brackets and dot syntax for nested object attributes, as following:   

* Instead of `flashvars: { autoPlay: true }`, use the following notation: `flashvars[autoPlay]=true` 
* Instead of `flashvars: { share: { plug: true } }`, use the following notation: `flashvars[share.plugin]=true`

```html
<iframe src="http://cdnapi.kaltura.com/p/1645161/sp/164516100/embedIframeJs/uiconf_id/33752651/partner_id/1645161?iframeembed=true&playerId=kaltura_player&entry_id=1_1josgev8&flashvars[autoPlay]=true&flashvars[share.plugin]=true" width="560" height="395" allowfullscreen webkitallowfullscreen mozAllowFullScreen frameborder="0"></iframe>
```

## Common player configuration scenarios

* Auto start video playback with a muted video, at a start point of 20 seconds:

```javascript
'flashvars': {
        'autoPlay': true,
        'autoMute': true,
        'mediaProxy.mediaPlayFrom': 20
    }
```

* Force a specific streamer type (learn more about streamer types):

```javascript
'flashvars': {
        'streamerType': 'hdnetworkmanifest'
    }
```

* Lead with HLS playback on all devices:

```javascript
'flashvars': {
        'LeadWithHLSOnFlash': true,
        'Kaltura.LeadHLSOnAndroid': true
    }
```

* Define a plugin configuration (this example overrides the volume control horizontal layout and sets it to vertical):

```javascript
'flashvars': {
        'volumeControl':{
                'layout': 'vertical',
                'align': 'right'
            },
    }
```

* Set Flashvars dynamically according to data derived from a server:

```javascript
$.ajax({
    method: "POST",
    url: "getMetaData.php",
    data: { entry_id: "1_sf5ovm7u" }
})
.done(function( metaData ) {
    kWidget.embed({
        'targetId' : 'kaltura_player',
        'wid': '_243342',
        'uiconf_id' : '12905712',
        'entry_id' : '1_sf5ovm7u',
        'flashvars' : {
            'youbora': {
                'trackEventMonitor': 'trackYouboraAnalyticsEvent',
                'bufferUnderrunThreshold': 1000,
                'userId': 'my-user-id',
                'contentMetadata': metaData
            }
        }
    });
});
```