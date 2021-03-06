<h1 align="center">ONT TAG Authentication Service Access Guide</h1>

[TOC]

## Overview

This article will guide Requester to access the Ontology network, and to use the authentication service provided by ONT TAG. The participants involved in the whole process include:

* Requester: Any institution or service provider that requires authentication for people, object or event. Requester is the demand side of authentication service in the Ontology Trust Eco-system.
* ONT TAG: Based on ONTID and Ontology Trust Ecosystem, it is an open, decentralized authentication platform that provides rich authentication services for people, asset, objects, and things. ONT TAG synergizes users and requesters for data exchange and all the data is encrypted protect User's private data.
* TrustAnchor: Trust Anchor refers to the partner that provides authentication verification services on the Ontology Trust Ecosystem. A trust anchor can be a government agency, university, bank, third-party certification service organization (such as CA institutions), biometric technology company, etc.



## How does ONT TAG work?

![Interaction Process Description](../res/authflow.png)


- A0：ONT TAG provides a public authentication service marketplace. A credential consumer can browse and select their desired TrustAnchor and its authentication service they need on the ONT TAG platform.
- A1：After the credential consumer confirms the authentication service, the credential consumer needs to register certain basic information to the ONT TAG platform. The information includes ONT ID of the Requester, basic introduction of the credential consumer and a callback address.
- A2：The credential consumer submits the user's data (from their end users) to the TrustAnchor Source, via ONT TAG, based on the requirements of the specific TrustAnchor.
- A3.1, A3.2：TrustAnchor authenticates user's data uploaded by credential consumer and completes the issuance of the verifiable credential, which will be stored on the blockchain. Transaction will then be made
- A4：After the TrustAnchor issues the verifiable credential, the encrypted public key of the ONT ID corresponding to the verifiable credential user, will be sent to the ONT TAG.
- A5, A6：ONT TAG pushes the verifiable credential to end user directly, and end user SDK will send the credential to credential consumer according to the callback address.
- A7: The credential consumer can verify the credential status on Ontology mainnet.

## How to integrate with ONT TAG?

### Step 1: Find the Right Service

TrustAnchor registers to the ONT TAG the authentication service and verifiable credential template information to the ONT TAG. ONT TAG provides the TrustAnchor authentication service marketplace. Requester can select the certification service from the marketplace.

The ONT TAG authentication services that are currently open to the public include:

#### Global Identity Authentication Service

* TrustAnchor Name : Ontology Global Identity TrustAnchor
* TrustAnchor ONT ID :  did:ont:ANNmeSiQJVwq3z6KvKo3SSKGnoBqvwYcwt
* TrustAnchor Account Address : ATGJSGzm2poCB8N44BgrAccJcZ64MFf187
* Service list

| Credential Templete Name | Credential Description |  Doc Link |
| :-----------------: | :----------------:| :------: |
|credential:sfp_passport_authentication | Global User Passport Authenticatioin   | [link](./ONTTA.md) |
|credential:sfp_idcard_authentication   | Global User ID Card Authentication | [link](./ONTTA.md) |
|credential:sfp_dl_authentication       | Global User Driver License Authentication  | [link](./ONTTA.md) |



### Step 2: Choose the Payment Model

There is a fee associated for using ONT TAG. ONT TAG supports two payment models. You need to choose the one that fits your need.

* Mode 1: Pay as You Go

The instant payment mode is completely open and autonomous, that is, each authentication request will consume ONG fee, so the authentication requester needs to establish an ONG transfer transaction (The payment address and specific amount are specified by each TrustAnchor). After receiving the authentication request, TrustAnchor will first sends the transaction to the blockchain, and the subsequent identity authentication process will continue after the transaction is successfully sent.

* Mode 2: Postpaid model

Please contact **[Ontology](contact@ont.io)** for cost and further integration information.


### Step 3:  ONT TAG Platform Registion

After Requester selects the authentication service provided by the TrustAnchor, Requester needs to register the relevant information on the ONT TAG platform, including the ONTID, basic introduction,  authentication service, and a callback address. Only registered Requester on the platform will receive the subsequent verifiable credential callback.


#### Requester Registers API

```json
Host：https://host/api/v1/onttag/authrequesters
Method：POST /HTTP/1.1
Content-Type: application/json
RequestExample：
{
	"callback_addr": "https://xxx",
	"description": "coinwallet",
	"name": "coinwallet",
	"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM",
	"ta_info": [{
		"credential_contexts": [
			"credential:cfca_authentication",
			"credential:sensetime_authentication"
		],
		"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM"
	}],
	"signature": "AQp2ka0OJG5K7jlnaV8jwWneye7knHWTNN+D3yUly="
}

SuccessResponse：
{
	"version": "1.0",
	"action": "Register",
	"error": 0,
	"desc": "SUCCESS",
	"result": true
}
```


| RequestField     |     Type |   Description   | Necessary|
| :--------------: | :--------:| :------: |:----:|
|    callback_addr |   String|  verifiable credential callback address  | Y|
|    description |   String|  Requester discription  | Y|
|    name|   String|  Requester's name |Y|
|    ontid|   String|  Requestes's ONT ID  | Y|
|    ta_info.credential_contexts |   list|  Select the list of verifiable credentials templates for the TrustAnchor in need   | Y|
|    ta_info.ontid |   String|  Select the required TrustAnchor's ONT ID    | Y|
|    signature |   String|  The Requester uses the ONT ID private key to sign the requested content according to the signature rules  |Y|


| ResponseField     |     Type |   Description   |
| :--------------: | :--------:| :------: |
|    result|   Boolean|  true：egister Success  false：Register Failure|


> In order to ensure data transmission's security, the callback interface must be in the form of https+domain name, and Requester must ensure that the registered callback interface is highly available and accepts the https post request that meets the ONT TAG standard.



### Step 4: Submit Certification to TrustAnchor

After Requester selects the TrustAnchor authentication service in the ONT TAG certification market, Requester needs to submit the authentication data to TrustAnchor. TrustAnchor will then authenticates the identity, issues the verifiable credential, deposits basic information of the verifiable credential, and passes it to ONT TAG through end-to-end encrypted transmission


- [Access Global Identity Authentication](./ONTTA.md)


### Step 5: Get Authentication Results

After TrustAnchor completes the user's information authentication and issues a verifialbe credential, the verifiable credential will then be sent to ONT TAG. ONT TAG pushes the signed verifialbe credential to Requester based on the callback address previously registered by Requester.


When the information is called back, the ONT TAG platform will bring the signature corresponding to its own ONT ID. Requester can verify the signature, as well as the credibility and the non-tamperable features of the callback request.

```json
Host：callback address
Method：POST /HTTP/1.1
Content-Type: application/json
RequestExample：
{
	"auth_flag": true,
	"auth_id": "xxxxxxxxxxx",
	"credential_context": "credential:sfp_passport_authentication",
	"description": "shuftipro passport authentication ",
	"encrp_origdata": "header.payload.signature.blockchain_proof",
	"ontid": "did:ont:AEnB1v4zRzepHY344g2K1eiZqdskhwGuN3",
	"owner_ontid": "did:ont:A9Kn1v4zRzepHY344g2K1eiZqdskhnh2Jv",
	"ta_ontid": "did:ont:A7wB7v4zRzepHY344g2K1eiZqdskhwHu9J",
	"txnhash": "836764a693000d2ca89ea7187af6d40c0a10c31b202b0551f63c6bc1be53fc5b",
	"signature": "AQp2ka0OJWTNN+D3yUlydyjpLpS/GJp6cFt9+wWeT25dBdGYSaErxVDpM1hnbC6Pog="
}
```


| RequestField     |     Type |   Description   | Necessary|
| :--------------: | :--------:| :------: |:----:|
|    auth_flag |   Boolean|  TrustAnchor authentication results true：authentication passed false：authentication failed  |Y|
|    auth_id |   String|  The authentication number passed to TrustAnchor when Requester is authenticated.  |Y|
|    credential_context |   String|  Verifiable credential template identifier  |Y|
|    description|   String|  The reason for the failure if the authentication fails; The description of verifiable credential, if the authentication is successful|Y|
|    encrp_origdata|   String|  encrypted verifiable credential |Y|
|    ontid|   String|  ONT TAG's ONT ID  |Y|
|    owner_ontid|   String|  User's ONT ID   |Y|
|    ta_ontid|   String|  TrustAnchor's ONT ID   |Y|
|    txnhash |   String|  Hash of verifiable credential deposit transaction |Y|
|    signature |   String|  ONT TAG uses ONT ID priviate key to sign the requested content |Y|




### Appendix

#### Error Code Dictionary

| Field | Type | Description |
| :--- | :--- | :--- |
| 0 | long | SUCCESS. success |
| 61001 | long | FAIL, param error. parameter error |
| 61002 | long | FAIL, already exist. already exist |
| 61003 | long | FAIL, not found. not found|
| 62003 | long | FAIL, communication fail. Internal communication failure |
| 62006 | long | FAIL, FAIL, verify signature fail. signature verification failure |
| 63001 | long | FAIL, inner error. inner error |


#### Get your own ONT ID

Registering an ONT ID on Ontology requires an ONG fee. First, you need to have a digital asset account, and there is at least 0.01 ONG in your account. Then use the account to pay the fee for registering ONT ID and complete the registration on the ONT ID blockchain.

> TestNet ONG can be applied from Ontology Developer Center：[TestNet ONG Applicaiotn Gateway ](https://developer.ont.io/applyOng)

#### Use ONT ID to sign and verify a signature

**Signature Rules：**

The JSON object in the HTTP Post request body needs to be sorted in ascending alphabetical order of the key, serialized into a standard JSON format string, then the request content string is signed and finally the signature is added to the request body with the signature as the key.

Take a registration request as an example：
After the JSON object of POST Request is sorted in ascending key order.
```json
{
	"callback_addr": "https://xxx",
	"description": "coinwallet",
	"name": "coinwallet",
	"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM",
	"ta_info": [{
		"credential_contexts": [
			"credential:sensetime_authentication"
		],
		"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM"
	}]
}
```
Convert it to standard JSON format string：

```
{"callback_addr":"https://xxx","description":"coinwallet","name":"coinwallet","ontid":"did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM","ta_info":[{"claim_contexts":["claim:sensetime_authentication"],"ontid":"did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM"}]}
```

Then Sign the JSON format string, after getting sigvalue, add the signature as the key to the JSON object of the Post request body.

The final authentication post body is：
```json
{
	"callback_addr": "https://xxx",
	"description": "coinwallet",
	"name": "coinwallet",
	"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM",
	"ta_info": [{
		"credential_contexts": [
			"credential:sensetime_authentication"
		],
		"ontid": "did:ont:AXXxiWCuJXmuPGnsBji4cqWqV1VrKx8nkM"
	}],
	"signature": "sigvalue"
}
```


### Code sample

For Construct transfer transaction, Apply ONT ID private key for signature, and so on, please refer to

- [Java demo](../../sample/Demo.java)
- [TS demo](../../sample/OntIdSignDemo.js)

[SDK developer documentation center](https://docs.ont.io/developer-tools/sdk)
