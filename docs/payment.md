#4.1 The payment resource.
An implementer may request this resource to obtain the root resource describing a payment.
All other resources related to the payment are found on the resource itself or linked to from this resource.

The uri of this resource is the base uri to request all other resources related to the payment.

##4.1.1 Properties of payment
 * **uri**
    * the uri to this one resource.
 * **originalAmount**
    * see [authorize transaction](transaction/#authorize)    
    * `decimal`
    * the amount initially authorized on the payment.
 * **reserved**    
    * `decimal`
    * the amount that is still available for either [capture](transaction/#capture) or [cancel](transaction/#cancel)    
 * **captured**
    * see [capture transaction](transaction/#capture)
    * `decimal`
    * the amount that have been captured.
    * also describes the maximum amount that can be credited.
 * **credited**
    * see [credit transaction](transaction/#credit)
    * `decimal`
    * the amount that has been credited.
 * **cancelled**
    * see [cancel transaction](transaction/#cancel)
    * `decimal`
    * the amount that has been cancelled on the payment.
    * This amount is released from `reserved`    
 * **currency**    
    * `NOK` or `SEK`
    * the currency used for this `payment`
 * **transactions**
    * ordered list of all [transactions](transaction) executed on the payment.
    * the collection that new [transactions](transaction) are posted.
 * **address**
    * link this payments' [address ](address) resource.
    * if the initial order is initiated with [requires-physical-address](configurationReference/#requires-physical-address) and is paid with invoice, any shipping MUST be delivered to this address.



##4.1.2 Resource URI
Resource:  `/payments/{paymentId}`, Where `paymentId` is the token recieved from the paymentsession through checkout.js
This resource requires authentication, authorizing the owner of the payment. see [Authentication](authentication/#back-end-authentication)

##4.1.2.1 Supported HTTP Verbs
Method:    `GET`


##4.1.2.1.1 Example request
```HTTP
GET scheme://host.tld/api/payments/94ac4cde-5cb1-4609-938d-8c510bcef1bb HTTP/1.1
Accept: application/json
Authorization: Token secretencodedtokenthatyoumustneverspillontotheinternet==
```
##4.1.2.1.2 response:
```HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
  "uri": "scheme://host.tld/api/payments/94ac4cde-5cb1-4609-938d-8c510bcef1bb",
  "originalAmount": 199,
  "reserved": 0,
  "captured": 199,
  "credited": 0,
  "cancelled": 0,
  "currency": "NOK",  
  "transactions": [
  {
    "uri": "scheme://host.tld/api/payments/94ac4cde-5cb1-4609-938d-8c510bcef1bb/transactions/9450400",
    "currency": "NOK",
    "amount": 199,
    "type": "Authorize"
  },
  {
    "uri": "scheme://host.tld/api/payments/94ac4cde-5cb1-4609-938d-8c510bcef1bb/transactions/9450402",
    "currency": "NOK",
    "amount": 199,
    "type": "Capture"
  }
  ],
  "address": {
  "uri": "scheme://host.tld/api/payments/94ac4cde-5cb1-4609-938d-8c510bcef1bb/address"
  }
}
```
##4.1.2.1.3 Possible other HTTP status codes
 * `404 Not Found`
 * `401 Unauthorized`
