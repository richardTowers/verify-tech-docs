## How SAML works with GOV.UK Verify


SAML messages take the form of requests and responses. Messages can
contain assertions about the user's identity. GOV.UK Verify uses the
following types of assertion:

* identity assertions – contain information about the user
* authentication context assertions – contain information related to how authentication was carried out, for example, the [level of assurance](#glossary-level-of-assurance)
* fraud event assertions – contain identifiers related to an identified fraud detected by the identity provider

The SAML profile defines these assertions. See the
[list of documents defining the SAML profile](#saml-documents).

All SAML messages that pass between the government service, the hub, and
identity providers are sent via the user's browser.

<a name="saml-flow-diagram"></a>

This diagram shows the SAML message flow within the GOV.UK Verify
federation. The numbers identify each stage in the flow. See below for
explanations.

![Diagram showing a SAML message flow in GOV.UK Verify architecture. Communication between government service, the hub, and identity providers is via SAML and passes through the user's browser. Communication between the hub and the Matching Service Adapter is via SAML using SOAP. Communication between the Matching Service Adapter, the local matching service, and the local matching data store is not SAML. The text below the image describes the SAML messages sent between entities in the federation](/documentation/saml/samlFlowGraphic.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [matching cycles](#matching-cycles-diagram)
* [create user accounts](#create-user-accounts-diagram)


1. The government service sends a SAML authentication request to the [GOV.UK Verify hub](#what-does-the-gov-uk-verify-hub-do). The request indicates that a user wants to access the service and needs to prove their identity using GOV.UK Verify.

    ![Diagram showing a SAML authentication request signed by the government service](/documentation/saml/step1Graphic.svg)

1. The GOV.UK Verify hub prompts the user to select an identity provider to authenticate them. The GOV.UK Verify hub forwards the SAML authentication request to the chosen identity provider.

    ![Diagram showing a SAML authentication request signed by the hub](/documentation/saml/step2Graphic.svg)

1. The chosen identity provider authenticates the user based on the required [level of assurance](#glossary-level-of-assurance). The identity provider then sends a SAML response to the GOV.UK Verify hub:

    ![Diagram showing a SAML response signed by the identity provider. It contains an authentication context assertion signed by the identity provider and encrypted for the hub. It also contains an identity assertion signed by the identity provider and encrypted for the hub](/documentation/saml/step3Graphic.svg)
     * SAML response [signed](#signing-certificates) by the identity provider
     * authentication context assertion signed by the identity provider and [encrypted](#encryption-certificates) for the GOV.UK Verify hub –  this asserts that the user's identity is authenticated; it also contains contextual information, including the [level of assurance](#glossary-level-of-assurance)
     * identity assertion signed by the identity provider and encrypted for the GOV.UK Verify hub – this contains the user's [matching dataset](#glossary-matching-dataset) and the [persistent identifier](#glossary-persistent-identifier)

     > **Note:**
     > For more information on the contents of the assertions, see the diagrams for [matching cycles](#matching-cycles-diagram) and [user account creation](#create-user-accounts-diagram).

1. The GOV.UK Verify hub sends a SAML attribute query to the government service’s Matching Service Adapter:

    ![Diagram showing a SAML attribute query signed by the hub. It contains an identity assertion signed by the identity provider and encrypted for the Matching Service Adapter](/documentation/saml/step4Graphic.svg)
    * SAML attribute query signed by the GOV.UK Verify hub
    * identity assertion signed by identity provider and encrypted for the Matching Service Adapter

1. The Matching Service Adapter translates the SAML attribute query into a JSON matching request and forwards it to the service’s local matching service.

    The local matching service tries to match the user with a record in
    the government service’s database.

1. The local matching service returns a JSON response (`match` or `no match`) to the Matching Service Adapter.
1. In the case of a `match` response, the Matching Service Adapter translates the JSON response into a SAML attribute query response and forwards it to the GOV.UK Verify hub:

    ![Diagram showing a SAML attribute query response signed by the Matching Service Adapter. It contains an assertion signed by the Matching Service Adapter and encrypted for the hub](/documentation/saml/step7Graphic.svg)
    * SAML attribute query response signed by the Matching Service Adapter
    * assertion signed by the Matching Service Adapter and encrypted for the GOV.UK Verify hub – this assertion contains the [hashed persistent identifier](#glossary-hashed-PID)

1. The GOV.UK Verify hub sends a SAML response to the service, authenticating the user:

    ![Diagram showing a SAML response signed by the hub. It contains an assertion signed by the Matching Service Adapter and encrypted for the government ](/documentation/saml/step8Graphic.svg)
    * SAML response (optionally) signed by the GOV.UK Verify hub
    * assertion signed by the Matching Service Adapter and encrypted for the government service

1. The government service retrieves the user's record from the datastore. This allows the government service to interact with the user.

