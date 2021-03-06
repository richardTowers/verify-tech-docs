## How a PKI works

### Public keys, private keys, and certificates

To encrypt and sign data, you need 2 pairs of keys:

* public and private **encryption** keys
* public and private **signing** keys

The keys consist of very long numbers linked in a particular way so the
public key can be derived from the private key but not the other way
around. Entities within a PKI generate their own keys. They always
retain their private keys. However, they share their public keys with
other entities to allow secure communication through
[encryption](#encryption-certificates) and [signing](#signing-certificates).

Public keys are shared using certificates. A certificate is a file that
contains:

* a copy of the certificate owner's public key
* information about the identity of the certificate owner
* an indication of the purpose of the certificate, for example, encryption or signing

Certificates are issued and signed by a certificate authority. The
certificate authority acts as the [trust anchor](#glossary-trust-anchor) in
the PKI. An entity that receives data from a third party can request
confirmation from the certificate authority that the third party's
certificate is valid.

### Keys and certificates in the GOV.UK Verify federation

The IDAP PKI and GOV.UK Verify federation use the X509 standard for
digital certificates.

In the GOV.UK Verify federation, the IDAP certificate authority issues
and signs certificates. As a government service using GOV.UK Verify, you
need to request 4 certificates:

* encryption and signing certificates for your service
* encryption and signing certificates for your Matching Service Adapter

To obtain a certificate:

1. Generate pair of keys ([private](#generate-private-keys) and [public](#generate-certificate-signing-requests)). You must generate a pair of encryption keys for each encryption certificate request and a pair of signing keys for each signing certificate request.
1. [Submit a certificate signing request](#submit-certificate-signing-requests) to the IDAP certificate authority.
1. The certificate authority generates your certificate and sends it to you.

The certificate authority also issues certificates for the GOV.UK Verify
hub and identity providers.

### Data encryption and signing

<a name="encryption-certificates"></a>

Encryption certificates provide the sender with assurance that only the
intended receiver can decrypt the message.

The sender extracts the public encryption key from the receiver's
encryption certificate and uses it to encrypt a message. The receiver
decrypts the message using their corresponding private encryption key.

![Diagram with an overview of how data encryption works. The GOV.UK Verify hub extracts the Matching Service Adapter public encryption key from the Matching Service Adapter encryption certificate.  The GOV.UK Verify hub encrypts data and sends it to the Matching Service Adapter. The Matching Service Adapter decrypts the data using the Matching Service Adapter private encryption key](/documentation/pki/pkiencryption.svg)

<a name="signing-certificates"></a>

Signing certificates provide the receiver with assurance of who authored
the message. They also guarantee that the message hasn't been tampered
with since the author signed it.

When signing data, you use your private signing key to create a digital
signature, and then send the signed message. The receivers check the
digital signature using your public signing key from your signing
certificate.

![Diagram with an overview of how data signing works. The government service signs data using their private signing key. The government service sends the signed data to the GOV.UK Verify hub. The hub checks the signature using the government service public signing key](/documentation/pki/pkisigning.svg)

You can use encryption and signing together or alone.

**Signing alone**

When a user indicates that they would like to prove their identity,
the government service sends a message to the GOV.UK Verify hub. The
message is unencrypted because it contains no personal user
information. However, the message is signed because the hub needs
assurance that the message originated from the government service.


**Encryption and signing**

When a user has verified their identity, the identity provider sends
a signed message to the hub containing the user's encrypted personal
information. This ensures the confidentiality of the message and that the message originated from the identity provider.

A message can contain embedded messages which may be encrypted and/or
signed by different entities within the GOV.UK Verify federation. For
more information, see the
diagram showing the [SAML message flow](#saml-flow-diagram) within the GOV.UK
Verify federation.
