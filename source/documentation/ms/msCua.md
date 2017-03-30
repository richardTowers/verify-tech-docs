## Create user accounts

You can create a new account for the user based on the [hashed persistent identifier](#glossary-hashed-PID) and a subset of attributes from the [matching dataset](#glossary-matching-dataset). It's optional to set up your matching service to support the creation of new accounts.

> **Important:** Before creating a user account you must request explicit user consent.

### Prerequisites


1.  Configure your MSA to create new user accounts. To do this, supply the local matching service's account
    creation URI in `accountCreationUrl:` in
    the [YAML configuration file](#configure-the-matching-service-adapter).

    <a name="list-attributes"></a>

1. On the '[request access to an environment](#request-access-to-environments)' form,
    enter the fully qualified URL to which the hub will make unknown user attribute query requests.


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

    > **Note:** You can't use historical data to create user accounts. For example, you can't use an out of date address to create a new user account. 



### Message flow

<a name="create-user-accounts-diagram"></a>

This diagram shows the message flow for creating user accounts. In this example, the service has been set up to create user accounts when matching fails. 

![Diagram showing user account creation. The Matching Service Adapter converts between SAML and JSON. The Matching Service Adapter contains a fully qualified URL to which the hub makes unknown user attribute query requests. The text below the image describes the steps](/documentation/ms/createanaccount.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [SAML message flow](#saml-flow-diagram)
* [matching cycles](#matching-cycles-diagram)

> **Note:** In this example, all 3 [matching cycles](#matching-cycles) previously failed to
> find a match for the user in the government service records. See the
> [diagram for matching cycles](#matching-cycles-diagram).

1.  Your local matching service sends a `no-match` response to the hub via the MSA.
1.  The GOV.UK Verify hub:
   * checks that your matching service supports the creation of user accounts 
   * identifies the attributes you previously said your service needs to create a user account
1. If your service supports the creation of user accounts, the hub sends a query to the MSA. It contains the:
   * [matching dataset](#glossary-local-matching-datastore)
   * [hashed PID](#glossary-hashed-PID)
   * [level of assurance](#glossary-level-of-assurance)
   * list of attributes to extract from the matching dataset
1. The MSA POSTs the following JSON to the local matching service's account creation URI endpoint:

    ```
    "hashedPid": "<string value>",
    "levelOfAssurance": "<the level of assurance, for example, LEVEL_1>"
    ``` 

1. Optionally, the local matching service stores the hashed PID and level of assurance in the local matching datastore. 

    You'll need to create a correlation between the user account and the hashed PID, so a returning user can match with [cycle 0](#cycle-0-persistent-identifier-match). You can choose to store the hashed PID at this point and create a correlation between the user account and the hashed PID at step 9. Alternatively, you can create the user account, store the hashed PID and set up the correlation at step 9.

1. The local matching service sends a JSON response to the MSA: 


    ```
    { "result": "success" }
    ```
    or

    ```
    { "result": "failure" }
    ```
 
    > **Notes:** 
    
    > * As shown above, `success` and `failure` must be in lower case
    > * A user account isn't created at this point. The final response the hub sends to your service will contain the attributes you need to create a user account.

    The local matching service may return `{ "result": "failure" }` if:
    * the level of assurance in the JSON request sent by the MSA is lower than the level of assurance required by the service
    * there are exceptional circumstances, such as maintenance, when you want to suspend user account creation

1.  The MSA extracts the required attributes from
    the matching dataset.
1.  The MSA sends the extracted attributes and the
    hashed persistent identifier to your service via the GOV.UK Verify
    hub.

    > **Note:** The Matching Service Adapter must send this data via the GOV.UK
    > Verify hub, to respect the following identity assurance principles:
    
    > * user control - the user must give informed consent for their information to be used to create an account; they must also be allowed to check their information before you create the account
    > * data minimisation – the service receives only the restricted set of attributes it needs, not the full matching dataset.
    
    > For more information see the [Identity Assurance Principles](https://www.gov.uk/government/consultations/draft-identity-assurance-principles/privacy-and-consumer-advisory-group-draft-identity-assurance-principles#the-nine-identity-assurance-principles).

1. The government service:
    * creates a user account using the attributes extracted from the matching dataset
    * sets up a correlation between the user account and the user's hashed PID
   
 > **Important:** If you create a new account you must gain consent from the user first.




