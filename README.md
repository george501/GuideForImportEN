# Guide to Profile Import with Postman

In this article we will explain how to import a profile into the **Altcraft** database using Postman. After reading this guide, any user can easily transfer a client profile without losing his data. So, let's begin:

![1](https://github.com/george501/GuideForImport/blob/main/logo.png?raw=true)

# Download and install Postman

To send a request we need the program **Postman**. Postman is an application that allows you to send and receive API requests. The current version of the application can be downloaded [on the official website of the program](https://www.postman.com/downloads/). If you don't want to download Postman to your device, the app has a web version. With its help, you can work with requests directly from the browser.

![2](https://github.com/george501/GuideForImport/blob/main/guide%201.png?raw=true)

To save your requests, you will need to register or log into an existing profile. When you first launch the program, it will suggest you to do this, and you can do this later by clicking on the button on the top panel of the application.

![3](https://github.com/george501/GuideForImport/blob/main/guide2.png?raw=true)

## Create new request

In order to create the request, click on the **New** button at the top of the application. You can select the format you need (HTTP), a window with our future request opens. It's also necessary to change the request format from **GET** to **POST**. In the URL, enter https://example.com/api/v1.1/profiles/get/, where instead of _example.com/_, enter the link to your URL API.

![4](https://github.com/george501/GuideForImport/blob/main/Ap5CoB5b64%20(1).gif?raw=true)

## Write a request

In order to write the request itself, you need to go to the **Body** section and select the _raw_ mode. To the right you can select the request format; in our example we use JSON, but you can use XML. An input field will appear below, where you need to enter a request.


![5](https://github.com/george501/GuideForImport/blob/main/guide%203.png?raw=true)

## Request propeties

The request must contain some required parameters, without which the request cannot be sent. There are also optional request parameters and a _"subscriptions"_ array. This array stores data about profile subscriptions. For each subscription you need to specify a unique parameter.

### Required parameters 

|Parameter | Description                                                                                    |
|---------|---------------------------------------------------------------------------------------------|
| `token`   | The token is required for authorization; it can be found in the platform user panel |
| `db_id`   | Database ID                                                                   |
| `data`    | profile information including subscriptions                                                          |

> [!NOTE]
> Parameter _matching_ is not required, if you are searching by email from a profile 

### Optional parameters

|Parameter | Description                                                                                    |
|---------|---------------------------------------------------------------------------------------------|
| `matching` | Subscriber search mode. Default - email |
| `skip_triggers` | Skip running triggers (default is false) |
| `skip_invalid_subscriptions` |	Skip invalid subscriptions (default – false) |
| `detect_geo` |	Enables auto-detection of geo data |
| `add_to_segments` |	Adding a profile to a segment |
| `remove_from_segments` | Removing a profile from a segment |

### Массив subscriptions

|Parameter | Description                                                                                    |
|---------|---------------------------------------------------------------------------------------------|
| `resource_id` | Resource ID |
| `status` | Status of subscription |
| `priority` | Priority of subscription |
| `custom_fields` | Standard and additional subscription fields |
| `cats` | Resource categories to subscribe to your profile |
| **Email channel** |
| `channel` | Channel type |
| `email` | Email |
| **SMS channel** |
| `channel` | Channel type |
| `phone` | Phone number |
| **Push channel** |
| `channel` | Channel type |
| `provider` | Provider type |
| `subscription_id`	| Subscription ID |
| **Telegram Bot channel**: |
| `channel` | Channel type |
| `cc_data` | Telegram bot chat id |
| **WhatsApp channel**: |
| `channel` | Channel type |
| `cc_data` | WhatsApp profile phone number |
| **Viber channel**: |
| `channel` | Channel type |
| `cc_data` | Viber profile phone number |


### Example of request

```JSON
{
     "token": "91f1dfa81c264a938b475677c60ce967",
     "db_id": 1,
     "matching": "email",
     "email": "example@example.com",
     "detect_geo": true,
     "data": {
        "_fname": "Olly",
        "_lname": "Lambert",

        "email": "example@example.com",
        "phones": ["+79000000000"],
        "subscriptions": [
            {
                "channel": "email",
                "email": "example@example.com",
                "resource_id": 2,
                "custom_fields": {
                    "_browser_name": "Firefox",
                    "_device_type": "web",
                    "custom_field_1": "test value"
                },
                "cats": [
                    "category_1",
                    "category_2"
                ]
            },
            {
                "channel": "sms",
                "phone": "+79000000000",
                "resource_id": 1
            },
            {
                "channel": "push",
                "subscription_id": "a81c264a938b475",
                "provider": "android-firebase",
                "resource_id": 1
            },
            {
                "channel": "telegram_bot",
                "cc_data": {"user_chat_id"}
            },
            {
                "channel": "whatsapp",
                "cc_data": {"user_phone"}
            },
            {
                "channel": "viber",
                "cc_data": {"user_phone"}
            }
        ],
        "_bdate": "1990-02-22T21:00:00Z",
        "_sex": 0,

        "_regdate": "2019-03-14T22:00:00Z",
        "_regip": "94.231.119.122",
        "_ip": "94.231.119.122",

        "_tz": "Europe/Moscow",
        "_postal_code": "390000",

        "_os": "Windows 10",
        "_browser": "Firefox",

        "_vendor": "form_#31",

        "custom_field": "custom_value"
    }
}
```

## Send a request

When your request is ready, click on the **Send** button. If the request was sent successfully, a response will appear in the window below:

```JSON
{
  "error": 0,
  "error_text": "Successful operation",
  "profile_id": "54759eb3c090d83494e2d804"
}
```

where _"profile_id"_ is the ID of the imported profile in the database

## Check if the profile appears in the database

Once the request has been sent, you can check if the profile is displayed correctly in the database using the request:

```JSON
{
    "db_id": 1,
    "email": "john@example.com",
    "token": "abcdefghijklmnqrstuvwxyz"
}
```

where _db_id_ is the database identifier, _token_ is your API token, and _email_ is the email by which the profile will be found.

## Congrats

Congratulations! If you received a success message and found the profile using the request, then you send your request correctly. If you receive an error in the response to your request, read the description of the error and try to resolve it. If this doesn't help, please let us know on Telegram.
