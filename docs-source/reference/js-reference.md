# JavaScript SDK reference

## Class reference

1. IdemeumManager
2. OIDCToken

## IdemeumManager Class Reference

### Initialization

??? note "IdemeumManager initialization"
	**Constructor**:
	
	```json
	
	IdemeumManager(String clientId)
	
	```

	**Description**: Initialize the class member for accessing the member functions.
	
	**Input params**:
	
	```json
	clientId: The clientId of the SDK procured from the Developer portal (dev_portal_link)
	```
	
	**Return**: Returns the class instance initialized
	
	**Success response**: N/A
	
	**Error response**: N/A


### Member functions

??? note "login()"
	**Description**: Initiate the passwordless login flow
	
	**Return**: Promise
	
	**Success response**: OIDCToken instance
	
	Error codes: 
	```json
	
	Sample response: {
	“code”:”code”
	“message”:”message”
	} 
	
	Code: 500
	Message: Server side error. Please contact idemeum

	Code: 400
	Message: Biometrics not enabled on your device

	Code: 404
	Message: This browser does not support webauthn

	Code: 401
	Message: Invalid token

	Code: 403
	Message: Invalid CSRF token specified

	Code: 400
	Message: Device does not support the biometric login

	Code: 400
	Message: Fido Assertion validation error

	Code: 401
	Message: User denied the login claims consent

	Code: 400
	Message: User handle is missing or empty 

	Code: 400
	Message: Missing required claims

	Code: 400
	Message: Attestation validation error

	Code: 403
	Message: Attempted to re-use previously completed login session

	Code: 400
	Message: Missing finger print header

	Code: 400
	Message: Missing CSRF Header

	Code: 400
	Message: Device fingerprint mismatch

	Code: 400
	Message: Login challenge code expired. Try again
	
	```
	
??? note "userClaims()"
	**Description**: Get the user claims after the login flow is completed or if the login session already exists and valid.
	
	**Return**: Promise
	
	**Success response**: OIDCToken instance
	```json
	Sample response: {
	  "aud": "string",
	  "email": "string",
	  "family_name": "string",
	  "given_name": "string",
	  "jti": "string",
	  "sub": "string"
	}
	```
	
	**Error response**: error as JSONObject
	```json
	Sample response: {
	“code”:”code”
	“message”:”message”
	} 
	
	Code: 499
	Message: Operation cancelled
 
	Code: 401
	Message: Invalid token

	Code: 401
	Message: User is not logged in
	```
	
??? note "isLoggedIn()"
	**Description**: Identifies if the login session exists and is still valid.
	
	**Return**: Promise
	
	**Success response**: success bool value as **true**
	
	**Error response**: success bool value as **false**


??? note "logout()"
	**Description**: Logout of the current/existing login session.
	
	**Return**: void
	
	**Success response**: N/A
	
	**Error response**: N/A
	


## OIDCToken

??? note "Public members"
	**Public members**:
	```json
	String accessToken
	String idToken
	long expires_in
	```
	
	**JSON**:
	```json
	{
	  "accessToken": "string",
	  "expires_in": 0,
	  "idToken": "string"
	}
	```