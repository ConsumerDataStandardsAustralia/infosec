---
title: Consumer Data Right Security Profile

language_tabs: # must be one of https://git.io/vQNgJ
  - http

toc_footers:
  - <a href='https://consumerdatastandards.org.au/'>Consumer Data Right Home</a>
  - <a href='https://github.com/ConsumerDataStandardsAustralia/infosec'>CDR InfoSec on GitHub</a>

search: true
---

# 1. InfoSec Profile 0.0.3

<aside class="warning">
This is an early draft of the CDR Information Security Profile and thus subject to change.
</aside>

## 1.1. History

| Author          | Date       | Version | Description               |
|-----------------|------------|---------|------------|
| LP			     | 22/11/2018 | 0.0.1   | Created |
| LP			     | 30/11/2018 | 0.0.2  | Created  |
| LP			     | 10/12/2018 | 0.0.3  | Created  |

The detailed change log for this artifact is available [here](https://github.com/ConsumerDataStandardsAustralia/infosec/blob/master/CHANGELOG.md).

## 1.2. Symbols and Abbreviated terms
-   **API**: Application Programming Interface
-   **CA**: Certificate Authority
-   **CL**: Credential Level
-   **CDR:** Consumer Data Right
-   **CDR-SP**: Consumer Data Right Security Profile
-   **DH:** Data Holder
-   **DR:** Data Recipient
-   **DTA:** Digital Transformation Agency
-   **FAPI:** Financial API
-   **HoK:** Holder of Key
-   **JSON:** The JavaScript Object Notation
-   **JWE:** JSON Web Encryption
-   **JWK:** JSON Web Key
-   **JWKS:** JSON Web Key Set
-   **JWS:** JSON Web Signing
-   **JWT:** JSON Web Token
-   **IP:** Identity Proofing
-   **LoA:** Level of Assurance
-   **LoAs:** Levels of Assurance
-   **MTLS:** Mutual Transport Layer Security
-   **OIDC:** Open ID Connect
-   **PII:** Personally Identifiable Information
-   **PKI:** Public Key Infrastructure
-   **PPID:** Pairwise Pseudonymous Identifier
-   **REST:** Representational State Transfer
-   **TDIF:** Trusted Digital Identity Framework
-   **TLS:** Transport Layer Security
-   **VoT:** Vector of Trust 

## 1.3. Requirements Notation and Conventions
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119) **[RFC2119]**.

# 2. Overview
This artifact details the [Consumer Data Right](https://www.accc.gov.au/focus-areas/consumer-data-right) **[CDR]** Information Security
Profile (CDR-SP). This profile will be built upon the foundation of the
[Financial-grade API Read Write Profile](https://openid.net/specs/openid-financial-api-part-2.html) **[FAPI-RW]** which in turn complies with the
[Open ID Connect 1.0 standard](http://openid.net/specs/openid-connect-core-1_0.html) **[OIDC]**. The CDR-SP will adhere to the **[OIDC]**
standard and **[FAPI-RW]** profile but will restrict implementation choices where
necessary. However, the CDR-SP may also extend the behaviour of the aforementioned standards
where allowed but will not override the intended behaviour outlined in these standards.

Whilst this is a technical artifact, it is guided by the core principles that
have to led to the creation of the Consumer Data Right. These are:

-   The CDR should be *customer focussed*.
-   The CDR should encourage *competition*.
-   The CDR should create *opportunities*.
-   The CDR should be *efficient and fair*.

# 3. CDR Federation
The CDR Federation will facilitate the secure exchange of consumer data and federation metadata between
multiple system entities which will assume one or more of the following roles:

-   **Data Holder**:
    -   Multiple Data Holders will be supported.
-   **Data Recipient**:
    -   Multiple Data Recipients will be supported.
- 	**Directory**:
    -   It is envisaged that only one directory will be supported and will be
        maintained by the Australian Competition and Consumer Commission (ACCC).  

## 3.1. Data Holder
The Data Holder (DH) is a system entity that authenticates a consumer
(resource owner or user), as part of an authorisation process initiated by a Data
Recipient, and issues an authorisation for that Data Recipient to access the consumer's
data via published APIs. 

A Data Holder MUST be accredited in order to participate in the CDR Federation.  Accreditation rules for Data Holders are beyond the scope of this artifact.

A Data Holder assumes the role of an **[OIDC]** [OpenID Provider](https://openid.net/specs/openid-connect-core-1_0.html#Overview).

## 3.2. Data Recipient
A Data Recipient (DR) is system entity that is authorised by a
Data Holder to access consumer resources (APIs). A Data Recipient MUST capture consumer consent prior to commencing an authorisation process with a Data Holder.

A Data Recipient MUST be accredited in order to participate in the CDR Federation.  Accreditation rules for Data Recipients are beyond the scope of this artifact. 

A Data Recipient assumes the role of an **[OIDC]** [Relying Party (Client)](https://openid.net/specs/openid-connect-core-1_0.html#Overview).

## 3.3. Directory
<aside class="warning">
The role and the functionality of the Directory is subject to change.
</aside>

The Directory is a central point of discovery for both Data Holders and Data
Recipients. Data Holders and Recipient must be registered as accredited entities in the Directory in order for them to participate as members of the CDR Federation.  The functionality of the Directory will include but will not be limited to:

- **Management of Identities and Access**: The Directory will allow registered persons, on behalf of Holders and Recipients, to manage the metadata of their associated organisations and systems.
- **Management of Certificates**: The Directory will faciliate the issuing, management and revocation of digital certificates.
- **Discoverability and Search**: The Directory will expose APIs and GUIs (Web applications) in order to support metadata queries across Directory entities. 

A full description of the Directory is beyond the scope of this document.

# 4. Authentication Flows
This profile supports the authentication flows specified by [FAPI](https://openid.net/wg/fapi/) **[FAPI]**.  These are:

-  The **[OIDC]** Hybrid Flow outlined at [section 3.3  of the OIDC standard] (https://openid.net/specs/openid-connect-core-1_0.html#HybridFlowAuth).
	- This MUST be supported by Holders. 
- The Client Initiated Backchannel Authentication flow **[CIBA]** outlined under the [FAPI CIBA profile](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default).
    -  This MAY be supported by Holders.

<a id="hybrid-flow"></a>
## 4.1. OIDC Hybrid Flow
The **[OIDC]** Hybrid Flow is a type of redirection flow where the customer’s user
agent is redirected from a Data Recipient’s (Relying Party) web site to a Data
Holder’s Authorisation endpoint in the context of an **[OIDC]** authentication
request. The Hybrid flow incorporates aspects of the both the implicit flow and
authorisation code flow detailed under **[OIDC]**.

Only a `response_type` (see [section 3](https://openid.net/specs/openid-connect-core-1_0.html#Authentication) of **[OIDC]**) of `code id_token` SHALL be allowed.

Request Object references SHALL not be support and therefore the `request_uri` parameter SHALL NOT be supported.

<a id="ciba-flow"></a>
## 4.2. Client-Initiated Backchannel Authentication (CIBA)
Client Initiated Backchannel Authentication (CIBA) enables a Data Recipient Client to
initiate the authentication of an end-user at a Data Holder by means of detached or out-band
mechanisms **[CIBA]**.  

Authorisation server rules for **[CIBA]** are covered under [section 5.2.2 of the FAPI CIBA profile](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default#markdown-header-522-authorization-server).  

Login hints MUST not reveal Personally Identifying Information (PII) about the consumer or end user.

Client rules for **[CIBA]** are outlined under [section 5.2.3 of the FAPI CIBA profile](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default#markdown-header-523-confidential-client). 

The polling mode for clients SHALL NOT be supported.

<a id="client-authentication"></a>
# 5. Client Authentication
This profile supports the 2 forms of
Client Authentication described under [section 5.2.2](https://openid.net/specs/openid-financial-api-part-2.html#authorization-server) of the **[FAPI-RW]** profile: 

- `private_key_jwt` specifed under [section 9](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) of **[OIDC]**.
- `tls_client_auth` specified under [section 2](https://tools.ietf.org/html/draft-ietf-oauth-mtls-12#section-2) of **[MTLS]**.

## 5.1. private\_key\_jwt


> Non-Normative Example

```
POST /token HTTP/1.1
Host: www.holder.com.au
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
  code=i1WsRn1uB1&
  client_id=s6BhdRkqt3&
  client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&
  client_assertion=eyJhbGciOiJQUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjEyNDU2In0.ey ...

# Decoded client assertion JWT
{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "12456"
}
{
  "iss": "12345",
  "sub": "12345",
  "iat": 1516239022,
  "exp": 1516239322,
  "aud": "https://www.holder.com.au/token",
  "jti": "37747cd1-c105-4569-9f75-4adf28b73e31"
}
```

The `private_key_jwt` client authentication method is enabled through the delivery of an encoded **[JWT]** signed using the Data Recipient Client's private key and thus faciliates non-repudiation. The **[JWT]** represents an assertion that MUST include the following claims:

- `iss`: The client ID of the bearer.
- `sub`: The client ID of the bearer.
- `aud`:  The URL of the endpoint being invoked.
- `iat`: A JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC issued at time.
- `exp`: A JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC expiry time.
-  `jti`: A unique identifier generated by the client for this authentication.

When invoking a protected endpoint, the aforementioned assertion MUST be sent with the `POST` method and MUST include the following parameters:

-  `grant_type`: This parameter MUST only be included when invoking the Token Endpoint and MUST be set to `authorisation_code` or `client_credentials`.
-  `code`: This parameter MUST only be included when invoking the Token Endpoint after utilising the [Hybrid Authentication flow](#hybrid-flow).  This is the value of the code parameter returned in the authorisation response. 
-  `client_id`: The ID of the calling Client.
-  `client_assertion_type`: This MUST be set to `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`.
-  `client_assertion`: The encoded assertion JWT.

## 5.2. tls\_client\_auth
This profile supports the [PKI Mutual TLS OAuth Client Authentication Method](https://tools.ietf.org/html/draft-ietf-oauth-mtls-12#section-2)
**[MTLS]**. TLS handshake requirements are covered in the [Mutual TLS section](#mutual-tls).  In addition to these requirements, the Client SHALL be successfully authenticated if the subject information in the certificate matches the expected DN configured or registered for the Client. 

# 6. OIDC Client Types
Only Confidential Clients SHALL be supported under this profile. Therefore, Public clients SHALL NOT be supported.

# 7. Tokens

## 7.1. ID Token

> Non-Normative Example - acr

```
{
  "iss": "https://www.holder.com.au",
  "sub": "a9ebbef6-1f0b-44eb-96cf-0c5b51b37ab2",
  "aud": "12345",
  "nonce": "n-0S6_WzA2Mj",
  "exp": 1311281970,
  "iat": 1311280970,
  "nbf": 1311280970,
  "auth_time": 1311280969,
  "acr": "urn:cds.au:cdr:3"
}
```

> Non-Normative Example - vot

```
{
  "iss": "https://www.holder.com.au",
  "sub": "a9ebbef6-1f0b-44eb-96cf-0c5b51b37ab2",
  "aud": "12345",
  "nonce": "n-0S6_WzA2Mj",
  "exp": 1311281970,
  "iat": 1311280970,
  "auth_time": 1311280969,
  "vot": "CL2",
  "vtm": "https://vector.com/trustmark".
}
```

ID Tokens are specified in [section 2](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) of the **[OIDC]** standard.  In accordance with **[FAPI-RW]**, ID Tokens must be signed and encrypted when returned
to a Data Recipient Client from both the Authorisation
Endpoint and Token Endpoint. Furthermore, as described under [section 5.2.2](https://openid.net/specs/openid-financial-api-part-2.html#authorization-server) of the **[FAPI-RW]** profile, ID Tokens MUST include the following claims (in addition to the mandatory claims specified in [section 2](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) of the **[OIDC]** standard):

- `nonce`: String value used to associate a Client session with an ID Token.
- `s_hash`: Hash of the state value.
- `c_hash`: Hash of the authorisation_code value.

The `amr` claim, which represents Authentication Methods References, MAY be returned as part of [Hybrid Flow authentication](#hybrid) but MUST be returned as part of a [CIBA authentication flow](#ciba-flow).

ID Tokens MUST be signed by Holders as specified in [section 8.6](https://openid.net/specs/openid-financial-api-part-2.html#jws-algorithm-considerations) of **[FAPI-RW]**.

The ID Token returned from the Authorisation Endpoint MUST NOT contain any Personally identifiable information (PII) claims.

An ID Token MUST not contain both a `vot` claim (see [Vectors of Trust](#vector-loas)) and an `acr` claim .

If the ID Token contains a `vot` claim, it MUST also contain a `vtm` claim:

- `vtm`: The trustmark URI as specified in [section 5](https://tools.ietf.org/html/draft-richer-vectors-of-trust-15#section-5) of **[VOT]** .

### 7.1.1. Hashing value for state and authorisation code
The `c_hash` value MUST be generated according to [section 3.3.2.11](https://openid.net/specs/openid-connect-core-1_0.html#HybridIDToken) of **[OIDC]**. 

The `s_hash` value MUST be generated according to [section 5.1](https://openid.net/specs/openid-financial-api-part-2.html#introduction) of **[FAPI-RW]**. 

## 7.2. Access Token
Access Tokens MUST be used as specified in [section 10.3] (https://tools.ietf.org/html/rfc6749#section-10.3) of **[OAUTH2]**. An
Access Token must expire `n` minutes after it is issued by the Data Holder where `n` is determined by **[CDR]** rules.

The process for refreshing an Access Token is described in [section 12.1](https://openid.net/specs/openid-connect-core-1_0.html#RefreshingAccessToken) of **[OIDC]**.

## 7.3. Refresh Token
Refresh Tokens MUST be used as specified in [section 12](https://openid.net/specs/openid-connect-core-1_0.html#RefreshTokens) of **[OIDC]**.
A Refresh Token must expire `n` days after it is issued where `n` is determined by **[CDR]** rules.

# 8. Scopes and Claims
Industry-specific scopes (for example, `bank_account`) will not be referenced in
this profile.

## 8.1. Scopes
The following scopes MUST be supported:

- `openid`: As described as [section 3.1.2.1](https://openid.net/specs/openid-connect-core-1_0.html#AuthRequest) of **[OIDC]**, this scope MUST be present on each authentication request.
- `profile`: Holders MUST support the `profile` scope as described in [section 5.4](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims) of **[OIDC]**.  This scope MAY be present on an authentication request.

## 8.2. Claims
The following [normal](https://openid.net/specs/openid-connect-core-1_0.html#NormalClaims) **[OIDC]** claims MUST be supported. This list includes, but is not limited to, **[OIDC]** [standard claims](https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims) :

-  `sub`: [Pairwise Pseudonymous Identifier (PPID)](#identifiers) for the End-User at the Holder.
-  `vot`: MUST contain a valid [VoT value](#vector-loas).  
-  `vtm`: The **[VoT]** trustmark URI. 
-  `acr`: Authentication Context Class Reference.  MUST contain a valid [ordinal LoA value](#ordinal-loa).
-  `amr`: Authentication Methods References.
-  `auth_time`: Time when the End-User authentication occurred. Its value is a JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC `auth_time`.
-   `name`: End-User's full name in displayable form including all name parts.
-   `given_name`: Given name(s) or first name(s) of the End-User.
-   `family_name`: Surname(s) or last name(s) of the End-User.
-   `updated_at`: Time the End-User's information was last updated. Its value is a JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC `updated_at` time.

<a id="identifiers"></a>
# 9. Identifiers and Subject Types
The identifier for an authenticated end-user (subject) is passed in the `sub` claim of an [ID Token](https://openid.net/specs/openid-cocnnect-core-1_0.html#IDToken) and [UserInfo response](https://openid.net/specs/openid-connect-core-1_0.html#UserInfoResponse) as defined by **[OIDC]**. The
Data Holder MUST generate the `sub` value as a Pairwise Pseudonymous Identifier (PPID)
as described in [section 8](https://openid.net/specs/openid-connect-core-1_0.html#SubjectIDTypes) of **[OIDC]** (Sector Identifier URIs SHALL NOT be supported under this profile). Furthermore, the identifier SHOULD also be
unique relative to the scenario in which the end user has authenticated. For
example, the identifier generated for the same person when they are using a
business account SHOULD be different to the identifier that is generated when that
same individual is authorising as an individual.

It is RECOMMENDED that the `sub` value is generated as a universally unique
Identifier (UUID) **[RFC4122]**.

<a id="levels-of-assurance-loas"></a>
# 10. Levels of Assurance (LoAs)
Levels Of Assurance (LoAs), returned after a successful authentication, MAY be represented in 2 different forms:

- [Single Ordinal](#ordinal-loa): A single LoA value is represented.
  - Holder's MUST support this mechanism. 
- [Vector](#vector-loas): One or more LoAs, represented by a vector value, are represented.
  - Holder's MAY support this mechanism.

In accordance with **[FAPI-RW]**:

-  *READ* operations SHALL only be allowed where __at least__ an LoA of 2 has been achieved which demonstrates, as outlined in the [Entity authentication assurance framework] (https://www.itu.int/rec/T-REC-X.1254) **[X.1254]**, that:
  - There is *some confidence* in the claimed or asserted identity of the entity. This LoA is used when moderate risk is associated with erroneous authentication. *Single-factor authentication is acceptable*. 
- *WRITE* operations SHALL only be allowed where __at least__ an LoA of 3 has been achieved which demonstrates, as outlined in the [Entity authentication assurance framework] (https://www.itu.int/rec/T-REC-X.1254) **[X.1254]**, that:
  - There is *high confidence* in the claimed or asserted identity of the entity. This LoA is used where substantial risk is associated with erroneous authentication. This LoA shall employ *multifactor authentication*.

<a id="ordinal-loa"></a>
## 10.1. Single Ordinal
A Single LoA value is carried in the `acr` claim which is described in [section 2](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) of **[OIDC]**.  

An LoA of 2 is represented by the URI: `urn:cds.au:cdr:2`

 - 	The authenticator used to attain this level MUST conform with the Credential Level `CL1` rules specified under the [Trusted Digital Identity Framework](https://www.dta.gov.au/our-projects/digital-identity/join-identity-federation/accreditation-and-onboarding/trusted-digital-identity-framework
) **[TDIF]** Authentication Credential Requirements specification.

An LoA of 3 is represented by the URI: `urn:cds.au:cdr:3`

 - 	The authenticators used to attain this level MUST conform with the Credential Level `CL2` rules specified under the [Trusted Digital Identity Framework](https://www.dta.gov.au/our-projects/digital-identity/join-identity-federation/accreditation-and-onboarding/trusted-digital-identity-framework
) **[TDIF]** Authentication Credential Requirements specification.


<a id="vector-loas"></a>
## 10.2. Vector

## 10.2.1. Overview
This profile incorporates support for [Vectors of Trust](https://tools.ietf.org/html/draft-richer-vectors-of-trust-15) **[VoT]**. A Vector, in this context, allows for the representation of multiple orthogonal components dimensions that may, but are not limited to, carry information relating to:

- Identity Proofing
- Primary Credential Usage
- Primary Credential Management
- Assertion/Federation Presentation

It is anticipated that due to their characteristics, which include composability, extensibility, and expressiveness, VoTs will be become the dominant standard for assurance representation at Identity Providers. Furthermore, as the **[CDR]** matures and incorporates requirements for Identity Proofing, Credential Management, and Assertion Presentation, these independent LoAs will be progressively added to the VoT **[CDR]** ecosystem. However, the dynamic capabilities of **[VoT]** will ensure that their addition does not break existing **[CDR]** implementations.

<a id="vot-values"></a>
## 10.2.2. VoT Values

The following VoT values SHALL be supported to represent authentication assurance levels when employing **[VoT]**.  These are carried in the `vot` claim of an ID Token:

 - `CL1`: This is Credential Level CL1 [defined](https://www.dta.gov.au/our-projects/digital-identity/join-identity-federation/accreditation-and-onboarding/trusted-digital-identity-framework) by the **[TDIF]** Authentication Credential Requirements specification.
   - This is equivalent to LoA 2.
 - `CL2`: This is Credential Level CL2 [defined](https://www.dta.gov.au/our-projects/digital-identity/join-identity-federation/accreditation-and-onboarding/trusted-digital-identity-framework) by the **[TDIF]** Authentication Credential Requirements specification.
   - This is equivalent to LoA 3.

<aside class="information">
The Trustmark URI is yet to be defined.
</aside>

# 11. Transaction Security
## 11.1. Ciphers
All HTTP calls MUST be made using HTTPS incorporating TLS >= 1.2. Only the following cipher suites SHALL be permitted in accordance with [section 8.5](https://openid.net/specs/openid-financial-api-part-2.html#tls-considerations) of **[FAPI-RW]**:

-   TLS\_DHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
-   TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
-   TLS\_DHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384
-   TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384

<a id="mutual-tls"></a>
## 11.2. Mutual TLS

<aside class="information">
Additional content will be added to this section when the Directory requirements are established. For example, verification/revocation of transport certificates.
</aside>

All Business-to-Business (B2B) service calls, that is back-channel communication between Recipient and Holder systems, MUST incorporate, unless stated otherwise, MTLS as part of the TLS handshake:

- The presented Client transport certificate MUST be issued by the CDR Certificate Authority (CA).  The Server MUST NOT trust Client transport certicates issued by other authorities. 
- The presented Server transport certificate MUST be issued by the CDR Certificate Authority (CA).  The Client MUST NOT trust Server transport certicates issued by other authorities.

## 11.3. Holder of Key Mechanism

MTLS MUST be supported as a Holder of Key (HoK) Mechanism.  

OAUTB SHALL NOT be supported.  

MTLS HoK allows issued tokens to be bound to a client certificate as specified in [section 3](https://tools.ietf.org/id/draft-ietf-oauth-mtls-07.html#SenderConstrainedAccess) of **[MTLS]**.

<a id="request-object"></a>
# 12. Request Object

> Non-Normative Example - acr as an Essential Claim

```
#Decoded Request Object JWT

{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "123"
}
{
 "iss": "12345",
 "aud": "https://www.recipient.com.au",
 "response_type": "code id_token",
 "client_id": "12345",
 "redirect_uri": "https://www.recipient.com.au/coolstuff",
 "scope": "openid",
 "state": "af0ifjsldkj",
 "nonce": "n-0S6_WzA2Mj",
 "claims":
  {
   "userinfo":
    {
     "given_name": null,
     "family_name": null
    },
   "id_token":
    {
     "acr": {"values": ["urn:cds.au:cdr:3"]}
    }
  } 
}
```

The Request Object is a signed and encoded JWT specified in [section 6.1](https://openid.net/specs/openid-connect-core-1_0.html#RequestObject) of **[OIDC]**.  As per **[FAPI-RW]** [section 5.2.2](https://openid.net/specs/openid-financial-api-part-2.html#authorization-server) and **[CIBA]** [section 5.2.2](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default#markdown-header-522-authorization-server), the `request` parameter MUST be present on requests to both the **[OIDC]** Hybrid Authorisation Endpoint and **[CIBA]** Backchannel Authorisation Endpoint. The Request Object enables **[OIDC]** requests to be passed in a single and self-contained parameter.

Requst Objects MUST be signed by Recipients as specified in [section 8.6](https://openid.net/specs/openid-financial-api-part-2.html#jws-algorithm-considerations) of **[FAPI-RW]**.

Recipient Clients MUST include a `consentId` value in the Request Object.  A high-level Consent overview is provided in the [consent section](#consent) of this artifact.

Holder Authorisation Servers MUST treat a Request Object that does not contain a `consentId` as invalid.

Request Object references SHALL not be supported and therefore the `request_uri` parameter SHALL NOT be supported.

## 12.1. Holder Authorisation Server VoT

> Non-Normative Example - vtr

```
Decoded Request Object JWT
{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "123"
}
{
 "iss": "12345",
 "aud": "https://www.recipient.com.au",
 "response_type": "code id_token",
 "client_id": "12345",
 "redirect_uri": "https://www.recipient.com.au/coolstuff",
 "scope": "openid",
 "state": "af0ifjsldkj",
 "nonce": "n-0S6_WzA2Mj",
 "vtr":" "CL2 CL1",
 "claims":
  {
   "userinfo":
    {
     "given_name": null,
     "family_name": null
    },
  } 
}
```

If a Holder supports **[VoT]**, they MUST accept Request objects which MAY contain:

- A `vtr` value. 
  - Allowed Values are specified in the [VoT values section](#vot-values) of this artifact.  
  - This value MUST contain a space-separated string that specifies the `vot` values that the Authorization Server is being requested to use for processing this Authentication Request, with the values appearing in order of preference. The VoT satisfied by the authentication performed is returned as the `vot` Claim Value. 
  - The `vot` Claim is requested as a Voluntary Claim by this parameter.
  - The `vtr` takes precedence over `acr_values`
- A `vot` essential claim. 
	- This is this VoT equivalent of an an `acr` essential claim.
	- If the `vot` Claim is requested as an Essential Claim for the ID Token with a values parameter requesting specific VoT values, the Holder Authorization Server MUST return a `vot` Claim Value that matches one of the requested values. The Authorization Server MAY ask the End-User to re-authenticate with additional factors to meet this requirement. If this requirement cannot be met, then the Holder Authorization Server MUST treat that outcome as a failed authentication.

## 12.2. Receipient Client using VoT

> Non-Normative Example - vot as an Essential Claim

```
Decoded Request Object JWT
{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "123"
}
{
 "iss": "12345",
 "aud": "https://www.recipient.com.au",
 "response_type": "code id_token",
 "client_id": "12345",
 "redirect_uri": "https://www.recipient.com.au/coolstuff",
 "scope": "openid",
 "state": "af0ifjsldkj",
 "nonce": "n-0S6_WzA2Mj",
 "claims":
  {
   "id_token":
    {
     "vot": {"values": ["CL2"]}
    }
  } 
}
```
	
For *WRITE* operations, a Recipient client:

- SHALL request user authentication at LoA 3 or greater by requesting the `vot` claim as an essential claim.

The relationship between LoAs and VoT values is specified in the [VoT values section](#vot-values) of this artifact.

# 13. Endpoints

## 13.1. OpenID Provider Configuration Endpoint

> Non-Normative Example

```
# Request

GET /.well-known/openid-configuration HTTP/1.1
Host: www.dh.com.au

# Response

HTTP/1.1 200 OK
Content-Type: application/json
{ 
  "issuer": "https://www.dh.com.au",
  "authorization_endpoint": "https://www.dh.com.au/authorise",
  "token_endpoint": "https://www.dh.com.au/token",
  "introspection_endpoint": "https://www.dh.com.au/introspect",
  "revocation_endpoint": "https://www.dh.com.au/revoke",
  "user_info_endpoint": "https://www.dh.com.au/userinfo",
  "jwks_uri": "https://www.dh.com.au/jwks",
  "registration_endpoint": "https://www.dh.com.au/register",
  "bc-authorize": "https://www.dh.com.au/bc-authorise",
  "scopes_supported": ["openid", "profile"],
  "response_types_supported": ["code id_token"],
  "response_modes_supported": ["fragment"],
  "grant_types_supported": ["authorization_code", "client_credentials"],
  "acr_values_supported": ["urn:cds.au:cdr:2","urn:cds.au:cdr:3"],
  "vot_values_supported": ["CL1","CL2"],
  "subject_types_supported": ["pairwise"],
  "id_token_signing_alg_values_supported": ["ES256", "PS256"],
  "request_object_signing_alg_values_supported": ["ES256", "PS256"],
  "token_endpoint_auth_methods_supported": ["private_key_jwt", "tls_client_auth"],
  "mutual_tls_sender_constrained_access_tokens": "true",
  "claims_supported": ["name", "given_name", "family_name", "vot", "acr", "auth_time", "sub"]
}
```

<aside class="warning">
OpenID Provider Configuration is directly impacted by the emerging requirements of the Directory and thus subject to change.
</aside>

| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  TLS | 
| Client Authentication Required| No|
| Bearer Token Required| No|

Data Holders MUST make their OpenID Provider Metadata available via a configuration endpoint as outlined in [Section 3 of the OpenID Connect Discovery standards] (https://openid.net/specs/openid-connect-discovery-1_0.html) **[OIDD]**. 

Where a Holder is supporting [Vectors of Trust](https://tools.ietf.org/html/draft-richer-vectors-of-trust-15) **[VoT**] or [CIBA](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default) **[CIBA]**, the published OpenID Provider metadata SHALL reflect that support.

At a minimum, the Provider metadata MUST include:

- `issuer`: URL that the Holder asserts as its Issuer Identifier.
- `authorization_endpoint`: URL of the Authorization Endpoint. 
- `token_endpoint`: URL of the Token Endpoint.
- `introspection_endpoint`: URL of the Introspection Endpoint.
- `revocation_endpoint`: URL of the Revocation Endpoint.
- `user_info_endpoint`: URL of the UserInfo Endpoint.
- `jwks_uri`: URL of the JWKS Endpoint.
- `scopes_supported`:  This list of supported scopes.
- `claims_supported`:  The list of supported claims.
- `acr_values_supported`:  The supported ACR values.

Holder's that support [Vectors of Trust](https://tools.ietf.org/html/draft-richer-vectors-of-trust-15) **[VoT**] MUST include:

- `vot_values_supported`:  The list of supported component values.

Holder's that support [CIBA](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default) **[CIBA]** MUST include:

- `bc-authorize`: The CIBA Authorisation Endpoint.

<a id="authorisation-endpoint"></a>
## 13.2. Authorisation Endpoint

> Non-Normative Example

```
# Request

GET /authorise?
 response_type=code%20id_token
   &client_id=12345
   &redirect_uri=https%3A%2F%2Fwww.recipient.com.au%2Fcoolstuff
   &scope=openid%20profile
   &nonce=n-0S6_WzA2Mj
   &state=af0ifjsldkj HTTP/1.1
   &request=eyJhbGciOiJQUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjEyMyJ9.ey ...
Host: www.holder.com.au

# Decoded request JWT
{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "123"
}
{
 "iss": "12345",
 "aud": "https://www.recipient.com.au",
 "response_type": "code id_token",
 "client_id": "12345",
 "redirect_uri": "https://www.recipient.com.au/coolstuff",
 "scope": "openid",
 "state": "af0ifjsldkj",
 "nonce": "n-0S6_WzA2Mj",
 "claims":
  {
   "userinfo":
    {
     "given_name": null,
     "family_name": null
    },
   "id_token":
    {
     "acr": {"values": ["urn:cds.au:cdr:3"]}
    }
  } 
}

```

| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  TLS | 
| Client Authentication Required| No|
| Bearer Token Required| No|

The requirements for the Authorisation Endpoint are specified in [section 3.3.2] (https://openid.net/specs/openid-connect-core-1_0.html#HybridAuthorizationEndpoint) of **[OIDC]** and further specified under section [5.2.2](https://openid.net/specs/openid-financial-api-part-2.html#authorization-server) of **[FAPI-RW]**.  This endpoint is invoked as part of the [Hybrid Authentication flow](#hybrid-flow).

Only a `response_type` (see [section 3](https://openid.net/specs/openid-connect-core-1_0.html#Authentication) of **[OIDC]**) of `code id_token` SHALL be allowed.

The `request_uri` parameter SHALL NOT be supported.

Please refer to the [Request Object section](#request-object) for a description of requirements relating to the `request` parameter.

## 13.3. Backchannel Authorisation Endpoint
| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| Yes|
| Bearer Token Required| No|

The requirements for the Backchannel Authorisation Endpoint are specified in the **[FAPI]** [Client Initiated Backchannel Authentication Profile](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default#markdown-header-523-confidential-client) **[CIBA]**.  This endpoint is invoked as part of the [Client-Initiated Backchannel Authentication flow](#ciba-flow).

The polling mode for clients SHALL NOT be supported.

Please refer to the [Request Object section](#request-object) for a description of requirements relating to the `request` parameter.

## 13.4. Token Endpoint
| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| Yes|
| Bearer Token Required| No|

The requirements for the Token Endpoint are specified in [section 5.3] (https://openid.net/specs/openid-connect-core-1_0.html#UserInfo) of **[OIDC]**.  

To obtain an Access Token, an ID Token, and a Refresh Token, the Client sends a Token Request to the Token Endpoint.

Holder's MUST support a Token Endpoint.

## 13.5. UserInfo Endpoint
| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| No|
| Bearer Token Required| Yes|

The requirements for the UserInfo Endpoint are specified in [section 3.3.3] (https://openid.net/specs/openid-connect-core-1_0.html#HybridTokenEndpoint) of **[OIDC]**.  

Holder's MUST support a UserInfo Endpoint.

## 13.6. JWKS Endpoint

> Non-Normative Example

```
{
  "keys": [
    {
      "kty":"EC",
      "crv":"P-256",
      "x":"MKBCTNIcKUSDii11ySs3526iDZ8AiTo7Tu6KPAqv7D4",
      "y":"4Etl6SRW2YiLUrN5vfvVHuhp7x8PxltmWWlbbM4IFyM",
      "use":"enc",
      "kid":"1"
    }
  ]
}
```

<aside class="warning">
The implementation of the JWKS Endpoint is directly impacted by the emerging requirements of the Directory and thus subject to change.
</aside>

<aside class="warning">
Private keys MUST NOT be published at this endpoint.
</aside>

| Description | Value   |  
|---|---|
| Hosted By  | Directory  |  
|  Transport Security |  TLS | 
| Client Authentication Required| No|
| Bearer Token Required| No|

The JWKS Endpoint returns a **[JSON]** document containing a JSON Web Key Set described in [section 5](https://tools.ietf.org/html/rfc7517#section-5) of **[JWK]**.  The JWK format is described in [section 4](https://tools.ietf.org/html/rfc7517#section-4) of **[JWK]**.  In addition to the mandatory fields specified in **[JWK]**, each JWK MUST include, at a minimum, the following fields:

-   `kid`: This is used to match a specific key withing a JWKS and thus must be unique within the set.
-   `use`: This is used to identify the intended use of the public key.  Supported values are `sig` and `enc`.

## 13.7. Introspection Endpoint

| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| Yes|
| Bearer Token Required| No|

Holder's MUST implement an Introspection Endpoint to allow Recipient clients to determine the status and expiry date of Refresh Tokens.  The requirements for an Introspection Endpoint are described in [section 2](https://tools.ietf.org/html/rfc7662#section-2) of **[RFC7662]**. 

Introspection of Refresh Tokens MUST be supported.

Introspection of Access Tokens and ID Tokens MUST NOT be supported.

An Introspection Endpoint Response SHALL only include the following fields:

- `active`: Boolean indicator of whether or not the presented token
      is currently active.
- `exp`:  A JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC expiry time.

## 13.8. Revocation Endpoint 

| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| Yes|
| Bearer Token Required| No|

Data Holders MUST implement a Revocation endpoint as described in [section 2](https://tools.ietf.org/html/rfc7009#section-2) of **[RFC7009]**. The Revocation Endpoint serves as a revocation mechanism that allows a Data Recipient Client to invalidate its tokens as required. Notifying the Holder authorisation server that the token is no longer needed allows the server to clean up data associated with that token and the underlying authorization grant.

Revocation of Refresh Tokens and Access Tokens MUST be supported.

## 13.9. Client Registration Endpoint

> Non-Normative Example

```
# Request

POST /clients HTTP/1.1
Content-Type: application/jwt
Accept: application/json
Host: www.holder.com.au

eyJhbGciOiJQUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjEyMzQ1In0.ey ...

# Decoded request JWT
{
  "alg": "PS256",
  "typ": "JWT",
  "kid": "12345"
}
{
  "iss": "https://www.recipient.com.au",
  "iat": 1516239022,
  "exp": 1516239322,
  "aud": "https://www.holder.com.au",
  "jti": "37747cd1-c105-4569-9f75-4adf28b73e31",
  "redirect_uris": ["https://www.recipient.com.au/coolstuff"],
  "token_endpoint_auth_method": "private_key_jwt",
  "grant_types": ["authorization_code","client_credentials"],
  "response_types": "code",
  "software_statement": "encodedsignedjwt",
  "id_token_signed_response_alg":"PS256",
  "request_object_signing_alg":"PS256"
}

# Response

HTTP/1.1 201 OK
Content-Type: application/json
{ 
  "client_id": "12345",
  "client_name": "Awesome Recipient Software",
  "redirect_uris": ["https://www.recipient.com.au/coolstuff"],
  "token_endpoint_auth_method": "private_key_jwt",
  "grant_types": ["authorization_code","client_credentials"],
  "response_types": "code",
  "id_token_signed_response_alg":"PS256",
  "request_object_signing_alg":"PS256"
}
```

<aside class="warning">
Dynamic Client Registration functionality is directly impacted by the emerging requirements of the Directory and thus subject to change.
</aside>

| Description | Value   |  
|---|---|
| Hosted By  | Holder  |  
|  Transport Security |  MTLS | 
| Client Authentication Required| No|
| Bearer Token Required| No|

### 13.9.1. Request

To register as a new Client at a Holder's Authorisation Server, a Data Recipient MUST `POST` its Client metadata to the Holder's Registration Endpoint in the form of an (encoded) signed [JWT](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32) **[JWT]**.  This process is specified in [OpenID Connect Registration](https://openid.net/specs/openid-connect-registration-1_0.html) **[OIDC-CR]**. The registering **[JWT]** is signed by the private key of the Client and MUST include a [software statement](https://tools.ietf.org/html/rfc7591#page-14).  The software statement is an  encoded [JWT](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32) **[JWT]** signed by the CDR Certificate Authority private key and thus supports non-repudiation.  The content of and mechanism for retrieving and generating a software statement is beyond the scope of this profile.

The registering **[JWT]** MUST include, at a minimum, the following fields:

- `iss`: The Recipient identifier specified at the Directory.
- `iat`: A JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC issued at time.
- `exp`: A JSON number representing the number of seconds from 1970-01-01T00:00:00Z to the UTC expiry time.
- `aud`: The Holder identifier specified at the Directory.
- `jti`: A unique identifier generated by the Recipient client.
- `redirect_uris`: An Array of Redirection URI values.
- `software_statement`: This is an encoded JWT which include several claims that describe the Recipient client application and the Recipient organisation.
- `id_token_signed_response_alg`: Token Endpoint preferred signing algorithm.
- `request_object_signing_alg`:  Request Object signing algorithm.
- `token_endpoint_auth_method`: The chosen Client authentication mechanism.

If the Client supports a [CIBA](https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default) **[CIBA]** notification endpoint, this may be specified as:

- `client_notification_endpoint`: A callback URI.

This request MUST be made with MTLS as specified in [section 11.2](#mutual-tls).  

Holders MUST ensure that the `CN` (Common Name) in the Client certificate `subject` field matches the `software_id` claim present in the software statement.  

Holders MUST verify that the embedded software statement has been signed by the CDR Certificate Authority.

### 13.9.2. Response

The Holder MUST respond in accordance with [OpenID Connect Registration](https://openid.net/specs/openid-connect-registration-1_0.html) **[OIDC-CR]** sections 3.2 and 3.3. 

<a id="consent"></a>
# 14. Consent

<aside class="warning">
Consent is a work-in-progress and thus subject to change.
</aside>

Prior to initiating an authentication request to a Holder's Authorisation Server, a Data Recipient MUST have captured indicative Consumer Consent and passed this to the Holder.  A Consent occurence is assigned a unique `consentId` and is referenced by the Holder as part of an authorisation process with a Consumer.  This process binds the Consent to the authorisation.  In order to support this functionality, a Holder MUST implement and host an API to support the creation of a Consent, the querying of a Consent, and the deletion of a Consent. In this instance the Recipient is to be considered the Resource Owner of the Consent occurance.

The specifics of the Consent API and processing of Consent are beyond the scope of this document.

A Data Holder Token Endpoint MUST:

- Support the `grant type` of `client_credentials` strictly for the purpose of passing an Access Token to a Recipient which can be then used to invoke the Consent API.


# 15. Normative References

| **Reference**  | **Description**                                                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|                             
| <a id="CIBA"></a>**[CIBA]**     | Financial Services – Financial API: Client Initiated Backchannel Authentication Profile 1.0: <https://bitbucket.org/openid/fapi/src/master/Financial_API_WD_CIBA.md?fileviewer=file-view-default> |
| <a id="FAPI-R"></a>**[FAPI-R]**   | Financial-grade API - Part 1: Read Only API Security Profile: <https://openid.net/specs/openid-financial-api-part-1.html>                                                         |
| <a id="FAPI-RW"></a>**[FAPI-RW]**  | Financial-grade API - Part 2: Read and Write API Security Profile: <https://openid.net/specs/openid-financial-api-part-2.html>                                                    |
| <a id="JSON"></a>**[JSON]**     | The JavaScript Object Notation (JSON) Data Interchange Format: <https://tools.ietf.org/html/rfc7159>                                                                              |
| <a id="JWA"></a>**[JWA]**      | JSON Web Algorithms (JWA): <https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40>                                                                                   |
| <a id="JWK"></a>**[JWK]**      | JSON Web Key (JWK): <https://tools.ietf.org/html/rfc7517>                                                                                                                 |
| <a id="JWT"></a>**[JWT]**      | JSON Web Token (JWT): <https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32>                                                                                            |
| <a id="JWS"></a>**[JWS]**      | JSON Web Signature (JWS): <http://tools.ietf.org/html/draft-ietf-jose-json-web-signature>                                                                                         |
| <a id="JWE"></a>**[JWE]**      | JSON Web Encryption (JWE): <http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption>                                                                                       |
| <a id="MTLS"></a>**[MTLS]**     | OAuth 2.0 Mutual TLS Client Authentication and Certificate Bound Access Tokens: <https://tools.ietf.org/id/draft-ietf-oauth-mtls-07.html>                                          |
| <a id="OAUTH2"></a>**[OAUTH2]**   | The OAuth 2.0 Authorization Framework: <https://tools.ietf.org/html/rfc6749>                                                                                                                                    |
| <a id="OIDC"></a>**[OIDC]**     | OpenID Connect Core 1.0 incorporating errata set 1: <http://openid.net/specs/openid-connect-core-1_0.html>                                                                        |
| <a id="OIDD"></a>**[OIDD]**     | OpenID Connect Discovery 1.0 incorporating errata set 1: <http://openid.net/specs/openid-connect-discovery-1_0.html>                                                              |
| <a id="OIDC-CR"></a>**[OIDC-CR]**  | OpenID Connect Dynamic Client Registration 1.0 incorporating errata set 1: <https://openid.net/specs/openid-connect-registration-1_0.html> |
| <a id="TDIF"></a>**[TDIF]** | Digital Transformation Agency - Trusted Digital Identity Framework <https://www.dta.gov.au/our-projects/digital-identity/join-identity-federation/accreditation-and-onboarding/trusted-digital-identity-framework>
| <a id="RFC2119"></a>**[RFC2119]**  | Key words for use in RFCs to Indicate Requirement Levels <https://tools.ietf.org/html/rfc2119>   |
| <a id="RFC7009"></a>**[RFC7009]**  | OAuth 2.0 Token Revocation: <https://tools.ietf.org/html/rfc7009>|
| <a id="RFC7523"></a>**[RFC7523]**  | JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants: <https://tools.ietf.org/html/rfc7523> |                                              
| <a id="RFC7662"></a>**[RFC7662]**  | OAuth 2.0 Token Introspection: <https://tools.ietf.org/html/rfc7662>| 
| <a id="VOT"></a>**[VOT]** | Vectors of Trust, draft-richer-vectors-of-trust-15 <https://tools.ietf.org/html/draft-richer-vectors-of-trust-15>      
| <a id="X.1254"></a>**[X.1254]**   | X.1254 - Entity authentication assurance framework: <https://www.itu.int/rec/T-REC-X1254-201209-I/en> |    

# 16. Informative References

| **Reference**  | **Description**                                                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|   
| <a id="BCP195"></a>**[BCP195]**   | Recommendations for Secure Use of Transport Layer Security (TLS) and Datagram Transport Layer Security (DTLS): <https://tools.ietf.org/html/bcp195>   
| <a id="CDR"></a>**[CDR]**      | Consumer Data Right: <https://www.accc.gov.au/focus-areas/consumer-data-right>                                                                                                    |
| <a id="FAPI"></a>**[FAPI]**      | Financial-Grade API - Home Page <https://openid.net/wg/fapi/>                                                                                                     |
| <a id="RFC4122"></a>**[RFC4122]**  | A Universally Unique IDentifier (UUID) URN Namespace: <https://tools.ietf.org/html/rfc4122> |
   

# 17. Appendix

## 17.1. Client Credentials grant type
![Part A](/images/clientCredentialsSequence.png)

## 17.2. Redirect Authentication Flow

### Part A - Recipient to Holder
![Part A](/images/redirPartA.png)
#### Steps
1. The user navigates to a Recipient Website.
2. The user selects their preferred Holder.
3. The user's browser is redirected to the Holder's Authorisation Endpoint.
4. *One* of the following may occur:
  1. The user may cancel the process at any point (in Parts **A**, **B** or **C**) and will be returned to the passed redirection URI for the Recipient with the relevant error code.
  2. The User is denied access.  This may happen as a result of too many failed attempts or other conditions relating to the user's account.  The user's browser will be redirected to the passed redirection URI for the Recipient with the relevant error code.
  3. The user sucessfully authenticatates and begins the consent/authorisation step (see Part **B**).


### Part B - Holder Authentication
![Part B](/images/redirPartB.png)
#### Steps
Part **B** illustrates the different authentication methods a Holder may present to the user.  It is important from a usability perspective that the Holder authentication choices presented to the user are consistent with those currently utilised by the user when accessing their existing Holder online accounts.  
The following options may be used:

1. All Credentials/Factors are captured through the Browser.  On success, the consent process begins (Part **C**) .
2. Two Factor Authentication (2FA)
    1. A userId and optionally a password are entered to the browser and submitted by the user.
    2. A code or notification is sent to a user's pregistered mobile/device application (detached authentication device).  This step is optional as a user's device application may generate codes in isolation, as is the case for Time-based One-Time Password (TOTP).
    3.  The user views the code or event on their detached authentication device.
    4. *One* of the following may occur:
        1. The user directly enters the code (or scans a QR Code) into the browser and submits the request.  On success, the consent process begins (Part **C**).
        2. The user does not enter the code into the browser.  The user acknowledges the authentication through the device and a secure message is sent from the device to the Holder via a backchannel. On receipt of the message, the Holder's website redirects the user's browser to the consent page (Part **C**).

### Part C - Post Consent Recipient to Holder
![Part C](/images/redirPartC.png)
#### Steps
This process continues from Part **B** afer a successful authentication.

1. The user provides consent authorising the presented scopes and/or data claims.
2. *One* of the following may occur:
  1. The Holder creates a new pairwise identifier for the user and Recipient combination.  This is the first time the user has authenticated to the Holder in the context of a request from this Recipient.
  2. This is a reauthentication.  The user has previously authenticated to the Holder in the context of an authentication request from this Recipient.  The existing pairwise identifier for the user and Recipient is allocated to the authorisation.
3.  The Holder creates the authorisation code and ID Token for the authorisation instance.
4.  The user's browser is redirected to the Recipient's redirect URI.  The ID Token and authorisation code generated in Step 3 are attached to the URL as query string parameters or as a fragment.  The Recipient web server processes the request.
5.  The Recipient decrypts the ID Token, verifies the signature and issuer of the ID Token, verifies the state/code hashes within the token, and also matches the presented state against it's own session state.  The Recipient Client then sends a POST request to the Holder Token Endpoint using Client Authentication and the Authorisation Code.
6. The Holder Endpoint authenticates the Recipient Client and matches the authorisation code. On success, the Endpoint responds with an Access Token, Refresh Token and an ID Token.  
7. The Holder creates an event relating to the authorisation.  This event is propogated/handled and may result in shared resource owners being notified about the authorisation.
8.  The Recipient verifies the ID Token and on success, invokes the UserInfo Endpoint using the Access Token as a Bearer Token.  The Holder verifies the token, applies the necessary Holder of Key verification check and on success, returns the requested UserInfo claims.
9.  The Recipient optionally begins calling the Holder APIs with the Access Token and renders the result to the user's browser.

## 17.3. Sample Holder Domain Model
![Domain Model](/images/holderDomain.png)
### Description
This diagram depicts the domain model of a hypothetical Data Holder. It is in no way prescriptive but illustrates the associations between the authorisation-related entities that may exist within a Holder's domain.
