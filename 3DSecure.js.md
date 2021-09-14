[[_TOC_]]


3DSecure.js is a client side JavaScript library that is included on the merchants site to interact with Cardinal Cruise. 

CDN
===

The below URLs can be used to transact with 3DSecure against different environments. Each build of songbird is directly tied to an environment. To change environments you need only change what URL you're using.

|Environment|Url|
|--- |--- |
|Production|https://sl.cdsglobal.co.uk/3dsecure/js/3DSecure.min.js|
|Staging|https://sltesting.cdsglobal.co.uk/3dsecure/js/3DSecure.min.js|


Supported Browsers

>**_Please note that while 3Dsecure uses the below supported list, specific payment brands may have a different list of supported browsers. Please refer to specific payment brand documentation to determine if a particular browser is supported by a particular payment brand. Specific payment brand restrictions are put in place by the payment brand. CDS Global has no say in what browsers they choose to support._**





|Desktop  |Mobile  |
|--|--|
|IE 10+ Edge 13+,  Firefox 42+, Chrome 48+,  Safari 7.1+|Safari 7.1+, Android Browser 4.4+,  Chrome Mobile 48+|





Events
======

Due to the nature of Javscript running in single threaded mode, the CDSpaymentWidget utilises events that you can monitor for when certain actions on the payment widget are complete.

Not all of the events need to be monitored, but it is advisable that you do so that you can capture errors that are bubbled up from the library.

Mandatory Events
----------------

The following assumes that you have configured the widget already, see [Configuration](/3DS-Intro/Configuration) on how to configure the widget.

There are two events associated with the configure methods.

### JWTConfigured

 
```
CDSPaymentWidget.on("JWTConfigured", function (event) {
...............
 });
```


This event fires when all of the information that you have supplied in the configure method is correct and you have been authenticated with the widget to be allowed to use it for the selected publisher.

#### Responses from JWTConfigured

The event will return the token that you should use to communicate with the other API methods, to grab the token you can enquire the event response such as


```
CDSPaymentWidget.on("JWTConfigured", function (event) {
    var tokenToUse = event.detail.data.token;
});
```


This token can then be used for the Encrypt method and Load methods.

**_The token is only valid for 10 minutes, after which time you will need to re-call Configure._**

### JWTConfigureError

 
```
CDSPaymentWidget.on("JWTConfigureError", function (event) {
      alert('Something has gone wrong');
  });
```


This event fires when the configuration data is incorrect or you have supplied the wrong authentication details.

### Responses from Configure Events

#### Thrown Errors

If mandatory fields are missed from the configuration method, a JavaScript error will be thrown which you can capture in your code and handle as necessary, the return errors will be one of the following.

`CDSPaymentWidget - Publisher Code must be supplied in the configure function`

`CDSPaymentWidget - A valid user name must be supplied in the configure function`

`CDSPaymentWidget - A valid password must be supplied in the configure function`

`CDSPaymentWidget - A valid secert key must be supplied in the configure function`

#### Returned Errors

The return errors will be in the variable of the event function, Integrate such as this example

  
```
CDSPaymentWidget.on("JWTConfigureError", function (event) {
      alert('Something has gone wrong' + event.detail.data.token);
  });
```


### PayloadEncrypted

The PayloadEncrypted event will fire once the library has Encrypted the data successfully and returns the encrypted payload and the token you are using.

You cannot use the widget without first making sure that the data in encrypted and that you are using a valid token

 
```
CDSPaymentWidget.on("PayloadEncrypted", function (event) {
    var encryptedPayload =event.detail.data.data;
    var token = event.detail.data.token;
});
```


#### Responses

*   `event.detail.data.data` This will be your payload encrypted into a JWT Token
    
*   `event.detail.data.token`This will be the same token that you generated from the configure
    

If something goes wrong within the Encrypted method you will be returned

*   `event.detail.data.data` A serialised object of the HTTP request with all relevant errors embedded
    
    *   You will need to interrogate the return HTTP response to read the various errors that have returned and then act accordingly on your next steps, ie retry, fail etc.
        
*   `event.detail.data.token`This will be the same token that you generated from the configure
    

Its is recommenced that you compare the token returned to the one that you received from the configure to make sure that no tampering has occurred between the posts.

### PaymentComplete

The event to monitor is PaymentComplete, this event will return various pieces of information, the response is documented below.

  
```
CDSPaymentWidget.on("PaymentComplete", function (event) {
      var returneddata = event.detail;
  });
```


Response

An Example response can be found below


```
{
  "decision": "ACCEPT",
  "publisherCode": "TABL",
  "returnUrl": null,
  "data": {
    "billingCustomerDataProtection": \[
      {
        "CommunicationChannelID": "string",
        "headerText": "string",
        "q1text": "string",
        "q2text": "string",
        "q1checked": true,
        "q2checked": true,
        "SectionId": 0,
        "MappedValue": "string"
      }
    \],
    "QuestionnaireResponses": \[
      {
        "Answer": "string",
        "QuestionName": "string"
      }
    \],
    "QuestionnaireProfileId": "string",
    "QuestionnaireName": "string",
    "PublisherCode": "TABL",
    "Payment": {
      "IsPayPal": false,
      "IsBillMe": true,
      "isContinuous": false,
      "IsDD": true,
      "IsCheck": true,
      "IsCC": true,
      "DirectDebitAmount": 0,
      "RenewalFlag": null,
      "CreditCardAmount": 0,
      "PaymentCountryCode": "string",
      "PaymentAmount": 0,
      "PaymentCurrency": "gbp",
      "PaymentDirectDebit": null,
      "PaymentCreditCard": {
        "CardCustomerName": "string string string string",
        "CardCVV": "xxx",
        "CardNumber": "400000XXXXXX1091",
        "CardEndDate": "202301",
        "CardIssueNumber": null,
        "CardStartDate": "",
        "CardType": "001",
        "CreditCardAmount": 15
      },
      "TransactionId": null,
      "PaymentCreditCardToken": {
        "CardCustomerName": "string string string string",
        "CardToken": "0169615632711091",
        "CAVV": null,
        "CardEndDate": "202301",
        "CardStartDate": "",
        "CardType": "VW",
        "CreditCardAmount": 15,
        "AuthenticationResult": 0,
        "AuthorizationCode": null,
        "AuthorizedDateTime": "0001-01-01T00:00:00",
        "AuthenticationStatusMessage": null,
        "AuthenticationTransactionID": null,
        "aav": null,
        "CAVVAlgorithm": null,
        "CommerceIndicator": null,
        "CollectionIndicator": null,
        "Decision": null,
        "eci": null,
        "eciRaw": null,
        "xid": null,
        "SessionId": null,
        "CardMask": "400000XXXXXX1091"
      },
      "IsTokenized": false
    },
    "OrderType": 0,
    "BillingCustomer": {
      "Title": "string",
      "Surname": "string",
      "ReferenceNumber": "string",
      "OtherNames": "string",
      "Forename": "string",
      "DataProtectionQuestionnaire": {
        "Responses": \[
          {
            "Answer": "string",
            "QuestionName": "string"
          }
        \],
        "Name": "string",
        "ProfileId": "string"
      },
      "DateOfBirth": "2019-12-09T10:08:11.412Z",
      "CustomerAddress": {
        "Address1": "String",
        "Address2": "String",
        "Address3": "String",
        "AddressCode": "String",
        "CommunicationPreference": "string",
        "CompanyName": "String",
        "ContactDetails": \[
          {
            "ContactChannel": "Email",
            "ContactValue": "leesambridge@hotmail.co.uk"
          }
        \],
        "Country": "United Kingdom",
        "CountryCode": "GBR",
        "County": "String",
        "Department": "String",
        "JobTitle": "String",
        "Organisation": "String",
        "PostCode": "STRING",
        "Town": "String",
        "StateCode": ""
      },
      "BillingCurrency": "gbp",
      "AgentRef": "string",
      "CustomerDataProtection": null
    },
    "DateCreated": "2019-12-09T10:08:11.412Z",
    "OrderItems": \[
      {
        "Tax": 0,
        "SubscriptionUpgradeInfo": {
          "EntityToUpgradeTo": "string",
          "EntityToUpgradeFrom": "string",
          "CustomerReference": "string",
          "CancellationOrderReference": "string",
          "NewOrderReference": "string"
        },
        "SubscriptionType": 0,
        "ItemNumber": "string",
        "ItemSKU": {
          "ProductType": 0,
          "PublicationCode": "string",
          "PromotionCode": "string"
        },
        "DeliveryCustomer": {
          "Title": "string",
          "Surname": "string",
          "ReferenceNumber": "string",
          "OtherNames": "string",
          "Forename": "string",
          "DataProtectionQuestionnaire": {
            "Responses": \[
              {
                "Answer": "string",
                "QuestionName": "string"
              }
            \],
            "Name": "string",
            "ProfileId": "string"
          },
          "DateOfBirth": "2019-12-09T10:08:11.412Z",
          "CustomerAddress": {
            "Address1": "String",
            "Address2": "String",
            "Address3": "String",
            "AddressCode": "String",
            "CommunicationPreference": "string",
            "CompanyName": "String",
            "ContactDetails": \[
              {
                "ContactChannel": "string",
                "ContactValue": "string"
              }
            \],
            "Country": "String",
            "CountryCode": "STRING",
            "County": "String",
            "Department": "String",
            "JobTitle": "String",
            "Organisation": "String",
            "PostCode": "STRING",
            "Town": "String",
            "StateCode": ""
          },
          "BillingCurrency": "GBP",
          "AgentRef": "string",
          "CustomerDataProtection": null
        },
        "Cost": 15,
        "RateCode": "string",
        "Currency": "GBP",
        "DeliveryCost": 0,
        "NumberOfIssues": "string",
        "SubscriptionClass": "string",
        "StartIssueDate": "2019-12-09T10:08:11.412Z",
        "QuestionnaireResponses": \[
          {
            "Answer": "string",
            "QuestionName": "string"
          }
        \],
        "QuestionnaireProfileId": "string",
        "QuestionnaireName": "string",
        "Quantity": 0,
        "QualificationSource": "string",
        "PaymentMethod": "CC",
        "PromotionChoiceCode": "string"
      }
    \],
    "ReturnURLFor3DSecure": "asdasda"
  },
  "reasonCode": 100,
  "reasonId": "a05d10b8-04a0-42af-9729-20eb945a4966",
  "continueReply": null,
  "orderReply": null,
  "newPaymentRequired": false,
  "errorMessage": "CyberSource\_ReasonCode\_Success",
  "friendlyErrorMessage": "",
  "carDetailsResponse": {
    "id": "a05d10b8-04a0-42af-9729-20eb945a4966",
    "isSuccess": true,
    "controlId": null,
    "errorMessage": null,
    "errorID": null,
    "authenticationResult": "0",
    "authorizationCode": "831000",
    "authorizedDateTime": "2020-08-05T10:01:46Z",
    "authenticationStatusMessage": "Success",
    "authenticationTransactionID": "195GXBfiMu9oo3ZbiQx0",
    "cavv": "MTIzNDU2Nzg5MDEyMzQ1Njc4OTA=",
    "aav": null,
    "cavvAlgorithm": null,
    "commerceIndicator": "vbv",
    "collectionIndicator": null,
    "decision": "ACCEPT",
    "eci": "05",
    "eciRaw": "05",
    "xid": "MTIzNDU2Nzg5MDEyMzQ1Njc4OTA=",
    "paresStatus": "Y",
    "veresEnrolled": null,
    "requestToken": "Axj77wSTQ23k9JD2WGos/4hRHCOLGWQCojhHFjLLOhd3ANqGTSTL0YrrL3ID5NDbeT0kPZYaiwAA9iW6",
    "requestId": "5966217058756454804012",
    "reasonCode": 100,
    "sessionId": null,
    "cardMask": "400000XXXXXX1091",
    "cardToken": "0169615632711091"
  }
}
```


##### Data Object - Type WebOrder

The first object returned is `data`this is the order that you sent in, in the first place, if you have asked for the order to be loaded directly to advantage, this is just for information as it would have been loaded to advantage, if its not being loaded to advantage, you will need to act appon this data and post to to one of CDS Globals other mechanisms for loading orders.

You can download our SDK to make De-Serialising into this type easier, the SDK can be found here [https://www.nuget.org/packages/CDSGlobal.Infinity.WebOrderLibrary/](https://www.nuget.org/packages/CDSGlobal.Infinity.WebOrderLibrary/)

##### Decision - String

The is the final decision from the payment widget, anything other than “ACCEPT”, means that the payment has failed.

##### Reason Code - Integer

This is the detailed reason code that is returned, anything other that a “100” is an unfulfilled payment, the widget will handle most of the errors that can come back from the payment providers, but their may be cases where this goes wrong, so please react to the error code, the full list of the error codes can be found [here](/3DS-Intro/Error-Codes)

CarDetailsReponse - Object

This object mainly contains acceptance criteria as well as the the information you will need to store the tokenised card in your systems, or to pass back to the CDS Global systems
