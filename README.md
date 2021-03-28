# RawSockets

===========================================
Approach:
===========================================

The main approach taken was to work on three layers - IP, TCP and Application layer. The URL is taken from the user, and the Application layer makes the request. A three way handshake is ensured to establish connection for the TCP layer. Once the data is packed by the TCP layer and IP layer, the final datagram is retrieved. Checksums of incoming packets are verified. Correct checksums are generated for the outgoing packets. A valid local port is chosen for the traffic flow. Seq number and Ack numbers are used for verifying. A 3min timeout has been implemented. Once the packets are received, the duplicates should be discarded and all packets should be reordered. Congestion control is implemented for flow of data for downloading the requested file.

==========================================
File structure
==========================================

The root directory is Rawsocket

./rawhttpget                   : shell script to launch program
./rawhttpget.py                : python script to implement the main TCP protocol
./util.py                      : help functions like unpack ip_datagram

Command to launch program:
sudo ./rawhttpget <url>

==========================================
Test commands (need root privileges)
==========================================

sudo ./rawhttpget http://webcrawler-site.ccs.neu.edu/static/rawhttp/2MB.log
sudo ./rawhttpget http://webcrawler-site.ccs.neu.edu/static/rawhttp/10MB.log
sudo ./rawhttpget http://webcrawler-site.ccs.neu.edu/static/rawhttp/50MB.log

===========================================
Implementation
===========================================

1. TCP and IP headers are built. Pack and unpack headers using struct

2. Filter receiving packets by comparing address and port number.

3. Check seq and ack_seq number values to validate packets.

4. Establish connection using a three way handshake.

5. Set basic timeout to 60s and only progress for 200 code.

6. Retransmit the packets if ACK is not fetched.

7. Use congestion control

8. After getting packets, reorder them. 

===========================================
Approach: 
===========================================

Send:
======================================
|IP_Header| + |TCP_Header| + |Payload|
======================================

Recv:
===================================
|IP_Header| + |TCP_Header| + |Data|
===================================

Further unpacked

=====================
|TCP_Header| + |Data|
=====================

Data retrieved from the TCP Segment.

======
|Data|
======

===========================================
Challenges:
===========================================
Checksum for bytes and string conversion
Figuring out that eth0 is not relevant to my system and was enp0s3.
Struct was used to simplify the packing and unpacking of TCP packets without which it was very time consuming while coding.
Parsing the URL was also a challenge. 
The VPN connection over the Virtualbox was a challenge because it would not disconnect and I would have to kill the process sometimes.
Struggled to receive ACK

