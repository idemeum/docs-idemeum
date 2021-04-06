# Biometric Login

**Biometric login** adds additional level of security by requiring users to authenticate with biometrics leveraging **platform authenticators** (i.e. *Touch ID, Face ID*) or external authenticators (i.e. *Yubikey*). 

## How biometric login works

Biometric login takes one-click login to the next level by adding biometric based authentication using [WebAuthn](https://en.wikipedia.org/wiki/WebAuthn).

The Web Authentication API (also known as WebAuthn) is a specification written by the [W3C](https://www.w3.org) and [FIDO](https://fidoalliance.org), with the participation of Google, Mozilla, Microsoft, Yubico, and others. The API allows servers to register and authenticate users using public key cryptography instead of a password.

Users register by verifying ownership of an email address with a **One Time Code**. Then, based on whether developer enabled biometric registration option, users will be able to register biometric with WebAuthn. Upon registration a public / private key pair is generated, where **public key** is uploaded to FIDO2 server at the idemeum backend, and **private key** is securely stored locally in the FIDO2/CTAP compliant authenticator protected by biometric. All subsequent login requests will be performed with the user providing biometric scan. 

All user interactions are happening right from the browser, whereby providing seamless experience across mobile and desktop platforms. The only requirement is that browser supports WebAuthn, and in case it does not, developers have an option to configure a fallback to one-click login. 

You can get more technical details about WebAuthn from our blog post.

[Passwordless guide](https://blog.idemeum.com/noob-guide-to-passwordless-authentication/){ .md-button }

## User identity lifecycle

!!! Warning "WebAuthn requires user verification"
	WebAuthn is a protocol for authenticating users with biometrics. But before WebAuthn can be used, user identity needs to be verified. Typically, WebAuthn is paired with other Auth methods. idemeum leverages email OTP to create user identity first, and then use WebAuthn to register biometrics for existing identity. Technically. WebAuthn is an extension to one-click login flow.
	

1. When a user is logging into an application with idemeum passwordless login for the first time, she will need to go through the **registration** process. Registration will involve verifying email address with OTP and then registering FIDO2/WebAuthn credentials. 

2. When a user already logged in into **any** app with idemeum passwordless login, she will have a cryptographically signed and encrypted JWT present as cookie in the browser and biometric registered. As a result the user will be able to seamlessly authenticate to any app with a biometric scan. JWT is used to enable Single Sign-On across applications. 

### 1. Registration

Registration consists of two parts:

1. User validates ownership of an email address. idemeum will send a **one-time code** to the email address provided in the login form. That code needs to be typed back in order to prove email ownership.
2. User will be prompted to register biometric (create public/private key pair using FIDO2/CTAP compliant authenticator).

Once registration is complete, idemeum user account is created, unique [Decentralized Identifier (DID)](https://www.w3.org/TR/did-core/) is assigned to the user, JWT token is saved as a cookie in the user's browser, and FIDO public/private key pair is generated.  

![WebAuthn registration](/assets/biometric/registration-flow.png)

!!! Note
    Registration flow will be repeated in the following cases:

   	1. User never used idemeum passwordless login before
   	2. User is using a new browser / or the cookies have been cleared from the browser
   	3. idemeum session is expired and user needs to prove email ownership again

### 2. Authentication

Authentication with WebAuthn is a breeze, especially coupled with Single Sign-On that idemeum provides. Once registered, when users land in any application that supports idemeum passwordless login, they will be able to authenticate with a simple biometric scan. That is all. Simple and frictionless. 

![WebAuthn authentication](/assets/biometric/authentication-flow.png)

## Platform support

idemeum supports biometric login across desktop and mobile (iOS and Android).

In fact, WebAuthn is browser dependent, and while most of the mainstream browsers support WebAuthn today, there is still some adoption lag. Here is the matrix of the browsers that we tested and support at idemeum: 
