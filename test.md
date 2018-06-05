# Account Aggregator API


<a name="overview"></a>
## Overview
# Summary
An Account Aggregator (AA) is an specialized entity meant to serve as an intermediary between a consumer (a User or a FIU) and entities such as banks, securities, insurance, pensions and other account/finance management service providers. This specification provides an Application Programming Interface to enable real time mechanisms for aggregating account information from multiple entities (Financial Information Providers).

The AA API specifications defines API for several entities, viz
1. Financial Information Provider (FIP API)
2. Account Aggregator (AA API)
3. Central Registry (CR API)
4. Financial Information User (FIU Callback API)

This AA API is implemented by the AA and defines the mechanisms for FIU to programatically connect to it. Furthermore these API may be consumed by the AA's own client side applications.

The AA Technical Implementation Guide provide further details for implementing these API functionality.
# Account Aggregator API
The Account Aggregator (AA) serves as an intermediary between a User or a FIU and Financial Information Providers (such as banks, securities, insurance, pensions and other account/finance management service providers). This Application Programming Iterface enable AA to manage the lifecycle of consent artefacts, mediates the secure flow of financial information from FIPs to FIUs based on explicit user consent and exposes Notification method so that FIU and FIP can notify it about the events generated during consent flow and data flow.
# References
1. NBFC Account Aggregator, Technical Standards (coming soon)
2. Reserve Bank of India (RBI). Master Direction- Non-Banking Financial Company - Account Aggregator (Reserve Bank) Directions. RBI/DNBR/2016-17/46, Master Direction [DNBR.PD.009/03.10.119/2016-17. 2016 ](https://rbidocs.rbi.org.in/rdocs/notification/PDFs/MD46859213614C3046C1BF9B7CF563FF1346.PDF) (Updated 2017).

3. Ministry of Electronics and Information Technology, [Electronic Consent Architecture](http://dla.gov.in/sites/default/files/pdf/MeitY-Consent-Tech-Framework%20v1.1.pdf)
---


### Version information
*Version* : 1.0.0


### Contact information
*Contact Email* : sdo@rebit.org.in


### License information
*License* : Apache 2.0  
*License URL* : http://www.apache.org/licenses/LICENSE-2.0.html  
*Terms of service* : http://a.rebit.org.in


### URI scheme
*Host* : development-6d5399a6-eval-test.apigee.net  
*BasePath* : /aa  
*Schemes* : HTTPS


### Tags

* Consent Flow : Consent Management APIs
* Data Flow : APIs for aggregation of FI
* Monitoring : Monitoring API Interface for checking availability of AA.
* Notifications : This API is used by the FIPs and FIUs to submit notifications to the AA.




<a name="paths"></a>
## Paths

<a name="consents-post"></a>
### POST /Consents

#### Description
This API is intended for FIU to obtain digitally signed consent artefacts. Once the User approves the consent request, AA generates the digitally signed consent artefacts and shares with FIU.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|
|**Body**|**Deep**  <br>*optional*||[ConsentsRequest](#consentsrequest)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[Response 200](#consents-post-response-200)|
|**400**|The request was formatted incorrectly|[ConsentsResponse](#consentsresponse)|
|**404**|The User's AA User ID was not found|[ConsentsResponse](#consentsresponse)|

<a name="consents-post-response-200"></a>
**Response 200**

|Name|Description|Schema|
|---|---|---|
|**ConsentsHandle**  <br>*optional*||string|
|**id**  <br>*optional*|**Example** : `"user_identifier@AA_identifier"`|string|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"4a4adbbe-29ae-11e8-a8d7-0289437bf331"`|string|
|**type**  <br>*optional*||string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Consent Flow


<a name="consents-consent-id-get"></a>
### GET /Consents/Consent/{id}

#### Description
This API is intended for fetching the information associated with the specific consent. The method will return only consents that have been created by the requester.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|
|**Path**|**id**  <br>*required*|The id of the Consent Artefact to retrieve|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[ConsentArtefact](#consentartefact)|


#### Produces

* `application/xml`


#### Tags

* Consent Flow


#### Security

|Type|Name|
|---|---|
|**apiKey**|**[FIP_api_key](#fip_api_key)**|
|**apiKey**|**[FIU_api_key](#fiu_api_key)**|


<a name="consents-consentshandle-get"></a>
### GET /Consents/{consentsHandle}

#### Description
This API is intended for checking the status of a previously submitted Consent Artefacts creation request


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|Header containig an access token to validate the user authorization.|string|
|**Path**|**consentsHandle**  <br>*required*|The id of the consent to retrive|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[Response 200](#consents-consentshandle-get-response-200)|
|**400**|The request was formatted incorrectly|[Response 400](#consents-consentshandle-get-response-400)|
|**404**|The request was formatted incorrectly|[Response 404](#consents-consentshandle-get-response-404)|

<a name="consents-consentshandle-get-response-200"></a>
**Response 200**

|Name|Description|Schema|
|---|---|---|
|**Consents**  <br>*optional*||[Consents](#consents)|
|**ConsentsHandle**  <br>*optional*||string|
|**Status**  <br>*optional*||string|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"795038d3-86fb-4d3a-a681-2d39e8f4fc3c"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="consents-consentshandle-get-response-400"></a>
**Response 400**

|Name|Description|Schema|
|---|---|---|
|**ConsentsHandle**  <br>*optional*||string|
|**Error**  <br>*optional*||[Error](#error)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"795038d3-86fb-4d3a-a681-2d39e8f4fc3c"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="consents-consentshandle-get-response-404"></a>
**Response 404**

|Name|Description|Schema|
|---|---|---|
|**ConsentsHandle**  <br>*optional*||string|
|**Error**  <br>*optional*||[Error](#error)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"795038d3-86fb-4d3a-a681-2d39e8f4fc3c"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Consent Flow


<a name="fi-fetch-sessionid-get"></a>
### GET /FI/fetch/{sessionId}

#### Description
Once FIU receives the notification <Data-Ready> from AA, using digitally signed consent request received from AA, this API can be used to fetch the financial information from the AA.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|
|**Path**|**sessionId**  <br>*required*||string|
|**Body**|**Deep**  <br>*optional*|Request strecture for creating consent artefacts.|[Deep](#fi-fetch-sessionid-get-deep)|

<a name="fi-fetch-sessionid-get-deep"></a>
**Deep**

|Name|Description|Schema|
|---|---|---|
|**DataConsumer**  <br>*optional*||[DataConsumer](#dataconsumer)|
|**FIRequestParams**  <br>*optional*||< [FIRequestParams](#fi-fetch-sessionid-get-firequestparams) > array|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"3dd436f8-0747-4a8f-9001-375e419430be"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-fetch-sessionid-get-firequestparams"></a>
**FIRequestParams**

|Name|Schema|
|---|---|
|**Consent**  <br>*optional*|[Consent](#consent)|
|**KeyMaterials**  <br>*optional*|< [KeyMaterial](#keymaterial) > array|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[Response 200](#fi-fetch-sessionid-get-response-200)|
|**202**|The sessionId was found but the FI is not yet ready or AA failed to fetch FI data|[Response 202](#fi-fetch-sessionid-get-response-202)|
|**400**|The request was formatted incorrectly|[Response 400](#fi-fetch-sessionid-get-response-400)|
|**404**|sessionId was not found|[Response 404](#fi-fetch-sessionid-get-response-404)|

<a name="fi-fetch-sessionid-get-response-200"></a>
**Response 200**

|Name|Description|Schema|
|---|---|---|
|**FI**  <br>*optional*||< [FI](#fi-fetch-sessionid-get-fi) > array|
|**KeyMaterial**  <br>*optional*||[KeyMaterial](#keymaterial)|
|**Session**  <br>*optional*||[Session](#session)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"3dd436f8-0747-4a8f-9001-375e419430be"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-fetch-sessionid-get-fi"></a>
**FI**

|Name|Schema|
|---|---|
|**Account**  <br>*optional*|[Account](#account)|
|**EncryptedData**  <br>*optional*|string (binary)|

<a name="fi-fetch-sessionid-get-response-202"></a>
**Response 202**

|Name|Description|Schema|
|---|---|---|
|**Error**  <br>*optional*||[Error](#error)|
|**Session**  <br>*required*||[Session](#session)|
|**Status**  <br>*required*||enum (PENDING, FAILED)|
|**timestamp**  <br>*required*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"3dd436f8-0747-4a8f-9001-375e419430be"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-fetch-sessionid-get-response-400"></a>
**Response 400**

|Name|Description|Schema|
|---|---|---|
|**Error**  <br>*optional*||[Error](#error)|
|**Session**  <br>*optional*||[Session](#session)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"3dd436f8-0747-4a8f-9001-375e419430be"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-fetch-sessionid-get-response-404"></a>
**Response 404**

|Name|Description|Schema|
|---|---|---|
|**Error**  <br>*optional*||[Error](#error)|
|**Session**  <br>*optional*||[Session](#session)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"3dd436f8-0747-4a8f-9001-375e419430be"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Data Flow


<a name="fi-request-post"></a>
### POST /FI/request

#### Description
The FIU or the User submits the Consent IDs of the consents required for fetching financial information from the FIP(s). A set of sessionIds are generated and returned. These SessionIDs enable the FIU or the User to fetch the information from the AA once available.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|
|**Body**|**Deep**  <br>*optional*||[Deep](#fi-request-post-deep)|

<a name="fi-request-post-deep"></a>
**Deep**

|Name|Description|Schema|
|---|---|---|
|**DataConsumer**  <br>*optional*||[DataConsumer](#dataconsumer)|
|**FIRequestParams**  <br>*optional*||< [FIRequestParams](#fi-request-post-firequestparams) > array|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"e8cc6822-d4bb-4eb1-9e1b-4996fbff8acb"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-request-post-firequestparams"></a>
**FIRequestParams**

|Name|Schema|
|---|---|
|**Consent**  <br>*optional*|[Consent](#consent)|
|**KeyMaterials**  <br>*optional*|< [KeyMaterial](#keymaterial) > array|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[Response 200](#fi-request-post-response-200)|

<a name="fi-request-post-response-200"></a>
**Response 200**

|Name|Description|Schema|
|---|---|---|
|**ScheduledFIRequests**  <br>*optional*||[ScheduledFIRequests](#fi-request-post-scheduledfirequests)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"e8cc6822-d4bb-4eb1-9e1b-4996fbff8acb"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="fi-request-post-scheduledfirequests"></a>
**ScheduledFIRequests**

|Name|Schema|
|---|---|
|**ScheduledFIRequest**  <br>*optional*|< [ScheduledFIRequest](#fi-request-post-scheduledfirequests-scheduledfirequest) > array|

<a name="fi-request-post-scheduledfirequests-scheduledfirequest"></a>
**ScheduledFIRequest**

|Name|Schema|
|---|---|
|**Consent**  <br>*optional*|[Consent](#consent)|
|**Sessions**  <br>*optional*|< [Session](#session) > array|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Data Flow


<a name="heartbeat-get"></a>
### GET /Heartbeat

#### Description
This API is used by FIUs to check availability of AAs


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|[Response 200](#heartbeat-get-response-200)|

<a name="heartbeat-get-response-200"></a>
**Response 200**

|Name|Description|Schema|
|---|---|---|
|**Error**  <br>*optional*||[Error](#error)|
|**Status**  <br>*optional*||enum (UP, DOWN)|
|**timestamp**  <br>*optional*||string (date-time)|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Monitoring


<a name="notification-post"></a>
### POST /Notification

#### Description
API handles the notifications corresponding to the events generated during consent flow and data flow i.e. CONSENT-REVOKED|CONSENT-PAUSED|CONSENT-EXPIRED|FI-REQUESTED|FI-READY|FI-DENIED


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**x-access-token**  <br>*required*|An access token for the user authentication.|string|
|**Body**|**Deep**  <br>*optional*||[Notification](#notification)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|OK|No Content|


#### Consumes

* `application/xml`


#### Produces

* `application/xml`


#### Tags

* Notifications


#### Security

|Type|Name|
|---|---|
|**apiKey**|**[FIU_api_key](#fiu_api_key)**|
|**apiKey**|**[FIP_api_key](#fip_api_key)**|




<a name="definitions"></a>
## Definitions

<a name="account"></a>
### Account

|Name|Description|Schema|
|---|---|---|
|**FIPId**  <br>*optional*|Identifier of the requesting FIP|string|
|**accountId**  <br>*optional*||string|
|**customerId**  <br>*optional*||string|
|**issuer**  <br>*optional*||string|
|**type**  <br>*optional*||enum (savings, current, FD)|
|**value**  <br>*optional*||string|


<a name="consent"></a>
### Consent
contains base 64 encoded string as text child node


|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*|The consent artefact identifier  <br>**Example** : `"654024c8-29c8-11e8-8868-0289437bf331"`|string|
|**status**  <br>*optional*||string|


<a name="consentartefact"></a>
### ConsentArtefact

|Name|Description|Schema|
|---|---|---|
|**AA**  <br>*optional*||[AA](#consentartefact-aa)|
|**Account**  <br>*optional*||[Account](#consentartefact-account)|
|**ConsentUse**  <br>*optional*||[ConsentUse](#consentartefact-consentuse)|
|**Data**  <br>*optional*||< [Data](#data) > array|
|**DataAccess**  <br>*optional*||[DataAccess](#consentartefact-dataaccess)|
|**FIP**  <br>*optional*|The Data Provider is an entity that provides financial information based on Customer’s consent. The FIP in the system acts as the Data Provider.|[FIP](#consentartefact-fip)|
|**Purpose**  <br>*optional*||[Purpose](#purpose)|
|**Signature**  <br>*optional*|Signature of AA as defined in W3C standards; Base64 encoded  <br>**Example** : `"Signature of AA as defined in W3C standards; Base64 encoded"`|string|
|**User**  <br>*optional*||[User](#consentartefact-user)|
|**createTime**  <br>*optional*||string (date-time)|
|**expireTime**  <br>*optional*||string (date-time)|
|**id**  <br>*optional*||string|
|**startTime**  <br>*optional*||string (date-time)|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="consentartefact-aa"></a>
**AA**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|

<a name="consentartefact-account"></a>
**Account**

|Name|Schema|
|---|---|
|**accountType**  <br>*optional*|string|
|**id**  <br>*optional*|string|

<a name="consentartefact-consentuse"></a>
**ConsentUse**

|Name|Schema|
|---|---|
|**logUri**  <br>*optional*|string|

<a name="consentartefact-dataaccess"></a>
**DataAccess**

|Name|Schema|
|---|---|
|**logUri**  <br>*optional*|string|

<a name="consentartefact-fip"></a>
**FIP**

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*|Identifier of the requesting FIP|string|

<a name="consentartefact-user"></a>
**User**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|
|**idType**  <br>*optional*|string|


<a name="consents"></a>
### Consents
*Type* : < [Consent](#consent) > array


<a name="consentsrequest"></a>
### ConsentsRequest
This call generated via AA client.


|Name|Description|Schema|
|---|---|---|
|**AA**  <br>*required*|Identifier of the requesting AA|[AA](#consentsrequest-aa)|
|**Account**  <br>*optional*|Following element captures Customer account at FIP|[Account](#consentsrequest-account)|
|**ConsentUse**  <br>*optional*||[ConsentUse](#consentsrequest-consentuse)|
|**Customer**  <br>*optional*||< [Customer](#consentsrequest-customer) > array|
|**Data-Items**  <br>*optional*|Data-Items defines the kind of FI data for which consent is being sought.|< [Data](#data) > array|
|**DataAccess**  <br>*optional*||[DataAccess](#consentsrequest-dataaccess)|
|**FIP**  <br>*required*|The Data Provider is an entity that provides financial information based on Customer’s consent. The FIP in the system acts as the Data Provider.|[FIP](#consentsrequest-fip)|
|**FIU**  <br>*optional*|Optional field to specify the identifier of the requesting FIU|[FIU](#consentsrequest-fiu)|
|**Purpose**  <br>*required*||[Purpose](#purpose)|
|**Signature**  <br>*optional*|Signature of AA as defined in W3C standards; Base64 encoded  <br>**Pattern** : `"^(?:[A-Za-z0-9+/]{4})*(?:[A-Za-z0-9+/]{2}==\|[A-Za-z0-9+/]{3}=)?$"`|string (byte)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*required*|The transaction identifier provides an end to end traceability. The AA should use this transaction identifier in the responses and notifications for FIU to correlate response with the request. The transaction identifier will be a UUID generated string.  <br>**Example** : `"4a4adbbe-29ae-11e8-a8d7-0289437bf331"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="consentsrequest-aa"></a>
**AA**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|

<a name="consentsrequest-account"></a>
**Account**

|Name|Schema|
|---|---|
|**accountTypes**  <br>*optional*|string|
|**id**  <br>*optional*|string|

<a name="consentsrequest-consentuse"></a>
**ConsentUse**

|Name|Schema|
|---|---|
|**logUri**  <br>*optional*|string|

<a name="consentsrequest-customer"></a>
**Customer**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|
|**idTypes**  <br>*optional*|string|

<a name="consentsrequest-dataaccess"></a>
**DataAccess**

|Name|Schema|
|---|---|
|**logUri**  <br>*optional*|string|

<a name="consentsrequest-fip"></a>
**FIP**

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*|Identifier of the requesting FIP|string|

<a name="consentsrequest-fiu"></a>
**FIU**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|


<a name="consentsresponse"></a>
### ConsentsResponse
This is a response to the RequestConsent API call


|Name|Description|Schema|
|---|---|---|
|**ConsentsHandle**  <br>*optional*||string|
|**Error**  <br>*optional*||[Error](#error)|
|**timestamp**  <br>*required*||string (date-time)|
|**txnid**  <br>*required*|The transaction identifier that was sent in the request is echoed back to the client  <br>**Example** : `"4a4adbbe-29ae-11e8-a8d7-0289437bf331"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|


<a name="data"></a>
### Data

|Name|Description|Schema|
|---|---|---|
|**Access**  <br>*optional*||[Access](#data-access)|
|**DataLife**  <br>*optional*|How long can the information requester store data|[DataLife](#data-datalife)|
|**Datafilter**  <br>*optional*|Data access filter, any encoded query string as per financial information provider API needs|object|
|**Duration**  <br>*optional*|The duration for which information is requested|[Duration](#data-duration)|
|**Frequency**  <br>*optional*|Frequency and number of repeats for periodic information access|[Frequency](#data-frequency)|
|**id**  <br>*optional*||string|
|**type**  <br>*optional*||enum (TRANSACTIONAL, PROFILE)|

<a name="data-access"></a>
**Access**

|Name|Description|Schema|
|---|---|---|
|**mode**  <br>*optional*|**Example** : `"STORE"`|enum (VIEW, STORE, QUERY, STREAM)|

<a name="data-datalife"></a>
**DataLife**

|Name|Schema|
|---|---|
|**unit**  <br>*optional*|enum (MONTH, YEAR, DATE, INF)|
|**value**  <br>*optional*|string|

<a name="data-duration"></a>
**Duration**

|Name|Schema|
|---|---|
|**unit**  <br>*optional*|enum (MONTH, YEAR, DATE, INF)|
|**value**  <br>*optional*|string|

<a name="data-frequency"></a>
**Frequency**

|Name|Schema|
|---|---|
|**unit**  <br>*optional*|enum (DAILY, MONTHLY, YEARLY)|


<a name="dataconsumer"></a>
### DataConsumer
The Data Consumer is an entity that can request, retrieve and store financial information as per the constraints defined in the Customer’s consent.  In most scenarios, the FIU acts as the Data Consumer.


|Name|Schema|
|---|---|
|**callbackURL**  <br>*optional*|string|
|**id**  <br>*required*|string|
|**type**  <br>*required*|enum (FIU, USER)|


<a name="error"></a>
### Error

|Name|Schema|
|---|---|
|**errCode**  <br>*optional*|integer|
|**errMsg**  <br>*optional*|string|


<a name="keymaterial"></a>
### KeyMaterial

|Name|Schema|
|---|---|
|**DHPublicKey**  <br>*optional*|[DHPublicKey](#keymaterial-dhpublickey)|
|**Nonce**  <br>*optional*|integer|
|**Session**  <br>*optional*|[Session](#session)|
|**Signature**  <br>*optional*|object|

<a name="keymaterial-dhpublickey"></a>
**DHPublicKey**

|Name|Schema|
|---|---|
|**KeyValue**  <br>*optional*|string|
|**Parameters**  <br>*optional*|string|
|**expiry**  <br>*optional*|string (date-time)|


<a name="notification"></a>
### Notification

|Name|Description|Schema|
|---|---|---|
|**Consent**  <br>*optional*||[Consent](#consent)|
|**Event**  <br>*optional*||[Event](#notification-event)|
|**From**  <br>*optional*||[From](#notification-from)|
|**Payload**  <br>*optional*||string|
|**To**  <br>*optional*||[To](#notification-to)|
|**timestamp**  <br>*optional*||string (date-time)|
|**txnid**  <br>*optional*|**Example** : `"0b811819-9044-4856-b0ee-8c88035f8858"`|string|
|**ver**  <br>*optional*|**Example** : `"1.0"`|string|

<a name="notification-event"></a>
**Event**

|Name|Schema|
|---|---|
|**type**  <br>*optional*|enum (FI-REQUESTED, FI-READY, FI-DENIED, CONSENT-REVOKED, CONSENT-PAUSED, CONSENT-EXPIRED)|

<a name="notification-from"></a>
**From**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**value**  <br>*optional*|string|

<a name="notification-to"></a>
**To**

|Name|Schema|
|---|---|
|**id**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**value**  <br>*optional*|string|


<a name="purpose"></a>
### Purpose
Purpose captures the reason for which this consent is being sought


|Name|Schema|
|---|---|
|**Category**  <br>*optional*|[Category](#purpose-category)|
|**code**  <br>*required*|string|
|**defUri**  <br>*required*|string|
|**refUri**  <br>*required*|string|
|**text**  <br>*optional*|string|

<a name="purpose-category"></a>
**Category**

|Name|Schema|
|---|---|
|**type**  <br>*optional*|string|


<a name="session"></a>
### Session

|Name|Schema|
|---|---|
|**sessionId**  <br>*optional*|integer|
|**startTime**  <br>*optional*|string (date-time)|




<a name="securityscheme"></a>
## Security

<a name="fiu_api_key"></a>
### FIU_api_key
*Type* : apiKey  
*Name* : fiu_api_key  
*In* : HEADER


<a name="fip_api_key"></a>
### FIP_api_key
*Type* : apiKey  
*Name* : fip_api_key  
*In* : HEADER


