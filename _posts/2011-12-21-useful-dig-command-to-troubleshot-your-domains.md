---
layout: post
title: 10 frequently asked questions about DNS solved with DIG
author: Anouar
date: 21/12/2011 07:15
redirect_to: https://cto.lu/articles/dns-queries-with-dig

illustration:
  src:   /assets/images/posts_illustration/2011-12-21-useful-dig-command-to-troubleshot-your-domains/dig.jpg
  title: "dig +trace"


tags: 
  - dns
  - dig
  - domains
  - faq
---

Everyday I have to deal with DNS queries, and I've noticed my colleagues and 
friends often ask me about the way to troubleshot their domain names' zone 
(that's what friends are useful for, no ?). I'll try to document my process as
clearly as possible in order to face that kind of problems.

## Domain Information Groper (DIG)

For that job, DIG is my favorite tool for interrogating DNS name servers. 
Here is the first description's paragraph in the manpage giving a
quite good overview of its aim. 

> dig (domain information groper) is a flexible tool for interrogating dns name
> servers. It performs DNS lookups and displays the answers that are returned
> from the name server(s) that were queried. Most DNS administrators use dig to
> troubleshoot DNS problems because of its flexibility, ease of use and clarity
> of output. Other lookup tools tend to have less functionality than dig.

I'm using quite often the `+short` option in my code sample to escape all the unecessary
output not really relevant to the subject. For mor information about
DIG, please refer to the man page, as usual on unix.

So let's start with our FAQ!

## 1. What is the website's IP address ?

When you're using a domain name, it is always converted to an IP address 
understandable by machines. If you want to know which IP addresses are 
associated to a domain name, you can query a resolver with `dig` 
to get this answer. By default, if the resolver gets it in cache, you will 
retrieve the cached answer.


Tips:

  - The resource records are cached for a duration of the TTL
  - If you want the value declared or cached on a nameserver add the option `@nsX.example.com.`
  - One or more records could be returned a once, and the programs use only one of them randomly (Round Robin)

**Command:**

    #!bash
    dig +short amazon.com

Output

    72.21.211.176
    72.21.214.128
    72.21.194.1


![H](/assets/images/posts_illustration/2011-12-21-useful-dig-command-to-troubleshot-your-domains/dig_a_domain_cache.png)



## 2. How to identify the name servers associated with a domain ?


When you want to answer questions like:

  - Where is hosted the zone ?
  - Is the Name Server Update already effective ?
  - Is the domain on 2, 3, 4 or more name servers ?

that means your are looking for information about the domain authoritative
name server.

There are 2 existing ways to get this authoritative nameserver: 
you can use either run `whois` or a `dig` command. 
Here-after I'm focusing only on the `dig`'s approach

Tips:

  - The NS records, like the other records, are subject to caching
  - The TTL of the NS records are often very long (1 or more days)
  - When you migrate from a provider to another, keep your old zone at
    least for the duration of the TTL

**Command:**

    #!bash
    dig NS +short anouar.im

Output

    n1.eurodns.com.
    n2.eurodns.com.
    n3.eurodns.com.
    n4.eurodns.com.



![H](/assets/images/posts_illustration/2011-12-21-useful-dig-command-to-troubleshot-your-domains/dig_ns_domain.png)


## 3. What does the delegation path to my zone look like ?


If you've understood the hierarchical distributed nature of the DNS, and
want to know what could be a path used on a standard query: use the
`+trace` option. It gives you all steps of the resolution by
bypassing the cached values.

Tips:

  - With `+trace`, DIG queries directly the authoritative name servers and is
    not affected by the cache of any resolver.
  - When you are troubleshouting use +trace to see if the delegation is
    operational


**Command:**

    #!bash
    dig google.com +trace

**Output:**

    ; <<>> DiG 9.7.3-P3 <<>> google.com +trace
    ;; global options: +cmd
    .     4204  IN  NS  h.root-servers.net.
    .     4204  IN  NS  i.root-servers.net.
    .     4204  IN  NS  j.root-servers.net.
    .     4204  IN  NS  k.root-servers.net.
    .     4204  IN  NS  l.root-servers.net.
    .     4204  IN  NS  m.root-servers.net.
    .     4204  IN  NS  a.root-servers.net.
    .     4204  IN  NS  b.root-servers.net.
    .     4204  IN  NS  c.root-servers.net.
    .     4204  IN  NS  d.root-servers.net.
    .     4204  IN  NS  e.root-servers.net.
    .     4204  IN  NS  f.root-servers.net.
    .     4204  IN  NS  g.root-servers.net.
    ;; Received 512 bytes from 192.168.245.11#53(192.168.245.11) in 72 ms

    com.      172800  IN  NS  g.gtld-servers.net.
    com.      172800  IN  NS  f.gtld-servers.net.
    com.      172800  IN  NS  j.gtld-servers.net.
    com.      172800  IN  NS  m.gtld-servers.net.
    com.      172800  IN  NS  h.gtld-servers.net.
    com.      172800  IN  NS  b.gtld-servers.net.
    com.      172800  IN  NS  k.gtld-servers.net.
    com.      172800  IN  NS  d.gtld-servers.net.
    com.      172800  IN  NS  i.gtld-servers.net.
    com.      172800  IN  NS  a.gtld-servers.net.
    com.      172800  IN  NS  c.gtld-servers.net.
    com.      172800  IN  NS  l.gtld-servers.net.
    com.      172800  IN  NS  e.gtld-servers.net.
    ;; Received 500 bytes from 192.33.4.12#53(c.root-servers.net) in 18 ms

    google.com.   172800  IN  NS  ns2.google.com.
    google.com.   172800  IN  NS  ns1.google.com.
    google.com.   172800  IN  NS  ns3.google.com.
    google.com.   172800  IN  NS  ns4.google.com.
    ;; Received 164 bytes from 192.5.6.30#53(a.gtld-servers.net) in 122 ms

    google.com.   300 IN  A 74.125.79.147
    google.com.   300 IN  A 74.125.79.103
    google.com.   300 IN  A 74.125.79.105
    google.com.   300 IN  A 74.125.79.99
    google.com.   300 IN  A 74.125.79.104
    google.com.   300 IN  A 74.125.79.106
    ;; Received 124 bytes from 216.239.34.10#53(ns2.google.com) in 20 ms



![DNS resolution workflow](/assets/images/posts_illustration/2011-12-21-useful-dig-command-to-troubleshot-your-domains/how_dns_works.png)

## 4. Which Mail Server is responsible for a domain ?

When you send an email to a recipient, your email client request the IP
of the mail server responsible for the domain. In a DNS zone the
responsible server is filled in a MX Record, which points to a
hostname.

Tips:

  - Add backup mail server, by giving them a lower priority
  - The RFC2181, Section 10.3, says the the value of an MX records should not be a CNAME
  - When you query on a domain, you will not get the results of the subdomains!

**Command:**

    #!bash
    dig MX adlani.com

**Output:**

    ; <<>> DiG 9.7.3-P3 <<>> MX adlani.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60156
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 0

    ;; QUESTION SECTION:
    ;adlani.com.      IN  MX

    ;; ANSWER SECTION:
    adlani.com.    3527  IN  MX  20 alt2.aspmx.l.google.com.
    adlani.com.    3527  IN  MX  20 aspmx2.googlemail.com.
    adlani.com.    3527  IN  MX  20 aspmx3.googlemail.com.
    adlani.com.    3527  IN  MX  20 aspmx4.googlemail.com.
    adlani.com.    3527  IN  MX  30 aspmx5.googlemail.com.
    adlani.com.    3527  IN  MX  10 aspmx.l.google.com.

    ;; Query time: 6 msec
    ;; SERVER: 10.0.1.1#53(10.0.1.1)
    ;; WHEN: Thu Dec 15 23:18:34 2011
    ;; MSG SIZE  rcvd: 183


## 5. Which domain name is this IP associated with ?


Thanks to the PTR records in the .arpa zones you are able, when filled in, to
retrieve the domain name associated with an IP address. 

A reverse DNS query converts the IP addresses into meaningful host names.

Tips:

  - This is useful for traffic analysis since the ISP is often declared in it
  - It's used in traceroute to display meaningful names

**Command:**

    #!bash
    dig +short -x 8.8.8.8

Output

    google-public-dns-a.google.com.


## 6. Which are the name servers of a TLD ?

Want to know which value answers the registry namservers to? Or just simply need
to know what are the authoritative name servers handling the Top Level Domain?

Since the top level domain is also considered as an usual domain, you can 
query it as you would for your domain name.

**Command:**

    #!bash
    dig +short NS nl.

**Output:**

    nl1.dnsnode.net.ns1.nic.nl.
    ns2.nic.nl.
    ns3.nic.nl.
    ns4.nic.nl.
    ns-nl.nic.fr.
    sns-pb.isc.org.


![H](/assets/images/posts_illustration/2011-12-21-useful-dig-command-to-troubleshot-your-domains/dig_ns_tld.png)

## 7. Which value is in cache in a given resolver ?


You want to query a particular authoritative name server or resolver, you just
need to specify it by using its IP addresses or hotname prefixed with `@`.

**Command:**

    #!bash
    dig google.com @8.8.8.8

**Output:**

    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26500
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 0

    ;; QUESTION SECTION:
    ;google.com.      IN  A

    ;; ANSWER SECTION:
    google.com.   300 IN  A 173.194.65.105
    google.com.   300 IN  A 173.194.65.104
    google.com.   300 IN  A 173.194.65.99
    google.com.   300 IN  A 173.194.65.106
    google.com.   300 IN  A 173.194.65.147
    google.com.   300 IN  A 173.194.65.103

## 8. When will the cache of an answer expire ?

During the value of the TTL, the resolvers are able to cache values. You can 
also check the TTL expiration on the resolvers if the result for a query
(associated with a changed host) is not the one expected:  

The TTL value is available on all the Resource Records when querying with 
`dig` with default output. This value is the second column of each Resource 
Records line, and is expressed in seconds.

**Command:**

    #!bash
    dig google.com +noall +answer

**Output:** (in our case 105 seconds)

    ;; global options: +cmd
    google.com.   105 IN  A 74.125.79.99
    google.com.   105 IN  A 74.125.79.103
    google.com.   105 IN  A 74.125.79.104
    google.com.   105 IN  A 74.125.79.105
    google.com.   105 IN  A 74.125.79.106
    google.com.   105 IN  A 74.125.79.147

## 9. Is the zone synchronized to all my NS ?

To check if a zone is synchronized over all your authoritative name
servers, we will rely on the SERIAL number. The SERIAL being an integer, it 
will be incremented each time an update is executed on a zone.
The recommended syntax is YYYYMMDDnn.

For that, I'm using the `+nssearch` option to read the SOA of all the
authoritative name servers.

**Command:**

    #!bash
    dig google.com +nssearch

**Output:**

    SOA ns1.google.com. dns-admin.google.com. 2011120502 7200 1800 1209600 300 from server ns3.google.com in 17 ms.
    SOA ns1.google.com. dns-admin.google.com. 2011120502 7200 1800 1209600 300 from server ns2.google.com in 20 ms.
    SOA ns1.google.com. dns-admin.google.com. 2011120502 7200 1800 1209600 300 from server ns4.google.com in 98 ms.
    SOA ns1.google.com. dns-admin.google.com. 2011120502 7200 1800 1209600 300 from server ns1.google.com in 115 ms.

If you are just interested in the serial number comparison, you can filter 
unwanted filed with `cut`, sort the result and rmove duplicate line. A
synchronized zone should have only 1 line outputed.

**Command:**

    #!bash
    dig google.com +nssearch | cut -d ' ' -f 4 | sort | uniq  -c

**Output:**

    4 2011121501

In this example, I have used the `-c` option of `uniq` to count the
number of name server found.

## 10. Is a zone existing on this name server ?

To verify if an existing zone is available on a name server, use the results
header of the DNS queries. To know if a domain is existing on a name server, 
all you have to do is to send a valid DNS record and to read the headers.

The fact that you receive or not a response from the name server does not mean
the zone is existing or not. Here the header matters much more than the response 
itself. In this case, we will rely on the `status` header.

<table class="table">
  <tr>
    <th class="green">NOERROR</th>
    <td>status means the zone exists</td>
  </tr>
  <tr>
    <th class="orange">NXDOMAIN</th>
    <td>means that the domain do not exists</td>
  </tr>
  <tr>
    <th class="red">REFUSED</th>
    <td>
      The name server refuses to perform the specified operation for policy reasons
    </td>
  </tr>
</table>



**For existing domain**

**Command:**

    #!bash
    dig SOA google.nl @ns1.nic.nl.

**Output:**

    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46214

---
**For non existing domain**

**Command:**

    #!bash
    dig SOA googleqwertyuiop.nl @ns1.nic.nl.

**Output**

    ;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 49145
---
**Not manged domain**


    #!bash
    dig SOA google.com @ns1.nic.nl.

    ;; ->>HEADER<<- opcode: QUERY, status: REFUSED, id: 38110

## References

 
 - **DNS Glossary**: [Men & Mice DNS Glossary](http://www.menandmice.com/knowledgehub/dnsglossary/)
 - **RFC 1034**: [DOMAIN NAMES - CONCEPTS and FACILITIES](http://tools.ietf.org/html/rfc1034)
 - **RFC 1035**: [DOMAIN NAMES - IMPLEMENTATION and SPECIFICATION](http://tools.ietf.org/html/rfc1035)

### Tools

- **Zone Validator**: [ZoneCheck](http://www.zonecheck.fr/)
- **DIG on web**: [Dig Web Interface](http://digwebinterface.com/)

### Related Articles

- [What's a domain name and what's behind the scene](/2011/12/what-is-a-domain-name-and-what-is-behind-the-scene.html)

I hope you enjoyed this post. If you'd like to discuss feel please feel free to a comment below or to [drop me an email](mailto:anouar+blog@adlani.com).
