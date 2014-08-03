# xt_HIDENAT

steganographic authentication of TCP packets triggering different DNAT configuration (xtables)

## GOAL

Client and Server shared a secret, they can perform a connection authenticated on this secret. If someone without the secret connect to the same service, the sever is aware that the users is not authenticated, and can behave properly.

### (exotic) Use case

The Tor Bridges used by Chinese users receive a connection by what is called "Chinese probes". Those probe check is the SSL connection intercepted, is in fact an HTTPS connection or a Tor one. If is Tor, the Bridge is blacklisted. 

with HIDENAT the Tor users do not need only the server:port, but also a secret. Using this secret the user can connect to the Bridge, and the Chinese probe instead see a plausible HTTPS server.

### (common) Use case

You are a system administrator of something, (let say, the service Bob on Holly server) and you don't want keep SSH exposed online. so if you perform an steganographic-authenticated connection to Bob, the destination port is Nat-ted to SSH.   

## Technology scenario and fictional characters

  * **Alice**: User performing connection, in posses of a secret
  * **Mallory**: network sniffer monitoring all the TCP connections and performing connection, or taking existing connection of Alice.
  * **Bob**: the service Alice wants connect to
  * **Charlie**: User performing a connection to Bob, without having any secret
  * **Holly**: an host running HIDENAT, the same technology of Alice
  * **Eve**: global passive observer monitoring and recording Alice's, Charlie's and Mallory's connections.

Alice is performing a connection to the service Bob, served by host Holly. 

If an eaversdropper Mallory see the connection, and perform one itself to Holly (at
the same TCP port), instead of be connected to Bob is connected to a Fake service
called Charlie.

## Technology 

The technology in study, do not want change anything in the current TCP stack 
protocol (as extension headers) or in the application server. 

The technology in study, would detect eventually also a TCP session hijack performed
actively by Mallory on Alice's connection.

If the Connections to Holly are encrypted is not possible to discriminate between Alice and Charlie
connections to Holly. (This do not protect from pattern recognition of packets frequency and size) 

## Authentication

Bob can update the table of the expected secret and associated port via HTTP REST API.
Alice too.


## What's guarantee

  * The SYN can be sent by Alice or Mallory
  * Bob answer with the SYNCookie
  * The ACK packet is sent by Alice or Mallory
  * Since the ACK packet, is possible for Bob understand if Alice (and which 'records' associated) or Mallory has established the connection, and perform DNAT to the appropriate port.


  * Eve has not the possibility to recognize if Alice is performing an authentication connection to Bob, or if Charlie or Mallory (both not authenticated) are connected to a different service.
  * If Mallory perform TCP hijacking of an established session (by Alice or Charlie, are indistiguishable), the attack is recognized by Bob (an alarm can be raised)

