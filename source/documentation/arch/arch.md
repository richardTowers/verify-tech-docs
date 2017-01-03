# Architecture


GOV.UK Verify lets users prove their identity online so they can access
government services. When a user wants to access a government service, a
trusted identity provider performs the checks needed to verify the
user’s identity.

<a name="architecture-diagram"></a>

Below is a high-level diagram of the GOV.UK Verify architecture.

![](/documentation/arch/arch_overview.svg)

The main elements of GOV.UK Verify architecture are listed below.

**GOV.UK Verify hub**

The [GOV.UK Verify hub](#what-does-the-gov-uk-verify-hub-do) is the infrastructure that manages interactions between users, government services, identity providers, and matching services for the purpose of authenticating a user who wants to use a government service.

The hub is at the centre of the GOV.UK Verify federation, providing a clear divide between identity providers and government services, avoiding a complex many-many integration. 

This diagram shows integration with the GOV.UK Verify hub (left) and many-many integration (right).

![Diagram showing difference between GOV.UK Verify architecture and many-many integration. With GOV.UK Verify, the hub centralises communication between government services and identity providers. With many-many integration, there is no central hub. Each government service connects to each identity provider.](/documentation/arch/manyintegration.svg)

The hub is stateless, which means that it doesn't store any part of the message exchange any longer than a session. Also, it ensures privacy as it doesn't hold any identity data.

The hub ensures that the required level of assurance \<gloss\_loa\> is observed.

> **Note:**
> GOV.UK Verify currently supports
> level of assurance 2 \<gloss\_loa2\> only.


**Identity provider**

A private sector organisation paid by government to verify a user’s
identity and assert verified data that identifies them to the
government service.

The organisations are certified as meeting relevant industry security
standards and the Identity Assurance Principles published by the
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
converts SAML into JSON (JavaScript Object Notation) and vice versa.


**Local matching service**

A local matching service finds a match between a user’s assured
identity and a record in the government service's data sources, to
allow the user to access the service. Because there’s no unique
identifier for UK citizens, locating the record involves matching user
information (for example name, address, date of birth) against the
service’s records.

**SAML (Security Assertion Markup Language)**

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
