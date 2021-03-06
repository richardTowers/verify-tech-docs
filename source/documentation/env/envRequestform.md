## Request access to environments

To access the integration and production environments, fill in the
'Request access to an environment' form. Your GOV.UK Verify engagement
lead provides this form subject to successful gate reviews. You'll
access the integration environment during [Stage
4](http://alphagov.github.io/identity-assurance-documentation/stage4/Stage4.html)
and the production environment during [Stage
5](http://alphagov.github.io/identity-assurance-documentation/stage5/Stage5.html).

> **Note:** Please allow up to 5 working days for the GOV.UK Verify team to set up
> and configure access to an environment.

Before filling in the form, you must
[obtain signed certificates](#request-certificates) from the appropriate
certificate authority.

First select the environment you want to access: integration or
production. If you're requesting access to the production environment,
you need to fill in the entire form. Information from your integration
environment is not carried over.

Fill in the form as indicated below:

1. **Service entityID**

    Enter the entityID for the service you want to connect to the
    integration environment. This is a globally unique identifier for your
    service within the environment. It's typically in URL format, for
    example https://identity.service.gov.uk. If you have more than 1
    service connected to the environment, make sure each has a unique
    entityID.

    > **Note:** This field must be unique for each connection you request for the integration or production environment.

1. **Matching service entityID**
 
    Your matching service is a unique entity within the federation. It 
    therefore needs an entityID, which must be different from your service
    entityID. For example https://identity.service-ms.gov.uk. Several
    services can use the same matching service.

1. **Matching service URL**

    Enter the fully qualified URL to which the hub will make matching
    service requests. For example, https://identity.service-integration.gov.uk/msa.

1. **Service start page URL**

    Enter the fully qualified URL used to access your service. This is 
    usually a page on GOV.UK, for example
    https://www.gov.uk/update-company-car-details.

1. **Assertion consumer service HTTPS URL**

    Enter the fully qualified URL to which the hub will send its
    responses. For example, https://test-rp.service.signin.gov.uk/authentication\_endpoint.

    > **Note:** This field must be unique for each connection you request for the integration or production environment.

1. **Service signature validation certificate**

    Upload a valid X509 certificate signed by either the IDAP test
    certificate authority or the IDAP certificate authority. This
    certificate validates the digital signature present on all messages
    sent from your service. It must correspond to the private key for your
    service. See [Request certificates](#request-certificates).

1. **Matching service signature validation certificate**

    Upload a valid X509 certificate signed by either the IDAP test
    certificate authority or the IDAP certificate authority. This certificate validates the digital signature present on all responses
    sent from the Matching Service Adapter. It must correspond to the
    private key for the Matching Service Adapter. You can retrieve the
    certificate from your Matching Service Adapter by accessing:
    /matching-service/SAML2/metadata. See [Request certificates](#request-certificates).

1. **Service encryption certificate**

    Upload a valid X509 certificate signed by either the IDAP test
    certificate authority or the IDAP certificate authority. The hub uses
    this certificate to encrypt the assertions within the responses that
    it sends to your service. Your service decrypts the assertions using
    your corresponding private key. See [Request certificates](#request-certificates).

1. **Matching service encryption certificate**

    Upload a valid X509 certificate signed by either the IDAP test
    certificate authority or the IDAP certificate authority. The hub uses
    this certificate to encrypt the assertions within the requests that it
    sends to the Matching Service Adapter. The Matching Service Adapter
    decrypts the the assertions using the corresponding private key. You
    can retrieve this certificate from your Matching Service Adapter by
    accessing: /matching-service/SAML2/metadata. See [Request certificates](#request-certificates).

1. **Requested cycle 3 attribute name**

    Enter the name of the additional information you want the hub to send 
    for [matching cycle 3](#cycle-3-additional-information-match), for example, driving licence number. 
    Matching cycle 3 is used if [matching cycle 1](#cycle-1-matching-dataset-match) fails. Make
    sure that your GOV.UK Verify engagement lead pre-approves this field
    as it requires development.

    > **Note:** If you've made a special arrangement with your GOV.UK Verify engagement lead, enter **previously arranged** in this field.

1. **Matching service user account creation URL**

    Enter the fully qualified URL to which the hub will make unknown user
    attribute query requests. This is optional. It is required only if you 
    want to enable [user account creation](#create-user-accounts) in your service.

1. **Attributes for creating user account**

    This applies if you have implemented [user account creation](#create-user-accounts)
    in your service. Select the attributes you want the hub to send back to your service:
    * `FIRST_NAME`
    * `MIDDLE_NAME`
    * `SURNAME`
    * `DATE_OF_BIRTH`
    * `CURRENT_ADDRESS`
    * `CYCLE_3`

    Your web design team can provide the remaining information:

1. **Service display name**

    Enter the service name that appears to the user throughout their hub
    journey. Use a sentence starting with a verb, for example *Sign in to
    your personal tax account*, rather than a title like *Self Assessment
    tax return*.

1. **Other ways to... display name**

    Enter an alternative service name, for example *access your personal 
    tax account*. This appears on the *Other ways to...* content in the 
    hub, which appears when a user can't prove their identity using
    GOV.UK Verify. If you don't provide an alternative service name, the
    GOV.UK Verify team sets it to the service display name by default.

1. **Other ways to complete the transaction**

    Enter valid HTML content, including URLs, that displays on the *Other 
    ways to...* content in the hub. This informs the user of alternative 
    methods of using your service. For example: *If you can’t verify your
    identity using GOV.UK Verify, request Government Gateway user ID and
    password replacements from HMRC. This can take about 10 working days
    (or longer if you live abroad). For help, contact the HMRC Online Self 
    Assessment helpline.*
