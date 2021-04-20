# Browsers we support with idemeum login

In this doc we will take a look at what browsers idemeum supports with **[one-click](/overview/oneclick/)**, **[biometric](/overview/biometric/)**, **[mobile app](/overview/loginapp/)** login flows. 
	
## Browser support matrix

| Browser | Windows | Mac OS | Android | iOS |
| ------- | ------- | ------ | ------- | --- |
| :fontawesome-brands-chrome: **Chrome**  | One-click<br>Biometric<br>Mobile app | One-click<br>Biometric<br>Mobile app | One-click<br>Biometric<br>Mobile app | One-click<br>Mobile app| 
| :material-firefox: **FireFox** | One-click<br>Biometric<br>Mobile app | One-click<br>Mobile app | One-click<br>Mobile app | One-click<br>Mobile app |
| :material-apple-safari: **Safari** | -  | One-click<br>Biometric[^1]<br>Mobile app | - | One-click<br>Biometric[^1]<br>Mobile app |
| :fontawesome-brands-edge: **Edge** | One-click<br>Biometric<br>Mobile app | One-click<br>Biometric<br>Mobile app | One-click<br>Mobile app |- | 

[^1]: Due to privacy controls Safari requires user gesture to trigger WebAuthn authentication. Therefore, additional button click is introduced in the flow in order to enable the biometrics flow. 


!!! Warning "WebAuthn requires browser native support"
	WebAuthn is browser dependent, and while most of the mainstream browsers support WebAuthn today, there is still some adoption lag. We always check the latest with official FIDO Alliance documentation so that we can test idemeum with the latest browser versions. Below you can find our update to date browser support matrix. 
	
	[FIDO Alliance official support matrix for WebAuthn](https://fidoalliance.org/fido2/fido2-web-authentication-webauthn/)



	







