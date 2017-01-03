# GOV.UK Verify environments

An environment is a fully-working deployment of the GOV.UK Verify hub.
GOV.UK Verify maintains a number of environments for development,
testing, and operational purposes. As a government service you need to
work in the integration and production environments.

## What is the integration environment?

The integration environment is a production-sized deployment of the
GOV.UK Verify hub. It's a restricted environment for security reasons.

The purpose of the integration environment is to make sure that your
service operates with the GOV.UK Verify hub. The integration environment
is designed to test full end-to-end flows and to showcase a working
system. It's not a development environment. Each government service
usually has a single connection to the integration environment.

## What is the production environment?

The production environment is where you deploy your live end-to-end
service. It's a restricted environment for security reasons.

## What do you need to do?

Before you access the integration environment, run
[SAML compliance tests](#run-saml-compliance-tests) to make sure that your
service is compliant with the SAML profile. On successful completion of
compliance tests, you'll access the integration environment.

For more information, see [Steps to integrate GOV.UK Verify in your service](#steps-to-integrate-gov-uk-verify-in-your-service).

To access the integration environment:

1.  Send the GOV.UK Verify operations team the IP address of the host
    running the Matching Service Adapter. This allows GOV.UK Verify to
    put in place firewall rules so the hub can communicate with the
    Matching Service Adapter.
1.  Send the GOV.UK Verify operations team the IP addresses of the
    browsers that you will use when testing in the integration
    environment. Access to the hub and test identity providers in the
    integration environment is restricted by IP address.
1.  Obtain signed certificates \<pkiRequestCert\> for the integration
    environment from the appropriate IDAP certificate authority.
1.  Send the GOV.UK Verify operations team your signed certificates for
    the environment.
1.  Provide additional details of your service in the
    '[Request access to an environment](#request-access-to-environments)' form.

In the integration environment, you'll [run
end-to-end testing](#run-end-to-end-testing). On successful completion of
end-to-end testing you'll access the production environment. To access
the production environment use the same procedure as above but skip step
2.
