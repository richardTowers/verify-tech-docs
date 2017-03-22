# Matching Service Adapter test

The [Matching Service Adapter](#glossary-MSA) is a software tool supplied
free of charge by GOV.UK Verify. It simplifies communication between
your local matching service and the GOV.UK Verify hub.

A matching service is composed of the Matching Service Adapter and a
[local matching service](#local-matching-service). Government services host their matching
service within their security domain.


<a name="matching-service-diagram"></a>

![Diagram showing that a matching service is composed of a Matching Service Adapter and a local matching service](/documentation/ms/matchingserviceGraphics.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [matching cycles](#matching-cycles-diagram)
* [create user accounts](#create-user-accounts-diagram)

The MSA is a stateless service. It runs on Java 8 using [Dropwizard](http://www.dropwizard.io/).

**Why does GOV.UK Verify use the Matching Service Adapter?**

The hub uses SAML as its
communication method. Government services usually use JSON for their local matching services. A SAML matching
service interface is therefore required. The Matching Service Adapter
converts SAML into JSON and vice versa. It also performs encryption,
decryption, and signing of SAML messages. For more information, see the
diagram showing the [SAML message flow](#saml-flow-diagram) within the GOV.UK
Verify federation.

It can be difficult and expensive to implement a SAML matching service
interface. GOV.UK Verify provides the Matching Service Adapter so you
can concentrate on the business logic and matching rules for your local
matching service.

**What do you need to do?**

We recommend that you use the Matching Service Adapter. If you do not
want to, please contact the [GOV.UK Verify support
team](mailto:idasupport+onboarding@digital.cabinet-office.gov.uk) to discuss
your alternatives.

To use the Matching Service Adapter, you need to:

1. [Build your local matching service](#build-a-local-matching-service) – you can use the [example of the JSON request](#json-request) that the Matching Service Adapter posts to your service and the [JSON schema](#json-request) for a matching request.
2. [Install the Matching Service Adapter](#install-the-matching-service-adapter).
3. [Configure the Matching Service Adapter](#configure-the-matching-service-adapter).

You will then be able to run
[SAML compliance tests between the hub and your matching service](#test-your-matching-service-with-the-saml-compliance-tool).

For more information, see [Steps to integrate GOV.UK Verify into your service](#steps-to-integrate-gov-uk-verify-into-your-service).
