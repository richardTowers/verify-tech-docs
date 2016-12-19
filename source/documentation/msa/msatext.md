# Matching Service Adapter

The Matching Service Adapter \<gloss\_msa\> is a software tool supplied
free of charge by GOV.UK Verify. It simplifies communication between
your local matching service and the GOV.UK Verify hub \<hub\>.

A matching service is composed of the Matching Service Adapter and a
local matching service \<ms\>. Government services host their matching
service within their security domain.


> >
> > -   GOV.UK architecture \<arch\>
> > -   matching cycles \<ms\_matchcyles\_diagram\>
> > -   user account creation \<ms\_cua\_diagram\>"

**Why does GOV.UK Verify use the Matching Service Adapter?**

The hub uses Security Assertion Markup Language \<saml\> (SAML) as its
communication method. Government services usually use JSON (JavaScript
Object Notation) for their local matching services. A SAML matching
service interface is therefore required. The Matching Service Adapter
converts SAML into JSON and vice versa. It also performs encryption,
decryption, and signing of SAML messages. For more information, see the
diagram showing the SAML message flow \<samlWorks\> within the GOV.UK
Verify federation.

It can be difficult and expensive to implement a SAML matching service
interface. GOV.UK Verify provides the Matching Service Adapter so you
can concentrate on the business logic and matching rules for your local
matching service.

**What do you need to do?**

We recommend that you use the Matching Service Adapter. If you do not
want to, please contact the [GOV.UK Verify support
team](idasupport+onboarding@digital.cabinet-office.gov.uk) to discuss
your alternatives.

To use the Matching Service Adapter, you need to:

1.  Build your local matching service \<msBuild\> – you can use the
    example of the JSON request \<samlCT\_JSONeg\> that the Matching
    Service Adapter posts to your service and the
    JSON schema \<JSONschema\> for a matching request.
2.  Install the Matching Service Adapter \<msa\_install\_msa\>.
3.  Configure the Matching Service Adapter \<ConfigureMSA\>.

You will then be able to run
SAML compliance tests between the hub and your matching service \<samlCThubMSA\>.

For more information, see steps.
