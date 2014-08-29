# HIDENAT

The cryptographical operation in HIDENAT are two: 

**Authentication**: is done during the three way handshake, is compatible with the [SYNCookie](http://en.wikipedia.org/wiki/SYN_cookies) logic. The apparently random bytes usable by the client, are PseudoRandom. 

**Continuantion**: is done after the three way handshake, is compatible with TCP/IP communication and indistinguisible by a normal TCP connection. only 

## Disclaimer/Note

I'm not an expert cryptographer, so I will not choose the best cryptographical approach for this solution. This I'm gonna to describe is just the concent prototype.

## Authentication

We have 5 bytes available for this steganographical auth/protection (3 from the random part of the ISN, 2 for the IP id), so used:

   * 256 possible users/key pair need 1 byte to be used
   * a portion of secret is 3 byte 
   * 1 byte is random padding or IV or such.


## Continuation

   * once the secret is know, the server generate a list of IP.ID expected, and this sequence and the random padding is shared is possible to the client send packet having IP.ID in the right sequence.
   * IP packet can be loss or duplicated, this mean that the sequence can be not stricly respected (for examle: ExpectedID1 should be followed by ExpectedID3 and then ExpectedID2 reach the host. what is important, is that EID are used only once, and when consumed a new list is generated)



