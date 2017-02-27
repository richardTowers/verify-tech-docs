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


### Choose a product and framework

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

### Connect your service to the Matching Service Adapter metadata

To integrate GOV.UK Verify into your service, you need to connect your service to the Matching Service Adapter metadata.

The Matching Service Adapter metadata:

* indicates the URL where your service will send users with their authentication requests - this is the GOV.UK Verify hub
* lists one or more Matching Service Adapter signing certificates, so your service can validate messages that are sent by your Matching Service Adapter
* describes the Matching Service Adapter as an identity provider

The Matching Service Adapter metadata is located in your Matching Service Adapter at /matching-service/SAML2/metadata

Your service must poll the Matching Service Adapter metadata every 10 minutes.

### Send a SAML authentication request to the GOV.UK Verify hub

1. Use your <a href="#choose-a-product-and-framework">chosen product and framework</a> to build a SAML authentication request in XML:

    ```
      <?xml version="1.0" encoding="UTF-8"?>
      <saml2p:AuthnRequest ...>
        <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" Format="urn:oasis:names:tc:SAML:2.0:nameid-format:entity">http://www.test-rp.gov.uk/SAML2/MD</saml2:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <ds:Reference URI="#_60f75dc5-f9eb-43cf-adfc-5814016a626c">
              <ds:Transforms>
                <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
              <ds:DigestValue>O+LkTbydEWNPSLThcblzSqd/BvlGAI0dWwGVgd6ixkE=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
      O8x8ILlqoiCKg8LMSqlajyX5JhLDxHSltUXYAalGnFb0L41Up5hQuFrEXBNxfNiUo3ChlZA+FIWw
      WkK5OSSqqJQ9IqgUFUapDVZUewerOGLQ/Qw80linrbc24w21JIWDnpoT8qrdt+c9EgkQTvKrwDmf
      JfXUcbTCvuhnOTVrG/5Fv64sruBu9CVTSnvj/Jvy1bwK2HsvMmxrAO8og+iFvMx1KB7YCG1Puj/Z
      frJRKYU3QgAehUR0hrUj1ReVGV4cx1Yy7FhUKnYpdsYRVxpv1McwkDXHVs5iao+0vv7rLGLw9U1d
      a7lBaFhC2AT1wi+ogaO8nzZ/d3G6p0tHrMSqQA==
          </ds:SignatureValue>
        </ds:Signature>
      </saml2p:AuthnRequest>
    ```
1. Send the authentication request to the URL indicated in the element `SingleSignonService` in the MSA metadata.

### Handle responses using SAML metadata

To consume SAML responses that are issued by the GOV.UK Verify hub:

1. Decrypt the assertion.
1. Validate the signature on the assertion contained in the response. This assertion is generated and signed by your Matching Service Adapter.

    You can use the X509 signing certificate contained in the Matching
    Service Adapter metadata to validate this signature.

> **Caution:** Use this procedure with care. You must trust assertions signed by the
> Matching Service Adapter only. The GOV.Verify hub never issues
> assertions for consumption by the service endpoint, so make sure that
> it’s *not* possible to trust the hub to issue assertions.
