---
layout: post
title:  "First Blog!"
date:   2023-12-09 16:30:30 -0700
tags: Cloud, AWS
---

# Introduction
This blog is a quick note on how to setup Fast API server on AWS EC2 instance

# What is Fast API
https://fastapi.tiangolo.com/

# Setting up EC2 instance
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

You will also need to add inbound rules in the security to allow Http and SSH access to the instance.

# Install FastAPI server
Follow the tutorial in https://fastapi.tiangolo.com/

You can use the example given in the tutorial to setup
To start the local server:
```
uvicorn main:app --reload
```
# Setting up Nginx as reverse proxy
- Install Nginx by
```bash
# ubuntu instance
sudo apt install nginx
# Amazon Linux instance
sudo yum install nginx 
```
- Create a default config file called default.conf in /etc/nginx/conf.d
- Add the following setting, notice that the proxy_pass is the uvicorn server local port and address
```
server {
    listen 80;
    server_name your_server_id.region.compute.amazonaws.com;
    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```
- Save above config, and restart the nginx by
```bash
sudo service nginx restart
```
- Checking nginx is running properly
```bash
sudo systemctl status nginx
```
- You may come across an error if you are using default EC2 DNS: nginx: 
> [emerg] could not build server_names_hash, you should increase server_names
- This is due to the default EC2 DNS Address is too long, you can add following line in the HTTP block
```
server_names_hash_bucket_size  128;
```
# Testing
After both FastAPI(Uvicorn) and Nginx server is up, you can try to curl the EC2 DNS address
```bash
curl http://your_server_id.region.compute.amazonaws.com/items/5?q=somequery
```
On Window, you will see the output
```
StatusCode        : 200
StatusDescription : OK
Content           : {"item_id":5,"q":"somequery"}
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Content-Length: 29
                    Content-Type: application/json
                    Date: Sat, 09 Dec 2023 23:55:13 GMT
                    Server: nginx/1.24.0

                    {"item_id":5,"q":"somequery"}
Forms             : {}
Headers           : {[Connection, keep-alive], [Content-Length, 29], [Content-Type, application/json], [Date, Sat, 09 Dec 2023 23:55:13 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 29
```