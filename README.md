# Spark Authentication API

This file summarizes how to ramp up your Spark app, and how to use the Spark authentication API to generate tokens to verify users when they log in or register to your application.

## Spark API overview

Spark RESTful APIs enable you to access and extend the 3D printing methodology developed by Autodesk. Along with developing and registering your app, you must also create authentication tokens using the OAuth 2.0 protocol, to verify users against the Spark server. For more information, see the [Authentication API Reference](http://docs.sparkauthentication.apiary.io/#reference/response-fields)

## Authentication tokens

An authentication token: 
* Grants access to the Spark API
* Identifies your app
* Is valid for a single two hour session
* Can be refreshed after the initial session expires with a Refresh token
* For users with expanded access, proves that a Spark member has logged in from your app

### Authentication token types

Provided your app is registered on the developer's portal, Spark authentication can use the *app key* and *app secret* allocated during app registration to generate an Access or Guest token:

* *Access* tokens grant expanded access to public data and authenticate the user, giving access to their private-member (Spark-user) data. To generate an access token, the member whose data is being accessed must log in or register with your app.

* *Guest* tokens grant restricted, read-only access to APIs returning public data, such as public assets and member public profiles.

## Application creation and authentication token process

1. Sign in to the [Spark Developer Portal](https://spark.autodesk.com/developers/) and create a new application [here](https://spark.autodesk.com/developers/getStarted). During app registration, the *app key* and *app secret* values are  created in the app registration **API Keys** tab.

2. In the Developer Portal app settings, change the *Callback URL* field to **http://localhost:8089/callback**.

3. Modify the Authentication API: Copy the **SparkOAuth.py** file, and replace the CONSUMER_KEY and CONSUMER_SECRET values with the values from the Developer Portal App page, as follows: 
  * CONSUMER_KEY = *app key*
  * CONSUMER_SECRET = *app secret*

4. Create an authentication token. Follow the relevant instructions in the Creating an authentication token topic.

5. Invoke the relevant Authentication API to request a token from the Spark server. When users log in to your app, the handshake protocol begins with a request for the authorization code, the exchange of code for token, token response, and finally, a call to your Spark API.

## Creating an authentication token

### Before you begin
Make the following preparations: 

* Authorization header: The HTTP request must have an Authorization header in the format *Authorization: Basic {base64 code}*. Encode the *app key* and *app secret* values from the app registration's API Keys tab in Base64 and create a single string, separated by a colon (*app key:app secret*). For more information on Base64 encoding, see (http://www.base64encode.org/).
  * Example: For *app key* = 456 and *app secret* = 789. Base64 encoding *456:789* yields NDU2Ojc4OQ==, The resulting authorization header is *Authorization: Basic NDU2Ojc4OQ==*.

* Prepare the following parameters: 
  * &code 
  * &redirect_uri
  * Constants: &grant_type and &response_type

* If you plan to generate the access token in the documentation, ensure that the parameters appear in the Console body section and the base64 code appears in the Console headers section.

### To create an access token

**WARNING**: Access token generation uses the **app secret**, which must not be exposed to users. If your app code is entirely client-side and you are unable to hide the app secret, use the implicit log-in option, which, while less secure, does not require an API call.

1.  Use a browser/window control in your application to redirect the user to the following web page when they log in or register on Spark : *https://sandbox.spark.autodesk.com/api/v1/oauth/authorize?response_type=code&client_id={app key}&redirect_uri={callback URL}*
  * Notes:
    * Replace the values for {app key} and {callback URL} with your unique values.
    * Production: If you're using a production URL, replace the sandbox URL prefix. Members and data created in the sandbox are not accessible from production.
    * Implicit log-in: Change the response_type parameter to token if you are using implicit log-in.

2.  Call the **Generate an Access Token API**. 
 
### To create a guest token

1. Prepare the Authorization header as described in the Before you begin topic.
2. Call the **Generate a Guest Token API**.


## Exceptions and Exception Handling
<<this is a placeholder topic>>
## Using the API  <<I'm not sure why this is here in the first place??>>

To begin, in a browser, open http://localhost:8089/signin.
