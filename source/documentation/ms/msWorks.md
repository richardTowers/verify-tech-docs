## How a matching service works


### Matching cycles

The matching process consists of 3 matching cycles. The cycles are
progressive attempts to match the user to the correct record in the
government service's data sources:

* [Cycle 0: persistent identifier match](#cycle-0-persistent-identifier-match)
* [Cycle 1: matching dataset match](#cycle-1-matching-dataset-match)
* [Cycle 3: additional information match](#cycle-3-additional-information-match)

> **Note:** Cycle 2 isn't currently implemented.

The GOV.UK Verify hub issues a single call for matching cycles 0 and 1,
then a subsequent call for matching cycle 3. In your
[matching strategy](#define-your-matching-strategy), you can run cycle 0 first. Then, if
there is no match, you can run cycle 1 before returning the response to
the Matching Service Adapter (MSA).

If there's no match after the 3 cycles, you can
[create a new account](#create-user-accounts) for the user. Some services may decide
to create a new account after only 1 matching cycle. This applies to
services where users are creating new digital accounts rather than
relying on existing government service data sources.

When an identity provider verifies a user's identity, they send an
assertion to the [Matching Service Adapter](#matching-service-adapter) via the hub.
This assertion contains information about the user's identity used in
the different matching cycles, including the [persistent identifier](#glossary-persistent-identifier) and the [matching dataset](#glossary-matching-dataset).

#### Cycle 0: persistent identifier match

This cycle works when a user has previously verified their identity with
the same identity provider.

When an identity provider verifies a user's identity, they assign a
unique persistent identifier (PID) to that user. The identity provider sends
the PID to the MSA, via the hub.
The MSA then hashes the PID.

Cycle 0 matches the user's [hashed persistent identifier](#glossary-hashed-PID) to an existing
hashed PID in your datastore. This
succeeds only if the user has previously been matched by your service
with the same identity provider. The first time a new user is matched
(with cycle 1), the hashed PID is written to the local
matching datastore. It is then used for subsequent cycle 0 matches.

#### Cycle 1: matching dataset match

This cycle uses a limited set of information called the [matching dataset](#glossary-matching-dataset)
to check for a match between the user and the correct record. 

The identity provider that verified the user's identity provides the matching dataset. The identity provider verifies the information in the matching dataset
(except gender). Your local matching
service can use any or all the details in the matching dataset to
confirm a match between the user and their existing record. Your
service's [matching strategy](#define-your-matching-strategy) specifies which details to
use.

If the local matching service finds a match, it creates a correlation
between the hashed PID and the existing record. This
enables cycle 0 matching to occur during subsequent transactions.

#### Cycle 2: (not used)

Cycle 2 uses trusted attributes related to identity, for example, being
a disabled badge holder, to enhance the matching process.

> **Note:** Cycle 2 isn't currently implemented.

#### Cycle 3: additional information match

If cycle 1 finds more than 1 potential match, cycle 3 asks the user for
some additional information, for example driving licence number. The hub
collects the additional information and sends it to the matching
service. The local matching service then uses it to refine the match.
When the local matching service finds a match, it saves the hashed
PID in the datastore.

This cycle is defined in the government service policy and may not be
required for all matches. The government service defines the information
the hub collects and how to use it for matching. For example, you decide
how many pieces of additional information to request. If you request 2
pieces of information and the user can only provide 1 of them, your
matching rules specify whether to match this user.

Use this cycle to enhance cycle 1 and not as an alternative to cycle 1.

## Matching cycles: message flow

<a name="matching-cycles-diagram"></a>

This diagram shows the message flow for matching cycles 0, 1, and 3. The
numbers identify each stage in the flow. See below for explanations. 


![Diagram showing the three matching cycles, 0, 1 and 3. The Matching Service Adapter converts between SAML and JSON. The text below the image describes the steps](/documentation/ms/matchingcycles.svg)

For more details, see the diagrams:

* [GOV.UK Verify architecture](#architecture-diagram)
* [SAML message flow](#saml-flow-diagram)
* [create user accounts](#create-user-accounts-diagram)

> **Note:** In this example, an identity provider has already verified a user's
> identity. For more details of this process, see the
> [SAML message flow diagram](#saml-flow-diagram).

1. The identity provider sends the following information to the hub:
    * the user's identity information, known as the [matching dataset](#glossary-matching-dataset)
    * a unique [PID](#glossary-persistent-identifier) for the identity, created by the identity provider

    The hub forwards the matching dataset and PID to the MSA.
1. The MSA hashes the PID to make it meaningless to other services. The MSA sends the [hashed PID](#glossary-hashed-PID) and the matching dataset to the local matching service.
1. The local matching service runs cycle 0:

    The local matching service tries to find a match between the user's hashed PID and a hashed PID in the your datastore. 

    **If cycle 0 finds a match go to step 4, otherwise go to step 5.**

1. The local matching service sends a `match` response to the MSA and forwards it and the hashed PID to the government service via the GOV.UK Verify hub.

1. The local matching service runs cycle 1:
    
    The local matching service tries to find a match between the user's matching dataset and a record in government service data sources. 
    
    **If cycle 1 finds a match go to step 6, otherwise go to step 8.**

1. The local matching service saves the hashed PID in a datastore along with the user's record. Future matches with cycle 0 will use this data when the same user returns, having been verified by the same identity provider.

1. The local matching service sends a `match` response to the MSA and forwards it and the hashed PID to the government service via the GOV.UK Verify hub.

1. The local matching service sends a `no-match` response to the MSA, which forwards it to the GOV.UK Verify hub.

1. The hub asks the user to provide additional information, for example, their driving licence number and sends it to the MSA. 

1. The local matching service runs cycle 3:

    The local matching service tries to find a match between the user's additinal information and a record in government service data sources. 

    **If cycle 3 finds a match, go to step 11, otherwise go to step 13.**

1. The local matching service saves the hashed PID in a datastore along with the user's record. Future matches with cycle 0 will use this data when the same user returns, having been verified by the same identity provider.

1. The local matching service sends a `match` response to the MSA, which forwards it and the hashed PID to the government service via the GOV.UK Verify hub.


1. The local matching service sends a `no-match` response to the MSA, which forwards it to the GOV.UK Verify hub.  In this case, the matching service can [create a new account](#create-user-accounts) for the user, provided your matching service supports this feature and your user journey seeks explicit user consent.


