# Steps to integrate GOV.UK Verify into your service

## Development

Prerequisites:

* <a href="#choose-a-product-and-framework">choose a product and framework</a> to integrate your service with GOV.UK Verify
* decide how to <a href="#store-private-keys">store and manage keys</a> and certificates so you can <a href="#rotate-your-keys">rotate your keys</a> when needed

> **Note:**
> You only need to deploy your code to an environment at the end of the development phase, when you test your matching service with the SAML compliance tool. You can do all development steps until this point on a development machine.

<table>
  <tbody>
    <tr>
      <th>Step</th>
      <th align="center">What you need to do</th>
    </tr>
    <tr>
      <td>1</td>
      <td align="center"><b>Set up the Matching Service Adapter for the SAML compliance tool.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#install-the-matching-service-adapter">Download and install the Matching Service Adapter.</a></li>
          <li><a href="#generate-self-signed-certificates">Generate self-signed certificates for the SAML compliance tool.</a> </li>
          <li><a href="#configure-the-matching-service-adapter">Configure the Matching Service Adapter for the SAML compliance tool.</a> </li>  
        </ol><br>
        Outcome: you can start building your service.<br><br>

        For more information, see <a href="#matching-service-adapter">Matching Service Adapter.</a>.
      </td>
    </tr>
    <tr>
      <td>2</td>
      <td align="center"><b>Build a local matching service.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#define-your-matching-strategy">Define your matching strategy</a> with your service manager.</li>
          <li>Use the <a href="#json-request">example JSON matching request</a> and the <a href="#json-schema">JSON schema</a>  to help build your local matching service.</li>
        </ol><br>
        Outcome: your service can match users’ verified identities to your data sources.<br><br>

        For more information, see <a href="#how-a-matching-service-works">How a matching service works</a>.
      </td>
    </tr>
    <tr>
      <td>3</td>
       <td align="center"><b>Build a service that produces and consumes SAML.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#connect-your-service-to-the-matching-service-adapter-metadata">Connect your service to the Matching Service Adapter metadata</a>.</li>
          <li><a href="#send-a-saml-authentication-request-to-the-gov-uk-verify-hub">Send an authentication request to the GOV.UK Verify hub.</a> </li>
          <li><a href="#handle-responses-using-saml-metadata">Handle the SAML response from the GOV.UK Verify hub.</a> </li>  
         </ol><br>
        Outcome: you’re ready to run SAML compliance tests.<br><br>

        For more information, see <a href="#how-saml-works-with-gov-uk-verify">How SAML works with GOV.UK Verify</a> and the <a href="https://www.gov.uk/government/publications/identity-assurance-hub-service-saml-20-profile">  'Identity Assurance Hub Service SAML 2.0 Profile'</a>.
      </td>
    </tr>
    <tr>
      <td>4</td>
       <td align="center"><b>Run SAML compliance tests.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#test-your-service-with-the-saml-compliance-tool">Test your service with the SAML compliance tool.</a></li>
          <li><a href="#test-your-matching-service-with-the-saml-compliance-tool">Test your matching service with the SAML compliance tool.</a> </li>
         </ol><br>
        Outcome: your service and matching service can consume and produce valid SAML.<br><br>

        For more information, see <a href="#how-saml-works-with-gov-uk-verify">How SAML works with GOV.UK Verify</a>.
      </td>
    </tr>
  </tbody>
</table>


## Integration

<table>
  <tbody>
    <tr>
      <th>Step</th>
      <th align="center">What you need to do</th>
    </tr>
    <tr>
      <td>1</td>
      <td align="center"><b>Request access to the integration environment.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li>Send the GOV.UK Verify operations team the IP address where you’ll host the Matching Service Adapter.</li>
          This allows GOV.UK Verify to put in place firewall rules so the hub can communicate with the Matching Service Adapter.<br>
          <li>Send the GOV.UK Verify operations team the IP addresses of the browsers you’ll use when testing in the integration environment.</li>
          Access to the hub and the test identity providers in the integration environment is restricted by IP address.<br>
          <li><a href="#request-certificates">Obtain signed certificates</a> for the integration environment from the IDAP test certificate authority.</li>
          <li>Send the GOV.UK Verify operations team your signed certificates for the integration environment.</li>
          <li>Fill in the ‘<a href="#request-access-to-environments">Request access to an environment</a>' form.</li>
        </ol><br>
        Outcome: you’re ready to connect the Matching Service Adapter and your service to the integration environment.<br><br>

        For more information, see <a href="#gov-uk-verify-environments">GOV.UK Verify environments</a> and <a href="#how-a-pki-works">How a PKI works</a>.
      </td>
    </tr>
    <tr>
      <td>2</td>
      <td align="center"><b>Connect the Matching Service Adapter and your service to the integration environment.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#install-the-matching-service-adapter">Download and install the Matching Service Adapter.</a></li>
          <li><a href="#configure-the-matching-service-adapter">Configure the Matching Service Adapter for the integration environment.</a> </li>  
        </ol><br>
        Outcome: you’re ready to run end-to-end testing with test users.<br><br>

        For more information, see <a href="#matching-service-adapter">Matching Service Adapter</a> and <a href="#gov-uk-verify-environments">GOV.UK Verify environments</a>.
      </td>
    </tr>
    <tr>
      <td>3</td>
       <td align="center"><b>Run end-to-end testing of all your user journeys in the integration environment.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#create-your-authentication">Set up authentication to manage test users.</a></li>
          <li><a href="#create-test-users">Create test users.</a> </li>
          <li><a href="#run-end-to-end-tests">Run end-to-end tests.</a> </li>  
         </ol><br>
        Outcome: your service can handle all the possible outcomes of end-to-end user journeys.<br><br>

        For more information, see <a href="#gov-uk-verify-environments">GOV.UK Verify environments</a>.
      </td>
    </tr>
    <tr>
      <td>4</td>
      <td align="center"><b>Request access to the production environment.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li>Send the GOV.UK Verify operations team the IP address where you’ll host the Matching Service Adapter.</li>
          This allows GOV.UK Verify to put in place firewall rules so the hub can communicate with the Matching Service Adapter.<br>
          <li><a href="#request-certificates">Obtain signed certificates</a> for the production environment from the IDAP certificate authority.</li>
          <li>Send the GOV.UK Verify operations team your signed certificates for the production environment.</li>
          <li>Fill in the ‘<a href="#request-access-to-environments">Request access to an environment</a>' form.</li>
        </ol><br>
        Outcome: you’re ready to connect the Matching Service Adapter and your service to the production environment.<br><br>

        For more information, see <a href="#gov-uk-verify-environments">GOV.UK Verify environments</a> and <a href="#how-a-pki-works">How a PKI works</a>.
      </td>

    </tr>
    <tr>
      <td>5</td>
         <td align="center"><b>Connect the Matching Service Adapter and your service to the production environment.</b> <br><br>
         To do this:<br><br>
         <ol>
          <li><a href="#install-the-matching-service-adapter">Download and install the Matching Service Adapter.</a></li>
          <li><a href="#configure-the-matching-service-adapter">Configure the Matching Service Adapter for the production environment.</a> </li>  
        </ol><br>
        Outcome: your service is ready to go live.<br><br>

        For more information, see <a href="#matching-service-adapter">Matching Service Adapter</a> and <a href="#gov-uk-verify-environments">GOV.UK Verify environments</a>.
      </td>
    </tr>

  </tbody>
</table>

## Maintenance

<table>
  <tbody>
    <tr>
      <th>Step</th>
      <th align="center">What you need to do</th>
    </tr>
    <tr>
      <td>1</td>
      <td align="center"><b>Rotate your keys.</b> <br><br>
        When the certificates containing your public keys are due to expire, <a href="#rotate-your-keys">replace your keys and certificates</a>.<br><br>
        Outcome: the encryption and signing certificates for your service and Matching Service Adapter are up to date.<br><br>

        For more information, see <a href="#how-a-pki-works">How a PKI works</a>.
      </td>
    </tr>
  </tbody>
</table>
