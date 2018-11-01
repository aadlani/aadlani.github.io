---
layout: post
title: "What's a domain name and what's behind the scene"
author: Anouar

date: 12/12/2011 22:10

illustration:
  src: /assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/crime_scene.jpg
  title: "How to backup with RSYNC, TAR, CRON and GPG"
  source: http://www.flickr.com/photos/11121568@N06/4121423119/in/photolist-7hcnvi-7rEnkx-8k11AE-8oKkbJ-9QVUvU-9QVTiU-9QVTY9-9QVUy9-9QVTJC-9QT3Zc-9QVW7J-9QT2tn-9QVUib-9QT2X4-9QVTqd-9QVVs9-9QT3UM-9QVVjs-9QT5av-9QT3tv-9QT3cz-9QVTGL-9QT524-9QVULy-9QVV3L-9QVTwG-9QT2yt-9QT2CH-9QVVoY-9QT4vn-9QT55D-9QT3pc-9QVW5w-9QT3E6-9QT39x-9QT4hT-g6eTAW-g6eZbw-g6eCen-g6eCtv-gvixzi-gvixTe-f4mare-93v4P8-hgifET-9QVUdb-9QT2oM-9QT2Mc-9QT3yr-9QT2EH-9QVTdy


tags: 
  - domains
  - nameserver
  - tld
  - dns
---

New to the domain name industry, or want to refresh your general 
knowledge about domain names ? I will try in this post to explain some basics 
of domain names to help you understand details involved. If you have been in
the industry since 10 years, then this article is definitely not for you

## What is a domain name ? (and what is not)

### Description

A Domain Name is the name used to identify resources belonging to the same 
authority on internet. It has been thought as an abstraction over IP addresses 
since it's more readable, memorizable and easier to distinguish.

Often people do not make the difference between an URL, a Fully Qualified Domain Name and a Domain Name.
I think this is the first step to understand the Domain Name System, so
let's define it.

### Do not confuse it with ...

A domain name is not an URL (Uniform Resource Protocol), and it's not 
(exactly) a FQDN but it is a part of both of them.

An URL, as defined by the RFC 1738, is used
to locate documents over the internet and is composed by the following
parts:

![URL Format](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/url_format.png)
    
The FQDN (Fully Qualified Domain Name) is a specific domain name that defines all 
its levels, and it's used to identify a resource

![fqdn Format](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/fqdn_format.png)

But domain name is definitely not:

  - a website
  - a web hosting
  - a mailbox

The domain names are obviously mostly present in all these services. But as 
described previously, they are used only to have an easily identifiable and 
memorizable name, and here are their unique roles.

### Basic Structure

Basically the domain name is composed in 2 parts, a name and an extension. 

<table class="table">
  <thead>
    <tr>
      <th>domain</th>
      <th>name</th>
      <th>extension</th>
      <th>tld</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>google.com</td>
      <td>google</td>
      <td>.com</td>
      <td>.com</td>
    </tr>
    <tr>
      <td>anouar.im</td>
      <td>anouar</td>
      <td>.im</td>
      <td>.im</td>
    </tr>
    <tr>
      <td>amazon.co.uk</td>
      <td>amazon</td>
      <td>.co.uk</td>
      <td>.uk</td>
    </tr>
  </tbody>
</table>

Notice: The term extension is not really accurate due to the hierarchical 
nature of the domain name system, (explained later on).

Now that we have a broad definition of a domain name, we will 
detail a little bit more deeply each of its components.

## The Domain Name Data Structure

### Overview

For me, the best way to describe the domain name is by defining its
logical structure as UML, to transmit the big picture.

![Domain Name Data Model](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/domain_model.png)


This data structure could be expressed in plain english this way:

- Domain has a name and belongs to a Top Level Domain
- Domain has 4 dates: the registration, the last update, the expiration and the release
- Domain has 4 contacts: the registrant, the administrative, technical and the billing
- Domain has many name servers (at least 2)
- Domain belongs to a sponsoring registrar

Again, these are general rules, you should bear in mind that each Registry
is responsible for its own rules. This structure is reflected in the
public whois results of generic domains, as you may figure out on the
whois of my `adlani.com` domain if you execute the whois command `whois
adlani.com` or if you go to an online whois service like
[WHOIS.DE](http://whois.de).

### Registrar

A *registrar* is a service provider accredited with one or more *registries*, 
that *registrants* use to buy and manage domain names.

That sounds non trivial, let's define some vocabulary to clarify it:

<table class="table">
  <tbody>
    <tr>
      <th>Registry</th>
      <td>Authority responsible in managing a Top Level Domain (ie. Verisign for .COM)</td>
    </tr>
    <tr>
      <th>Registrar</th>
      <td>Service Provider accredited with one or more registries</td>
    </tr>
    <tr>
      <th>Registrant</th>
      <td>Owner of registered domain names</td>
    </tr>
  </tbody>
</table>

Most of the registries have a Registry/Registrar, which means you 
could not register a .fr domain name directly with AFNIC, but you have to go 
through an accredited registrar.

When you have registered the domain with a sponsoring registrar, its name is usually published in the whois.

More details about this subject will be provided on section "The Registry, the
registrar and the registrant".

### Name Servers

The domain name should declare during the registration process several
(2+) authoritative name servers so the registry shall delegate to them.

This set of name servers is here to ensure that even if one of them is
inaccessible, the other(s) will take over by responding to the queries.

The role of the authoritative and the difference with the resolver will
be described more precisely in the section "An hierarchical distributed
system".

### Contacts

In order to be contacted by your registrar, by the registry or by anyone
else as required by ICANN, you are requested to fill in contact details
of your domain.

There are 4 different kind of contacts

<table class="table">
  <tbody>
    <tr>
      <th>Owner/Registrant</th>
      <td>represents the legal owner of a Domain Name</td>
    </tr>
    <tr>
      <th>Administrative Contact</th>
      <td></td>
    </tr>
    <tr>
      <th>Technical Contact</th>
      <td>Person in charge of the technical aspect of the domain name (mainly name server)</td>
    </tr>
    <tr>
      <th>Billing Contact</th>
      <td>Financial Contact Person, not often used on principal tlds</td>
    </tr>
  </tbody>
</table>

It is highly recommended to keep your domains up-to-date. In case of
inaccurate whois report and if your registrar is not able to contact you, they will delete the domain name.

> A Registered Name Holder's willful provision of inaccurate or
> unreliable information, its willful failure promptly to update
> information provided to Registrar, or its failure to respond for over
> fifteen calendar days to inquiries by Registrar concerning the
> accuracy of contact details associated with the Registered Name
> Holder's registration shall constitute a material breach of the
> Registered Name Holder-registrar contract and be a basis for
> cancellation of the Registered Name registration.
> <small><a href="http://www.icann.org/en/registrars/ra-agreement-21may09-en.htm#3.7.7.2">Article 3.7.7.2 - ICANN Registrar Accreditation Agreement</a></small>

ICANN requests its accredited registrars to apply the Whois Data
Reminder Policy, which means:

> At least annually, a registrar must present to the registrant the
> current Whois information, and remind the registrant that provision of
> false Whois information can be grounds for cancellation of their
> domain name registration. Registrants must review their Whois data,
> and make any corrections.
> <small><a href="http://www.icann.org/en/registrars/wdrp.htm">Whois Data Reminder Policy - ICANN</a></small>


### Dates

The domain names are not sold for lifetime. It's more like a rent, cause you 
register it for a defined period (usually between 1 and 10 years). At the end of 
this period you have the ability to renew it or not. The life cycle of domain names 
is mainly based on dates, that's why there are several date fields published in 
the whois. I used "mainly" because domain names shall be deleted for 
non-compliance to the registry rules or following a court decision.

<table class="table">
  <tbody>
    <tr>
      <th>Registration date</th>
      <td>Date the domain name was registered with the registry</td>
    </tr>
    <tr>
      <th>Last update date</th>
      <td>Date the registry received the last update (UPDATE, TRADE, RENEW)</td>
    </tr>
    <tr>
      <th>Expiration date</th>
      <td>Date the domain will not be active anymore</td>
    </tr>
    <tr>
      <th>Release date</th>
      <td>Date the registrant will have no more chance to renew it and will be publicly available</td>
    </tr>
  </tbody>
</table>


## Domain Name Life Cycle

### Overview

![Domain Name Life Cycle](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/domain_lifecycle.png)

### Availability

A domain name is considered as available if no one has already registered it 
and if the naming convention of the registry allows it.

As soon as a domain registration has been executed, it becomes registered

### Registration period

A registered domain, will be displayed as taken until the deletion by the 
registry, which happens after the expiration, following a court decision or 
if the domain has broken some registry rules.

At the registration, the registrant has to choose the duration of the initial 
period. This period can go from 1 to 10 years selectable by year.

Once registration done, the domain name is the property of the registrant 
and is managed through the registrar. 

During this period, the following actions are possible:

<table class="table">
  <tbody>
    <tr>
      <th>NS Update</th>
      <td>Change his authoritative name servers</td>
    </tr>
    <tr>
      <th>Contact Update</th>
      <td>Update the administrative, billing or technical contact</td></tr>
    <tr>
      <th>Domain Trade</th>
      <td>Change the owner of the domain</td></tr>
    <tr>
      <th>Domain Transfer</th>
      <td>Transfer the domain name to another registrar</td></tr>
  </tbody>
</table>

At the end of this period, you will be sent a reminder by the registrar to renew your 
domain name before the expiration date. If no action has been taken the domain is entering
the Auto Renew Grace Period

### Auto Renew Grace period

During this, the domain name is still active at the registry during 30 
days (auto renewed). The registrant has still the ability to renew it or to
let the domain expire.

This procedure has been put in place to prevent renewals issues during absence 
without cut of service. But after this period, the registrar will ask the registry to delete this domain which leads to the redemption period.

### Redemption period

During the Redemption Period, the domain name is de-activated from the root 
zone. That is to say, any service attached to this domain name will stop 
working immediately.

The aim of this period is to allow the domain name registrant, if he missed 
all the registrar's reminders, to detect that his domain has expired and to let 
him reactivate it. he's the only one able to renew it.  
After this period the domain is to be deleted.

### Pending Delete

At the end of the redemption period, a pending delete status will automatically be applied on the domain. 
At this step, no registrar shall be able to act on the domain, neither reactivate it. 
It will be dropped from the registy database in the 5 coming days.

After that, the domain becomes available again.

## An hierarchical distributed naming system

### Overview

A domain name is composed by one or more parts separated by dots (.).
Those parts represent the different levels in the hierarchical tree, and each one
is an independent zone in the tree structured name space. 
To represent this tree, the domain name shall be read part by part from right to left. 
`www.example.com.` should be interpreted like `.com.example.www` and represented as following:

![Dns Hierarchy](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/dns_hierarchy.png)


This representation could be simplified to:

<table class="table">
  <tr>
    <th class="red">Layer 0</th>
    <td>Root</td>
    <td>.</td>
  </tr>
  <tr>
    <th class="blue">Layer 1</th>
    <td>Top Level Domain</td>
    <td>com, net, org, fr, de, nl, eu, asia</td>
  </tr>
  <tr>
    <th class="orange">Layer 2</th>
    <td>Second Level Domain</td>
    <td>co.uk, com.es, google.com, mit.edu</td>
  </tr>
  <tr>
    <th class="green">Layer 3+</th>
    <td>Third Level Domain</td>
    <td>www.google.com, mail.google.com, amazon.co.uk </td>
  </tr>
</table>

You understand better now why I've said previously that the terminology of "extension" was not really
appropriated: the domain name string shall be read from right to left.

It is said as distributed because of the fact that each domain (each element 
in each layers) is managed in an independent zone file hosted on several 
authoritative name servers. 

### Name Servers

A name server is a program able to translate domain names into IP addresses. 
This process is named domain name resolution. A name server can also be Authoritative or Recursive (sometimes both but it's not recommended)

![DNS resolution workflow](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/how_dns_works.png)

### Authoritative Name Servers

Authoritative name servers are responsible for providing the official 
response for the zones they are currently hosting. 
They are declared during the domain name registration process and are (usually) published in the whois 
record. They are usually provided by the registrar for free with the domain 
name registration, but you can also subscribe to professional DNS service 
or use your own ones.

### Recursive Name Servers

Recursive Name Servers, also referred as DNS or Resolvers, are the tools able to 
convert the URL you've entered in your browser to addresses understandable by machines. 
They are querying step by step all the authoritative name servers from the domain hierarchy. 
That's where the recursive comes from.

The results provided by the authoritative name servers, during the Time To Live value (TTL is available on  
all the DNS resource records), will be cached by the resolvers for performance matter.


The following video is the best illustration of how a DNS resolution is done.

<iframe width="853" height="480" src="//www.youtube.com/embed/2ZUxoi7YNgs?rel=0" frameborder="0" allowfullscreen></iframe>

## Top-level, Second Level domains

You've probably already heard (or will) hear about all those words making part of the domain name industry vocabulary:
Top Level Domains, Second Level Domain, and TLDs.

### Top Level Domain

A top level domain, TLD, is the domain name just under the ROOT of the domain name system.
In other words it's the "extension" of your domain name (com, net, de, nl,...).

![Top Leve Domains](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/dns_hierarchy_tld.png)

The TLDs are delegated directly from the root zone by IANA (who is responsible 
of it). An up-to-date list of all the existing TLDs is available on IANA's 
website [here](http://www.iana.org/domains/root/db/)

There are 4 different types of TLDs existing: Generic, Country Code, 
Sponsored, Infrastructure

<table class="table">
  <thead>
    <tr>
      <th>Type</th>
      <th>Description</th>
      <th>Example</th>
    <tr>
  </thead>
  <tbody>
    <tr>
      <th>Generic TLD (gTLDs)</th>
      <td>
        The generic TLDs, also known as gTLDs, are the most famous since some of them 
        were part of the initial release and usually are not restricted.
      </td>
      <td>
        com, net, org, info, ...
      </td>
    </tr>
    <tr>
      <th>Country Code TLD (ccTLDs)</th>
      <td>
        The ccTLDs, are the Top Level Domains allocated to a country based on the 2 
        letters version of its [ISO 3166-1 code](http://en.wikipedia.org/wiki/ISO_3166-1).
      </td>
      <td>
        fr, de, us, nl, es, ...
      </td>
    </tr>
    <tr>
      <th>Infrastructure TLD</th>
      <td>
        The infrastructure TLDs are reserved by IANA for technical purpose and 
        contains only the .arpa top level domain.

        For more information about the arpa zone, please refer to <a href="http://www.iana.org/domains/arpa/">the documentation 
        available on IANA's website</a>
      </td>
      <td>.arpa </td>
    </tr>
    <tr>
      <th>Sponsored TLD </th>  
      <td>
        The sponsored TLDs, considered also has a sub category of the gTLDs, are 
        specialized in the specific community.
      </td>
      <td>coop, museum, edu, aero </td>
    </tr>
  </tbody>
</table>



### Second Level Domain

Usually called second-level domains, they are registered with your Domain Name Registrar. 
The second level domain are composed with a name/label and a TLD.

Example: anouar.im, google.com, ...

![Second Leve Domains](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/dns_hierarchy_sld.png)

Other cases of SLD offer the registration of third level domains as a 
specialized alternative to TLDs for their customers.
 

- uk: Because of the registration restriction on the TLD
  - .co.uk    - general use
  - .gov.uk   - government
  - .ltd.uk   - limited companies
  - .me.uk    - general use
-fr: To provide additional categories
  - .assoc.fr - association use
  - .tm.fr    - trademarks use
  - ...

Some are just provided by private companies owning short second level
domain such as `.co.**`, `.com.**` or `***.com`.
It permits to override the limitation on some top level domains:

 - co.ee
 - co.nl
 - com.de
 - de.com
 - ...


## The Registry, the registrar and the registrant

![Registry Registrar Registrant](/assets/images/posts_illustration/2011-12-12-what-is-a-domain-name-and-what-is-behind-the-scene/registry_registrar_registrant.png)

### The Registry

> **reg&middot;is&middot;try** /&#712;rej&#601;str&#275;/ *Noun*
> 
> 1. A place or office where registers or records are kept

The registry is the authority responsible of managing a Top Level Domain 
(ie. Verisign for .COM). 

In a more traditional market, the registry could be compared to wholesaler. 

Each registry fixes its own rules. This is the reason why I've said previously 
that the elements shall vary from a registry to another.

Some examples of naming convention rules could be:

 - minimum 3 letters
 - minimum registration period of 2 years
 - maximum registration period of 10 years
 - no sexual or pornographic connotation
 - maximum 10 domains per registrant
 - registrant living in a certain country
 - should not be a reserved domain
 - ...

The accreditation rules are also fixed by the registry and define the 
mandatory steps needed to be an official registrar.
An example of those steps could be:

 - to be ICANN accredited
 - to credit an account with a certain amount
 - to have authoritative name server in a certain country
 - to pass a MCQ on the management rules of the registry
 - to pass a technical test to validate API calls

All registrars are required to be accredited with the registry in order
to resell domain names.

### The Registrar

> **reg&middot;is&middot;trar** /&#712;rej&#601;str&auml;r/ *Noun*
> 
> 1. An official responsible for keeping a register or official records.

An accredited registrar is a Service Provider allowed to sell the
TLDs provided by a registry.

More basically, if a customer wants to register a domain name the registrar, 
being an authorized retailer, can provide it from the registry.

As an intermediate between the registry and the registrant, 
registrars have to handle at the same time a B2C and B2B relationship.

The B2C being between it and the registrant, to whom it offers a web
interface to order and manage his domain names.

The B2B being between it and the registry, whom it communicates with over
APIs to automate the provisioning,

### The Registrant

> **reg&middot;is&middot;trant** /&#712;rej&#601;str&#601;nt/ *Noun*
> 
> 1. A person who registers.

A registrant is an individual or an organization that owns a domain name.
The registrant can be yourself or your company when you register your own 
domain name.

## References

 - **DNS Glossary**: [Men & Mice DNS Glossary](http://www.menandmice.com/knowledgehub/dnsglossary/)
 - **RFC 1034**: [DOMAIN NAMES - CONCEPTS and FACILITIES](http://tools.ietf.org/html/rfc1034)
 - **RFC 1035**: [DOMAIN NAMES - IMPLEMENTATION and SPECIFICATION](http://tools.ietf.org/html/rfc1035)
 - **RFC 3172**: [Management Guidelines & Operational Requirements for the ARPA](http://tools.ietf.org/html/rfc3172)
 - **RFC 1738**: [Uniform Resource Locators](http://tools.ietf.org/html/rfc1738)
 - **ICANN**:    [Internet Corporation for Assigned Names and Numbers](http://www.icann.org/)
 - **IANA**:     [The Internet Assigned Numbers Authority](http://www.iana.org/)
 - **Root zone DB**: [Root Zone Database](http://www.iana.org/domains/root/db)

I hope you enjoyed this post. If you'd like to discuss feel please feel free to a comment below or to [drop me an email](mailto:anouar+blog@adlani.com).

