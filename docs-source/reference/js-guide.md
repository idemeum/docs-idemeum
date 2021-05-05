# Go passwordless with JavaScript SDK

This integration guide will help you quickly implement passwordless login for your single-page application. 

!!! tip "Resources to help"
	Here are all the resources that will help you navigate how idemeum JavaScript SDK works, what user experience it empowers, and how you can drop it in quickly into your SPA app. 
	
	* [CodeSandbox demo](https://codesandbox.io/s/idemeum-javascript-passwordless-sdk-app-3noir)
	* [GitHub source repo](https://github.com/idemeum/idemeum-js-sdk)
	* [Live login flows demo](https://jsdemo.idemeum.com)
	
## Integration overview

Our goal was to make idemeum SDK simple, yet powerful and flexible so that you can address the use cases you need. idemeum JS SDK provides 4 methods to help you with your login needs: `login`, `logout`, `userClaims`, `isLoggedIn`. By leveraging these methods you can enable passwordless, secure, and private login for your application.

##What we will do

In this guide we will go through the following steps to implement idemeum JS SDK:

1. Initialize idemeum SDK
2. Manage authentication state with `isLoggedIn`
3. Log the user in and out with `login` and `logout`
4. Get and validate user claims with `userClaims`

## 1. Initialize idemeum SDK

### Basic HTML setup

Our application will display a simple log in button. Upon clicking a button, user will be authenticated by idemeum. As a developer you can choose how to authenticate the user (**one-click**, **biometric**, or **mobile app**). Upon successful authentication JWT tokens will be returned to the application and the user will be greeted.

As a first step, let's set up a simple `index.html` page that we will be using for our application. We will set up some very simple css styles in order to format how things are organized in our page.



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

```css

/* our css style sheet that we save in styles.css and then import in out index page */

body {
  font-family: sans-serif;
}

#initial {
  align-self: center;
  justify-self: center;
  background-color: #eee;
  text-align: center;
  width: 300px;
  padding: 27px 18px;
}

```

### Import idemeum JS SDK

We can now import idemeum JavaScript SDK. For this guide we will simply import the script from idemeum CDN. 

```html

<script src="https://asset.idemeum.com/sdk/idemeum_1.0.0.min.js"></script>

```

### Initialize idemeum SDK

We can now initialize idemeum SDK instance. Do not forget to use your `clientId` that you obtained from idemeum developer portal. 

```JavaScript hl_lines="3"

      var idemeum = new IdemeumManager(
        // 👇 Replace clientId with the the one you get from idemeum developer portal
        (clientId = "00000000-0000-0000-0000-000000000000")
      );

```

## 2. Manage user authentication state

idemeum SDK helps you manage the authentication state of the user, so that you can determine if the user is logged in or not and then take actions depending on the outcome. idemeum `isLoggedIn` returns Boolean value to identify the user authentication state. 

* If the user is logged in, we will greet the user and display user claims.
* In case the user is not logged in, we will not show any content and will simply display the `login` button. 

As you can see in the code below we are simply using `login` method for button `onclick` event. 

```JavaScript

      function isUserLoggedIn() {
        // Process the user logged-in state. We will create log out button and gated content.
        idemeum.isLoggedIn().then(
          function (data) {
            //  Display user claims if the user is logged in
            renderUserClaims();
          },
          function (errorData) {
			// Display the login button if the user is NOT logged in
            html = `<button id="btn-login" onclick="login()">Log in</button>`;
            document.getElementById("initial").innerHTML = html;
          }
        );
      }

```

And we can trigger `#!html isUserLoggedIn()` simply when the body of the document loads.


```html
	<body
	    onload="setTimeout(function () {
	    isUserLoggedIn();
	  }, 500);"
	  >
```

## 3. Log the user in

When user clicks `Log in` button, idemeum SDK will trigger the `login` method. Let's define what will need to happen in our application. On success our application will receive ID and Access tokens from idemeum. We will need to process and validate those tokens. In case there is failure, we can process that as well in our code. 

```JavaScript

      function login() {
        idemeum
          .login()
          .then(function (signinResponse) {
            // Your application will receive ID and Access tokens from idemeum
            // renderUserClaims() (defined below) validates the oidc token and fetches the user approved claims
            renderUserClaims();
          })
          .catch(function (errorResponse) {
            // If there is an error you can process it here
            console.log(err);
          });
      }

```

## 4. Get and validate user claims

idemeum SDK returns ID and Access tokens upon successful user login. For token validation you can:

1. Validate token yourself using any of the open source JWT token validation [libraries](https://jwt.io)
2. Use idemeum SDK that provides `userClaims` method to validate tokens

In our guide we will rely on idemeum SDKs to validate tokens and extract user identity claims. In the code below we will take user claims (first name, last name, and email), and we will display these claims when the user is logged in. 

```JavaScript

      function renderUserClaims() {
        idemeum
          .userClaims()
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
                    <button id="btn-logout" onclick="idemeum.logout();isUserLoggedIn();">Log out</button>`;
            document.getElementById("initial").innerHTML =
              htmlStart + htmlProfile + htmlEnd;
          })
          .catch(function (errorResponse) {
            // If there is an error you can process it here
          });
      }

```

We are done with our simple SPA application! :congratulations:

Check the full working code example below.

<hr>


## Full working example

Here is the example of the full code we have discussed so far. 


```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>idemeum JS SDK</title>
    <link rel="stylesheet" type="text/css" href="/src/styles.css" />
    <script src="https://asset.idemeum.com/sdk/idemeum_1.0.0.min.js"></script>
    <script>
      // Sample to initialize idemeum JS SDK
      var idemeum = new IdemeumManager(
        // 👇 Replace clientId with the the one you get from idemeum developer portal
        (clientId = "00000000-0000-0000-0000-000000000000")
      );
      // Sample to evaluate login state of the user
      function isUserLoggedIn() {
        // Process the user logged-in state.
        idemeum
          .isLoggedIn()
          .then(function (data) {
            //  Display user claims if the user is logged in
            renderUserClaims();
          })
          .catch(function (errorData) {
            // Display the login button if the user is NOT logged in
            html = `<button id="btn-login" onclick="login()">Log in</button>`;
            document.getElementById("initial").innerHTML = html;
          });
      }
      function login() {
        idemeum
          .login()
          .then(function (signinResponse) {
            // Your application will receive ID and Access tokens from idemeum
            // renderUserClaims() (defined below) validates the oidc token and fetches the user approved claims
            renderUserClaims();
          })
          .catch(function (errorResponse) {
            // If there is an error you can process it here
            console.log(err);
          });
      }
      function renderUserClaims() {
        idemeum
          .userClaims()
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
                    <button id="btn-logout" onclick="idemeum.logout();isUserLoggedIn();">Log out</button>`;
            document.getElementById("initial").innerHTML =
              htmlStart + htmlProfile + htmlEnd;
          })
          .catch(function (errorResponse) {
            // If there is an error you can process it here
          });
      }
    </script>
  </head>
  <body
    onload="setTimeout(function () {
    isUserLoggedIn();
  }, 500);"
  >
    <h2>idemeum JS authentication sample</h2>
    <h4>Welcome to Application!</h4>
    <div id="initial"><p>Loading...</p></div>
  </body>
</html>

```

<hr>

## Questions?

[Let us know](/gethelp/) if you need any help or have questions.