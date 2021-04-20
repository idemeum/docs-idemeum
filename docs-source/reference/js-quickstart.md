# :material-language-javascript: JavaScript Login SDK

This quick-start guide will help you quickly add Auth functionality to your JavaScript single-page application. 

!!! Note "Integration steps"
	As a developer you have to deal with certain Auth related tasks when you are building your application. Our goal with JS SDK was to abstract as much complexity, yet give you enough flexibility so that you can address the use cases you need.
	
	In this guide we will need to do the following steps to set up auth for our JavaScript app:
	
	1. Initialize idemeum SDK
	2. Get and validate user tokens
	2. Manage user authenitcation state

<hr>

## 1. Initialize idemeum SDK

### Basic HTML setup
As a first step, let's set up a simple `index.html` page that we will be using for our application. We will set up some very simple css styles in order to format how thing are organized in our page.

This HTML page will display a simple log in button. Upon clicking a button, user will be authenticated by idemeum. As a developer you can choose how to authenticate the user (**one-click**, **biometric**, or **mobile app**). Upon successful authentication JWT tokens will be returned to the application and the user will be greeted.

```html

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>idemeum JS SDK</title>
    <link rel="stylesheet" type="text/css" href="/src/styles.css" />
  </head>
  <body>
    <h2>idemeum JS authentication sample</h2>
    <h4>Welcome to Application!</h4>
    <div id="initial">Loading...</div>
  </body>
</html>

```

### Import idemeum JS SDK

We can now import idemeum JavaScript SDK. For this guide we will simply import the script from idemeum CDN. 

```html

<script src="https://asset.idemeum.com/webapp/SDK/idemeum.js"></script>

```

### Initialize idemeum SDK

We can now initialize idemeum SDK instance. 

```JavaScript hl_lines="4"

      // Sample to initialize idemeum JS SDK
      var oidc;
      var idemeum = new IdemeumManager({
        clientId: "00000000-0000-0000-0000-000000000000", // 👈 Replace clientId with the the one you get from idemeum developer portal

        // OnSuccess of the login, this function will be executed
        onSuccess: function (successResponse) {
          // Your application will receive ID and Access tokens from idemeum
          oidc = successResponse.oidc;
          // getUserClaims validates the oidc token and fetches the user approved claims
          renderUserClaims();
        },
        onError: function (errorResponse) {
          // If there is an error you can process it here
        }
      });

```

## 2. Get and validate tokens

idemeum SDK returns ID and Access tokens upon successful user login. For token validation you can:

1. Validate token yourself using any of the open source JWT token validation [libraries](https://jwt.io)
2. Use idemeum SDK that provides built-in capability to validate tokens

In our guide we will rely on idemeum SDKs to validate tokens and extract user identity claims. 

Let's set up a function `#!JavaScript renderUserClaims()` to render user claims. We will take ID and Access tokens and validate them using `#!JavaScript getUserClaims()` by passing OIDC token as an input value. 

We will then create HTML to display to the user when she is logged in along with a `Logout` button. 


```JavaScript hl_lines="3"

      function renderUserClaims() {
        idemeum
          .getUserClaims(oidc)
          .then(function (userClaimsResponse) {
            //fetch user approved claims from OIDC token
            htmlStart = `<div>
        	              <p align="left">You are currently logged in.</p>
                          <pre id="ipt-user-profile" align="left">User profile:<br>`;
            htmlProfile =
              "<b><h3 style='color:Tomato;'>First Name:" +
              userClaimsResponse.given_name +
              "</h3></b><br>" +
              "<b><h3 style='color:Tomato;'>Last Name:" +
              userClaimsResponse.family_name +
              "</h3></b><br>" +
              "<b><h3 style='color:Tomato;'>Email:" +
              userClaimsResponse.email;

            htmlEnd = `      
                    </pre>
                    </div>
                    <button id="btn-logout" onclick="idemeum.logout()">Log out</button>`;
            document.getElementById("initial").innerHTML =
              htmlStart + htmlProfile + htmlEnd;
          })
          .catch(function (errorResponse) {});
      }

```

## 3. Manage user authentication state

Now we can create code to manage user authentication state. We need to determine if the user is logged in, and then define what content to render. We will also need to have the capability to log the users out. 

```JavaScript

      // Sample to evaluate login state of the user
      function isUserLoggedIn() {
        // Process the user logged-in state. We will create log out button and gated content.
        if (idemeum.isUserLoggedIn()) {
          //  Display user claims
          renderUserClaims();
        } else {
          html = `<button id="btn-login" onclick="idemeum.signin()">Log in</button>`;
          document.getElementById("initial").innerHTML = html;
        }
      }

```

And we can trigger `#!html isUserLoggedIn()` simply when the body of the document loads.


```html
	<body onload="isUserLoggedIn()">
```


## :material-file-code: Full working example

We have completed simple idemeum JavaScript SDK integration with our single-page application.

Here is the example of the full code we have discussed so far. 

???+ Example "Single-page app integration with idemeum JS SDK"
	```html
	<!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="UTF-8" />
	    <title>idemeum JS SDK</title>
	    <link rel="stylesheet" type="text/css" href="/src/styles.css" />
	    <script src="https://asset.idemeum.com/webapp/SDK/idemeum.js"></script>
	    <script>
	      // Sample to initialize idemeum JS SDK
	      var oidc;
	      var idemeum = new IdemeumManager({
	        clientId: "00000000-0000-0000-0000-000000000000", // 👈 Replace clientId with the the one you get from idemeum developer portal

	        // OnSuccess of the login, this function will be executed
	        onSuccess: function (successResponse) {
	          // Your application will receive ID and Access tokens from idemeum
	          oidc = successResponse.oidc;
	          // getUserClaims validates the oidc token and fetches the user approved claims
	          renderUserClaims();
	        },
	        onError: function (errorResponse) {
	          // If there is an error you can process it here
	        }
	      });

	      function renderUserClaims() {
	        idemeum
	          .getUserClaims(oidc)
	          .then(function (userClaimsResponse) {
	            //fetch user approved claims from OIDC token
	            htmlStart = `<div>
	        	              <p align="left">You are currently logged in.</p>
	                          <pre id="ipt-user-profile" align="left">User profile:<br>`;
	            htmlProfile =
	              "<b><h3 style='color:Tomato;'>First Name:" +
	              userClaimsResponse.given_name +
	              "</h3></b><br>" +
	              "<b><h3 style='color:Tomato;'>Last Name:" +
	              userClaimsResponse.family_name +
	              "</h3></b><br>" +
	              "<b><h3 style='color:Tomato;'>Email:" +
	              userClaimsResponse.email;

	            htmlEnd = `      
	                    </pre>
	                    </div>
	                    <button id="btn-logout" onclick="idemeum.logout()">Log out</button>`;
	            document.getElementById("initial").innerHTML =
	              htmlStart + htmlProfile + htmlEnd;
	          })
	          .catch(function (errorResponse) {});
	      }

	      // Sample to evaluate login state of the user
	      function isUserLoggedIn() {
	        // Process the user logged-in state. We will create log out button and gated content.
	        if (idemeum.isUserLoggedIn()) {
	          //  Display user claims
	          renderUserClaims();
	        } else {
	          html = `<button id="btn-login" onclick="idemeum.signin()">Log in</button>`;
	          document.getElementById("initial").innerHTML = html;
	        }
	      }
	    </script>
	  </head>
	  <body onload="isUserLoggedIn()">
	    <h2>idemeum JS authentication sample</h2>
	    <h4>Welcome to Application!</h4>
	    <div id="initial">Loading...</div>
	  </body>
	</html>
	```






