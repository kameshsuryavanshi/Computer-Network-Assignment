# CN Assignment 4

## Name : Kameshbhai Pravinbhai Suryavanshi

## Reg. No: 23MCS0041

## Sub : CN Lab

## Implement the following flow control protocol
i) Go back n ARQ

```c
#include<bits/stdc++.h>
#include<ctime>
#define ll long long int
using namespace std;

void transmission(ll & i, ll & N, ll & tf, ll & tt) {
  while (i <= tf) {
    int z = 0;
    for (int k = i; k < i + N && k <= tf; k++) {
      cout << "Sending Frame " << k << "..." << endl;
      tt++;
    }
    for (int k = i; k < i + N && k <= tf; k++) {
      int f = rand() % 2;
      if (!f) {
        cout << "Acknowledgment for Frame " << k << "..." << endl;
        z++;
      } else {
        cout << "Timeout!! Frame Number : " << k << " Not Received" << endl;
        cout << "Retransmitting Window..." << endl;
        break;
      }
    }
    cout << "\n";
    i = i + z;
  }
}

int main() {
  ll tf, N, tt = 0;
  srand(time(NULL));
  cout << "Enter the Total number of frames : ";
  cin >> tf;
  cout << "Enter the Window Size : ";
  cin >> N;
  ll i = 1;
  transmission(i, N, tf, tt);
  cout << "Total number of frames which were sent and resent are : " << tt <<
    endl;
  return 0;
}

```

![Untitled](Untitled.png)

![Untitled](Untitled%201.png)

### ii) Selective repeat ARQ

```c
#include<iostream>

using namespace std;

int main()
{
    int w,i,f,frames[50];

    cout<<"Enter window size: ";
    cin>>w;

    cout<<"\nEnter number of frames to transmit: ";
    cin>>f;

    cout<<"\nEnter "<<f<<" frames: ";

    for(i=1;i<=f;i++)
        cin>>frames[i];

    cout<<"\nWith sliding window protocol the frames will be sent in the following manner (assuming no corruption of frames)\n\n";
    cout<<"After sending "<<w<<" frames at each stage sender waits for acknowledgement sent by the receiver\n\n";

    for(i=1;i<=f;i++)
    {
        if(i%w==0)
        {
            cout<<frames[i]<<"\n";
            cout<<"Acknowledgement of above frames sent is received by sender\n\n";
        }
        else
            cout<<frames[i]<<" ";
    }

    if(f%w!=0)
        cout<<"\nAcknowledgement of above frames sent is received by sender\n";

    return 0;
}

```

![Untitled](Untitled%202.png)

### Que :- Simulate a network and send packets between the networks. Trace out the each layer activities, services, protocols using any one simulation tools and plot the performance analysis.

Configure network :-

![Untitled](Untitled%203.png)

### **Network Setup:**

- **Devices:** 6 devices (computers or end devices)
- **Switches:** 2 switches
- **Routers:** 2 routers

### **Router 1:**

- LAN IP: 192.168.10.1
- Serial Interface: 192.168.50.1
- Connected Devices:
    - Device 1: IP 192.168.10.2
    - Device 2: IP 192.168.10.2
    - Device 3: IP 192.168.10.3

### **Router 2:**

- LAN IP: 192.168.20.1
- Serial Interface: 192.168.50.2
- Connected Devices:
    - Device 4: IP 192.168.20.2
    - Device 5: IP 192.168.20.2
    - Device 6: IP 192.168.20.3
    

![Untitled](Untitled%204.png)

![Untitled](Untitled%205.png)

![Untitled](Untitled%206.png)

![Untitled](Untitled%207.png)

![Untitled](Untitled%208.png)

![Untitled](Untitled%209.png)

### output:-

![Untitled](Untitled%2010.png)

![Untitled](Untitled%2011.png)

![Untitled](Untitled%2012.png)

1. **Layer Activities:**
    - **Physical Layer:** Handles the physical connection of devices (e.g., Ethernet cables, serial cables).
    - **Data Link Layer:** Manages data transfer between directly connected devices (e.g., switches handling MAC addresses).
    - **Network Layer:** Handles routing and logical addressing (e.g., routers using IP addresses for routing).
    - **Transport Layer:** Manages end-to-end communication (e.g., TCP/IP protocol ensuring reliable data delivery).
    - **Application Layer:** Supports network applications (e.g., web browsers, email clients).
2. **Services:**
    - **Switches:** Provide local network connectivity by forwarding data frames based on MAC addresses.
    - **Routers:** Connect different networks, perform routing based on IP addresses, and provide network access control.
    - **TCP/IP Protocol:** Ensures reliable data delivery, error checking, and flow control between devices.
3. **Protocols:**
    - **Ethernet Protocol:** Used by switches for local network communication.
    - **IP Protocol:** Used by routers for logical addressing and routing between networks.
    - **TCP Protocol:** Provides reliable, connection-oriented data delivery with error detection and correction.
    - **HTTP, FTP, etc. (Application Layer Protocols):** Used by network applications for specific services like web browsing, file transfer, etc.
4. **Packet Flow:**
    - Device 1 wants to communicate with Device 4:
        - Device 1 sends an HTTP request (Application Layer) to its default gateway (Router 1).
        - Router 1 checks the destination IP (192.168.20.2) and forwards the packet to Router 2 via the serial interface (Network Layer).
        - Router 2 receives the packet, performs routing based on the destination IP, and forwards it to Device 4 (Data Link and Physical Layers).
        

GitHub link :- 

[https://github.com/kameshsuryavanshi/TCP-IP_simulation](https://github.com/kameshsuryavanshi/TCP-IP_simulation)