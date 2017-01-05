## Create user accounts

You can create a new account for the user based on the [hashed persistent identifier](#glossary-hashed-PID) and a subset of attributes from the [matching dataset](#glossary-matching-dataset). It's optional to set up your matching service to support the creation of new accounts.

> **Important:** Before creating a user account you must request explicit user consent.

1.  Make sure your Matching Service Adapter exposes its user account
    creation URI endpoint. This is the fully qualified URI to which the
    hub makes unknown user attribute queries (see step 4 in the
    [diagram showing user account creation](#create-user-accounts-diagram). The
    Matching Service Adapter then POSTs the following JSON to the local
    matching service's user account creation URI endpoint:
    
    ```
    "hashedPid": "<a string value>",
    "levelOfAssurance": "<the level of assurance, for example, LEVEL_1>"
    ``` 
    
    You must specify the Matching Service Adapter's user account creation
    URI endpoint on the form you fill in when you
    [request access to an environment](#request-access-to-environments). This form is
    available from your engagement lead.

1.  Make sure the local matching service's user account creation URI endpoint returns the following JSON, choosing success or failure as appropriate:

    ```
    "result": "success | failure"
    ```
 
    > **Note:** As shown in the above example, `success` and `failure` must be in
    > lower case.

1.  Configure your Matching Service Adapter to create new user accounts.
    To do this, supply the local matching service's user account
    creation URI for the attribute `unknownUserCreationServiceUri:` in
    the configuration file. See [Configure the Matching Service Adapter](#configure-the-matching-service-adapter) for more information.

    <a name="list-attributes"></a>

1.  On the '[request access to an environment](#request-access-to-environments)' form,
    enter the list of attributes you want the hub to return to your
    service when creating new user accounts. The options are:

    ```
    FIRST_NAME
    MIDDLE_NAME
    MIDDLE_NAME_VERIFIED
    SURNAME
    SURNAME_VERIFIED
    DATE_OF_BIRTH
    DATE_OF_BIRTH_VERIFIED
    CURRENT_ADDRESS
    CURRENT_ADDRESS_VERIFIED
    CYCLE_3
    ```

    > **Note:** You cannot use historical data to create user accounts.

### Create user accounts: message flow

<a name="create-user-accounts-diagram"></a>

This diagram shows the message flow for creating user accounts. The
numbers identify each stage in the flow. See below for explanations.

![Diagram showing user account creation. The Matching Service Adapter converts between SAML and JSON. The Matching Service Adapter contains a fully qualified URL to which the hub makes unknown user attribute query requests. The text below the image describes the steps](/documentation/ms/accountcreationGraphic.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [SAML message flow](#saml-flow-diagram)
* [matching cycles](#matching-cycles-diagram)

> **Note:** In this example, all 3 [matching cycles](#matching-cycles) failed to
> find a match for the user in the government service records. See the
> [diagram for matching cycles](#matching-cycles-diagram).

1.  Your local matching service sends a `no-match` response to the
    Matching Service Adapter.
1.  The Matching Service Adapter forwards the `no-match` response to the
    GOV.UK Verify hub.
1.  The GOV.UK Verify hub:
      * checks that your matching service supports the creation of new user accounts
      * identifies the attributes you previously specified as necessary to create a new user account

1.  If your service supports the creation of new user accounts, the hub
    sends an unknown user attribute query to the Matching Service
    Adapter. It contains the:
     * [hashed persistent identifier](#glossary-hashed-PID)
     * [matching dataset](#glossary-matching-dataset) 
     * list of attributes to extract from the matching dataset

1.  The Matching Service Adapter extracts the required attributes from
    the matching dataset (if available).
1.  The Matching Service Adapter sends the extracted attributes and the
    hashed persistent identifier to your service via the GOV.UK Verify
    hub.

    > **Note:** The Matching Service Adapter must send this data via the GOV.UK
    > Verify hub, to respect the following identity assurance principles:
    
    > * user control - the user must give their informed consent to the information being sent
    > * data minimisation – the service receives only the restricted set of attributes it needs, not the full matching dataset.
    
    > For more information see the [Identity Assurance Principles](https://www.gov.uk/government/consultations/draft-identity-assurance-principles/privacy-and-consumer-advisory-group-draft-identity-assurance-principles#the-nine-identity-assurance-principles).

1.  Optionally, the government service creates a correlation between the
    matching dataset and the hashed persistent identifier in its matching
    datastore. This will be used for matching cycle 0 when the same user
    returns.
1.  Optionally, the government service creates a new account based on
    the attributes extracted from the matching dataset.

    > **Important:** If you create a new account you must gain consent from the user first.
