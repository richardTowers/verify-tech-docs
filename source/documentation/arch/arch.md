# Architecture


GOV.UK Verify lets users prove their identity online so they can access
government services. When a user wants to access a government service, a
trusted identity provider performs the checks needed to prove the
user’s identity.

<a name="architecture-diagram"></a>

Below is a high-level diagram of the GOV.UK Verify architecture.

![Diagram showing GOV.UK Verify architecture. The hub is at the centre. It is connected to the government service, identity providers, and the Matching Service Adapter. Communication with the hub is via SAML with PKI. The Matching Service Adapter is connected to a local matching service, which is connected to a local matching datastore. Communication between these elements is not via SAML. The text below the image describes all elements in the architecture](/documentation/arch/arch_overview.svg)

For more details, see the diagrams:

* [SAML message flow](#saml-flow-diagram)
* [matching cycles](#matching-cycles-diagram)
* [create user accounts](#create-user-accounts-diagram)


The main elements in GOV.UK Verify architecture are listed below.

**GOV.UK Verify hub**

The GOV.UK Verify hub is the infrastructure that manages interactions between users, government services, identity providers, and matching services for the purpose of authenticating a user who wants to use a government service.

The hub is at the centre of the GOV.UK Verify federation, providing a clear divide between identity providers and government services, avoiding a complex many-many integration. 

This diagram shows integration with the GOV.UK Verify hub (left) and many-many integration (right).

![Diagram showing difference between GOV.UK Verify architecture and many-many integration. With GOV.UK Verify, the hub centralises communication between government services and identity providers. With many-many integration, there is no central hub. Each government service connects to each identity provider.](/documentation/arch/manyintegration.svg)

The hub is stateless, which means that it doesn't store any part of the message exchange any longer than a session. Also, it ensures privacy as it doesn't hold any identity data.

The hub ensures that the required [level of assurance](#glossary-level-of-assurance) is observed.

> **Note:**
> GOV.UK Verify currently supports
> [level of assurance 2](#glossary-level-of-assurance-2) only.


**Identity provider**

A private sector organisation paid by government to verify a user’s
identity and assert verified data that identifies them to the
government service.

The organisations are certified as meeting relevant industry security
standards and the [Identity Assurance Principles](https://www.gov.uk/government/consultations/draft-identity-assurance-principles/privacy-and-consumer-advisory-group-draft-identity-assurance-principles#the-nine-identity-assurance-principles) published by the
Cabinet Office and the [National Cyber Security Centre
(NCSC)](https://www.ncsc.gov.uk/).


**Government service**

A transactional government service that needs proof of a person’s identity to complete a transaction, for example:

* view or share your driving licence information (Driver and Vehicle Licensing Agency)
* claim a redundancy payment (Insolvency Service)


**Matching Service**

The function of finding a match between a user’s verified identity and
a record in a government service’s data sources. The matching service
is composed of the Matching Service Adapter and the local matching
service.


**Matching Service Adapter**

The [Matching Service Adapter](#matching-service-adapter) is a software tool provided by
GOV.UK Verify. It simplifies communication between your local matching
service and the GOV.UK Verify hub. The Matching Service Adapter
converts Security Assertion Markup Language (SAML) into JavaScript Object Notation (JSON) and vice versa.


**Local matching service**

A local matching service finds a match between a user’s assured
identity and a record in the government service's data sources, to
allow the user to access the service. Because there’s no unique
identifier for UK citizens, locating the record involves matching user
information (for example name, address, date of birth) against the
service’s records.

**Security Assertion Markup Language (SAML)**

[SAML](#saml) is a data format for exchanging information securely.
All exchanges between the entities in the GOV.UK Verify federation use
SAML but the local matching service managed by the government service
usually uses JSON.

For more information, see the
diagram showing the [SAML message flow](#saml-flow-diagram) within the GOV.UK
Verify federation.

**PKI (Public Key Infrastructure)**

[PKI](#public-key-infrastructure) implements secure electronic transactions between the
entities in the GOV.UK Verify federation.
