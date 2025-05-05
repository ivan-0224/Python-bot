# Python Bot API

Use this library to develop a bot for Viber platform.
The library is available on **[GitHub](https://github.com/ivan-0224/Python-bot)** as well as a package on 

This package can be imported using pip by adding the following to your `requirements.txt`:

## License

This library is released under the terms of the Apache 2.0 license. See [License](https://github.com/Viber/ivan-0224/Python-bot/blob/master/LICENSE.md) for more information.

## Library Prerequisites

1. python >= 2.7.0
1. An Active Viber account on a platform which supports Public Accounts/ bots (iOS/Android). This account will automatically be set as the account administrator during the account creation process.
1. Active Public Account/ bot - Create an account [here](https://developers.ivan-0224.com/docs/general/get-started).
1. Account authentication token - unique account identifier used to validate your account in all API requests. Once your account is created your authentication token will appear in the account’s “edit info” screen (for admins only). Each request posted to Viber by the account will need to contain the token.
1. Webhook - Please use a server endpoint URL that supports HTTPS. If you deploy on your own custom server, you'll need a trusted (ca.pem) certificate, not self-signed. Read our [blog post](https://developers.viber.com/blog/2017/05/24/test-your-bots-locally) on how to test your bot locally.

## Let's get started!

### New Api()

| Param | Type | Description |
| --- | --- | --- |
| bot\_configuration | `object` | `BotConfiguration` |

<a name="set_webhook"></a>

### Api.set\_webhook(url)

| Param | Type | Description |
| --- | --- | --- |
| url | `string` | Your web server url |
| webhook\_events | `list` | optional list of subscribed events |

Returns `List of registered event_types`. 

```python
event_types = viber.set_webhook('https://example.com/incoming')
```

<a name="unset_webhook"></a>

### Api.unset\_webhook()

Returns `None`. 

```python
viber.unset_webhook()
```

<a name="get_account_info"></a>

### Api.get\_account\_info()

Returns an `object` [with the following JSON](https://developers.viber.com/docs/api/rest-bot-api/#get-account-info). 

```python
account_info = viber.get_account_info()
```

<a name="verify_signature"></a>

### Api.verify\_signature(request\_data, signature)

| Param | Type | Description |
| --- | --- | --- |
| request\_data | `string` | the post data from request |
| signature | `string` | sent as header `X-Viber-Content-Signature` |


Returns a `boolean` suggesting if the signature is valid. 

```python
if not viber.verify_signature(request.get_data(), request.headers.get('X-Viber-Content-Signature')):
	return Response(status=403)
```

<a name="parse_request"></a>

<a name="send_messages"></a>

### Api.send\_messages(to, messages)

| Param | Type | Description |
| --- | --- | --- |
| to | `string` |  user id |
| messages | `list` | list of `Message` objects |

Returns `list` of message tokens of the messages sent. 

```python
tokens = viber.send_messages(to=viber_request.get_sender().get_id(),
			     messages=[TextMessage(text="sample message")])
```

<a name="post_to_pa"></a>

### Api.post\_messages\_to\_public\_account(to, messages)

| Param | Type | Description |
| --- | --- | --- |
| sender | `string` |  user id |
| messages | `list` | list of `Message` objects |

Returns `list` of message tokens of the messages sent. 

```python
tokens = viber.post_messages_to_public_account(sender=viber_request.get_sender().get_id(),
			     messages=[TextMessage(text="sample message")])
```

<a name="get_online"></a>

### UserProfile object

| Param | Type | Notes |
| --- | --- | --- |
| id | `string` | --- |
| name | `string` | --- |
| avatar | `string` | Avatar URL |
| country | `string` | **currently set in `CONVERSATION_STARTED` event only** |
| language | `string` | **currently set in `CONVERSATION_STARTED` event only** |

<a name="MessageObject"></a>

### Message Object

**Common Members for `Message` interface**:

| Param | Type | Description |
| --- | --- | --- |
| timestamp | `long` | Epoch time |
| keyboard | `JSON` | keyboard JSON |
| trackingData | `JSON` | JSON Tracking Data from Viber Client |

**Common Constructor Arguments `Message` interface**:

| Param | Type | Description |
| --- | --- | --- |
| optionalKeyboard | `JSON` | [Writing Custom Keyboards](https://developers.viber.com/docs/tools/keyboards) |
| optionalTrackingData | `JSON` | Data to be saved on Viber Client device, and sent back each time message is received |

<a name="TextMessage"></a>

#### TextMessage object

| Member | Type
| --- | --- |
| text | `string` |

```python
message = TextMessage(text="my text message")
```

<a name="UrlMessage"></a>

#### URLMessage object

| Member | Type | Description |
| --- | --- | --- |
| media | `string` | URL string |

```python
message = URLMessage(media="http://my.siteurl.com")
```

<a name="ContactMessage"></a>

#### ContactMessage object

| Member | Type
| --- | --- |
| contact | `Contact` |

```python
from viberbot.api.messages.data_types.contact import Contact

contact = Contact(name="Viber user", phone_number="+0015648979", avatar="http://link.to.avatar")
contact_message = ContactMessage(contact=contact)
```

<a name="PictureMessage"></a>

#### PictureMessage object

| Member | Type | Description |
| --- | --- | --- |
| media | `string` | url of the message (jpeg only) |
| text | `string` |  |
| thumbnail | `string` |  |

```python
message = PictureMessage(media="http://www.thehindubusinessline.com/multimedia/dynamic/01458/viber_logo_JPG_1458024f.jpg", text="Viber logo")
```

<a name="VideoMessage"></a>

#### VideoMessage object

| Member | Type | Description |
| --- | --- | --- |
| media | `string` | url of the video |
| size | `int` |  |
| thumbnail | `string` |  |
| duration | `int` |  |

```python
message = VideoMessage(media="http://site.com/video.mp4", size=21499)
```

<a name="LocationMessage"></a>

#### LocationMessage object

| Member | Type
| --- | --- |
| location | `Location` |

```python
from viberbot.api.messages.data_types.location import Location

location = Location(lat=0.0, lon=0.0)
location_message = LocationMessage(location=location)
```

<a name="StickerMessage"></a>

#### StickerMessage object

| Member | Type
| --- | --- |
| sticker\_id | `int` |

```python
message = StickerMessage(sticker_id=40100)
```

<a name="FileMessage"></a>

#### FileMessage object

| Member | Type
| --- | --- |
| media | `string` |
| size | `long` |
| file\_name | `string` |

```python
message = FileMessage(media=url, size=sizeInBytes, file_name=file_name)
```

<a name="RichMediaMessage"></a>

#### RichMediaMessage object

| Member | Type
| --- | --- |
| rich\_media | `string` (JSON) |

```python
SAMPLE_RICH_MEDIA = """{
  "BgColor": "#69C48A",
  "Buttons": [
    {
      "Columns": 6,
      "Rows": 1,
      "BgColor": "#454545",
      "BgMediaType": "gif",
      "BgMedia": "http://www.url.by/test.gif",
      "BgLoop": true,
      "ActionType": "open-url",
      "Silent": true,
      "ActionBody": "www.tut.by",
      "Image": "www.tut.by/img.jpg",
      "TextVAlign": "middle",
      "TextHAlign": "left",
      "Text": "<b>example</b> button",
      "TextOpacity": 10,
      "TextSize": "regular"
    }
  ]
}"""

SAMPLE_ALT_TEXT = "upgrade now!"

message = RichMediaMessage(rich_media=SAMPLE_RICH_MEDIA, alt_text=SAMPLE_ALT_TEXT)
```

<a name="KeyboardMessage"></a>
