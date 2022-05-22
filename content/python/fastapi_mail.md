---
title: "python send mail"
draft: true
---

### fastapi send mail API
```sh
import uvicorn
from fastapi import FastAPI, BackgroundTasks, UploadFile, File, Form
from starlette.responses import JSONResponse
from fastapi_mail import FastMail, MessageSchema, ConnectionConfig
from pydantic import BaseModel, EmailStr
from typing import List, Text


class EmailSchema(BaseModel):
    email: List[EmailStr]
    html: Text

conf = ConnectionConfig(
	# update username, password, from
    # example qq mail
    # https://service.mail.qq.com/cgi-bin/help?subtype=1&&id=28&&no=1001256
    MAIL_USERNAME="qq123456",
    MAIL_PASSWORD="password",
    MAIL_FROM="qq123456@qq.com",
    MAIL_PORT=587,
    MAIL_SERVER="smtp.qq.com",
    
    MAIL_TLS=True,
    MAIL_SSL=False,
    USE_CREDENTIALS=True,
    VALIDATE_CERTS=True,
)

app = FastAPI()


html = """
<p>Thanks for using Fastapi-mail</p>
"""


@app.post("/email")
async def simple_send(email: EmailSchema) -> JSONResponse:

    message = MessageSchema(
        subject="Fastapi-Mail module",
        recipients=email.dict().get(
            "email"
        ),  # List of recipients, as many as you can pass
        body=email.dict().get("html", html),
        subtype="html",
    )

    fm = FastMail(conf)
    await fm.send_message(message)
    return JSONResponse(status_code=200, content={"message": "email has been sent"})


@app.post("/file")
async def send_file(
    background_tasks: BackgroundTasks,
    file: UploadFile = File(...),
    email: EmailStr = Form(...),
) -> JSONResponse:

    message = MessageSchema(
        subject="Fastapi mail module",
        recipients=[email],
        body="Simple background task ",
        attachments=[file],
    )

    fm = FastMail(conf)

    background_tasks.add_task(fm.send_message, message)

    return JSONResponse(status_code=200, content={"message": "email has been sent"})

if __name__ == '__main__':
    uvicorn.run('main:app', reload=True, host='127.0.0.1', port=8000)
    # https://sabuhish.github.io/fastapi-mail/example/
```

### yagmail
```sh
import yagmail

username = "xxx@qq.com"
password = "xxx"
host = "smtp.qq.com"

mail = yagmail.SMTP(user=username, password=password, host=host)

mail.send(to=username, subject="这是主题", contents=["这是内容", r"./logs/1695814_1.png"])
print("finish !")
```
