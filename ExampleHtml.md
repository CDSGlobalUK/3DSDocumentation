

```
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">

<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>


<script src="https://sltesting.cdsglobal.co.uk/3dsecure/js/3DSecure.min.js"></script>

<div class="text-center">
    <h1 class="display-4">Test Harness</h1>

    <div>
        <button id="fire" type="button">Partial - Fire stand alone</button>
    </div>
    <!--Widget will be injected here-->
    <div id="CardEntryUI" width="800" height="800">

    </div>
</div>
<script>
    //Test Payload, documentation for the payload can be found here
    //https://appstesting.cdsglobal.co.uk/infinity/webAPI/swagger/index.html
    //under the WebOrder Schema
    //Turns JSON in javascript string
    var datatoLoad = JSON.stringify({
        PublisherCode: "TABL",
        Data: {
            billingCustomerDataProtection: [
                {
                    communicationChannelID: "string",
                    headerText: "string",
                    q1text: "string",
                    q2text: "string",
                    q1checked: true,
                    q2checked: true,
                    sectionId: 0,
                    mappedValue: "string"
                }
            ],
            questionnaireResponses: [
                {
                    answer: "string",
                    questionName: "string"
                }
            ],
            questionnaireProfileId: "string",
            questionnaireName: "string",
            publisherCode: "TABL",
            payment: {

                paymentCurrency: "gbp",
                paymentAmount: 0,
                paymentCountryCode: "string",
                isCC: true,
                isCheck: true,
                isDD: true,
                isContinuous: false,
                isBillMe: true
            },
            orderType: "New_Customer",
            billingCustomer: {
                agentRef: "string",
                billingCurrency: "gbp",
                companyName: "string",
                customerAddress: {
                    address1: "string",
                    address2: "string",
                    address3: "string",
                    addressCode: "string",
                    communicationPreference: "string",
                    companyName: "string",
                    contactDetails: [
                        {
                            contactChannel: "Email",
                            contactValue: "leesambridge@hotmail.co.uk"
                        }
                    ],
                    country: "United Kingdom",
                    countryCode: "GBR",
                    county: "string",
                    department: "string",
                    jobTitle: "string",
                    organisation: "string",
                    postCode: "string",
                    town: "string",
                    stateCode: ""
                },
                dateOfBirth: "2019-12-09T10:08:11.412Z",
                dataProtectionQuestionnaire: {
                    responses: [
                        {
                            answer: "string",
                            questionName: "string"
                        }
                    ],
                    name: "string",
                    profileId: "string"
                },
                departmentName: "string",
                errors: [
                    {
                        code: "DBError",
                        errorType: "Unknown",
                        message: "string",
                        propertyName: "string",
                        propertyValue: {}
                    }
                ],
                externalReference: "string",
                foreName: "string",
                jobTitle: "string",
                memberNumber: "string",
                otherNames: "string",
                referenceNumber: "string",
                surName: "string",
                taxRegistrations: [
                    {
                        countryCode: "string",
                        taxRegistrationId: "string",
                        valid: true
                    }
                ],
                title: "string"
            },
            dateCreated: "2019-12-09T10:08:11.412Z",
            orderItems: [
                {
                    tax: 0,
                    subscriptionUpgradeInfo: {
                        entityToUpgradeTo: "string",
                        entityToUpgradeFrom: "string",
                        customerReference: "string",
                        cancellationOrderReference: "string",
                        newOrderReference: "string"
                    },
                    subscriptionType: "Personal",
                    itemNumber: "string",
                    itemSKU: {
                        productType: "Subscription",
                        PublicationCode: "string",
                        promotionCode: "string"
                    },
                    currency: "GBP",
                    deliveryCustomer: {
                        agentRef: "string",
                        billingCurrency: "GBP",
                        companyName: "string",
                        customerAddress: {
                            address1: "string",
                            address2: "string",
                            address3: "string",
                            addressCode: "string",
                            communicationPreference: "string",
                            companyName: "string",
                            contactDetails: [
                                {
                                    contactChannel: "string",
                                    contactValue: "string"
                                }
                            ],
                            country: "string",
                            countryCode: "string",
                            county: "string",
                            department: "string",
                            jobTitle: "string",
                            organisation: "string",
                            postCode: "string",
                            town: "string",
                            stateCode: ""
                        },
                        dateOfBirth: "2019-12-09T10:08:11.412Z",
                        dataProtectionQuestionnaire: {
                            responses: [
                                {
                                    answer: "string",
                                    questionName: "string"
                                }
                            ],
                            name: "string",
                            profileId: "string"
                        },
                        departmentName: "string",
                        errors: [
                            {
                                code: "DBError",
                                errorType: "Unknown",
                                message: "string",
                                propertyName: "string",
                                propertyValue: {}
                            }
                        ],
                        externalReference: "string",
                        foreName: "string",
                        jobTitle: "string",
                        memberNumber: "string",
                        otherNames: "string",
                        referenceNumber: "string",
                        surName: "string",
                        taxRegistrations: [
                            {
                                countryCode: "string",
                                taxRegistrationId: "string",
                                valid: true
                            }
                        ],
                        title: "string"
                    },
                    deliveryCode: "string",
                    cost: 15,
                    copies: "string",
                    renewalFlag: "string",
                    rateCode: "string",
                    deliveryCost: 0,
                    itemTitle: "string",
                    numberOfIssues: "string",
                    subscriptionClass: "string",
                    startIssueDate: "2019-12-09T10:08:11.412Z",
                    questionnaireResponses: [
                        {
                            answer: "string",
                            questionName: "string"
                        }
                    ],
                    questionnaireProfileId: "string",
                    questionnaireName: "string",
                    quantity: 0,
                    qualificationSource: "string",
                    priceType: "string",
                    paymentMethod: "CC",
                    promotionChoiceCode: "string"
                }
            ]
        },
        ReturnUrl: "https://www.google.co.uk",
        PushDirectToAdvantage: false
    });
    $(document).ready(function () {



        function post3dSecureData() {
            //Setup up CDS Widget, use TABL for now.
            //setup options include - all of which are mandatory
            //publisherCode             :   the code of the publisher you are ordering from
            //userName                  :   the user name of the Infinity WebAPi Account, this is needed if you are pushing the order directly to advantage from the widget, ie fire and forget
            //password                  :   the password of the Infinity WebAPi Account
            //secretKey                 :   a secret key for the publisher, this will be supplied to you
            //Optional Parameters
            //argStyleSheet             :   this takes either a styesheet object or a string with all the styles in, this can be used to customise the look and feel of the widget, by
            //                              default it will use the CDS stylesheet
            //pushDirectToAdvantage     :   Defaults to false, setting this as true will attempt to push the order you supplied to advantage, any errors will be return to you to deal with,
            //                              Card errors will be dealt with by the widget.                                            
            
            CDSPaymentWidget.configure({
                publisherCode: "TABL",
                secretKey: "xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
                userName: "xxxxxxxxxxxxx",
                password: "xxxxxxxxxxxxx",
                cancelUrl: "Https://www.google.co.uk",
                returnUrl: "Https://www.google.co.uk",
                payload: datatoLoad 
            });

        };

        //Setup this event in your code to capture the result of the payment widget, we will handle the errors 
        //You will be able to integrate the event.detail object to see the orginal order and the payment token that it brings back, you can then send this on to our
        //Web API at  /api/Order/Load based on the above swagger document, again, this will not currently work, but it allows you to call our widget and test cards etc
        // use this card number to test a 3D secure 4(11 zeros)1091 any valid expiry and a random cvv code,
        //the return will be encrypted which you will need to decode with your key, but for testing this hasn't been turned on yet
        CDSPaymentWidget.on("PaymentComplete", function (event) {
            $('#result').html(JSON.stringify(event.detail));
        });

        //This event will fire when you have call the Encrpyt method and return the encrpyted data with the token, this can be then used to load the payment widget
        CDSPaymentWidget.on("PayloadEncrypted", function (event) {
            CDSPaymentWidget.validate(event.detail.data.data, event.detail.data.token);
        });

        //This event will fire when the configuration has been setup correctly
        CDSPaymentWidget.on("JWTConfigured", function (event) {
            //Check to see if a payload was supplied on configure, if so use that one instead of global variable
            if (event.detail.payload !== undefined) {
                datatoLoad = JSON.stringify( event.detail.payload);
            }
            //Encrypt the payload with the token that has come back from the event
            CDSPaymentWidget.encrypt(datatoLoad, event.detail.data.token)
        });

        //This event will fire when the configuration has been setup incorrectly, ie you have use incorrect settings to initalise the widget
        CDSPaymentWidget.on("JWTConfigureError", function (event) {
            alert('Something has gone wrong');
        });
        
          CDSPaymentWidget.on("PayloadValidated", function (event) {
            if (event.detail.data.errors !== null && event.detail.data.errors!=="[]") {
                $('#result').val(event.detail.data.errors);

                setUpAceEditor();
            }
            else {
                CDSPaymentWidget.load(event.detail.data.data, event.detail.data.token);
            }
        });
        
        //Test Harness to start the widget.
        $("#fire").click('click', function () {
            post3dSecureData();
        });


    });
</script>

<div id="result"></div>
```


