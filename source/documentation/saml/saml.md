# SAML

Security Assertion Markup Language \<glosssaml\> (SAML) is an XML-based,
open-standard data format for securely exchanging information about
identities.

SAML can be used as the basis of a system of federated identity. This
allows entities within a federation to refer to a user's electronic
identity, even if it is managed differently by each entity.

**Why does GOV.UK Verify use SAML?**

GOV.UK Verify uses SAML to securely exchange information within a
federated system of identity.

SAML is a mature standard and is widely used by industry. SAML libraries
and components are available for most programming languages and
platforms.

**What do you need to do?**

You must [build a government service](#build-your-service) that sends SAML
authentication requests to, and receives SAML responses from, the GOV.UK
Verify hub. To do this, you need to understand
[how SAML works with GOV.UK Verify](#how-saml-works) and
decide [how to add the SAML funcionality to your service](#saml-integration).

When you build your service, you must
[run SAML compliance tests](#run-saml-compliance-tests).

For more information, see [Steps to integrate GOV.UK Verify in your service](#steps-to-integrate-gov-uk-verify-in-your-service).


<a name="saml-documents"></a>

You must adhere to the following specifications and guidance:

-   '[Identity Assurance Hub Service SAML 2.0
    Profile](https://www.gov.uk/government/publications/identity-assurance-hub-service-saml-20-profile)'
    – describes the SAML specifications you must respect to connect to
    the GOV.UK Verify hub
-   '[Identity Assurance Hub Service Profile – Authentication
    Contexts](https://www.gov.uk/government/publications/identity-assurance-hub-service-profile-authentication-contexts)'
    – describes how the level of assurance is specified
-   '[Identity Assurance Hub Service Profile – SAML
    Attributes](https://www.gov.uk/government/publications/identity-assurance-hub-service-profile-saml-attributes)'
    – describes the matching dataset attributes and fraud event
    assertion attributes
-   '[GOV.UK Verify
    guidance](https://www.gov.uk/service-manual/identity-assurance/index.html)'

The SAML profile used by GOV.UK Verify builds on the [OASIS
documentation for the SAML 2.0
standard](https://www.oasis-open.org/standards#samlv2.0).
