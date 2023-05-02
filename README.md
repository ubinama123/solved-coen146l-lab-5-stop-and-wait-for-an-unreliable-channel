Download Link: https://assignmentchef.com/product/solved-coen146l-lab-5-stop-and-wait-for-an-unreliable-channel
<br>
<h5><strong>TFv2 – Stop and Wait for an Unreliable Channel</strong></h5>

You have developed a TCP client/server to transfer file. A similar implementation can be made for UDP and let us name it as TFv1. To ensure a reliable transfer over UDP, you need to implement a reliable Stop and Wait protocol. This version of file transfer is then TFv2.




TFv2 will be built on top of UDP, and it is supposed to provide a reliable transport service to TFv1. Messages are sent one at a time, and each message needs to be acknowledged when received, before a new message can be sent. TFv2 implements basically the protocol rdt2.2 presented in the text book (depicted as well at the end of this document).




TFv2 consists of a client and a server. Communication is unidirectional, i.e., data flows from the client to the server. The server starts first and waits for messages. The client starts the communication. Messages have seq number 0 or 1. Before sending each message, a 1-byte checksum is calculated and added to the header. After sending each message, the client waits for a corresponding ACK. When it arrives, if it is not the corresponding ACK (or if the checksum does not match), the message is sent again. If it is the corresponding ACK, the client changes state and returns to the application, which can now send one more message. This means TFv2 blocks on writes until an ACK is received.




The server, after receiving a message, checks its checksum. If the message is correct and has the right seq number, the server sends an ACK0 or ACK1 message (according to the seq number) to the client, changes state accordingly, and deliver data to the application.




The protocol should deal properly with duplicate data messages and duplicate ACK messages.  Follow the FSM in the book!




TFv2 message contains the header and the application data. No reordering is necessary, since TFv2 is sending the exact message given by the application, one by one.




The checksum must be calculated for messages from the server and client. To calculate the checksum, calculate the XOR off all the bytes (header or header + data) when member cksum (see below) is zero. To verify your protocol, use the result of a random function to decide whether to send the right checksum or just zero. This will fake the error effect.




<h5><strong>PROTOCOL</strong></h5>

HEADER

seq_ack      int (32 bits)           // SEQ for data and ACK for Acknowledgement

len              int (32 bits)           // Length of the data in byes (zero for ACKS)

cksum         int (32 bits)           // Checksum calculated (by byte)




PACKET

header

data            char (10 bytes)




SENDER

<ul>

 <li>Member seq_ack is used as SEQ, and the data is in member data.</li>

 <li>Each packet may have 10 or less bytes of data, and the sender only sends the necessary bytes.</li>

 <li>After transmitting the file, a packet with no data (len = 0) is sent to notify the receiver that the file is complete.</li>

</ul>




RECEIVER

<ul>

 <li>Member seq_ack is used as ACK, and data is empty (len = 0).</li>

</ul>













<strong>Requirements to complete the lab</strong>

Show the TA correct execution of the programs you wrote and upload source code to Camino.




Be sure to retain copies (machine and/or printed) of your source code. You will want these for study purposes and to resolve any grading questions (should they arise)




Please start each program with a descriptive block that includes minimally the following information:

/*

* Name: &lt;your name&gt;

* Date:

* Title: Lab5 – ….

* Description: This program … &lt;you should

* complete an appropriate description here.&gt;

*/




<strong> </strong>

<strong>rdt2.2</strong><strong> sender – FSM</strong>










<strong> </strong>

<strong> </strong>

<strong> </strong>

<strong> </strong>

<strong>rdt2.2</strong><strong> receiver – FSM</strong>





