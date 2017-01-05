# Local Matching Service

The purpose of a matching service is to find a match between a user’s
verified identity and a record in a government service’s data sources.
It allows the government service to be sure that the Jane Smith trying
to access the service is the same Jane Smith already held on file.

Government services host their matching service within their security
domain. A matching service is composed of the
[Matching Service Adapter](#matching-service-adapter) and a local matching service.


<a name="matching-service-diagram"></a>

![Diagram showing that a matching service is composed of a Matching Service Adapter and a local matching service](/documentation/ms/matchingserviceGraphics.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [matching cycles](#matching-cycles-diagram)
* [create user accounts](#create-user-accounts-diagram)


**Why does GOV.UK Verify use a matching service?**

UK citizens do not have a unique identifier. Government services need to
match other information about the user, for example address or date of
birth, against their data sources.

**What do you need to do?**

As a government service, it's your responsibility to develop and
[build your local matching service](#build-a-local-matching-service). You need to understand
[how a matching service works](#how-a-matching-service-works).

To help build your local matching service, you can use the:

* [example of a JSON request](#json-request) that the Matching Service Adapter posts to your service
* [JSON schema](#json-schema) for a matching request

Some government services don't have experience of matching verified
identities to government data sources. The process can be complex, so if
you need further help with building a local matching service, contact
your engagement lead or the [GOV.UK Verify
team](http://rp-environment-request.ida.digital.cabinet-office.gov.uk/contact_form).

For more information, see [Steps to integrate GOV.UK Verify into your service](#steps-to-integrate-gov-uk-verify-into-your-service).
