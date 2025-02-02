# CN Assignment 1

## Name : Kameshbhai Pravinbhai Suryavanshi

## Reg. No: 23MCS0041

## Sub : CN Lab

### **1. Study about Networking Commands**

1. **ifconfig**: View and configure network interfaces.
    
    ```bash
    ifconfig
    ```
    
    ![Untitled](Untitled.png)
    
2. **ip**: Show or manipulate routing, devices, policy routing, and tunnels.
    
    ```bash
    ip addr show
    ```
    
    ![Untitled](Untitled%201.png)
    
3. **ping**: Test the reachability of a host.
    
    ```bash
    ping google.com
    ```
    
    ![Untitled](Untitled%202.png)
    
4. **traceroute**: Print the route packets take to a network host.
    
    ```bash
    traceroute google.com
    ```
    
    ![Untitled](Untitled%203.png)
    
5. **nslookup**: Query DNS for information about a domain or IP address.
    
    ```bash
    nslookup google.com
    ```
    
    ![Untitled](Untitled%204.png)
    
6. **netstat**: Print network connections, routing tables, interface statistics, masquerade connections, etc.
    
    ```bash
    netstat -a
    ```
    
    ![Untitled](Untitled%205.png)
    
7. **route**: Show or manipulate the IP routing table.
    
    ```bash
    route -n
    ```
    
    ![Untitled](Untitled%206.png)
    
8. **arp**: Display and modify the Address Resolution Protocol cache.
    
    ```bash
    arp -a
    ```
    
    ![Untitled](Untitled%207.png)
    
9. **dig**: DNS lookup utility.
    
    ```bash
    dig google.com
    ```
    
    ![Untitled](Untitled%208.png)
    
10. **host**: DNS lookup utility.
    
    ```bash
    host example.com
    ```
    
    ![Untitled](Untitled%209.png)
    
11. **curl**: Transfer data from or to a server.
    
    ```bash
    curl <http://google.com>
    ```
    
    ![Untitled](Untitled%2010.png)
    
12. **wget**: Non-interactive downloader.
    
    ```bash
    wget <http://google.com/file.tar.gz>
    ```
    
    ![Untitled](Untitled%2011.png)
    
13. **nmap**: Network exploration tool and security/port scanner.
    
    ```bash
    nmap -sP 192.168.1.0/24
    ```
    
    ![Untitled](Untitled%2012.png)
    
14. **tcpdump**: Dump traffic on a network.
    
    ```bash
    sudo tcpdump -i eth0
    ```
    
    ![Untitled](Untitled%2013.png)
    
15. **iptables**: Tool to manage packet filtering rules in the Linux kernel.
    
    ```bash
    sudo iptables -L
    ```
    
    ![Untitled](Untitled%2014.png)
    
16. **traceroute6**: Print the route packets take to a network host (IPv6).
    
    ```bash
    traceroute6 example.com
    ```
    
    ![Untitled](Untitled%2015.png)
    

### **2.Implement the following using Socket programming.
a) Client-Server communication using UDP.**

Client.py

```c
import socket
def udp_client():
    server_address = ('localhost', 12345)

    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    while True:
        message = input("Enter message to send (type 'exit' to quit): ")
        if message.lower() == 'exit':
            break
        client_socket.sendto(message.encode('utf-8'), server_address)
        response, _ = client_socket.recvfrom(1024)
        print(f"Server response: {response.decode('utf-8')}")

    client_socket.close()

if __name__ == "__main__":
    udp_client()
```

Server.py

```c
import socket
def udp_server():
    server_address = ('localhost', 12345)
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_socket.bind(server_address)

    print(f"UDP server is listening on {server_address}")

    while True:
        print("Waiting for a message...")
        data, client_address = server_socket.recvfrom(1024)
        message = data.decode('utf-8')
        print(f"Received message from {client_address}: {message}")
        response = f"Server received your message: {message}"
        server_socket.sendto(response.encode('utf-8'), client_address)

if __name__ == "__main__":
    udp_server()
```

![Untitled](Untitled%2016.png)

## **b) Client-Server Communication using TCP**

client.py

```c
import socket
def tcp_client():
    server_address = ('localhost', 12345)
		client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    client_socket.connect(server_address)
    print(f"Connected to {server_address}")

    while True:
        message = input("Enter message to send (type 'exit' to quit): ")
        if message.lower() == 'exit':
            break
        client_socket.send(message.encode('utf-8'))
        response = client_socket.recv(1024)
        print(f"Server response: {response.decode('utf-8')}")
    client_socket.close()

if __name__ == "__main__":
    tcp_client()
```

server.py

```c
import socket
def tcp_server():
    server_address = ('localhost', 12345)
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(server_address)
    server_socket.listen(5)

    print(f"TCP server is listening on {server_address}")

    while True:
        print("Waiting for a connection...")
        client_socket, client_address = server_socket.accept()
        print(f"Accepted connection from {client_address}")
        data = client_socket.recv(1024)
        message = data.decode('utf-8')
        print(f"Received message from {client_address}: {message}")
        response = f"Server received your message: {message}"
        client_socket.send(response.encode('utf-8'))
        client_socket.close()

if __name__ == "__main__":
    tcp_server()
```

![Untitled](Untitled%2017.png)

### c) CHAT Application

client.py

```c
import socket
import threading

def receive_messages(client_socket):
    try:
        while True:
           message = client_socket.recv(1024).decode('utf-8')
            print(message)
    except Exception as e:
        print(f"Error receiving messages: {e}")
    finally:
        client_socket.close()

def send_messages(client_socket):
    try:
        while True:
						message = input()
           client_socket.send(message.encode('utf-8'))
    except Exception as e:
        print(f"Error sending messages: {e}")
    finally:
        client_socket.close()

def chat_client():
    server_address = ('localhost', 12345)
    client_socket = socket.socket(socket.AF_INET,
											 socket.SOCK_STREAM)
    client_socket.connect(server_address)

    client_name = input("Enter your name: ")
    client_socket.send(client_name.encode('utf-8'))

    receive_thread = threading.Thread(target=receive_messages, 
																		args=(client_socket,))
    send_thread = threading.Thread(target=send_messages,
																 args=(client_socket,))

    receive_thread.start()
    send_thread.start()

if __name__ == "__main__":
    chat_client()
```

server.py

```c
import socket
import threading

clients = []

def handle_client(client_socket, client_address, client_name):
    try:
        broadcast(f"{client_name} has joined the chat!",
																			 client_socket)
        while True:
            message = client_socket.recv(1024).decode('utf-8')
            if not message:
                break
            broadcast(f"{client_name}: {message}", client_socket)
    except Exception as e:
        print(f"Error: {e}")
    finally:
        clients.remove((client_socket, client_name))
        client_socket.close()
        broadcast(f"{client_name} has left the chat.", None)
def broadcast(message, sender_socket):
    for client, _ in clients:
        if client != sender_socket:
            try:
                client.send(message.encode('utf-8'))
            except Exception as e:
                print(f"Error broadcasting message: {e}")

def chat_server():
    server_address = ('localhost', 12345)
    server_socket = socket.socket(socket.AF_INET,
									 socket.SOCK_STREAM)
		server_socket.bind(server_address)
		server_socket.listen(5)
    print(f"Chat server is listening on {server_address}")
    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Accepted connection from {client_address}")
        client_name = client_socket.recv(1024).decode('utf-8')
        clients.append((client_socket, client_name))
        client_thread = threading.Thread(target=handle_client, 
						args=(client_socket, client_address, client_name))
        client_thread.start()

if __name__ == "__main__":
    chat_server()
```

![Untitled](Untitled%2018.png)