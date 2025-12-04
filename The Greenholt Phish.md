# The Greenholt Phish - Writeup

## What is the Transfer Reference Number listed in the email's Subject?

The number is literally shown in the subject line of the email.

A: `09674321`

## Who is the email from?

Looking at the "From" address we can see that the address is clearly shown.

A: `info@mutawamarine.com`

## What email address will receive a reply to this email? 

The "Reply to" address enables the recipient to send an email back to the sender and the message will be sent to this specific address rather than the "From" address.

A: `info.mutawamarine@mail.com`

## What is the Originating IP?

By opening up the email, selecting the more dropdown list and viewing the email source we can inspect a very detailed email information about its contents and where the email had originated from. 

We can search for the keyword `X-Originating-Ip` in the header to make it a little easier.

```
X-Originating-IP: [x.x.x.x]
Received: from 10.197.41.148  (EHLO sub.redacted.com) (x.x.x.x)
  by mta4212.mail.bf1.yahoo.com with SMTP; Wed, 10 Jun 2020 05:58:54 +0000
Received: from hwsrv-737338.hostwindsdns.com ([192.119.71.157]:51810 helo=mutawamarine.com)
	by sub.redacted.com with esmtp (Exim 4.80)
	(envelope-from <info@mutawamarine.com>)
	id 1jissD-0004g5-Ts
	for webmaster@redacted.org; Wed, 10 Jun 2020 01:02:04 -0400

```
When we analyse headers, we always look from the bottom-most Received field upwards. This will show the path from the originating server to the final email provider, in this case its yahoo.

A: `192.119.71.157`


## Who is the owner of the Originating IP? (Do not include the "." in your answer.)

Running the command `whois 192.119.71.157`, we can inspect the WHOIS information about the IP., more specifically the owner of the IP range.

```
NetRange:       192.119.64.0 - 192.119.127.255
CIDR:           192.119.64.0/18
NetName:        HOSTWINDS-18-2
NetHandle:      NET-192-119-64-0-1
Parent:         NET192 (NET-192-0-0-0-0)
NetType:        Direct Allocation
OriginAS:       
Organization:   Hostwinds LLC. (HL-29)
RegDate:        2012-11-12
Updated:        2021-09-23
Comment:        https://www.hostwinds.com
Comment:        Abuse Contact: abuse@hostwinds.com
Ref:            https://rdap.arin.net/registry/ip/192.119.64.0
```

The organisation `Hostwinds LLC. (HL-29)` owns the IP range `192.119.64.0 - 192.119.127.255` which was where the email had been sent from.

A: `Hostwinds LLC`

## What is the SPF record for the Return-Path domain?

A SPF record, or Sender Policy Framework is a DNS record that prevents the spoofing of emails by specifying which email servers are allowed to send emails on behalf of a particular domain. This verifies that the incoming email is being sent from a trusted source.

We enter `dig mutawamarine.com txt` into a terminal window. Below is the output.

```
; <<>> DiG 9.18.41-1~deb12u1-Debian <<>> mutawamarine.com txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34180
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;mutawamarine.com.		IN	TXT

;; ANSWER SECTION:
mutawamarine.com.	3600	IN	TXT	"v=spf1 include:spf.protection.outlook.com -all"
mutawamarine.com.	3600	IN	TXT	"MS=ms97822417"
mutawamarine.com.	3600	IN	TXT	"MS=842BCB91F2AB2807BE05D25DC690D1226B349676"

;; Query time: 30 msec
;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)
;; WHEN: Thu Dec 04 00:29:45 GMT 2025
;; MSG SIZE  rcvd: 186

```
We can see that there is a TXT record that contains the SPF record for this domain in the answer section. Below the SPF record is quite possibly the Microsoft verification record to be able to send emails from this domain.

A: `v=spf1 include:spf.protection.outlook.com -all`

## What is the DMARC record for the Return-Path domain?

By entering this command `nslookup -type=txt _dmarc.mutawamarine.com` into a terminal window, we can inspect the DMARC record for this domain.

DMARC stands for Domain-based Message Authentication, Reporting & Conformance. It is used to confirm that the email is sent from a domain that it claims to be from. Using this in conjunction with SPF helps to confirm the email legitimacy and protects legitmate domains from spoofing and phishing.

```
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
_dmarc.mutawamarine.com	text = "v=DMARC1; p=quarantine; fo=1"

Authoritative answers can be found from:

```

We can see that there is a DNS TXT record for DMARC in the reply.

A: `v=DMARC1; p=quarantine; fo=1`

## What is the name of the attachment?

This is clearly stated at the bottom of the email.

A: `SWT_#09674321____PDF__.CAB`

## What is the SHA256 hash of the file attachment?

By downloading the attachment and then running the command `sha256sum <filename>` we can calculate the sha256 hash of the file.

A: `2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f`


## What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)


## What is the actual file extension of the attachment?




