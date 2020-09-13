# WebSocket Demo
Source code is from Python [websockets module](https://websockets.readthedocs.io/en/stable/intro.html)

## Steps
Idealy run this demo in an isolated Python virtualenv, Python >= 3.6.1.

After activate the virtual environment, install websockets module:
```
python -m pip install websockets
```

### Insecure Example
In one terminal, executing:
```
./websocket_server.py
```

Open another terminal, executing:
```
./websocket_client.py
```

Open Wireshark or using tcpdump, listening on Loopback interface and port `8765`:
```bash
## request
Frame 11: 338 bytes on wire (2704 bits), 338 bytes captured (2704 bits) on interface 0
Null/Loopback
Internet Protocol Version 6, Src: ::1, Dst: ::1
Transmission Control Protocol, Src Port: 59121, Dst Port: 8765, Seq: 1, Ack: 1, Len: 262
Hypertext Transfer Protocol
    GET / HTTP/1.1\r\n
    Host: localhost:8765\r\n
    Upgrade: websocket\r\n
    Connection: Upgrade\r\n
    Sec-WebSocket-Key: z35SkqS0tSdZO3kgzKPJ0A==\r\n
    Sec-WebSocket-Version: 13\r\n
    Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits\r\n
    User-Agent: Python/3.7 websockets/8.1\r\n
    \r\n
    [Full request URI: http://localhost:8765/]
    [HTTP request 1/1]
    [Response in frame: 13]


## response
Frame 13: 323 bytes on wire (2584 bits), 323 bytes captured (2584 bits) on interface 0
Null/Loopback
Internet Protocol Version 6, Src: ::1, Dst: ::1
Transmission Control Protocol, Src Port: 8765, Dst Port: 59121, Seq: 1, Ack: 263, Len: 247
Hypertext Transfer Protocol
    HTTP/1.1 101 Switching Protocols\r\n
    Upgrade: websocket\r\n
    Connection: Upgrade\r\n
    Sec-WebSocket-Accept: GgofGhOWFvzchx4NucKxXIW3zo0=\r\n
    Sec-WebSocket-Extensions: permessage-deflate\r\n
    Date: Sun, 13 Sep 2020 22:20:24 GMT\r\n
    Server: Python/3.7 websockets/8.1\r\n
    \r\n
    [HTTP response 1/1]
    [Time since request: 0.000393000 seconds]
    [Request in frame: 11]
    [Request URI: http://localhost:8765/]
```


### Secure Example
In the same directory, generate self-signed certificate:
```
openssl req \
    -newkey rsa:2048 -new -nodes -x509 -days 3650 \
    -keyout ./localhost.pem -out ./localhost.pem \
    -subj "/C=US/ST=CA/L=San Jose/O=XXX/OU=Org/CN=localhost"
```

In one terminal, executing:
```
./websocket_tls_server.py
```

Open another terminal, executing:
```
./websocket_tls_client.py
```

Open Wireshark or using tcpdump, listening on Loopback interface and port `8765`:
```bash
## request
Frame 9: 274 bytes on wire (2192 bits), 274 bytes captured (2192 bits) on interface 0
Null/Loopback
Internet Protocol Version 6, Src: ::1, Dst: ::1
Transmission Control Protocol, Src Port: 59126, Dst Port: 8765, Seq: 1, Ack: 1, Len: 198
Secure Sockets Layer
    TLSv1.2 Record Layer: Handshake Protocol: Client Hello
        Content Type: Handshake (22)
        Version: TLS 1.0 (0x0301)
        Length: 193
        Handshake Protocol: Client Hello


## response
Frame 11: 1299 bytes on wire (10392 bits), 1299 bytes captured (10392 bits) on interface 0
Null/Loopback
Internet Protocol Version 6, Src: ::1, Dst: ::1
Transmission Control Protocol, Src Port: 8765, Dst Port: 59126, Seq: 1, Ack: 199, Len: 1223
Secure Sockets Layer
    TLSv1.2 Record Layer: Handshake Protocol: Server Hello
    TLSv1.2 Record Layer: Handshake Protocol: Certificate
    TLSv1.2 Record Layer: Handshake Protocol: Server Key Exchange
    TLSv1.2 Record Layer: Handshake Protocol: Server Hello Done
```
Since it is secured, you cannot see the content of the traffic.

