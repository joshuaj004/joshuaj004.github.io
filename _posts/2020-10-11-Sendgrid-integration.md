---
layout: post
title:  "Sendgrid Integration"
categories: emails
date: 2020-10-11 16:05:00
---

A few days ago, I signed up for a sendgrid account and was wanting to be able to programmatically send emails. I was getting stuck on adding sendgrid to the program b/c it was complaining that sendgrid wasn't being added. I had run `poetry add sengrid` and it still wasn't being recognized and when I tried to rebuild my docker images, it was saying that no changes had been made. I finally solved this by deleting the existing images and forcing a rebuild. From there, I just completed [sendgrid's tutorial](https://app.sendgrid.com/guide/integrate/langs/python). I used a slightly modified version of this code snippet:

```python
import os
from sendgrid import SendGridAPIClient
from sendgrid.helpers.mail import Mail

message = Mail(
    from_email='from_email@example.com',
    to_emails='to@example.com',
    subject='Sending with Twilio SendGrid is Fun',
    html_content='<strong>and easy to do anywhere, even with Python</strong>')
try:
    sg = SendGridAPIClient(os.environ.get('SENDGRID_API_KEY'))
    response = sg.send(message)
    print(response.status_code)
    print(response.body)
    print(response.headers)
except Exception as e:
    print(e.message)
```


![](/../assets/2020-10-11-16-08-42.png)

And voila - email sent!