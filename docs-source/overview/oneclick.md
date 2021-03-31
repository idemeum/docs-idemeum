# One-Click Login

**One-click login** is the simplest and most intuitive authentication flow for your users. It introduces very little friction, as all users need to do is to click a button to login. 

## How One-Click works

Behind the scenes one-click login is using **JSON Web Tokens (JWT)** to authenticate users. 

!!! Abstract "What is JWT?"
	[JSON Web Token (JWT)](https://en.wikipedia.org/wiki/JSON_Web_Token) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

Users register by verifying ownership of an email address with a One Time Code. Once the user is authenticated, idemeum returns a JSON Web Token that will be saved locally in the user's browser as a cookie. Each subsequent request will include the JWT, allowing the user to access any application that supports idemeum passwordless login. JWT enables users to **Single Sign-On** across applications - it provides small overhead and can be used across domains.

This is a stateless authentication mechanism as the user state is never saved in the idemeum backend. JWT points to a [DID](https://www.w3.org/TR/did-core/), which is unique for each user, and does not include **any** identity information. After the user validates his identity and presents his JWT to idemeum backend, she can access identity claims and privately share with the target application.

[More about security and privacy](https://blog.idemeum.com/idemeum-keeps-identity-secure-and-private/){ .md-button }

## User identity lifecycle

1. When a user is logging into an application with idemeum passwordless login for the first time, she will need to go through **registration** process.

2. When a user already logged in into **any** app with idemeum passwordless login, she will have a JWT present as cookie in the browser, and she will be able to seamlessly authenticate to any app with one-click. 

Let's get deeper into each of the flows. 

### 1. Registration

User registration is very simple - **it requires validating the ownership of an email address with the one-time code.**

When user logs in to your application, he is presented with a passwordless login form. Upon providing an email address, idemeum will send a **one-time code** to users' inbox. That code needs to be typed back in order to prove email ownership. Once registration is complete, idemeum user account is created, Decentralized Identifier (DID) is assigned to the user, and JWT token is saved as a cookie in the user's browser. User identity claims are encrypted by the user generated private key and are stored in a dedicated HSM protected area of idemeum cloud. Only users have access to their identity data. 

![One-click registration](/assets/one-click/flow.png)


!!! Note
    Registration flow will be executed in the following cases:

   	1. User never used idemeum passwordless login before
   	2. User is using a new browser / or the cookies have been cleared from the browser
   	3. idemeum session is expired and user needs to prove email ownership again

### 2. Authentication

Once the user completes the registration, the authentication step is very simple. Click a button, confirm the release of information, and you are in. As idemeum is a private IDP, users will be able to Single-Sign On across applications and domains. 

![One-click authentication](/assets/one-click/auth.png)

## Platform support

idemeum supports one-click login flow across desktop and mobile. 
