

# Unhosted

From P2P Foundation

Jump to: navigation, search

**= Freedom from web 2.0's monopoly platforms**

URL = [http://www.unhosted.org](http://www.unhosted.org)

    

    

## Contents

- 1 Definition
  - 1.1 Naming The App Class

- 2 Description
- 3 Unhosted Manifesto
- 4 Interview
- 5 More Information

# Definition

"Unhosted is a project for strengthening free software against hosted software. With our protocol, a website is only source code. Dynamic data is encrypted and decentralised, to per-user storage nodes. This benefits free software, as well as scalability, robustness, and online privacy."

## Naming The App Class

Michiel De Jong

"The following terms:

- unhosted app
- html5 app
- offline web app
- client-side only app
- (w3c or opera) widget

all mean 'a web app whose functionality is not hosted server-side'."

# Description

Jan-Christoph Borchardt:

"Unhosted apps are web applications able to run locally in your browser – because they are pure JavaScript, like many web apps already. You identify with your user address which then connects your remote storage to the app, loads your data and decrypts it locally – all in your browser, nothing leaking to the app server.

This makes it easier and highly secure for users: You have your data in one place, like a »My Documents« folder that you can use with web apps. And you don’t need to get a separate account for every application you use, nor export and import it over – you only remember your storage user address. Your data is not being snooped or encumbered in proprietary platforms. It also makes it easier for app developers because they neither have to worry about hosting all the data and user accounts nor about server load – all the computing takes place in your browser. With the app being just JavaScript it becomes very easy to develop and deploy new apps which everyone can use. Technically speaking, we define a standard combining things like WebFinger, OAuth, WebDAV, Cross-Origin Resource Sharing (CORS), preferably BrowserID and ideally with end-to-end-encryption on top." (NextNet mailing list, October 2011)

    

# Unhosted Manifesto

"Manifesto for the unhosted web

**Freedom from Web2.0's monopoly platforms.**

Free/libre and Open Source Software (FLOSS) frees us from having to install proprietary software on our terminals. But installable software is losing ground to hosted software (websites). The server software is often open source (e.g. LAMP), but the website itself as a software product is almost always proprietary. There is an obvious reason for this: Even if an Affero license allows us to download the website's source code, only a commercial company can finance the thousands of servers needed to host a successful website. To make things worse, hosted software has more power over its users than installable software, because it forces you to put your user data on servers owned by the same company that publishes the software. If you want to use Google Docs, you have to reveal your work to a Google-owned server (what Richard Stallman calls "careless computing").

    

**Unhosted**

I left my dayjob and started the UNHOSTED project to try and stop this. We needed to break the one-to-one link between the software publisher who writes a website (e.g. "Google, Inc") and the "hostage provider" who hosts that website (e.g. also "Google, Inc"). Unhosted creates a simple grease layer in the form of an open web standard (UJ/0.1) between the hosted software and the servers that host it, so this is decoupled.

    

**Hacky Holidays dev contest**

We managed to get a proof-of-concept working, and it is in alpha now. If you decide to try out the code, then please submit an inventive unhosted web app to our "Hacky Holidays" dev contest, whose deadline is New Year's Day. To be unhosted, a website's code will need to be very ajaxy first, so that all the servers do is store and serve json data. No server-side processing. This is because we need to switch from transport-layer encryption to client-side payload encryption (we no longer necessarily trust the server we're talking to). From within the app's source code, that should run entirely in JavaScript and HTML5, json-objects can be stored, retrieved, sent, and received. The user will have the same experience (we even managed to avoid needing a plugin), but the website is unhosted in the sense that the servers you talk to only see encrypted data and don't even know which application you are running.

    

**FLOSS-as-a-Service on top of a commodity infrastructure**

A hosted website provides two things: processing and storage. An unhosted website only hosts its source code (or even just a bootloader for it). Processing is done in the browser, with ajax against encrypted cloud storage. These unhosted storage nodes can be provided by whoever provides your email hosting: your employer, ISP, university, mobile operator, public library, a hobbyist friend who runs a server at home, a hosting company, etc. They become just like mailservers, BGP switches, fibre links and other commodity infrastructure: independent of which application you run on top of them. And they only get to see encrypted data. We probably don't need a lot of storage nodes until unhosted apps really start to attract end users. When they do, everything will degrade gracefully, and users will have an incentive to look for a faster and bigger storage node to migrate to. To maximize the control of the user over his data, migration from one storage node to another will be an integral part of the protocol. Unhosted can in the future be combined with recent projects like Mozilla's Open Web Applications for free-speech "app channels", and is inspired on architectures like tahoe-LAFS for the underlying encrypted cloud storage.

    

**how it works**

Unhosted consists of two parts: the unhosted storage node (a very simple server that receives json-data over HTTP, stores it, and allows you to retrieve it) and a JavaScript library that allows you to interact with these storage nodes from code that runs within the browser. All data is RSA-encrypted and signed before it is sent, and unencrypted and verified when it is received. We used Tom Wu's big numbers library for this. Normally it doesn't make a lot of sense to encrypt user data in the browser using a website's JavaScript, and then send that data to that same website: If the server wanted to see what is inside the encrypted data stream, all it would have to do would be to tinker with the JavaScript. So usually, in-browser encryption is not very useful. But in this case, the host storing the data is different from the host providing the JavaScript. So then it starts making sense. A FLOSS programmer would host his website as purely static content - only the source code. The user logs in with a globally unique identifier (a guid, like an email address) that indicates on which storage node that user's data should be stored. With Cross-Origin Resource Sharing (CORS), the RSA-encrypted JSON-data is sent there, and retrieved from there. Payload-encryption is possible with PGP-encrypted email, but never reached the mainstream so far, probably because it requires extra clicks and actions. With unhosted, we tried to make it "zero-UX": The user doesn't even notice whether the website he is using is hosted or unhosted, unless you tell him. Just like https is zero-UX in that it requires zero clicks from the end user. The webdeveloper only has to call unhosted.set() and unhosted.get() from inside his JavaScript code.

    

**fabric-based pki**

When we implemented all this, and played with a few example apps, we noticed how cumbersome it is to have to deal with people's public keys all the time. If I want to send a message to a new contact, I need not only his guid, but also his public key. In a normal, hosted situation, where you trust the server that hosts (provides persistance for) a user profile, this is easy, you just ask the server what that user's public key is (spki, for instance with WebFinger). But we don't want to trust unhosted storage nodes if we don't have to. That is why we invented what we call a fabric-based public key infrastructure (pki). We call it fabric based, because unlike CA, FOAF and SPKI, it relies on complete strangers to sign other people's keys. We define a deterministic way to "walk" from a given guid to another one. Every guid signs the public keys of "neighbouring" guids, even if he doesn't know who these people are. It helps to create a fabric of users, where the hosting providers have no power to stage man-in-the-middle attack. This pki is the most complicated part of unhosted, and is still quite new and in need of some peer review by people who know about this.

    

**current state of the project**

Apart from the StarSign pki, the rest of unhosted (setting and getting a key, or sending an receiving a message) seems to work in the examples that come with the tarball (see download link above). But it is only a proof-of-concept! Please be aware that the code is still very rough, and it is not well-written yet. We didn't follow any coding standards. Feel free to make fun of it - we ourselves do as well. :) But please don't judge this project by its current code quality, it is only meant to test if this could work, and to get the idea across. We do not, at all, propose anybody use this code in production. And if (unlike me!) you have any JavaScript skills, then please also consider to contribute, instead. We also received some reactions about us currently using twitter instead of identi.ca, and google groups instead of our own lists server. We are thinking about how we can change this, and not be like "vegetarian butchers". For now, please join the project if you like its goals. Even (or especially) if you have some different ideas from what we have done so far. What is done so far is only the start of a meme. We need more people on board to make this a succes!

    

**unhosted applications**

The unhosted infrastructure could form the support for open-source web apps like (or compatible with) diaspora, federated social web, vendor relationship management, bitcoin, and hopefully all hosted services we currently use. Websearch will be the most challenging one because of its inherent centralization, but social search variants (propagating friends-of-friends' recommendations) should definitely be possible on an unhosted infrastructure. Once that is achieved, open source software can become as successful in the "Software as a Service" world as it is in "installed" software. It will help keep Google, Facebook and Apple in check, so they compete on merit only. If you think this project is worthy of pursuing then please download our source code, join our mailing list, or @mention us on identi.ca/unhosted and twitter. We need web developers to start using this as a scalable way to design FLOSS web apps. And if you like a challenge, participate in our Hacky Holidays dev contest, and be one of the first developers to create an unhosted web app. We depend on web developers to create an unhosted web, where all apps are free software, and without commercial companies spying on our data." ( [http://www.unhosted.org/manifesto.html](http://www.unhosted.org/manifesto.html) )

    

# Interview

"Chris Woolfrey: **Would you like to explain the Unhosted project in your own words?**

**Michiel de Jong**: There are several ways you could explain it; my favourite angle is the software freedom angle. Software freedom used to mean the right to control (use, share, study and improve) the source code / the program that the application executes – the definition that FSFE use. Back in the day, that was enough. It was taken for granted that you already had control the data that the application handled; of course you do, it’s on your computer, or on a server where you have full access to at least the data that your applications are using.

For installed software, both desktop and server, that view used to be accurate: if you controlled the source code you had software freedom. But then, slowly, installed software was pushed further away from the user by hosted software (stuff like Google Docs, Facebook and Twitter). Hosted websites like these aren’t primarily a source of information; they are interactive applications, and in this context software freedom doesn’t exist.

It’s absurd that hosted software makes you surrender your data to the author of the application in question, but it’s what happens. It happened slowly, because informational websites became dynamic websites, and those dynamic websites then started accepting user input and slowly became interactive software. Now fully hosted software is widely used, and people use it to replace locally installed desktop applications.

    
In the shift from local applications to hosted applications software freedom got left behind. Nobody talks about locally installed software any more, they talk about hosted software, yet some people say “I run an entirely Free Software stack on my laptop; only the firmware of the graphics card is proprietary”, and that’s a mistake, because so much of the ‘software’ that they use is not installed locally on their laptop, it is merely viewed through their web browser.

The Unhosted project aims to invent and promote a way to fix these issues. Software freedom nowadays needs to be not only code-freedom; it must be code-freedom plus data-freedom.

    
**CW: How does Unhosted achieve this?**

MdJ: We’re separating the code of an application from its data. When you log in to an Unhosted web application, the URI in the address bar determines where the code lives, but the domain proceeding the ‘@’ symbol in your username determines where your data lives; this frees your data from the hands of the application server, and frees the application server from the burden of your data.

This means that free of charge hosted Free Software web applications become feasible again. After all, there is an obvious Free Software replacement for Microsoft Windows: GNU/Linux, just as an obvious Free replacement for Microsoft Office is Libre Office. But what Free Software can so obviously replace Google Docs? Why can’t you go to ‘www.libredocs.org’ and use Free Software on the web, just like you can with desktop software?

The simple answer is that the costs of running software remotely on a server and providing it as a service are too high to be able to provide it free of charge. In order to write Free Software, all that is required is the time and skills of the developers concerned. But there is no way to make Free Software available to the world online which doesn’t involve a monetary cost, because doing so requires the use of servers, and whoever owns those servers will charge you a monthly fee. Our architecture for separating code and data, leaving the processing in the browser, fixes that: it makes it very cheap to host Free Software web applications because all you have to host is the application logic, the code files, not the data that drives it.

    
That’s the ‘free the application from the burden of your data’ part. And then there’s the other part: that software equals code plus data, but software freedom equals code-freedom plus data-freedom. With Unhosted, data-freedom is achieved because when you sign in to some application you decide which domain gets to host your data for you. You can get an account with a public provider – they’re in the process of being set up – or tell your university or employer’s sysadmin to run a node for the faculty or for the office, then basically everybody who has an email address ‘@wherever’ would get an Unhosted account with that same user name.

    
**CW: Are there privacy benefits of using Unhosted applications when compared to traditional web applications which store both code and data remotely?**

MdJ: When using an Unhosted application, all your data is encrypted by your web browser before it is sent to the server where your Unhosted account resides. That way the data stored in your Unhosted account can exist on any commodity server, because although you rely on that server to give you access to your data, the data itself is securely stored and encrypted, and you need not worry about your Unhosted account host reading your messages, for example. The data stored by an Unhosted application is encrypted by your web browser before it is sent and stored in your Unhosted account, and it then gets decrypted when it is sent back to your web browser when it is required. The server storing your Unhosted web application data is blind therefore; it sends your data to and from Unhosted websites without being able to read its contents.

Normally, using JavaScript for cryptography doesn’t make a lot of sense because if a website includes JavaScript scripts for encrypting data then those same scripts could be used to eavesdrop on the encrypted data. But in these cases the cryptography scripts originate from the same untrusted source that the encrypted data would then be sent to and received from. In the case of Unhosted it’s different since we separate the domain that delivers the application code from the domain where the data is stored. The Unhosted account provider will not have access to the application’s JavaScript cryptography scripts, so the Unhosted web application can encrypt things that the Unhosted account provider won’t be able to decrypt.

    
**CW: What kind of applications do you think are best suited to using Unhosted? What types of web application do you expect to adopt Unhosted first?**

MdJ: Any application which doesn’t store a large amount of user data can be easily adapted to use Unhosted. Applications like Google Docs which require the storage of a lot of important user data would benefit most from moving to Unhosted however. For parallel computing it will also be a great boost. But for other things, like search engines, it would require some clever algorithms to allow it to work in a more decentralized way. In general, any web application that requires the storage of a large amount of user-specific data could benefit from becoming Unhosted.

    
**CW: It sounds like there’s a lot of scope for Unhosted to have a big impact on other web-based Free Software projects; how does your work fit in with things like Diaspora, Appleseed, and YaCy?**

MdJ: Unhosted was sort of born on the Diaspora developer’s mailing-list. We were talking about how Diaspora switched from PGP to SSL, and how end-to-end encryption would be nicer, so I started trying to write Ajax payload encryption. It was meant to be an addition to Diaspora. Later I realised that it could be used much more widely than just for Diaspora." ( [http://blogs.fsfe.org/fellowship-interviews/?p=299](http://blogs.fsfe.org/fellowship-interviews/?p=299) )

    

# More Information

See: [Free Network Services](http://p2pfoundation.net/Free_Network_Services "Free Network Services")

Retrieved from " [?title=Unhosted&oldid=63759](?title=Unhosted&oldid=63759) "

  [Categories](http://p2pfoundation.net/Special:Categories "Special:Categories") :
- [Technology](http://p2pfoundation.net/Category:Technology "Category:Technology")
- [Standards](http://p2pfoundation.net/Category:Standards "Category:Standards")
- [P2P Infrastructure](http://p2pfoundation.net/Category:P2P_Infrastructure "Category:P2P Infrastructure")

----------
This page was forked with permission from [http://p2pfoundation.net/Unhosted](http://p2pfoundation.net/Unhosted)
----------
  ![Attribution-ShareAlike 3.0 Unported](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)