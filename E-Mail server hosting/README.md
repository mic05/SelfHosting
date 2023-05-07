# Email server hosting

- [Email server hosting](#email-server-hosting)
  - [1. Introduction](#1-introduction)
  - [2. Requirements](#2-requirements)
  - [3. Get a domain](#3-get-a-domain)
  - [4. Set up mail server](#4-set-up-mail-server)
  - [5. Set up spam filter](#5-set-up-spam-filter)
  - [6. Set PTR record for your domain](#6-set-ptr-record-for-your-domain)
  - [7. Set A / AAAA records](#7-set-a--aaaa-records)
    - [Example](#example)
  - [8. Set SPF record](#8-set-spf-record)
    - [How is the SPF record built?](#how-is-the-spf-record-built)
    - [What mechanisms and modifiers can be used in a SPF record?](#what-mechanisms-and-modifiers-can-be-used-in-a-spf-record)
    - [Example](#example-1)
    - [What do I have to set as an SPF record?](#what-do-i-have-to-set-as-an-spf-record)
    - [SPF HELO/EHLO](#spf-heloehlo)
  - [9. Set MX records](#9-set-mx-records)
    - [What are MX records?](#what-are-mx-records)
    - [What do I have to set?](#what-do-i-have-to-set)

## 1. Introduction

Email server hosting is a very controversial topic. Opinions of feasibility differ greatly here, as there are a few things to consider if you want to run an email server yourself.  
Hosting an email server requires quite a bit of knowledge and preparation, and most importantly, the right way of where to run an email server.  
In the following pages I will try to go into more detail about hosting mail servers and provide the necessary knowledge.

But first of all let us look at the advantages and disadvantages of hosting a mail server:

**Advantages:**

- Privacy.  
    You own your data and nobody is reading your received mails in your inbox and nobody has access to it. There aren't any automated analysis of the contents in your mails.
- Control of service.  
    You control your service and are not depended on some mail provider and its decisions regarding spam filtering or implementation of security by sending mails.
- Trust of the recipient.  
    Everybody can create a mail account on the services of gmail.com, outlook.com, t-online.de, gmx.net, etc. If you have your own domain, no one can create a mailbox and send from it. Therefore, there could be more trust of the recipient because it's your domain and only your domain.

**Disadvantages:**

- Complex setup and maintenance.  
    To get started hosting mail can be a pain in the ass because there are some important and complex aspects that need to be considered for setting up the mail server. Also you have to check regularly that your domain isn't listed on a blacklist.
- No/difficult hosting at home.  
    Hosting a mail server at home could be a show stopper. If your ISP (**I**nternet **S**ervice **P**rovider) does block port 25 or if you have a dynamic IP address or if you can't set a PTR record for your IP address, you can't host your mail server at home and have to rely on some cloud provider for hosting your mail server.
- Hosting on public clouds?  
    Not every public cloud lets you host your own mail server. Azure for example blocks port 25 by default and therefore is not designed for hosting mail servers.

**To be considered:**

- Security.  
    If you host anything there should always be the question of security. If you rely on some of the big companies for your mail communication (e.g. gmail.com, outlook.com, t-online.de, gmx.net, etc.), these companies are managing the security for connecting to the mail server to access your mails or for the communication between their mail servers and the recipients mail servers. This can either be seen as advantage or disadvantage, choose for yourself.

## 2. Requirements

To host your own mail server, there are some requirements witch are must have and some witch should be implemented but don't have to be.

**What you need:**

- Domain provider.  
    To order a domain and for setting domain entries a domain provider is needed. Here are some examples of domain providers: [ovh.com](http://ovh.com), [hetzner.de](http://hetzner.de), [strato.de](http://strato.de), [cloudflare.com](http://cloudflare.com), [ionos.de](http://ionos.de)
- Domain.  
    You need some domain for accessing your mail server and of course as your mail address.
- IP address.  
    In order to run a mail server, a fixed IP address is a must have. It's strongly recommended that you can set a valid [PTR DNS record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record), otherwise your mails are likely to be rejected from other mail servers.  
    *Nice to have:* Both IPv4 and IPv6. IPv6 is coming more and more and will be the future in network communication. As of today a mail server can't be IPv6 only and needs to have an IPv4 address because many mail servers don't have an IPv6 address. But if you implement IPv6 today you have nothing to worry about in the future.
- Server.  
    Some kind of server is needed to install your mail server on. There are many mail servers our there, some of witch are: [iRedMail](https://www.iredmail.org/), [Mailcow](https://mailcow.email/), [Mail-In-A-Box](https://mailinabox.email/), [Docker Mailserver](https://github.com/docker-mailserver/docker-mailserver), [Microsoft Exchange Server](https://docs.microsoft.com/de-de/exchange/exchange-server?view=exchserver-2019), etc.

If you have everything of the list above, you are very good to go.

## 3. Get a domain

In order to get into mail server hosting you have to get a domain. This is the part of the mail address after the '@' (e.g. info@**domain.com**).

A domain can be purchased from many providers. For example, you can look at the following domain providers:

- [ovh.com](http://ovh.com)
- [hetzner.de](http://hetzner.de)
- [strato.de](http://strato.de)
- [cloudflare.com](http://cloudflare.com)
- [ionos.de](http://ionos.de)

## 4. Set up mail server

This is supposed to be a general guide to mail server hosting, so we will not go into more detail about setting up a mail server here.

There are plenty of guides out there, check out some of the mail servers in the [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted#communication---email---complete-solutions) list on github.

<span style="text-decoration: underline;">**Please consider the following for the IP address from which your server sends mails:**</span>

- Only host your mail server at your company or your home if you can set a [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record) for the public IP addresses your mail server is sending mails from.
- You can't host your mail server if you don't have a static IP address.
- You can't host your mail server if you don't have an IPv4 address.
- It's best to have IPv4 and IPv6 address but this is not a must have.
- If you host your mail server at some cloud provider, please be sure that you can set a [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record).
- Regardless of where the mail server is hosted, make sure that port 25 TCP isn't blocked.  
    This is the case for example in Azure: You have to contact support for unblocking this port.  
    As a hoster for mail servers you can use for example [Hetzner](http://hetzner.de) from Germany.
- Regardless of where the mail server is hosted, make sure that your public IP address isn't on some blacklist. There are a lot of blacklist checkers out there, you can use [MXToolbox](https://mxtoolbox.com/blacklists.aspx) for example for this.

## 5. Set up spam filter

This is supposed to be a general guide to mail server hosting, so we will not go into more detail about setting up a spam filter here.

**Please note:** Having a dedicated spam filter is optional and not a must have. I strongly recommend to have a dedicated spam filter that is acting as a smarthost. I mean it this way: Your mail server isn't sending or receiving mails directly and doesn't have any incoming ports open from the internet side. For this the spam filter or smarthost is responsible. Sending mails are sent from the mail server to the spam filter and then sent to the target mail server and receiving mails are sent from the senders mail server to your spam filter and then forwarded to your mail server.

<div drawio-diagram="12"><img src="https://github.com/mic05/SelfHosting/raw/main/resources/images/mail_server_with_spamfilter.png" alt=""/></div>

There are plenty of spam filter solutions out there. You can check out [Proxmox Mail Gateway](https://www.proxmox.com/de/proxmox-mail-gateway) for example.

If your spam filter is functioning as a smart host and your mail server is not sending and receiving your mails directly, please note the [recommendations under the mail server section](#4-set-up-mail-server).

## 6. Set PTR record for your domain

The [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record) is not a recommendation but a must have for your mail server. Many mail providers reject your mail if you have none or a false [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record). This is defined in [RFC 1912 2.1](https://datatracker.ietf.org/doc/html/rfc1912#section-2.1).

You have to check the documentation of your cloud hosting provider or contact your ISP (Internet Service Provider) for instructions on how to set the [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record).

Set your [PTR record](https://github.com/mic05/SelfHosting/wiki/DNS-records#ptr-resource-record) to the domain of either your mail server or your spam filter (Set it to the domain of the server which is sending your mails).

## 7. Set A / AAAA records

An [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) or [AAAA record](https://github.com/mic05/SelfHosting/wiki/DNS-records#aaaa-resource-record) is a must have, not an optional thing.
If your mail server domain is `mail1.domain.com` for example, you have to set an [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) and/or an [AAAA record](https://github.com/mic05/SelfHosting/wiki/DNS-records#aaaa-resource-record) to the ipv4 and/or ipv6 address of your mail server. If you have a spam filter or smarthost which is sending the mails, you have to set the record(s) for this one.

:warning: **Please note**: The [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) or [AAAA record](https://github.com/mic05/SelfHosting/wiki/DNS-records#aaaa-resource-record) of the server sending the mail out must match the PTR record. If this is not the case, some mail servers might reject your mails.

### Example
If the IP of your mail server or spam filter is `90.270.83.56` and your domain is `mail1.domain.com`, you have to set the following in your DNS:
```
mail1.domain.com.           IN A      90.270.83.56
56.83.270.90.in-addr.arpa   IN PTR    mail1.domain.com
```


## 8. Set SPF record

SPF stands for Sender Policy Framework and is defined in [RFC 7208](https://datatracker.ietf.org/doc/html/rfc7208). The SPF record defines which mail servers are allowed to send mails for your domain.

A mail server receiving mail is checking the SPF record of the domain of the mail it is receiving. If the mail server which is sending the mail is permitted to send mail for the domain according to the SPF record, the receiving mail server is likely to trust the sending mail server and accepts the mail.

### How is the SPF record built?

The SPF record consists of two parts and is a [TXT record](https://github.com/mic05/SelfHosting/wiki/DNS-records#txt-resource-record). Lets see some example:

```
v=spf1 mx -all
```

The first part is the `v=` which represents the version of the SPF record. Currently, there is only version spf1.  
The second part is the `mx -all` which tells what servers are allowed and are not allowed to send mails for the domain.

The SPF record is always processed from left to right. The first match ends the reading of the SPF entry.

### What mechanisms and modifiers can be used in a SPF record?

The SPF records always starts with `v=spf1`. The mechanisms follow afterwards which tell what servers are allowed and are not allowed to send mails for the domain.

There are different qualifiers for the mechanisms:

<table border="1" id="bkmrk-qualifier-meaning-ex" style="border-collapse: collapse; width: 100.864%; height: 147.984px;"><thead><tr style="height: 29.7969px;"><td style="width: 9.51792%; height: 29.7969px;">

**Qualifier**

</td><td style="width: 9.76514%; height: 29.7969px;">

**Meaning**

</td><td style="width: 80.7169%; height: 29.7969px;">

**Explanation**

</td></tr></thead><tbody><tr style="height: 29.7969px;"><td class="align-center" style="width: 9.51792%; height: 29.7969px;">+</td><td style="width: 9.76514%; height: 29.7969px;">pass</td><td style="width: 80.7169%; height: 29.7969px;">

Mechanism is allowed to send mail. This is default an can be omitted.

</td></tr><tr style="height: 29.7969px;"><td class="align-center" style="width: 9.51792%; height: 29.7969px;">-</td><td style="width: 9.76514%; height: 29.7969px;">fail</td><td style="width: 80.7169%; height: 29.7969px;">

Mechanism is not allowed to send mail. Most spam filters reject the mail.

</td></tr><tr style="height: 29.7969px;"><td class="align-center" style="width: 9.51792%; height: 29.7969px;">~</td><td style="width: 9.76514%; height: 29.7969px;">softfail</td><td style="width: 80.7169%; height: 29.7969px;">

Mechanism is probably not allowed to send mail. Most spam filters put the email in spam folder.

</td></tr><tr style="height: 28.7969px;"><td class="align-center" style="width: 9.51792%; height: 28.7969px;">?</td><td style="width: 9.76514%; height: 28.7969px;">neutral</td><td style="width: 80.7169%; height: 28.7969px;">

Mechanism is not explicitly allowed to send mail.

</td></tr></tbody></table>

The following mechanisms and modifiers are possible:

<table border="1" id="bkmrk-mechanism-%2Fmodifier-" style="border-collapse: collapse; width: 100%; height: 928.016px;"><thead><tr style="height: 46.5938px;"><td style="width: 12.4841%; height: 46.5938px;">

**Mechanism /  
Modifier**

</td><td style="width: 87.5159%; height: 46.5938px;">

**Explanation**

</td></tr></thead><tbody><tr style="height: 63.3906px;"><td class="align-center" style="width: 12.4841%; vertical-align: middle; height: 63.3906px;">all</td><td style="width: 87.5159%; height: 63.3906px;">

This matches always regardless of other entries in the SPF record. This should only be used as the rightmost mechanism. Mechanisms after this will never be tested. If this mechanism is present, any "redirect" modifier is ignored regardless of the ordering of the mechanisms.

</td></tr><tr style="height: 96.9844px;"><td class="align-center" style="width: 12.4841%; vertical-align: middle; height: 96.9844px;">include</td><td style="width: 87.5159%; height: 96.9844px;">

This includes the SPF record of another domain and is always specified with a domain afterwards.   
Example: `v=spf1 include:domain.com`  
That means that the SPF record of domain.com is now included into your SPF record. The record is not included literally in your SPF record but only the evaluated result is included. That means that for example a mechanism `-all` in the referenced SPF record is not included in your SPF record.

</td></tr><tr style="height: 141.766px;"><td class="align-center" style="width: 12.4841%; vertical-align: middle; height: 141.766px;">a</td><td style="width: 87.5159%; height: 141.766px;">

This matches the IP address of a specified domain into your SPF record.  
Example: `v=spf1 a:domain.com`  
That means that the [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) or [AAAA record](https://github.com/mic05/SelfHosting/wiki/DNS-records#aaaa-resource-record) of the specified domain `domain.com` is taken for the mechanism.  
This record can also be specified without a domain.  
Example: `v=spf1 a`  
That means that the [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) of the domain which has this SPF record is taken for the mechanism.

</td></tr><tr style="height: 124.969px;"><td class="align-center" style="width: 12.4841%; height: 124.969px; vertical-align: middle;">mx</td><td style="width: 87.5159%; height: 124.969px;">

This matches the IP address of the [MX record](https://github.com/mic05/SelfHosting/wiki/DNS-records#mx-resource-record) of a specified domain into your SPF record.  
Example: `v=spf1 mx:domain.com`  
That means that the [MX record](https://github.com/mic05/SelfHosting/wiki/DNS-records#mx-resource-record) of the specified domain `domain.com` is taken for the mechanism.  
This record can also be specified without a domain.  
Example: `v=spf1 mx`  
That means that the MX record of the domain which has this SPF record is taken for the mechanism.

</td></tr><tr style="height: 35.3906px;"><td class="align-center" style="width: 12.4841%; height: 35.3906px; vertical-align: middle;">ptr</td><td style="width: 87.5159%; height: 35.3906px;">

Do not use this mechanism. Read more of this in [RFC 7208 5.5](https://datatracker.ietf.org/doc/html/rfc7208#section-5.5).

</td></tr><tr style="height: 63.3906px;"><td class="align-center" style="width: 12.4841%; height: 63.3906px; vertical-align: middle;">ip4</td><td style="width: 87.5159%; height: 63.3906px;">

This matches the specified IPv4 address or network into your SPF record.  
Example: `v=spf1 ip4:20.56.98.10/32`  
That means that the IP address 20.56.98.10 is taken for the mechanism.

</td></tr><tr style="height: 63.3906px;"><td class="align-center" style="width: 12.4841%; height: 63.3906px; vertical-align: middle;">ip6</td><td style="width: 87.5159%; height: 63.3906px;">

This matches the specified IPv6 address or network into your SPF record.  
Example: `v=spf1 ip6:2001:db8:3c4d:15::/64`  
That means that the IP network 2001:db8:3c4d:15::/64 is taken for the mechanism.

</td></tr><tr style="height: 35.3906px;"><td class="align-center" style="width: 12.4841%; height: 35.3906px; vertical-align: middle;">exists</td><td style="width: 87.5159%; height: 35.3906px;">

This mechanism is only used for very complex scenarios. If interested, read [RFC 7208 5.7](https://datatracker.ietf.org/doc/html/rfc7208#section-5.7).

</td></tr><tr style="height: 113.781px;"><td class="align-center" style="width: 12.4841%; vertical-align: middle; height: 113.781px;">redirect</td><td style="width: 87.5159%; height: 113.781px;">

This modifier redirects the SPF record to the SPF record of a specified domain.  
Example: `v=spf1 redirect=_spf.domain.com`  
That means that the SPF record of the domain \_spf.domain.com is taken for the mechanism.  
Please note: This modifier has an "=" for separating the operator and the value.  
This modifier should be used as the rightmost modifier and mechanism.  
This modifier is ignored if an "all" mechanism is present in the record.

</td></tr><tr style="height: 142.969px;"><td class="align-center" style="width: 12.4841%; vertical-align: middle; height: 142.969px;">exp</td><td style="width: 87.5159%; height: 142.969px;">

This modifier is used for an explanation if a receiver rejects a message.  
Example: `v=spf1 mx -all exp=spffail.domain.com`  
That means that only the mx servers of the domain are allowed to send mails for the domain. If other servers send mails for the domain and a receiver rejects those messages because of the SPF record, the receiving mail server will lookup the [TXT record](https://github.com/mic05/SelfHosting/wiki/DNS-records#txt-resource-record) of the specified domain `spffail.domain.com` and will include this explanation string into the bounce message or mail log.  
An example for a explanation string could be: `Emails from domain.com should only be sent by allowed servers.`

</td></tr></tbody></table>

### Example

To understand the SPF records some better, here is an example.

```
domain.com.       IN A      90.270.83.57
domain.com.       IN MX     1 mail1.domain.com
domain.com.       IN TXT    v=spf1 mx a a:domain2.com ip4:56.13.78.9/32 include:domain2.com -all
mail1.domain.com. IN A      90.270.83.56

domain2.com       IN A      90.271.25.13
domain2.com       IN AAAA   2001:db8:3c4d:15::40
domain2.com       IN TXT    v=spf1 mx -all
domain2.com       IN MX     1 mail.domain2.com
mail.domain2.com  IN A      217.56.9.167
```

Above SPF record means that the [MX](https://github.com/mic05/SelfHosting/wiki/DNS-records#mx-resource-record) and [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) of `domain.com` and the [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) and [AAAA record](https://github.com/mic05/SelfHosting/wiki/DNS-records#aaaa-resource-record) of `domain2.com` as well as the stated IP after ip4: and the [MX record](https://github.com/mic05/SelfHosting/wiki/DNS-records#mx-resource-record) of `domain2.com` are allowed to send mails for domain.com and no other server is.  
That means that IP addresses `90.270.83.56`, `90.270.83.57`, `90.271.25.13`, `2001:db8:3c4d:15::40`, `56.13.78.9` and `217.56.9.167` are allowed to send mails for `domain.com`.

### What do I have to set as an SPF record?

You have to check which servers are sending out mails for your domain. If you only have one mail server which is handling all mails for your domain, you can simply and safely set `v=spf1 mx -all` as your SPF record.  
If for example your website is sending out mail directly and not over your mail server because some form in the website does this, you have to set `v=spf1 mx a -all`.

Simply follow above table and include all ip addresses which are sending mails with your domain.

### SPF HELO/EHLO

What is HELO? When mail servers communicate with each other and send or receive an email, the beginning of the communication looks something like this:

```
  Sender                               Receiver

telnet mail.domain2.com 25
                                     250 service ready
HELO mail1.domain.com
                                     250 OK
MAIL FROM:<info@domain2.com>                                     
                                     250 OK
```

You can see that the sending mail server is telling his name after HELO. This is usually the FQDN (Fully Qualified Domain Name) of the mail server.

Verifying HELO/EHLO names is recommended by the SPF RFC. So publishing records for these hostnames is an important part of the SPF protocol. To publish a HELO rule, a SPF record is usually created that is associated with the HELO FQDN used by your mail server (e.g. "mail1.domain.com").

So to say it shortly: You can set an [TXT record](https://github.com/mic05/SelfHosting/wiki/DNS-records#txt-resource-record) on each hostname of your servers which are sending mails. For example this should look something like this:

```
domain.com.       IN A      90.270.83.57
domain.com.       IN MX     1 mail1.domain.com
domain.com.       IN TXT    v=spf1 mx a a:domain2.com ip4:56.13.78.9/32 include:domain2.com -all
mail1.domain.com. IN A      90.270.83.56
mail1.domain.com  IN TXT    v=spf1 a -all
```

As you can see the FQDN `mail1.domain.com` now has an SPF record that says that the [A record](https://github.com/mic05/SelfHosting/wiki/DNS-records#a-resource-record) of `mail1.domain.com` is allowed to send. Therefore when the mail server `mail1.domain.com` is sending an email and is telling `HELO mail1.domain.com`, the receiving mail server can safely tell that this is a mailserver allowed to send mail when the IP address of the sending server matches the A record of `mail1.domain.com`.

## 9. Set MX records

### What are MX records?

An MX record defines under which FQDN (Fully Qualified Domain Name) the mailserver to this domain is reachable.

Normally there are several MX records with different priorities so that in case of failure the email can be delivered to another email server. Another advantage of multiple MX records is that one email server can be maintained while the other is still accessible.

Example:

```
domain.com.       IN MX     1 mail1.domain.com
domain.com.       IN MX     2 mail2.domain.com

mail1.domain.com. IN A      90.270.83.56
mail2.domain.com. IN A      90.270.83.57
```

If a mail server is delivering a mail to info@domain.com, the mail server looks up the mx record of domain.com with the lowest priority first. After that the mail server looks up the AAAA and A record of the domain in the mx record (in this example the A record of mail1.domain.com). If this server is not reachable, the next mx record is being queried until the mail could be delivered.

### What do I have to set?

So simply set your MX record to the hostname of your mail server, e.g. `mail1.domain.com`.