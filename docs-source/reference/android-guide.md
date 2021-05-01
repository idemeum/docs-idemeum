# Go passwordless with Android SDK

This integration guide will help you quickly implement passwordless login for your Android mobile application. 

!!! tip "Resources to help"
	Here are all the resources that will help you navigate how idemeum Android SDK works, what user experience it empowers, and how you can drop it in quickly into your Android app.  
	
	1. [Android sample app](https://github.com/idemeum/idemeum-android-sample)
	2. [GitHub source repo](https://github.com/idemeum/idemeum-android-sdk)
	3. [Download apk to try](https://asset.idemeum.com/sampleapps/androiddemo_idemeum.apk)


## Integration overview

Similar to our other SDKs, idemeum Android SDK provides 4 methods to help you with your login needs: `login`, `logout`, `userClaims`, `isLoggedIn`. By leveraging these methods you can enable passwordless, secure, and private login for your application.

## What we will do

In this guide we will go through the following steps to implement idemeum Android SDK:

1.	Initialize idemeum SDK
2.	Manage authentication state with `isLoggedIn`
3.	Log the user in and out with `login` and `logout`
4.	Get and validate user claims with `userClaims`

## 1. Initialize idemeum SDK

### Add idemeum SDK dependency to gradle

In your app's `build.gradle` dependencies section, add the following:

```java

implementation 'com.idemeum:identity-android-sdk:1.0.0'

```

### Create URL scheme

Add URL scheme code in `Android Manifest` XML file.

```Java
<activity android:name="com.idemeum.androidsdk.ui.RedirectUriReceiverActivity"   android:exported="true">
<intent-filter>
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
	<data android:host="auth"
		android:scheme="idemeum" />
</intent-filter>
</activity>
```

### Initialize idemeum SDK

We can now initialize the `IdemeumManager` instance of idemeum SDK. Do not forget to use your `clientId` that you obtained from idemeum developer portal.

```Java
 //Replace clientId with the the one you get from idemeum developer portal
 IdemeumManager mIdemeumManager = IdemeumManager.getInstance("ClientId");
 
```

##2. Manage user authentication state

idemeum SDK helps you manage the authentication state of the user, so that you can determine if the user is logged in or not and then take actions depending on the outcome. With idemeum `isLoggedIn` we can obtain Boolean value for idemeum authentication state. 

* If the user is logged in, we will greet the user and display user claims.
* In case the user is not logged in, we will not show any content and will simply display the login button.

```Java

mIdemeumManager.isLoggedIn(this, isLoggedIn - > {
    // Process the user logged-in state.	       
    if (isLoggedIn) {
        //  Validate ID token and display user claims if the user is logged in
        validateToken();
    } else {
        // Display the login button if the user is NOT logged in
        showLoginButton();
    }

});

```

##3. Log the user in and out with `login` and `logout` methods

When the user clicks the Login button, idemeum SDK will trigger the `login` method. Let's define what will need to happen in our application. On success our application will receive ID and Access tokens from idemeum. We will need to process and validate those tokens. In case there is failure, we can process that as well in our code.

```Java

mIdemeumManager.login(this, new IdemeumSigninListener() {
    @Override
   	 public void onSuccess(OIDCToken oidcToken) {
        // Your application will receive ID and Access tokens from idemeum
        // validateToken() (defined below) validates the oidc token and
        // fetches the user approved claims
        validateToken();
    }
    @Override
    public void onError(int statusCode, String error) {
        // If there is an error you can process it here
    }
});

```

When the user clicks the Logout button, idemeum SDK will trigger the logout method.

```Java

mIdemeumManager.logout(this);

```

## 4. Validate and get user claims with `userClaims`

idemeum SDK returns ID and Access tokens upon successful user login. For token validation you can:

1. Validate token yourself using any of the open source JWT token validation [libraries](https://jwt.io)
2. Use idemeum SDK that provides userClaims method to validate tokens and extract user claims    	

In our guide we will rely on idemeum SDKs to validate tokens and extract user identity claims. 

```Java

private void validateToken() {

    // validate token 
    mIdemeumManager.userClaims(this, new TokenValidationListener() {

        @Override
        public void onSuccess(JSONObject mClaims, String message) {
            //user claims will be received as JSONObject here
            fetchDataFromClaims(mClaims);
        }

        @Override
        public void onError(int errorCode, String errorMsg) {
            // If there is an error you can process it here
        }

    });
}

private String fetchDataFromClaims(JSONObject mClaims) {

    StringBuilder mStringBuilder = new StringBuilder();
    mStringBuilder.append("You are currently logged in.")
        .append("\n\n")
        .append("User profile:")
        .append("\n\n");

    try {
        if (mClaims.has("given_name")) {
            mStringBuilder.append("First Name: ").append(mClaims.getString("given_name"));
            mStringBuilder.append("\n\n");
        }
        if (mClaims.has("family_name")) {
            mStringBuilder.append("Last Name: ").append(mClaims.getString("family_name"));
            mStringBuilder.append("\n\n");
        }
        if (mClaims.has("email")) {
            mStringBuilder.append("Email Address: ").append(mClaims.getString("email"));
            mStringBuilder.append("\n\n");
        }
    } catch (JSONException e) {
        e.printStackTrace();
        return mStringBuilder.toString();
    }

    return mStringBuilder.toString();
}

```

<hr>

Congratulations :congratulations: ! You completed integration with idemeum Android SDK. 

## Questions?

[Let us know](/gethelp/) if you need any help or have questions.






 
 

