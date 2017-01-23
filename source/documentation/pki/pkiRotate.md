## Rotate your keys

When the certificates containing your public keys are due to expire, run the key rotation process to replace the keys and certificates. This allows you to introduce a new set of keys and certificates with no service interruption.

As a government service you are responsible for maintaining the following keys:

* government service encryption key
* government service signing key
* Matching Service Adapter encryption key
* Matching Service Adapter signing key

The certificates for these keys are usually issued at the same time.
You'll probably need to renew them all together.

### Initial steps for all keys

1. [Generate a new private key](#generate-private-keys).
1. [Generate a certificate signing request](#generate-certificate-signing-requests) and [submit it](#submit-certificate-signing-requests) to the IDAP certificate authority.
1. The IDAP certificate authority issues the new certificate to the government service.

### Rotate your service encryption key

1. Add the new private encryption key to your service endpoint. Your service can now use the new and old private keys to decrypt SAML messages.
1. Send the new certificate to the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team installs the new certificate on the hub. The GOV.UK Verify hub now uses the new key to encrypt SAML messages for your service.
1. Remove the old encryption key from your service endpoint.


### Rotate your service signing key

1. Send the new signing certificate to the the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team installs the new certificate on the GOV.UK Verify hub. The GOV.UK Verify hub now trusts SAML messages signed with the new and old keys.
1. Replace the old private signing key with the new key on your service endpoint. Your service now signs SAML messages with the new key only.
1. Inform the GOV.UK Verify operations team that new key is live.
1. The GOV.UK Verify operations team removes the old certificate from the GOV.UK Verify hub. The GOV.UK Verify hub now trusts SAML messages signed with the new key only.

### Rotate your Matching Service Adapter encryption key

1. Install the new private encryption key and encryption certificate on the Matching Service Adapter. To do this, add the fields `privateSecondaryEncryptionKeyConfiguration` and `publicSecondaryEncryptionKeyConfiguration` to the [YAML configuration file](#create-a-yaml-configuration-file) as indicated in the highlighted sections below:

    <pre>
      privateEncryptionKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_encryption_old.pk8

      <b>privateSecondaryEncryptionKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_encryption_new.pk8</b>

      publicEncryptionKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_encryption_old.crt
       keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

      <b>publicSecondaryEncryptionKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_encryption_new.crt
       keyName: http://www.test-rp-ms.gov.uk/SAML2/MD</b>
    </pre>

    The Matching Service Adapter can now use both the new and old keys to decrypt SAML messages.

    > **Note:** While both keys are in use, you may see error messages in the logs
    > with the description "Unwrapping failed". These messages appear
    > because the Matching Service Adapter attempts to decrypt the SAML
    > message using each key in turn. You can safely ignore these messages.
    > However, do not ignore any other error messages related to SAML
    > decryption.


1. Send the new certificate to the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team replaces the old certificate with the new certificate on the GOV.UK Verify hub. The GOV.UK Verify hub now uses the new key to encrypt SAML messages for your service.
1. Promote the secondary private encryption key and encryption certificate to the primary position.
1. Remove the old private encryption key and encryption certificate:

        privateEncryptionKeyConfiguration:
         keyUri: deploy/keys/test_rp_msa_encryption_new.pk8
        
        publicEncryptionKeyConfiguration:
         keyUri: deploy/keys/test_rp_msa_encryption_new.crt
         keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

### Rotate your Matching Service Adapter signing key

This procedure involves publishing 2 SAML signing certificates in the Matching Service Adapter SAML metadata.

1. Send the new signing certificate to the GOV.UK Verify operations team.
1. Install the new signing certificate on the Matching Service Adapter. To do this, add the field `publicSecondarySigningKeyConfiguration` to the [YAML configuration file](#create-a-yaml-configuration-file) as indicated in the bold section below:

    <pre>
      privateSigningKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_signing.pk8

      publicSigningKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_signing.crt
       keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

      <b>publicSecondarySigningKeyConfiguration:
       keyUri: deploy/keys/test_rp_msa_signing_new_cert.crt
       keyName: http://www.test-rp-ms.gov.uk/SAML2/MD/Secondary</b>
    </pre>

1. Check that your service endpoint has loaded the newly generated Matching Service Adapter metadata from: /matching-service/SAML2/metadata. The service endpoint now trusts assertions signed with the new key.
1. Update the field `privateSigningKeyConfiguration` with the new private signing key.
1. Promote the secondary signing certificate to the primary position.
1. Remove the old signing certificate:

        privateSigningKeyConfiguration:
         keyUri: deploy/keys/test_rp_msa_signing_new.pk8

        publicSigningKeyConfiguration:
         keyUri: deploy/keys/test_rp_msa_signing_new.crt
         keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

1. Restart the Matching Service Adapter. SAML messages are now signed with the new key.
1. Inform the GOV.UK Verify operations team that the new key is live.


