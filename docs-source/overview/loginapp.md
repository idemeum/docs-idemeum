# Login With idemeum App

**Login with idemeum app** is the most secure, multi-factor, decentralized, and completely private authentication method for your users. 

Users install idemeum app, set it up in two minutes, and they can start logging in into resources that support idemeum passwordless authentication using idemeum app. 

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/anaAkNl8a_I' frameborder='0' allowfullscreen></iframe></div>


## How idemeum app works

### Registration

As a first step users download and install idemeum app. You can scan the QR-code with your camera app to download idemeum. 

![Download idemeum app](/assets/mobileapp/qrcode.png)

Setting up idemeum is very simple - it will literally take you 5 minutes. As idemeum is decentralized, the onboarding is user-driven. There is nothing you will need except downloading and setting up an application.

!!! Tip "idemeum app is 100% private"
	Please, note that idemeum app is 100% private. **ALL** information you upload or input into the app will stay in the app. We will **NOT** store any of your personal information in our backend.
	
Let's go through the steps to set it up:

* Launch idemeum app on your mobile device.
* The app will ask you to enable biometrics. This is the key step to generate crypto keys on your mobile device and register your public key with your backend.
* As a next step you will need to add your identity information - your email address, your phone number, and your ID document (i.e. driver's license). idemeum app will verify these pieces of information and will store them on a mobile device. NO information will exist in our backend. The email and phone number will be verified using one-time-code and SMS. You just need to grab the codes and input them into the app.
* To verify drivers license you will need to scan your drivers license in the idemeum app and then do a selfie for liveness detection and to make sure you own that document. The app will guide you through the steps. This step is optional and you can skip it. 
* You are now all set with idemeum app.

### Authentication

Once the user has an app installed, authentication becomes very simple - navigate to application, scan the QR-code to login, authenticate with biometrics, and you are in. Here is how high level authentication with idemeum app looks like. 

![idemeum app flow](/assets/mobileapp/qrauth.png)

idemeum uses asymmetric cryptography to authenticate your identity with idemeum backend. When you log in to an online resource, idemeum backend sends your app a challenge with the request to sign it with your private key. By scanning your biometric with idemeum app you automatically unlock your private key, sign the requested challenge, and send it back to our backend.

![idemeum app authentication](/assets/mobileapp/authn.png)

!!! Info "QR code vs. Push Notification"
	The very first time user is logging in using idemeum app, she will need to scan the QR-code and then authenticate. All subsequent logins will be in the form of push notifications to users' devices.
	
Once the user authenticates with the biometric on a mobile device, the requested identity claims (such as email address or phone number) will be **released directly from the app**, not our backend. idemeum mobile app is decentralized and identity claims are only stored on a mobile device. 

It is important to note that idemeum app is passwordless and **multi-factor**. That is why it is called passwordless MFA. You authenticate with two factors: **something you have** (your mobile device with certificate on it) and **something you are** (your biometrics).

### How is app 100% private?

When you create your digital identity with idemeum app, you are assigned a [Decentralized Identifier (DID)](https://www.w3.org/TR/did-core/) that uniquely represents your digital identity among others. You DID is a globally unique persistent identifier that does not require a centralized registration authority because it is generated and registered cryptographically.

![idemeum app privacy](/assets/mobileapp/app-privacy.png)

idemeum also generates an asymmetric cryptographic key pair that will be unique for your digital identity. Also known as public-key cryptography this style of cryptography uses a public key to encrypt data that can only be decrypted with a paired private key. Public key, which can be known to public, is stored in idemeum backend and is associated with your DID. Private key, which must be kept secret, is safely stored on your mobile device and is protected by hardware based security. On iOS devices we leverage Secure Enclave, and on Android devices we use TEE / Strong Box. 

## Platform support

idemeum mobile app is available on iOS and Android. 

For the authentication flows, we support: 

1. **Desktop authentication** -> login on desktop, use mobile phone to scan the QR code
2. **Mobile authentication** -> login on mobile device, leverage universal links to authenticate with idemeum app installed on the same device. 

