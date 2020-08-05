<h1 align="center">ONT TAG Product and Service Introduction </h1>

## What is ONT TAG?

ONT TAG is an open and decentralized authentication platform based on ONT ID and the Ontology trust ecosystem. It provides KYC services for people, finances, things, and affairs. The Ontology trust ecosystem has gathered trust anchors that provide global identity authentication services, including IdentityMind, CFCA, SenseTime, Shufti Pro, etc., as well as email, mobile, and social media authentication methods.

## What can ONT TAG do for you?

If you have your own app or platform, ONT TAG can help your users complete authentication. You can integrate ONT TAG into your platform or product for KYC, identity authentication services, login, and so on, depending on your business requirements.

ONT TAG has the following advantages:

* The Ontology trust ecosystem is already connected to global certification services, covering users in 218 countries and regions;
* Provides low cost, one-time authentication, which can be used multiple times, and reduces the cost of multiple authentications;
* The protocol uses [end-to-end encryption specifications](https://github.com/ontio/ontology-DID/blob/master/docs/en/end-to-end-encryption.md) to protect user data throughout;
* All authentication actions and results are attested to the Ontology blockchain;
* The specification supports cryptographic algorithms such as zero-knowledge proof, and users can selectively present their own identity information to maximize user privacy.

[>> Start using the ONT TAG service](./auth.md)。


## Learn how ONT TAG works

> ONT TAG is based on ONT ID. To understand how to use ONT ID and verifiable credentials, please first familiarize yourself with [ONT ID](https://docs.ont.io/ontology-elements/ontid).

The ONT TAG open authentication service is based on the following principles:
![](../res/process.png)

The main duties of ONT TAG are:
* Registering trust anchors and their verification services to the Ontology ecosystem, and providing a verification service discovery function;
* Registering authentication and verifiable credential templates, and providing a template discovery function;
* User verifiable credential information delivery;
* Matching veriable credential templates with user authentication requirements.

The particles involved in the process include:
* Requester: A dApp, organization, or service that requires authentication for its users. The authentication service demander in the Ontology trust ecosystem.
* Trust anchor: Trust anchors in the Ontology trust ecosystem provide authentication services for users worldwide and can issue verifiable credentials. Behind them are global identity authentication service providers, which can help users achieve identity authentication.
