#:fontawesome-solid-low-vision: How idemeum makes identity private

Here at idemeum we made user data privacy and security part of our DNA. That is why we designed and built **Privacy-aware Identity Management Architecture**. This is our secret sauce that allows you to offer your users identity privacy and data protection with modern security controls. 

Let's dig deeper into how exactly idemeum offers digital identity privacy and security. 

## How idemeum enables identity privacy

Here at idemeum we find this definition of privacy helpful:

!!! info "Definition"
	[Privacy](https://en.wikipedia.org/wiki/Privacy) is an individual's claim to control the terms under which personal information — information identifiable to the individual — is **acquired**, **disclosed**, and **used**. 

Let's see how idemeum handles data acquisition, disclosure, and usage. 

### Data acquisition

Our philosophy here is to **give users a choice** for how and where they want their data to be persisted:

* When users login with **one-click login** or **biometric login**, their identity information will be collected, verified and persisted **privately in our cloud**. Digital identity data will be encrypted, and idemeum will not have appropriate permissions to decrypt it. That means users will control and own their digital identity data.
* When users choose to use **idemeum mobile app**, their identity information will be collected, verified and persisted privately only **on user's mobile devices**. No personal information that users create in the app will be stored in our cloud.

What is more, idemeum is architected to only work with **declared data** - data that was willingly shared by users. When users create idemeum profile they would typically provide first-name, last-name, email address and phone number. This is the only type of personal data that will be persisted. idemeum does not create or work with any **inferred data** - data developed around users without their input.

### Data disclosure

Our guiding principles here are **transparency** and **control**. 

Users have complete **transparency** into their identity data through **user cabinet**. For idemeum mobile app, user cabinet is managed in the mobile app. For web-based login flows (one-click and biometric), the user cabinet is managed in the cloud. At any time users can navigate to their idemeum cabinet and see what profile they have with idemeum and what identity information is maintained. Any identity claim (whether that is email, phone number, or name) can be updated or removed entirely at any time.

And most importantly, users have **control** over who and how their digital identity data is shared with. The whole purpose of persisting users' information is to provide seamless Single Sign-On across applications. Every time users go through the login process they are presented with a **consent screen**, so that users can choose and pick what data they want to share with the target application. 

Users' digital identity data is not disclosed to any third parties for any purpose without user consent.

### Data use

We follow the principles of **data minimization** and **purpose limitation**.

Users' digital identity data is used for only one purpose - log users into applications they choose to. And the data is only collected to the extent necessary to log users successfully into a target application. Digital identity data is not used for any other purposes, including advertising, sharing with third parties, or monetization purposes.

To get more details about how we designed architecture to enable privacy and security read our blog. 

[Learn more about privacy and security](https://blog.idemeum.com/idemeum-keeps-identity-secure-and-private/){ .md-button }
