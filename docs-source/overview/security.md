# idemeum makes digital identity secure :shield:

idemeum offers three types of login: **one-click login**, **login with biometrics**, and **login with idemeum app**. Let's dig deeper into how each flow is secured with Privacy-aware Identity Management Architecture. 

## One-click or biometric login

![](/assets/security/architecture1.png)

There are four major architectural components that enable idemeum login. Each component carries a certain role with a number of security protections built-in. 

1. **Client-side SDK** - interface for integrating idemeum login flows into a website or mobile app. SDK orchestrates idemeum end-to-end flows including login initiation, token handling, and returning the control back to the website or mobile app.
2. **Web client** - angular-based single page application that verifies user claims, performs one-click or biometric (FIDO2) login, and manages user decentralized identity (DID). 
3. **Identity Provider (IDP)** - cloud service responsible for AWS identity federation using SAML 2.0 standards and acquiring temporary, limited-privilege credentials for the authenticated user. These credentials are used by the web client to directly interact with AWS key management service for data security.
4. **Key Management Service (KMS)** 🔑 - AWS managed service that makes it easy to create and control idemeum master key (CMK), and data encryption keys (DEK) used to encrypt user claims. AWS KMS CMKs are protected by hardware security modules (HSMs) that are validated by the FIPS 140-2 Cryptographic Module Validation Program.

Let's now get deep into each of the components to get the details about security controls.

### Client-side SDK

When integrating with idemeum each application gets CLIENT_ID assigned so that it can interact with idemeum backend. Since CLIENT_ID is used in the front-end code, it may be exposed. Therefore, to prevent malicious applications impersonation and phishing, application owners can set the **Allowed Origins** (whitelisted domains) that are allowed to use the CLIENT_ID.

SKD also offers application developers to validate OIDC ID token in two ways: **front-end validation** (in SDK code) or **redirect to endpoint** (provided by application developer to point to the backend endpoint). Token validation verifies standard claims like issuer, audience, expiration, issue time. It also validates the signature of the ID Token according to JWS using the algorithm specified in the JWT alg Header Parameter. The nonce value is also checked to mitigate replay attack.

### Web client

1. **Secure login strongbox** - login flow is implemented in a dedicated strongbox that is protected by short-term challenge and browser context (fingerprint). This prevents any part of the flow to be executed outside of the strongbox and mitigates phishing and replay attacks. The fingerprint is also associated with the DID JWT and hence protects against DID cookie stealing and replay attack.
2. **Signed and Encrypted DID JWT cookie** - authenticated user session is represented as a DID JWT and saved as a cookie in the browser. This JWT is signed and encrypted to protect the integrity and confidentiality of the users' DID.
3. **Encryption SDK** - encryption SDK is a client-side encryption library designed to make it easy to encrypt and decrypt users claims using industry standards and best practices. This client-side encryption in browser works directly in conjunction with the AWS key management service. idemeum cloud is not involved in the process and has no control of the data encryption keys used to encrypt the claims. SDK is built on security concepts like envelope encryption, key commitment, data key cache and symmetric cryptography.
4. **Encrypted message and algorithm suite** - users' claims are packaged into a portable formatted data structure called encrypted message, which includes encrypted data along with encrypted copies of the DEK keys, the algorithm ID, and an encryption context and a message signature. Encrypted message is securely persisted in the cloud. The idemeum cloud does not have access control to decrypt the data keys. The default algorithm suite is AES-GCM with an HMAC-based extract-and-expand key derivation function (HKDF), Elliptic Curve Digital Signature Algorithm (ECDSA) signing, and a 256-bit encryption key. 
5. **Authenticated encryption with additional data (AEAD)** - non-secret data that is provided to encryption and decryption operations to add an additional integrity and authenticity check on the encrypted data. Typically, the decrypt operation fails if the AAD (additional authenticated data) provided to the encrypt operation does not match the AAD provided to the decrypt operation.

### Identity Provider (IDP)

The idemeum IDP is responsible for authenticating user and requesting temporary, limited-privilege credentials from the AWS Security Token Service (AWS STS). This is done to create and provide trusted users with temporary security credentials that can control access to key management service. Access to key management service is required to encrypt and decrypt user claims by the encryption SDK in the idemeum web client as explained in the previous section. 

To use idemeum IDP, the first thing we do is to create an IAM identity provider entity to establish a trust relationship between the AWS account and the idemeum IDP.  The trust is established using SAML 2.0 standards. At login time, the idemeum IDP creates a SAML assertion (as part of the authentication response) that is used to get the temporary security credentials from AWS STS. These credentials are generated dynamically when requested and are limited-privilege and short-term, as the name implies. 

### Key management service 🔑 

The data in AWS KMS consists of idemeum customer master key (CMK) and the encryption key material they represent. This key material exists in plaintext only within AWS KMS hardware security modules (HSMs) and only when in use.

AWS KMS generates key material for idemeum customer master key (CMK) in FIPS 140-2 Level 2–compliant hardware security modules (HSMs). When not in use, key material is encrypted by an HSM and written to durable, persistent storage. The key material for CMK and the encryption keys that protect the key material never leave the HSMs in plaintext form.

The CMK is used by the encryption SDK to encrypt and decrypt data encryption keys (DEK's) that are used to encrypt and decrypt user claims. Since all the encryption operations happen in the web client, idemeum cloud does not have the appropriate permissions to decrypt the DEK’s

The access to the CMK is managed using key policy. The key policy does specify a specific role that should be assumed by temporary credentials in order to access the CMK. Access to CMK is also controlled by ABAC (attribute-based access control) and policy conditions that should be met before performing encryption operations. 

## Login with idemeum app

![](/assets/security/architecture2.png)

We created idemeum app (decentralized, verified, mobile identity) with the purpose to enable people to own and control their digital identity 100%. idemeum app is decentralized, meaning no user information (**correct, absolutely no personally identifiable information (PII)**) is persisted in the cloud when idemeum app is set up. All identity claims are securely stored on a mobile device. 

### Decentralized Identifier (DID) 

In the idemeum app user DID is secured using public-key cryptography, where DID private key is managed in the secure hardware, such as Trusted Execution Environment (TEE) and dedicated Hardware Security Module (HSM). 

### Biometric Support

idemeum app uses FIDO2 standards for supporting platform and roaming authenticators for user registration and authentication. Biometric validation is enforced when user logins into any service provider and also when managing the user cabinet.

### Verified and Encrypted Claims 

Users manage their identity claims in the idemeum app. The claims (first name, last name, address, email, phone, and others) are identity proofed to ensure that claimed identity matches the actual identity. The claims are secured using symmetric cryptography where key material is managed in the TEE and HSM.



