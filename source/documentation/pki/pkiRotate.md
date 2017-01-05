## Rotate your keys

When the certificates containing your public keys are due to expire, run the key rotation process to replace the keys and certificates.

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

Use the procedure below if your service supports 2 encryption keys simultaneously.

1. Add the new private encryption key to your service endpoint. This means that your service can use the new and old private keys to decrypt SAML messages.
1. Send the new certificate to the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team installs the new certificate on the hub. This means that the GOV.UK Verify hub uses the new key to encrypt SAML messages for your service.
1. Remove the old encryption key from your service endpoint.

Use the procedure below if your service does not support 2 encryption keys simultaneously.

1. Send the new encryption certificate to the the GOV.UK Verify operations team.
1. Take your service offline.
1. The GOV.UK Verify operations team installs the new certificate on the hub. This means that the GOV.UK Verify hub uses the new key to encrypt SAML messages for your service.
1. Replace the old private encryption key with the new key on your service endpoint. This means that your service uses only the new key to decrypt SAML messages.
1. Put your service back online.


### Rotate your service signing key

1. Send the new signing certificate to the the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team installs the new certificate on the GOV.UK Verify hub. This means that the GOV.UK Verify hub trusts SAML messages signed with the new and old keys.
1. Replace the old private signing key with the new key on your service endpoint. This means that your service signs SAML messages with the new key only.
1. Inform the GOV.UK Verify operations team that new key is live.
1. The GOV.UK Verify operations team removes the old certificate from the GOV.UK Verify hub. This means that the GOV.UK Verify hub trusts SAML messages signed with the new key only.

### Rotate your Matching Service Adapter encryption key

1. First install the new private encryption key on the Matching Service Adapter. Then configure the Matching Service Adapter for 2 encryption keys. To do this, add the new private encryption key to the Matching Service Adapter configuration file as indicated in the bold section below:

    <pre>
       privateSigningKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_signing.pk8

       <b>privateEncryptionKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_encryption_new.pk8

       privateSecondaryEncryptionKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_encryption_old.pk8</b>
   
       publicSigningKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_signing.crt
        keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

       publicEncryptionKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_encryption.crt
        keyName: http://www.test-rp-ms.gov.uk/SAML2/MD
    </pre>

    The Matching Service Adapter can now use both the new and old keys to decrypt SAML messages.


1. Send the new certificate to the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team replaces the old certificate with the new certificate on the GOV.UK Verify hub. This means that the GOV.UK Verify hub uses the new key to encrypt SAML messages for your service.
1. Remove the old private encryption key from the Matching Service Adapter. The Matching Service Adapter can no longer use the old key to decrypt SAML messages.

    > **Note:** While both keys are in use, you may see error messages in the logs
    > with the description "Unwrapping failed". These messages appear
    > because the Matching Service Adapter attempts to decrypt the SAML
    > message using each key in turn. You can safely ignore these messages.
    > However, do not ignore any other error messages related to SAML
    > decryption.

### Rotate your Matching Service Adapter signing key

There are 3 methods to rotate your Matching Service Adapter signing key. We recommend that you use SAML metadata as this minimises service downtime and reduces the complexity of the the key rotation procedure. For more information, see [Use the SAML metadata](#use-the-saml-metadata).

### Use SAML metadata

This procedure allows zero service downtime when you rotate your
Matching Service Adapter signing key. It involves publishing 2 SAML
signing certificates in the Matching Service Adapter SAML metadata.

> **Important:** To use this procedure you must be using version 493 or later of the
> Matching Service Adapter. Contact your engagement lead if you want to
> update your Matching Service Adapter.

1. Send the new signing certificate to the GOV.UK Verify operations team.
1. Configure your Matching Service Adapter to support 2 signing certificates. To do this, add the field `publicSecondarySigningKeyConfiguration` to the Matching Service Adapter configuration file as indicated in the bold section below:

    <pre>
      privateSigningKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_signing.pk8

      <b>publicSigningKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_signing.crt
        keyName: http://www.test-rp-ms.gov.uk/SAML2/MD

      publicSecondarySigningKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_signing_new_cert.crt
        keyName: http://www.test-rp-ms.gov.uk/SAML2/MD/Secondary</b>

      publicEncryptionKeyConfiguration:
        keyUri: deploy/keys/test_rp_msa_encryption.crt
        keyName: http://www.test-rp-ms.gov.uk/SAML2/MD
    </pre>

1. Check that your service endpoint has loaded the newly generated Matching Service Adapter metadata from: /matching-service/SAML2/metadata. This means the service endpoint trusts assertions signed with the new key.
1. Update the field `privateSigningKeyConfiguration` with the new private signing key and restart the Matching Service Adapter. This means that SAML messages are now signed with the new key.
1. Inform the GOV.UK Verify operations team that the new key is live.
1. Remove the old signing certificate from the Matching Service Adapter configuration file.

#### Your service supports 2 Matching Service Adapter signing keys

Use the procedure below if your service supports 2 Matching Service
Adapter signing keys simultaneously but you are not using SAML metadata
from your Matching Service Adapter.

1. Send the new signing certificate to the GOV.UK Verify operations team.
1. The GOV.UK Verify operations team installs the new certificate on the GOV.UK Verify hub. This means that the GOV.UK hub trusts SAML messages signed with the new and old certificates.
1. Add the new certificate to your service endpoint. This means that your service trusts SAML messages signed with the new and old keys.
1. Replace the old private signing key with the new key on the Matching Service Adapter. This means that SAML is now signed with the new key.
1. Remove the old certificate from your service endpoint. This means that your service trusts SAML messages signed with the new private signing key only.
1. Inform the GOV.UK Verify operations team that the new key is live.
1. The GOV.UK Verify operations team removes the old certificate from the GOV.UK Verify hub. This means that the GOV.UK Verify hub trusts SAML messages signed with the new private signing key only.

#### Your service does not support 2 Matching Service Adapter signing keys

Use the procedure below if your service does not support 2 Matching Service Adapter signing keys simultaneously. This involves service downtime during key rotation.

1. Take your service offline.
1. Replace the old certificate with the new certificate on your service endpoint. This means that your service trusts SAML messages signed with the new key.
1. Replace the old private signing key with the new key on the Matching Service Adapter. This means that SAML is now signed with the new key.
1. Inform the GOV.UK Verify operations team that the new key is live.
1. The GOV.UK Verify operations team removes the old certificate from the GOV.UK Verify hub. This means that the GOV.UK Verify hub trusts SAML messages signed with the new private signing key only.
1. Put your service back online.

