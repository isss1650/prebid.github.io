---
layout: page
title: Ad Unit Reference
description: Ad Unit Reference
top_nav_section: dev_docs
nav_section: reference
pid: 10
---

<div class="bs-docs-section" markdown="1">

# Ad Unit Reference
{:.no_toc}

The ad unit object is where you configure what kinds of ads you will show in a given ad slot on your page, including:

+ Allowed sizes
+ Allowed media types (e.g., banner, native, and/or video)

It's also where you will configure bidders, e.g.:

+ Which bidders are allowed to bid for that ad slot
+ What information is passed to those bidders via their [parameters]({{site.baseurl}}/dev-docs/bidders.html)

This page describes the properties of the `adUnit` object.

* TOC
{:toc}

## `adUnit`

See the table below for the list of properties on the ad unit.  For example ad units, see the [Examples](#addAdUnits-Examples) below.


{: .table .table-bordered .table-striped }
| Name         | Scope    | Type                                  | Description                                                                                                                                                         |
|--------------+----------+---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `code`       | Required | String                                | Unique identifier you create and assign to this ad unit.  Used to set query string targeting on the ad. If using GPT, we recommend setting this to slot element ID. |
| `sizes`      | Required | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.  For 1.0 and later, prefer one of the `mediaTypes.*.sizes` listed below.    |
| `bids`       | Required | Array[Object]                         | Each bid represents a request to a bidder.  For a list of properties, see [Bids](#bids) below.                                                                      |
| `mediaTypes` | Optional | Object                                | Defines the media type of the ad.  For a list of properties, see [Media Types](#mediatypes) below.                                                                  |
| `labelAny`   | Optional | Array[String]                         | Used for [conditional ads][conditionalAds].  Works with `sizeConfig` argument to [pbjs.setConfig][setConfig].                                                       |
| `labelAll`   | Optional | Array[String]                         | Used for [conditional ads][conditionalAds]. Works with `sizeConfig` argument to [pbjs.setConfig][setConfig].                                                        |

<a name="bids" />

### `adUnit.bids`

See the table below for the list of properties in the `bids` array of the ad unit.  For example ad units, see the [Examples](#addAdUnits-Examples) below.

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type          | Description                                                                                                                              |
|------------+----------+---------------+------------------------------------------------------------------------------------------------------------------------------------------|
| `bidder`   | Required | String        | Unique code identifying the bidder. For bidder codes, see the [bidder param reference]({{site.baseurl}}/dev-docs/bidders.html).          |
| `params`   | Required | Object        | Bid request parameters for a given bidder. For allowed params, see the [bidder param reference]({{site.baseurl}}/dev-docs/bidders.html). |
| `labelAny` | Optional | Array[String] | Used for [conditional ads][conditionalAds].  Works with `sizeConfig` argument to [pbjs.setConfig][setConfig].                            |
| `labelAll` | Optional | Array[String] | Used for [conditional ads][conditionalAds]. Works with `sizeConfig` argument to [pbjs.setConfig][setConfig].                             |

<a name="mediatypes" />

### `adUnit.mediaTypes`

See the table below for the list of properties in the `mediaTypes` object of the ad unit.  For example ad units showing the different media types, see the [Examples](#addAdUnits-Examples) below.

{: .table .table-bordered .table-striped }
| Name     | Scope                                                        | Type   | Description                                                                                    |
|----------+--------------------------------------------------------------+--------+------------------------------------------------------------------------------------------------|
| `banner` | Required, unless either of the other properties are present. | Object | Defines properties of a banner ad.  For examples, see [`adUnit.mediaTypes.banner`](#banner).   |
| `video`  | Required, unless either of the other properties are present. | Object | Defines properties of a video ad.  For examples, see [`adUnit.mediaTypes.video`](#video).      |
| `native` | Required, unless either of the other properties are present. | Object | Defines properties of a native ad.  For properties, see [`adUnit.mediaTypes.native`](#native). |

<a name="banner" />

#### `adUnit.mediaTypes.banner`

{: .table .table-bordered .table-striped }
| Name    | Scope    | Type                                  | Description                                                                             |
|---------+----------+---------------------------------------+-----------------------------------------------------------------------------------------|
| `sizes` | Required | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`. |
| `name`  | Optional | String                                | Name for this banner ad unit.  Can be used for testing and debugging.                   |

<a name="native" />

#### `adUnit.mediaTypes.native`

The `native` object contains the following properties that correspond to the assets of the native ad.

{: .table .table-bordered .table-striped }
| Name          | Scope    | Type                                  | Description                                                                                                                       |
|---------------+----------+---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------|
| `sizes`       | Required | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.                                           |
| `type`        | Optional | String                                | Used as a shorthand, e.g., `type: 'image'` implies required fields `image`, `title`, `sponsoredBy`, `clickUrl`.                   |
| `title`       | Required | Object                                | The title of the ad, usually a call to action or a brand name.  For properties, see [`native.title`](#native-title).              |
| `body`        | Required | Object                                | Text of the ad copy.  For properties, see [`native.body`](#native-body).                                                          |
| `sponsoredBy` | Required | String                                | The name of the brand associated with the ad.  For properties, see [`native.sponsoredBy`](#native-sponsoredby).                   |
| `icon`        | Optional | Object                                | The brand icon that will appear with the ad.  For properties, see [`native.icon`](#native-icon).                                  |
| `image`       | Optional | Object                                | A picture that is associated with the brand, or grabs the user's attention.  For properties, see [`native.image`](#native-image). |
| `clickUrl`    | Optional | Object                                | Where the user will end up if they click the ad.  For properties, see [`native.clickUrl`](#native-clickurl).                      |
| `cta`         | Optional | Object                                | *Call to Action* text, e.g., "Click here for more information".  For properties, see [`native.cta`](#native-cta).                 |

<div class="alert alert-warning">
  <strong>Prebid Native Ad Validation</strong>
  <p>
   Prebid.js validates the assets on native bid responses like so:
  <ul>
      <li>
       If the asset is marked as "required", it checks the bid response to ensure the asset is part of the response
      </li>
      <li>
       However, Prebid.js does not do any additional checking of a required asset beyond ensuring that it's included in the response; for example, it doesn't validate that the asset has a certain length or file size, just that that key exists in the response JSON
      </li>
    </ul>
  </p>
</div>

<a name="native-image" />

##### `adUnit.mediaTypes.native.image`

{: .table .table-bordered .table-striped }
| Name            | Scope    | Type                                  | Description                                                                                                                         |
|-----------------+----------+---------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------|
| `required`      | Required | Boolean                               | Whether this asset is required.                                                                                                     |
| `sizes`         | Optional | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.                                             |
| `aspect_ratios` | Optional | Array[Object]                         | Instead of `sizes`, you can define allowed aspect ratios.  For properties, see [`image.aspect_ratios`](#native-image-aspectratios). |

<a name="native-image-aspectratios" />

###### `adUnit.mediaTypes.native.image.aspect_ratios`

{: .table .table-bordered .table-striped }
| Name           | Scope    | Type    | Description                                                                                          |
|----------------+----------+---------+------------------------------------------------------------------------------------------------------|
| `min_width`    | Optional | Integer | The minimum width required for an image to serve (in pixels).                                        |
| `ratio_height` | Required | Integer | This, combined with `ratio_width`, determines the required aspect ratio for an image that can serve. |
| `ratio_width`  | Required | Integer | See above.                                                                                           |

<a name="native-title" />

##### `adUnit.mediaTypes.native.title`

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                          |
|------------+----------+---------+------------------------------------------------------|
| `required` | Required | Boolean | Whether a title asset is required on this native ad. |
| `len`      | Optional | Integer | Length of title text, in characters.                 |

<a name="native-sponsoredBy" />

##### `adUnit.mediaTypes.native.sponsoredBy`

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                               |
|------------+----------+---------+-----------------------------------------------------------|
| `required` | Required | Boolean | Whether a brand name asset is required on this native ad. |

<a name="native-clickUrl" />

##### `adUnit.mediaTypes.native.clickUrl`

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                              |
|------------+----------+---------+----------------------------------------------------------|
| `required` | Required | Boolean | Whether a click URL asset is required on this native ad. |

##### `adUnit.mediaTypes.native.body`

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                       |
|------------+----------+---------+---------------------------------------------------|
| `required` | Required | Boolean | Whether body text is required for this native ad. |

<a name="native-icon" />

##### `adUnit.mediaTypes.native.icon`

{: .table .table-bordered .table-striped }
| Name            | Scope    | Type                                  | Description                                                                                                                       |
|-----------------+----------+---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------|
| `required`      | Requird  | Boolean                               | Whether an icon asset is required on this ad.                                                                                     |
| `sizes`         | Optional | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.                                           |
| `aspect_ratios` | Optional | Array[Object]                         | Instead of `sizes`, you can define allowed aspect ratios.  For properties, see [`icon.aspect_ratios`](#native-icon-aspectratios). |

<a name="native-icon-aspectratios" />

###### `adUnit.mediaTypes.native.icon.aspect_ratios`

{: .table .table-bordered .table-striped }
| Name           | Scope | Type | Description |
|----------------+-------+------+-------------|
| `min_width`    | Optional | Integer | The minimum width required for an image to serve (in pixels).                                        |
| `ratio_height` | Required | Integer | This, combined with `ratio_width`, determines the required aspect ratio for an image that can serve. |
| `ratio_width`  | Required | Integer | See above.                                                                                           |

<a name="video" />

#### `adUnit.mediaTypes.video`

{: .table .table-bordered .table-striped }
| Name             | Scope    | Type                   | Description                                                                                                                                             |
|------------------+----------+------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------|
| `context`        | Required | String                 | The video context, either `"instream"` or `"outstream"`.  Example: `context: "outstream"`                                                               |
| `playerSize`     | Optional | Array[Integer,Integer] | The size (width, height) of the video player on the page, in pixels.  Example: `playerSize: [640, 480]`                                                 |
| `mimes`          | Optional | Array[String]          | Required by OpenRTB when using [Prebid Server]({{site.baseurl}}/dev-docs/get-started-with-prebid-server.html).  Example: `mimes: ['video/mp4']`         |
| `protocols`      | Optional | Array[Integer]         | Required by OpenRTB when using [Prebid Server]({{site.baseurl}}/dev-docs/get-started-with-prebid-server.html).  Example: `protocols: [1,2,3,4,5,6,7,8]` |
| `playbackmethod` | Optional | Array[Integer]         | Required by OpenRTB when using [Prebid Server]({{site.baseurl}}/dev-docs/get-started-with-prebid-server.html).  Example: `playbackmethod: [2]`          |

## Examples

+ [Banner](#adUnit-banner-example)
+ [Video](#adUnit-video-example)
+ [Native](#adUnit-native-example)

<a name="adUnit-banner-example">

##### Banner

For an example of a banner ad unit, see below.  For more detailed instructions, see [Getting Started]({{site.baseurl}}/dev-docs/getting-started.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        banner: {
            sizes: [[300, 250], [300, 600]]
        },
        bids: [{
            bidder: 'appnexus',
            params: {
                placementId: '9880618'
            }
        }, ]
    }
})
```

<a name="adUnit-video-example">

##### Video

For an example of an instream video ad unit, see below.  For more detailed instructions, see [Show Video Ads]({{site.baseurl}}/dev-docs/show-video-with-a-dfp-video-tag.html).

```javascript
pbjs.addAdUnits({
    code: 'video',
    mediaTypes: {
        video: {
            context: "instream",
            sizes: [640, 480],
        },
    },
    bids: [{
        bidder: 'appnexus',
        params: {
            placementId: '9333431',
            video: {
                skippable: true,
                playback_methods: ['auto_play_sound_off']
            }
        }
    }]
});
```

For an example of an outstream video ad unit, see below.  For more detailed instructions, see [Show Outstream Video Ads]({{site.baseurl}}/dev-docs/show-outstream-video-ads.html).

```javascript
pbjs.addAdUnit({
    code: 'video1',
    mediaTypes: {
        video: {
            context: 'outstream',
            sizes: [640, 480]
        }
    },
    renderer: {
        url: 'http://cdn.adnxs.com/renderer/video/ANOutstreamVideo.js',
        render: function(bid) {
            ANOutstreamVideo.renderAd({
                targetId: bid.adUnitCode,
                adResponse: bid.adResponse,
            });
        }
    },
    ...
})
```

<a name="adUnit-native-example">

##### Native

For an example of a native ad unit, see below.  For more detailed instructions, see [Show Native Ads]({{site.baseurl}}/dev-docs/show-native-ads.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        native: {
            image: {
                required: true,
                sizes: [150, 50]
            },
            title: {
                required: true,
                len: 80
            },
            sponsoredBy: {
                required: true
            },
            clickUrl: {
                required: true
            },
            body: {
                required: true
            },
            icon: {
                required: true,
                sizes: [50, 50]
            },
        },
        bids: [{
            bidder: 'appnexus',
            params: {
                placementId: '9880618'
            }
        }, ]
    }
})
```

## Related Topics

+ [Publisher API Reference]({{site.baseurl}}/dev-docs/publisher-api-reference.html)
+ [Conditional Ad Units][conditionalAds]
+ [Show Native Ads]({{site.baseurl}}/dev-docs/show-native-ads.html)
+ [Show Video Ads]({{site.baseurl}}/dev-docs/show-video-with-a-dfp-video-tag.html)
+ [Show Outstream Video Ads]({{site.baseurl}}/dev-docs/show-outstream-video-ads.html)
+ [Prebid.org Video Examples]({{site.baseurl}}/examples/video/)
+ [Prebid.org Native Examples]({{site.baseurl}}/examples/native/)

</div>

<!-- Reference Links -->

[conditionalAds]: {{site.baseurl}}/dev-docs/conditional-ad-units.html
[setConfig]: {{site.baseurl}}/dev-docs/publisher-api-reference.html#module_pbjs.setConfig