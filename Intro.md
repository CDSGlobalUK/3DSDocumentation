[[_TOC_]]


Getting Started
===============

Prerequisites
=============


To implement the widget its important that the following steps are completed.

Cybersource/Cardinal Setup
--------------------------

*   3DS2.0 has to be turned on in cybersource for the publisher you are working with.
    
*   The MIDS for this publisher must also be setup to use 3DS 2.1 for this publisher with Cardinal.
    
*   A P12 file must be generated from the Cybersource back office against the MID you are using
    
    *   The documenation can be found [here](https://developer.cybersource.com/api/developer-guides/dita-gettingstarted/authentication/createCert.html) on how to generate a P12 file for the client and MID.
        
*   A 3D secure Secret key file also needs to be generated in Cybersource back office.
    
    *   The documentation can be found [here](https://developer.cybersource.com/api/developer-guides/dita-gettingstarted/authentication/createSharedKey.html) on how to generate this.
        

CDS Global Web Order SDK
------------------------

### Downloading the SDK

To make it easier to format the order payload Json string, there is an SDK that you can develop against, this allows you to seriliaze the order from a typed object into a string to send to the Widget. The SDK has validation across the object so that it is easy to populate the order object with the correct data.

To download the SDK, follow these steps.

Go to this Nuget location and follow the instructions on how to download the package into your solution

[https://www.nuget.org/packages/CDSGlobal.Infinity.WebOrderLibrary/](https://www.nuget.org/packages/CDSGlobal.Infinity.WebOrderLibrary/)

### Finding Help Files for the SDK

The SDK help files can be found by following one of these two routes

**Option 1: -**Go to C:\\Users\\username.nuget\\packages\\cdsglobal.infinity.weborderlibrary\\

*   All the versions that you have downloaded will be here, chose the latest version folder
    

**Option 2: -**Once you have install the Nuget package to you project

1.  Go to the Dependencies->Packages folder in the solution explorer
    
2.  Click on the CDSGobal.Infinity.WebOrderLibrary
    
3.  The path to where it is installed will be in the path property, explore to that folder in windows explorer
    
4.  Your visual studio should look like the below

![8523a757-c116-4c21-a597-a936b1a003bd.png](/.attachments/8523a757-c116-4c21-a597-a936b1a003bd-1f589749-72ae-4430-a428-7158208f95f1.png)

![](attachments/1589280791/1610743831.png)

**Option 1 & 2:**\- In the folder you will find a Help folder, enter it and double click on the Index.html file.

Publisher Table Setup
---------------------

*   A populated publisher in the 3D secure database, with the above information populated.
    

|Column Name|Data Type|Example Value|Mandatory|Notes|
|--- |--- |--- |--- |--- |
|PublisherCode|nvarchar(10)|TABL|Yes||
|Issuer|nvarchar(100)|5d04c2cc0e423d0f7cf82a40|Yes|This is a value you will need to get from the Cybersource back office|
|MerchantSSO|nvarchar(100)|5cfeb6f6031e73048c27224f|Yes|This is a value you will need to get from the Cybersource back office|
|ConfirmUrl|nvarchar(2048)|http://paymentssystem/Payment/Confirm|No||
|JWTKey|nvarchar(MAX)|4750a092-2547-40ad-92c9-2694f9374f08|Yes|This is a value you will need to get from the Cybersource back office|
|Audience|nvarchar(2048)|http://somesite.com|Yes|Url of the system using the widget|
|EnableCCA|bit|0|Yes|Currently set to off, but see the below information on what CCA doeshttps://cardinaldocs.atlassian.net/wiki/spaces/CC/pages/196642/Consumer+Authentication|
|MerchantID|nvarchar(100)|tab_gbp_web|Yes|You will need to get this value from Cybersource, but in most cases its the name of the MID|
|Active (currently not used)|bit|1|Yes|Whether the publisher is active in the widget or not|
|KeyFilename|nvarchar(MAX)|tab_gbp_web.p12|Yes|The name of the p12 file that has been created, this file should be placed in the keys folder of the project.|
|Password|nvarchar(MAX)|XXXXXX|Yes|The associated password to the P12 file|
|SendToProduction|bit|0|Yes|Whether to use production end points or staging|
|MerchantKeyId|nvarchar(MAX)|7d8b5b1a-1e9c-4b8a-9677-2c8ede4f9332|Yes|Used for Authoring and capturing the credit card details as well as tokenisations, documenation on how to get this can be found Create a Shared Secret Key for HTTP Signature Authentication|
|MerchantSecretKey|nvarchar(MAX)|yFR7wtrvxuUyf9+YsdOwjBYh7tOU2L7JEhCcZ5ivZQM=|Yes|Used for Authoring and capturing the credit card details as well as tokenisations, documenation on how to get this can be found Here|
|Use3DSecure|bit|1|Yes|Allows the widget to just Auth and capture, it will not attempt to use 3D secure if set to false|
|DoNotCapturePayment|bit|0|Yes|If set to 1 the widget will only Auth the card and return the tokenisied card details for you to capture at a later time|
|CDSSecretKey|nvarchar(MAX)|0c4fd484-c6aa-43b9-8597-03a4502ed8ff|Yes|A secret key that is use to Auth the payment widget Javascript library|

Third Party Table Setup
-----------------------

If you need to allow a Third Party to access various publishers, its best to set them up in the the Third Party table, this allows different Keys for each Third Party against their individual publishers.

**_If a Third party is not supplied, the CDS Keys will default to the ones against the publisher table, but this only allows a one to one relationship, ie one third party can access that publisher and no other third parties can then use that publisher._**

|Column Name|Data Type|Example Value|Mandatory|Notes|
|--- |--- |--- |--- |--- |
|CDSSecretKey|nvarchar(MAX)|0c4fd484-c6aa-43b9-8597-03a4502ed8ff|Yes|A secret key that is use to Auth the payment widget Javascript library|
|Username|nvarchar(MAX)|NUNIT|Yes|This is the username thats needed to access the server side API’s and the webapi, this will be supplied to the Thrid Party, the Key, username and publisher are used to make sure the Third party is only trying to access their allocated publisher|
|Active (currently not used)|bit|1|Yes|Whether the Third Party is active in the widget or not.  If off, they will not be able to Authenicate with the widget|
|Audience|nvarchar(2048)|http://somesite.com or Third party name etc|Yes|Used in Signing of the JWT|


Adding 3D Secure to Your Front End
==================================

Now that we're able to authenticate with Cardinal servers using JWTs, we can start adding the 3D Secure library to your site. 

Step 1: Include the Script
--------------------------

3Dsecure.js is added to your site just like any other client side script - through a script tag. For performance reasons, it's generally a good practice to put script includes after all of your content, but before the closing HTML body tag. This will help prevent the page from blocking other resources like image downloading while downloading scripts. Just be sure that the 3Dsecure script include comes before the script tag containing the rest of the 3Dsecure steps.

3D Secure URL's will change with each environment. Refer to the [3DSecure CDN links](https://cds-global.atlassian.net/wiki/spaces/UP/pages/1589608488/3DSecure.js) for a list of available endpoints.

**Script Include**

<script src="https://sltesting.cdsglobal.co.uk/3dsecure/js/3DSecure.min.js"></script>

Step 2: Configure It
--------------------

There are a number of configuration options for Cardinal Cruise. Using the Cardinal.configure() function allows you to pass a configuration object into Songbird. You should only call this function once per page load. During your development, you will most likely want control over the logging volume from the library. You can do this through the use of Cardinal.configure, as seen below:

Looking for more configuration options?

**Looking for more configuration options?**

Refer to the [configurations section](https://cds-global.atlassian.net/wiki/spaces/UP/pages/1589215394/Configuration) for all available configuration options

**CDSPaymentWidget.configure**

  
```
CDSPaymentWidget.configure({
      publisherCode: "TABL",
      secretKey: "{secretkeygoeshere}",
      userName: "{username}",
      password: "{password}"
  });
```


Step 3: Monitor for Successfully Configuration
----------------------------------------------

See [JWTConfigured Event](https://cds-global.atlassian.net/wiki/spaces/UP/pages/1589608488/3DSecure.js#Events) for the events that you need to Monitor, but using the basic example below means that you know that the Config has been setup correctly and you now have a Token that can be used for further calls.


```
CDSPaymentWidget.on("JWTConfigured", function (event) {
    var tokenToUse = event.detail.data.token;
});
```


Step 4: Encrypt Your Order Payload
----------------------------------

Now you will need to Encrypt your order payload so that its not sent as clear text, to do this you need to call

 CDSPaymentWidget.encrypt(JsonPayload,token)

The best way to do this is by calling this method from the JWTConfirgured Event as such

 
```
CDSPaymentWidget.on("JWTConfigured", function (event) {
      //Encrypt the payload with the token that has come back from the event
      CDSPaymentWidget.encrypt(datatoLoad, event.detail.data.token)
  });
```


Step 5: Monitor for Successfully Encrypted Payload
--------------------------------------------------

See [Encrypted Event](https://cds-global.atlassian.net/wiki/spaces/UP/pages/1589608488/3DSecure.js#PayloadEncrypted) for the events that you need to Monitor, but using the basic example below means that you know that the Config and the encrypt method has been called.

 CDSPaymentWidget.on("PayloadEncrypted", function (event) {
    var encryptedPayload =event.detail.data.data;
    var token = event.detail.data.token;
});

Step 6: Validate the payload
----------------------------

Now that we have setup the widget and the payloads, we can now validate the payload that we are going to send to make sure that it conforms to the required format and data that we are going to send.

to do this it is recommended that you do this from the PayloadEncryptedEvent, such as below

  
```
CDSPaymentWidget.on("PayloadEncrypted", function (event) {
         CDSPaymentWidget.validate(event.detail.data.data, event.detail.data.token);
  });
```


Its best to monitor this event, and then check the returned object for errors, if you receive errors then deal with them in your application. The errors can be de-serialized in to a List of ErrorResponses that is available in the SDK.

If no errors are received you can move on to the next steps.

You can also validate the payload from the SDK before you send it the the javascript library, once you have constructed the WebOrder object, you can call object.Validate(), which will validate the payload in your server code.

Step 7: Load The Encrypted Payload Into The Widget
--------------------------------------------------

Now that we have setup the widget and the payloads, we can now load the Widget into your application, this can be done with one API call.

Before you do this, you need an element on your page that the Widget can be loaded into, the element needs to have a certain Id, see below for an example of this

  <div id="CardEntryUI" width="800" height="800" />

The API call to load the widget into the above element is called load, it takes two parameters, one being the encrypted payload and the other being the token that you have been using in the previous steps.

  `CDSPaymentWidget.load(encryptedPayload, token);`

Its is recommended that you do this from the Payloadvalidated event such as:-

  
```
CDSPaymentWidget.on("PayloadValidated", function (event) {
     if (event.detail.data.errors !== null && event.detail.data.errors!=="\[\]") {
            $('#result').val(event.detail.data.errors);

           //Handle errors here
        }
        else {
            CDSPaymentWidget.load(event.detail.data.data, event.detail.data.token);
        }
  });
```
 

Note:- The library will Inject various JavaScript content and Libraries into your page head as well as injecting the default style sheet or the stylesheet that you have supplied.

The library should not conflict with libraries that you are using, but its its advisable to make sure that there are no conflicts that are unforeseen.

Step 8: Monitor For Completed Payment Process
---------------------------------------------

The widget will take over after step 6, and manage the payment process, you will need to monitor for the process finishing so that you can decide your next steps.

The event to monitor is PaymentComplete, more in depth documentation can be found [here](https://cds-global.atlassian.net/wiki/spaces/UP/pages/1589608488/3DSecure.js#PaymentComplete), but for basic payment complete monitoring you should setup an event such as below.

  
```
CDSPaymentWidget.on("PayloadValidated", function (event) {
      var returneddata = event.detail;
  });
```


If you have supplied a return URL in the configure section, the payment widget will have a “Continue” button appear when the user clicks on this it will follow that URL, but its still important that you interrogate the returned data for any information that you need.
