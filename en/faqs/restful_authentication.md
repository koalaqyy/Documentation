
---
title: How to pass the basic HTTP authentication
description: How to pass the HTTP authentication?
platform: All Platforms
updatedAt: Fri Aug 16 2019 12:26:50 GMT+0800 (CST)
---
# How to pass the basic HTTP authentication
## Inrtoduction

Before using the Agora RESTful API, you need to fill in the `Authorization` parameter in the HTTP request header for authentication. Refer to the following code samples to get the `Authorization` parameter.

You need to fill in the **Customer ID** and **Customer Certificate** in the code.  Login https://dashboard.agora.io, click the account name on the top right of the dashboard, and enter the **RESTful API** page from the drop-down list to get the **Customer ID** and **Customer Certificate**.

> The Customer ID and Customer Certificate are used for RESTful API access only.

## Implementation

### Java

- Get the Authorization value

	```java
	// Fill in your Customer ID and Customer Certificate.
	String plainCredentials = "customerId:customerCertificate";
	// The base64Credentials here is the Authotization parameter you want, which is the base64 encoding of the credential.
	String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
	```
- Pass the authentication

	```java
	Request request = new Request.Builder()
	...
		// Pass in the Authorization value in the HTTP request header.
		.addHandler("Authorization", "Basic <base64Credentials>")
	...
	```

### Swift
	
- Get the Authorization value

	```swift
	let username = "customerId"
	let password = "customerCertificate"
	let loginString = String(format: "%@:%@", username, password)
	let loginData = loginString.data(using: String.Encoding.utf8)!
	// The base64LoginString here is the Authorization parameter you want, which is the base64 encoding of the login string.
	let base64LoginString = loginData.base64EncodedString()
	```
- Pass the authentication

	```swift
	// Pass in the Authorization value in the HTTP request header.
	let headers = ["Authorization", "Basic <base64LoginString>"]
	```

## Considerations

When sending the HTTP request, ensure that the format of Authorization is `Basic <base64Credentials>` or `Basic <base64LoginString>`.
