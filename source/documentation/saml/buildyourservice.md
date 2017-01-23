## Build your service


### Use extended validation certificates

You must use extended validation (EV) certificates for all public
interactions with your service. When you use EV certificates, the user's
web browser clearly indicates the name of the organisation they are
interacting with.

Using EV certificates is one of a range of measures to protect your
users from sites pretending to be you. See best practices for [using TLS
to protect
data](https://www.ncsc.gov.uk/guidance/tls-external-facing-services)
from the National Cyber Security Centre (NCSC).

### SAML integration

#### Choose a product and framework

Your government service must be able to send SAML authentication
requests to, and receive SAML responses from, the GOV.UK Verify hub.

You can choose from a wide range of off-the-shelf products and
frameworks to integrate your service with GOV.UK Verify.

GOV.UK Verify has experience integrating and supporting services using
Shibboleth-SP (Service Provider) and OpenSAML libraries. This has worked
well for teams using Java. However, please use the most appropriate
products and frameworks for your team. See [Choosing
technology](https://www.gov.uk/service-manual/making-software/choosing-technology.html)
in the Government Service Design Manual.

Contact your engagement lead if you need further information.

#### Connect your service to the Matching Service Adapter metadata

To integrate GOV.UK Verify into your service, you need to connect your service to the Matching Service Adapter metadata.

The Matching Service Adapter metadata:

* indicates the URL where your service will send users with their authentication requests - this is the GOV.UK Verify hub
* lists one or more Matching Service Adapter signing certificates, so your service can validate messages that are sent by your Matching Service Adapter
* describes the Matching Service Adapter as an identity provider

The Matching Service Adapter metadata is located in your Matching Service Adapter at /matching-service/SAML2/metadata

Your service must poll the Matching Service Adapter metadata every 10 minutes.

##### Handle responses using SAML metadata

To consume SAML responses that are issued by the GOV.UK Verify hub:

1. Decrypt the assertion.
1. Validate the signature on the assertion contained in the response. This assertion is generated and signed by your Matching Service Adapter.

    You can use the X509 signing certificate contained in the Matching
    Service Adapter metadata to validate this signature.

> **Caution:** Use this procedure with care. You must trust assertions signed by the
> Matching Service Adapter only. The GOV.Verify hub never issues
> assertions for consumption by the service endpoint, so make sure that
> it’s *not* possible to trust the hub to issue assertions.
