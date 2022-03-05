# What can Swaks do?

The [Swaks](http://www.jetmore.org/john/code/swaks/ "Swaks download page") can send all kind of test mails.The best part of Swaks is you can change any part in the email you like.Such as, to, from, header, attachments, which server to speak to, etc.

So, for now we know about what can Swaks do. But how could this tool use as an hacking tool?

The answer is the pishing email. Before we know how to use Swaks how to create a pishing email we need to know how SPF run.

# Principle of Email Delivery

To understand the tool Swaks, I will firstly introduce the process of sending an email. [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol "SMTP Wiki Page") is stand for simple mail transfer protocol which allow you to send your email to the SMTP server. 
 
SMTP is using the  [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol "TCP Wiki Page") protocol which is a transport protocol to ensure the tranmission of the information packet. 

There also exists two protocols allow you to receive the email from the mail server which are [IMAP](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol "IMAP Wiki Page") and [POP](https://en.wikipedia.org/wiki/Post_Office_Protocol "POP Wiki Page") protocol.


| Name of Protocol       | Function       |
| ---------------------  |----------------|
|SMTP (Simple Mail Transfer Protocol)|a communication protocol for mail transmission|
|TCP (Transmission Control Protocol)| a transprot protocol to ensure transmission of packets|
|IMAP (Internet Message Access Protocol)|an Internet standard protocol to retrieve email for email server|
|POP (Post Office Protocol)|an Internet standard protocol to retrieve email for email server|


# Example of Email Delivery

![alt text](https://raw.githubusercontent.com/KairoGoo/ece9069/main/Picture2.png) 
(References: https://www.afternerd.com/blog/smtp/)

Bob is sending an email to Alice, Bob needs to generate with his email address and Alice email address. 

Sending them from the email client to the email provider server through the SMTP protocol. Since Bob is using Gmail, then the address of the SMTP server should be 'SMTP.gmail.com'. 

And it will looking for Alice email domain which is 'Yahoo' and located into the Yahoo email server. The email will stay in the Yahoo email server. Alice will only receive her email until she login in her email account and download the email using PCP and IMAP  protocol.


# What is SPF (Sender Policy Framework)

The Sender Policky Framework is a mechanism that helps the server to distinguish the pishing email and the real email. 

When we send a email from abc@gmail.com to efg@hotmail.com, the SMTP will not check is the sender's message match to the IP address and the Domain. After the efg@hotmail.com's server receive the email, it will check the SPF database for the Domain name first. In this case the Domain name is gmail.com.If it has the Domain name and the IP address is match, then this is a real email. If the IP address is not under the Domain name then this should be a pishing email.

Now you may think what about if the SPF's database do not have that Domain, then that is depans on the Serve. Such as the outlook will receive it as a real email, but the gmail will mark that as pishing/trash and throw it into bin.

# How to avoid SPF

To avoid SPF, we need to use aother tool which is smtp2go. 

This is a s fast and scalable email service provider, for sending transactional and marketing emails and viewing reports on email delivery.

We use smtp2go as a big box, our email is inside of it. Then the SPF will check the outter case, and think its match and let us pass.

For example,


Outter Case:
* Domain:smtp2go.ca 
* IP:1.1.1.1.1     

Inner Case: 
* Domain:xxbank@xx.ca 
* IP:1.2.3.4.5 



# Commands in Swaks

| Command                | Meaning       |
| ---------------------  |:-------------:|
| `--ehlo [helo-string]` | Fake the ehlo Header message           | 
| `--header [header-and-data]` |   Fake the From, Subject, Message-Id, X-Mailer and etc. Header message    |   
| `--data [data-portion]` |  Fake the DATA part’s all content, could use .eml file.    |
| `--attach [attachment-specification]` |   Add the attach files.
  |


# Using `Swaks` examples
 In order to accross the `SPF` interception, we need to let the `from` part as same as the `server` part.

 For Example:
 ```
 swaks --to <email address> --from `info@smtp2go.com` --data emt.eml --server `mail.smtp2go.com` -p 2525 -au <username> -ap <password> --header "Subject:INTERAC e-Transfer: Alex sent you money." --h-From: 'Alex<notify@payments.interac.ca>'
```
The `--from` part address should as same as the `--server` part. Also, the `smtp2go.com` is a trusted SMTP server, and it like a jump board we need. At last, the `--h-From` parameter, if we did not set it, then, our email’s from address will be the `from` part, but if we set it, this part will replace the original `from` part. In other words, we can set this part to whatever we want. 



# Send a spam

now, for example, we want to pretend that we are scotiabank, and send an offer to a customer, normally, for safe, the client may check the domain to make sure this email is exactly from scotiabank, and then, they may check the content of this email, if an email made very roughly, they may think it is a spam and ignore it. So we go to our inbox, and download a real scotiabank’s email and modified it.

First, we download a original .eml file in gmail application. And modify some important information. 
![image](https://github.com/ivanleey1111/ece9069presentation1/blob/main/imgs/file.png)

And using:
```
 swaks --to <email address> --from `info@smtp2go.com` --data emt.eml --server `mail.smtp2go.com` -p 2525 -au <username> -ap <password> --header "Subject:Receive your offer." --h-From: 'ScotiaBank<offers@scotiabank.ca>'
```

to send to our email address.

# Try to make a fake e-transfer email

Because we think making a ads email may not attract victim, so that we try to make a fake e-transfer email.

First, download a real e-transfer email, and then modify some content, like we should change the sender name, the amount and also the most important thing is change the bank logo's link, we need to change this link to point to our fake login interface.

Also using the same command like we mentioned before. 

We using springmvc to build a fake scotiabank log in website, if victim click the bank logo, and it will jump to the fake website, after input the username and password, the script will automatic send email which contains victim's username and password. 








