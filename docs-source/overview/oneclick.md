# One-Click Login

**One-click login** is the simplest and most intuitive authentication flow for your users. It introduces very little friction, as all users need to do is to click a button to login. 

[Try One-Click Login](https://jsdemo.idemeum.com/one-click){ .md-button }

<hr>

## User experience with One-Click Login

One-Click login experience consists of two parts: **registration** and **authentication**. 

### 1. Registration

When a user logs into an application with idemeum passwordless login for the first time, she will need to go through a **registration** process. The process is very simple - it requires validating the ownership of an email address with one-time code.

![One-click registration](/assets/one-click/flow.png)

The first time user accesses an application, she is presented with a passwordless login form. Upon providing an email address, idemeum will send a **one-time code** to user's inbox. That code needs to be typed back in order to prove email ownership. Once registration is complete, idemeum user account is created, Decentralized Identifier (DID) is assigned to the user, and JWT token is saved as a cookie in the user's browser.

At that point, user account is created with both idemeum and target application. User can manage her identity claims through identity cabinet. The claims are secured using client-side encryption (user managed encryption keys are backed by envelope encryption and AWS key management service). The master key is protected by hardware security models (HSMs) in AWS cloud. Only users have access to their encrypted identity data.

!!! Warning "When to register?"
    Registration flow will be executed in the following cases:

   	1. User never used idemeum passwordless login before.
   	2. User is using a new browser / or the cookies have been cleared from the browser. In this case user will need to verify the email address, but she will not need to enter her identity claims again. 
   	3. idemeum session is expired and user needs to prove email ownership again. Typical idemeum session expiration is 30 days. 
	
### 2. Authentication

When a user already logged in into **any** app with idemeum passwordless login, she will have a JWT present as cookie in the browser, and she will be able to seamlessly authenticate to any app with one-click. Click a button, confirm the release of information, and you are in. As idemeum is a private IDP, users will be able to Single-Sign On across applications and domains.

![One-click authentication](/assets/one-click/auth.png)

<hr>

## One-Click technical overview

































































Behind the scenes one-click login is using cryptographically signed and encrypted **JSON Web Tokens (JWT)** to authenticate users. 

!!! Abstract "What is JWT?"
	[JSON Web Token (JWT)](https://en.wikipedia.org/wiki/JSON_Web_Token) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

One-click binds the user’s email with her browser / device using cryptography and fingerprinting. Once the user is authenticated, idemeum returns a cryptographically signed & encrypted JSON Web Token that will be saved locally in the user's browser as a cookie. Each subsequent request will include the JWT, allowing the user to access any application that supports idemeum passwordless login. JWT enables users to **Single Sign-On** across applications and can be used across domains.

This is a stateless authentication mechanism as the user state is never saved in the idemeum backend. JWT points to a [DID](https://www.w3.org/TR/did-core/), which is unique for each user, and does not include **any** identity information. After the user validates his identity and presents his JWT to idemeum backend, she can access identity claims and privately share with the target application.

[More about security and privacy](https://blog.idemeum.com/idemeum-keeps-identity-secure-and-private/){ .md-button }




## Platform support

idemeum supports one-click login flow across desktop and mobile. 
