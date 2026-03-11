# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1. Start server.py, create socket, bind to localhost:3024, and listen for connections.
2. Run client.py, create socket and connect to the server.
3. Client asks user to choose Download (GET) or Upload (POST).
4. Client sends the selected HTTP request to the server.
5. Server processes request: GET → send index.html, POST → save data to upload.txt.
6. Server sends response back and client displays the result.

## Program 
## server.py
~~~
import socket

s = socket.socket()
s.bind(("localhost",3024))
s.listen(1)

print("Server running...")

while True:
    c,addr = s.accept()
    
    request = c.recv(1024).decode()
    print("Request received")

    if "GET" in request:
        f = open("index.html","r")
        data = f.read()
        f.close()

        response = "HTTP/1.1 200 OK\n\n" + data
        c.send(response.encode())

    elif "POST" in request:
        data = request.split("\n\n")[1]

        f = open("upload.txt","w")
        f.write(data)
        f.close()

        c.send("HTTP/1.1 200 OK\n\nFile Uploaded".encode())

    c.close()
~~~
## client.py
~~~
import socket

s = socket.socket()
s.connect(("localhost",3024))

ch = input("1.Download 2.Upload : ")

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
~~~
## index.html
~~~
<html>
    <head>
        <title>index html server</title>
    </head>
    <body>
        <h1>Hello from python socket server</h1>
        
    </body>
   
</html>
~~~
## OUTPUT
<img width="1216" height="674" alt="Screenshot 2026-03-11 110837" src="https://github.com/user-attachments/assets/9a3b39f9-ed2b-4c07-b478-54be3b362c0b" />
<img width="964" height="434" alt="Screenshot 2026-03-11 111241" src="https://github.com/user-attachments/assets/a398ac4b-ad88-45d5-9dd4-9766bbd3cd66" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
