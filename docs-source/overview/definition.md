# What is idemeum?

## Meet passwordless and private identity platform

[idemeum](https://idemeum.com) is a digital identity platform that provides a set of tools and SDKs to implement passwordless auth flows for mobile and web applications.

idemeum platform and its architecture rests on three fundamental principles: 

1. **Passwordless auth** - idemeum is focused on reducing user friction and providing authentication flows that do not rely on passwords. Today we support `one-click login`, `login with biometrics`, and `login with idemeum app`. 
2. **Private identity** - we put security and privacy at the heart of our DNA. We developed [Privacy-aware Identity Infrastructure](/overview/privacy/) that is unlike traditional identity stores. We leverage tools such as client-side encryption, hardware based key management, and key delegation to offer users identity privacy and control. 
3. **Single Sign-On** - idemeum offers unmatched user experience by enabling [Single-Sign On](single sign on) across domains and applications, so that users log in once and can access any application with a click of a button. 

![idemeum platform overview](/assets/what/platform.png)

## Platform characteristics

Let's dig deeper into what defines idemeum digital identity platform. 

|   Component    | Description                          |
| ----------- | ------------------------------------ |
|   **Platform**   | Application developers outsource login flows end to end to us. We do not provide you with numerous tools, SDKs, and APIs to built embedded auth. We value your time and abstract all this complexity: implementing auth flows, security, scalability, privacy. We give you one SDK to connect to idemeum and implement auth for your apps.  |
| **Identity Provider **      | idemeum is an Identity Provider (IDP). We provide Single Sign-On to user across applications and domains. Once users login with idemeum to an application, the session gets created, so that users can seamlessly login to any other application that supports idemeum. |
| **Passwordless**    | No traditional login experience for idemeum. We are passwordless end to end :material-security:. Our goal is to remove friction for your users and help you build trusted relationship with your customers. |
| **Private by design**   | We are the only SSO Identity Provider that offers users privacy with their digital identity. No, we do not use blockchain 😀. With our mobile app we offer a decentralized model where digital identity data is only stored on users' devices. With our cloud we also designed a privacy preserving infrastructure so that only end users have access to their data. |

## Auth flows overview

idemeum offers three types of passwordless flows today.

|   Component    | Description                          |
| ----------- | ------------------------------------ |
|   **[One-Click](/overview/oneclick)**  | Simplest form of authenticating idemeum users into web, native, and mobile applications. One click login leverages asymmetric crypto key pair to authenticate users.  |
|   **[Biometric](/overview/biometric)** | Seamless authentication coupled with security of biometrics. Biometric login is based on [FIDO2/Webauthn](https://en.wikipedia.org/wiki/WebAuthn) behind the scenes.  |
|   **[idemeum app](/overview/loginapp)**   | Most secure, and most importantly 100% private authentication method. Your phone becomes your personal identity hub - all  identity claims and information are stored in the idemeum app only.  |

### Auth flows demo


You can explore and experience all idemeum passwordless flows with our JavaScript demo app. 
	
[Live Demo](https://jsdemo.idemeum.com){ .md-button }



