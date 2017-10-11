#PayFabric APIs - Version 1.0
PayFabric APIs are organized around Representational State Transfer (**REST**) architecture and are designed to have predictable, resource-oriented URLs and use HTTP response codes to indicate API errors. Below are the API endpoints:

1. Live Server:    ``https://www.payfabric.com/rest/v1/api``
1. Sandbox Server: ``https://sandbox.payfabric.com/rest/v1/api``

Check out the [Quick Start Guide](https://github.com/PayFabric/Portal/wiki) to learn how to register for a PayFabric account, configure your account, and get started with the PayFabric REST API.

#Authentication
PayFabric clients have two ways to authenticate. 

Clients running in server-side programming environments can include an authorization field in the 
HTTP header of each request that is made to PayFabric. The authorization includes a _Device ID_ and 
a _Device Password_. These are the credentials for this application. You can generate these credentials
via the PayFabric web portal. These credentials give you complete access to the PayFabric API, so you 
should only use this authentication method in secure environments.

For clients running on consumer devices (e.g. smartphones) PayFabric highly recommends the use of 
_Security Tokens_. Security tokens are one-time use authorization credentials.

```c#
var url =  "http://www.payfabric.com/rest/v1/api/token/create";
HttpWebRequest httpWebRequest = WebRequest.Create(url) as HttpWebRequest;
httpWebRequest.ContentType = "application/json; charset=utf-8";
httpWebRequest.Headers["authorization"] = "5DE0B1D9-213C-4B05-80CA-D8A125977E20|6ytesddd*7";
HttpWebResponse httpWebResponse = httpWebRequest.GetResponse() as HttpWebResponse;
Stream responseStream = httpWebResponse.GetResponseStream();
StreamReader streamReader = new StreamReader(responseStream);
string result = streamReader.ReadToEnd();
streamReader.Close();
responseStream.Close();
httpWebRequest.Abort();
httpWebResponse.Close();
```
If everything goes fine, you can get a response as json format like this
```c#
{
    "Token": "4ts3gxu3o5an"
}
```
Next, you parse this json text, extract the token string, and put this token string into other call's custom "authorization" header. One thing you have to be noticed is the token is valid to use once. PayFabric will revoke a security token once the token arrives and completes the authentication process.

#Handling Exceptions
PayFabric uses HTTP response codes to indicate the status of requests. Below is a description of the 
meanings of the most common response codes that you will encounter. In general, status codes in 
the range of 200 to 299 indicate success. 400 to 499 indicate that the client has made a mistake.
500 and beyond indicate that PayFabric experienced an error. 

| Code        | Description | 
| ------------- | :------------- | 
| 200 OK | Request Successful | 
| 400 Bad Request | The request is missing required information. Verify that you are accessing the correct URL (including all query string parameters). If sending a JSON payload, check that you have provided all required elements, and make sure that there are no typos in your element names (JSON elements are case-sensitive!) |
| 401 Unauthorized | The authentication string provided by the client (either Device ID / Device Password, or Security Token) is invalid. |  
| 404 Not Found | The requested resource could not be located. Check for typos in your resource URI. |  
| 412 Precondition Failed | Missing fields or mandatory parameters. See description of status code 400. |  
| 500 Internal Server Error| PayFabric server encountered an error. |

Some programming languages such as .NET throw run-time exceptions when an HTTP response returns a status code other than 200. You can use these exceptions to programmatically recover or at least exit gracefully from errors. Below is an example in C# of catching exceptions.
```c#
try
{
   // Do Something
}
catch (WebException ex)
{
  using (StreamReader reader = new StreamReader(ex.Response.GetResponseStream()))
  {
    string errorMsg = reader.ReadToEnd();
    Console.WriteLine("Error Message: " + errorMsg);
  }
}
Catch (Exception ex)
{
   // Error Handling
}
```

# Payment APIs
PayFabric sends and receives payloads as structured [JSON Objects](Sections/Objects.md). 
Many of these objects are used in both requests and responses. Some of the objects (like Address or Cardholder) are embedded
as child elements of other objects.

## Transactions
* [Create a Transaction](Sections/Transactions.md#create-a-transaction)
* [Update a Transaction](Sections/Transactions.md#update-a-transaction)
* [Add a Payment Method](Sections/Transactions.md#add-a-payment-method)
* [Process a Transaction](Sections/Transactions.md#process-a-transaction)
* [Create and Process a Transaction](Sections/Transactions.md#create-and-process-a-transaciton)
* [Retrieve a Transaction](Sections/Transactions.md#retrieve-a-transaction)
* [Retrieve Transactions](Sections/Transactions.md#retrieve-transactions)
* [Referenced Transaction](Sections/Transactions.md#referenced-transactions-void-capture-ship-or-credit)
* [Refund a Customer](Sections/Transactions.md#refund-a-customer)

## Wallets / Credit Cards / Echecks
* [Create a Credit Card](Sections/Wallets.md#create-a-credit-card)
* [Create an eCheck](Sections/Wallets.md#create-an-echeck)
* [Update a Credit Card / eCheck](Sections/Wallets.md#update-a-credit-card-echeck)
* [Retrieve a Credit Card / eCheck](Sections/Wallets.md#retrieve-a-credit-card-echeck)
* [Retrieve Credit Cards / eChecks](Sections/Wallets.md#retrieve-credit-cards-echecks)
* [Retrieve Credit Cards / eChecks (Query with Paging)](Sections/Wallets.md#retrieve-credit-cards-echecks-query-with-paging)
* [Unlock Credit Card / eCheck](Sections/Wallets.md#unlock-credit-card-echeck)
* [Remove Credit Card / eCheck](Sections/Wallets.md#Remove-credit-card-echeck)

## Payment Gateways
* [Retrieve a Payment Gateway Profile](Sections/PaymentGatewayProfiles.md#retrieve-a-payment-gateway-profile)
* [Retrieve Payment Gateway Profiles](Sections/PaymentGatewayProfiles.md#retrieve-payment-gateway-profiles)

## Addresses
* [Retrieve a Shipping address](Sections/Addresses.md#retrieve-a-shipping-address)
* [Retrieve Shipping addresses](Sections/Addresses.md#retrieve-shipping-addresses)
* [Retrieve Shipping addresses (Query with Paging)](Sections/Addresses.md#retrieve-shipping-addresses-query-with-paging)

API Versions
------------
For our other supported versions of the APIs please see the below:

* [Version 2.0](https://github.com/PayFabric/APIs/tree/v2)
* [Latest Version](https://github.com/PayFabric/APIs)
