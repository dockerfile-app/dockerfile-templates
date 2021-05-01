# About Python Django

<a target="_blank" rel="noopener noreferrer" href="https://www.djangoproject.com/">
    <img alt="Python Django Logo" align="right" src="/python-django/logo.png" width="100px" />
</a>

[Django](https://www.djangoproject.com/) is a high-level Python Web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of Web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source. 

## Sample Dockerfile
```Dockerfile
FROM python:3

WORKDIR /usr/src/app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .

EXPOSE 80
CMD ["python", "manage.py", "runserver", "0.0.0.0:80"]
```
