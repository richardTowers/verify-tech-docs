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

#### Use the SAML metadata

The SAML metadata contains the
[SAML encryption and signing certificates](#data-encryption-and-signing) used
within the GOV.UK Verify federation. There are two sets of SAML
metadata.

**Matching Service Adapter metadata**

The Matching Service Adapter metadata contains:

 * one or more Matching Service Adapter signing certificates
 * the URL where the government service will send authentication requests
 * an entity descriptor that defines the role of the Matching Service Adapter as an identity provider

The Matching Service Adapter metadata is located in your Matching Service Adapter at /matching-service/SAML2/metadata

Your service must be able to consume the Matching Service
Adapter metadata. This will tell your service:

 * where to send auth requests to
 * how to trust the assertions that are sent from the Matching Service Adapter

**Federation metadata**

The federation metadata contains:

 * identity provider signing certificates
 * hub signing certificates and encryption certificates

The federation metadata is signed, so the Matching Service Adapter can
check that it is genuine.

The federation metadata is distributed by the Matching Service Adapter.

To use the federation metadata, you need two truststores:
 
 * the medatdata truststore, to check the integrity of the metdadata signing certificate
 * the ??? truststore, to check the integrity of the identity provider and hub certificates contained in the federation metadata

####Handle responses using SAML metadataß

Your service must be able to consume SAML responses that are issued by
the GOV.UK Verify hub. You must use the Matching Service Adapter
metadata to do this.

1.  Validate the signature on the response. This checks that the
    response is correctly signed by the GOV.UK Verify hub (entityID:
    `https://signin.service.gov.uk`).

    You can use the hub X509 signing certificates contained in the
    federation metadata to validate this signature.

1.  Decrypt the assertion.
1.  Validate the signature on the assertion contained in the response.
    This assertion is generated and signed by your Matching Service
    Adapter.

    You can use the X509 signing certificate contained in the Matching
    Service Adapter metadata to validate this signature.

> **Caution:** ßUse this procedure with care. You must trust assertions signed by the
> Matching Service Adapter only. The GOV.Verify hub never issues
> assertions for consumption by the service endpoint, so make sure that
> it’s *not* possible to trust the hub to issue assertions.
