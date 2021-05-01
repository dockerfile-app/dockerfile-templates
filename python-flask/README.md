# About Python Flask

<a target="_blank" rel="noopener noreferrer" href="https://palletsprojects.com/p/flask/">
    <img alt="Python Flask Logo" align="right" src="/python-flask/logo.png" width="100px" />
</a>

[Flask](https://palletsprojects.com/p/flask/) is a lightweight WSGI Python web application framework. It is designed to make getting started quick and easy, with the ability to scale up to complex applications.

## Sample Dockerfile
```Dockerfile
FROM python:3.8.5-alpine
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["main.py"]

EXPOSE 80
```
