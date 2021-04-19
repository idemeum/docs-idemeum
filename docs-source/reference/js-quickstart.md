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
As a first step, let's set up a simple `index.html` page that we will be using for our application. We will set up some very simple css styles in order to forma the presentation on the page. This HTML page will display a welcome message and will have a section where only logged in users can see their data.  

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
    <h4>Welcome to application!</h4>
    <div id="initial">Loading...</div>
  </body>
</html>

```

### Import idemeum JS SDK

We can now import idemeum JavaScript SDK. For this guide we will simply import the script from idemeum CDN. 

```html

<script src="https://asset.idemeum.com/webapp/SDK/idemeum.js"></script>

```

### Initialized idemeum SDK

We can now initialize idemeum SDK instance. 

```JavaScript hl_lines="4"

// Sample to initialize idemeum JS SDK

var idemeum = new IdemeumManager({
	clientId: "00000000-0000-0000-0000-000000000000", // 👈 Replace clientId with the the one you get from idemeum developer portal

	onSuccess: function (successResponse) {
		// Your application will recieve ID and Access tokens from idemeum
		oidc = successResponse.oidc;
		idToken = oidc.idToken;
		accessToken = oidc.accessToken;
        },
        
		onError: function (errorResponse) {
          // If there is an error you can process it here
        }
      });

```

## 2. Get and validate tokens

idemeum SDK provides built-in capability to validate ID and Access tokens. Let's set up a function to get the tokens validated and user claims extracted. 

```JavaScript

// Sample code to use idemeum SDK to get and validate user claims (ID token)

function getUserClaims() {
    idemeum.getUserClaims().then(function (userClaims) {

    // You can use user identity claims for various purposes depending on your use cases
	
    }).catch(function (errorResponse) {
		
      // Handle the error response here
	  
    });
}  

```

## 3. Manage user authentication state

Now we can create code to manage user authentication state. We need to determine if the user is logged in, and then define what content to render. We will also need to have the capability to log the users out. 

```JavaScript
// Sample to evaluate login state of the user
      
function isUserLoggedIn() {
	let html = "";

	// Process the user logged-in state. We will create log out button and gated content.
	if (true) {
		// Get user ID token, validate it and extract identity claims
		html = `
			<button id="btn-logout" onclick="idemeum.logout()">Log out</button>
            <div>
        		<p>You are currently logged in.</p>
        			<label>User profile:
       	 				<pre id="ipt-user-profile"></pre>
        			</label>
             </div>`;
        } else {
			// Process the user not logged in state. We will create login button.
          	html = `<button id="btn-login" onclick="idemeum.login()">Log in</button>`;
        }
		
        document.getElementById("initial").innerHTML = html;
      }

```

And we can trigger `#!JavaScript ` simply when the body of the document loads.


```html
	<body onload="isUserLoggedIn()">
```


## :material-file-code: Full working example

We have completed simple idemeum JavaScript SDK integration with your single-page application. Here is the example of the full code we have discussed so far. 

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
	      var idemeum = new IdemeumManager({
	        clientId: "00000000-0000-0000-0000-000000000000", // 👈 Replace clientId with the the one you get from idemeum developer portal

	        onSuccess: function (successResponse) {
	          // Your application will recieve ID and Access tokens from idemeum
	          oidc = successResponse.oidc;
	          idToken = oidc.idToken;
	          accessToken = oidc.accessToken;
	          expires_in = oidc.expires_in;
	        },
	        onError: function (errorResponse) {
	          // If there is an error you can process it here
	        }
	      });

	      // Sample code to get and validate user information from ID token
	      function getUserClaims() {
	        idemeum
	          .getUserClaims()
	          .then(function (userClaims) {
	            // You can use user identity claims for various purposes depending on your use cases
	          })
	          .catch(function (errorResponse) {
	            // Handle the error response here
	          });
	      }

	      // Sample to evaluate login state of the user
	      function isUserLoggedIn() {
	        let html = "";

	        // Process the user logged-in state. We will create log out button and gated content.
	        if (true) {
	          // Get user ID token, validate it and extract identity claims

	          html = `
	                    <button id="btn-logout" onclick="idemeum.logout()">Log out</button>
	                    <div>
	        	              <p>You are currently logged in.</p>
	        	              <label>User profile:
	        		              <pre id="ipt-user-profile"></pre>
	        	              </label>
	                    </div>`;
	        } else {
	          // Process the user not logged in state. We will create login button.
	          html = `<button id="btn-login" onclick="idemeum.login()">Log in</button>`;
	        }

	        document.getElementById("initial").innerHTML = html;
	      }
	    </script>
	  </head>
	  <body onload="isUserLoggedIn()">
	    <h2>idemeum JS authentication sample</h2>
	    <h4>Welcome to application!</h4>
	    <div id="initial">Loading...</div>
	  </body>
	</html>
	```






