# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
## Client:
```
import socket
s = socket.socket()
s.connect(("localhost", 5050))
ch = input("1.Download  2.Upload : ")
if ch == "1":
    req = "GET / HTTP/1.1\nHost: localhost\n\n"
    s.send(req.encode())
    data = s.recv(4096)
    print(data.decode())
else:
    msg = input("Enter data to upload: ")
    req = "POST / HTTP/1.1\nHost: localhost\n\n" + msg
    s.send(req.encode())
    data = s.recv(1024)
    print(data.decode())

s.close()
```
## Server:
```
import socket
s = socket.socket()
s.bind(("localhost", 5050))
s.listen(1)
print("Server running...")

while True:
    c, addr = s.accept()
    request = c.recv(4096).decode()
    print(f"Request received ")

    if "GET" in request:
        try:
            with open("index.html", "r") as f:
                data = f.read()
            response = "HTTP/1.1 200 OK\n\n" + data
        except FileNotFoundError:
            response = "HTTP/1.1 404 Not Found\n\nFile not found"
    elif "POST" in request:
        body = request.split("\r\n\r\n", 1)[-1]
        with open("upload.txt", "w") as f:
            f.write(body)
        response = "HTTP/1.1 200 OK\n\nFile Uploaded"
    else:
        response = "HTTP/1.1 400 Bad Request\n\nUnknown method"

    c.send(response.encode())
    c.close()
```
## Index:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>hello word</p>
</body>
</html>
```

## OUTPUT
## Client:

<img width="1077" height="493" alt="Screenshot 2026-05-25 083917" src="https://github.com/user-attachments/assets/032ca3ab-2ba0-4ee4-87f7-d718f1df3c8b" />

## Server:

<img width="1073" height="266" alt="Screenshot 2026-05-25 083938" src="https://github.com/user-attachments/assets/663d6d06-34d1-47db-9984-3e66454cfe82" />

## Upload:

<img width="590" height="270" alt="Screenshot 2026-05-25 083958" src="https://github.com/user-attachments/assets/00c7de48-f411-4ac6-a0c6-c25134745c13" />
<img width="590" height="227" alt="Screenshot 2026-05-25 084701" src="https://github.com/user-attachments/assets/39e41d2e-6411-4a41-ab60-ed92032782f6" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
