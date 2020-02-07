# UniSender Python client


Python open source client for [UniSender API](https://www.unisender.com/ru/support/api/api/)

### Features
- Client for low-level access.
- SimpleClient to quickly start mailing

### Requirements
- Python >= 3.6
- requests

### Installation

pip install git+git://github.com/antidasoftware/unisender-python-client.git


### Basic usage

Create an account in the [service](https://www.unisender.com) and get the API key in your account settings.


```python
from unisender import Client

cl = Client(
  api_key="your_api_key",
  platform="example",
  lang="ru"
)
```
##### Client configuration

- **base_url** [str] - API URL (default: "https://api.unisender.com")
- **lang** [str] - API messages language, available: "ru", "en", "it" (default: "en")
- **format** [str] - API response format, only "json" (default: "json")
- **api_key*** [str] - Unisender account api key,
- **platform*** [str] - API tracking marker

##### API Methods call example

See details about called methods in the [API docs](https://www.unisender.com/ru/support/api/api)

```python
# create contacts fields
cl.create_field(name='username', type='string')

# import contacts to API
response = cl.import_contacts(
	field_names=['email', 'name'],
    data=[
        ['example1@gmail.com', 'John Lennon'],
        ['example2@gmail.com', 'Paul McCartney']
    ]
)
```

### Advanced usage

For fast mailing start your need use SimpleClient.

```python
from datetime import datetime, timedelta

recipients = [
    {'email': 'example_3@gmail.com', 'name': 'Dave Guard'},
    {'email': 'example_4@mail.ru', 'name': 'Bon Shane'},
    {'email': 'example_5@yandex.ru', 'name': 'Nick Reynolds'},
]
```
 SimpleClient provide **create_email_campaign** method for quick start mailing.

 There are three mailing modes:

 1. Send custom email message with body as HTML
```python
time_now = datetime.now() + timedelta(hours=2)

create_email_campaign(
    email_type='html',
    email_data={
        "subject":     'Mail subject',
        "sender_name": 'John Lenon',
        "sender_email": 'example_1@gmail.com',
        "body": '<html>...</html>',
        "categories": ['First', 'Second']
    },
    recipients=recipients,
    start_time=time_now,
)

```
2. Send email message from UniSender custom template:

```python
time_delay = datetime.now() + timedelta(hours=2)

create_email_campaign(
    email_type='template',
    email_data={
        "sender_name": 'John Lenon',
        "sender_email": 'example_1@gmail.com',
        "template_id": 4185485
    },
    recipients=recipients,
    start_time=time_delay,
)
```
3. Send email message from UniSender system template:

``` python
fixed_time = datetime.strptime('2020-02-07 17:00', '%Y-%m-%d %H:%M')

create_email_campaign(
    email_type='system_template',
    email_data={
        "sender_name": 'John Lenon',
        "sender_email": 'example_1@gmail.com',
        "system_template_id": 54485
    },
    recipients=recipients,
    start_time=fixed_time,
    timezone='UTC'
)
```
- **email_type** [str] - one of "html", "template", "system_template"
- **email_data*** [dict] - email message data. Fields description see in ["createEmailMessage" docs](https://www.unisender.com/ru/support/api/messages/createemailmessage/)
- **recipints*** [list] - recipient list, will be imported into the API as contacts
- **start_time** [datetime] - mailing start time
- **timezone** [str] - time zone that indicates propagation time. Available only "UTC" (default: time zone from your account settings)