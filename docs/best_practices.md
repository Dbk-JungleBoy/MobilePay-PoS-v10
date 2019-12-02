# <a name="best_practices"></a> Best Practices

This section is an overview over best practices when integrating to MobilePay PoS. The purpose of these best practices is to optimize the customer and merchant experience, to reduce errors and to keep the integration as simple as possible without reducing customer and merchant experience. 

## Check-in before or after payment has been initiated
While the system supports customers checking in both before or after a payment has been initiated, best practice is to urge customers to check in before.
As an example: In a supermarket a customer can check in and then start bagging their groceries. When all the groceries have been scanned the cash register will initiate the payment and allow the customer to swipe. 
This is an advantage over both cash and card payments, as the customer has no need to go back to the terminal/payment area again. This results in a faster transaction for all involved parties.

## Payment and loyalty payment flows
There are two main payment flows in the V10 API.
Initiating a payment followed by a capture (the [Payment Flow](payment_flows#payment_flow)), and a flow where initiate payment is split into two parts: Prepare-Ready and then followed by a capture (the [Prepared Payment Flow](payment_flows#prepared_payment_flow)).
The recommendation is to use the Payment Flow if there is no need for knowing the customer before setting the amount.
Using the Payment Flow will save a call to the API and in the end improve the overall time it takes to complete a payment.
Cases in which knowing the customer before setting the amount includes loyalty payments where a discount is based on the customer's loyalty ID. 

## Specify planned capture delay
It is required to specify when the payment is expected to be captured. The values are either *Immediately*, *Within24Hours*, and *Within14Days*. These are values that control how payments are handled by MobilePay Support and it is therefore beneficial that these values are set as accurate as possible.
The three values differ in the following way:
* **Immediately**: If reservations with this value are not captured **the following day** it is possible for MobilePay Support to reach out to make sure the reservervations are handled (either cancelled or captured) although this is not guaranteed.
* **Within24Hours**: If reservations with this value are not captured **the following 2-3 days** it is possible for MobilePay Support to reach out to make sure the reservervations are handled (either cancelled or captured) although this is not guaranteed.
* **Within14Days**: If reservations with this value are not captured MobilePay Support cannot help since the reservation will be expired and, hence, cannot be captured. Make sure this value is only used when appropriate.

## Order IDs
Order IDs are not required to be unique however this is highly recommended.
In error cases where the ``paymentId`` is lost it can be retrieved based on the ``posId`` and the ``orderId`` by calling the ``GET /api/v10/payments`` endpoint. If the ``orderId`` is unique the endpoint will return the expected ``paymentId``. However, if the ``orderId`` in not unique the mapping is overwrited and the endpoint will return the latest ``paymentId`` that was associated with the specified ``orderId``.

## Capture or cancellation of old reservations
All reservations should be captured or cancelled as soon as possible practically. If an error occurs that result in either cancellation or capture being impossible the PoS client is responsible for persisting which payments should be captured at a later stage. When processing for capturing at the later stage, it is important that only payments that should be captured are processed.
It is bad practice to poll for every outstanding payments that are in the *Reserved* state, since that could lead to payments being captured which should have been cancelled or expired.

## Polling
It is possible to get information on a payment using ``GET /api/v10/payments/{paymentId}``, and it is possible to get information about an active check-in using ``GET /api/v10/pointofsales/{posId}/checkin``. 
For both of these endpoints, it is allowed to do polling at most once per second. Polling times are controlled by the backend. The response will always contain a time interval that specifies, when the endpoint should at the earliest be polled again. The recommendation is to poll as fast as allowed by the backend to ensure maximal merchant and customer experience.

## Payment restrictions
It is possible to restrict which card types can be used for a payment. This restriction is available in order to support that some countries have restrictions on which cards can be used for certain products.
It is also possible to set a minimum age for the paying customer. As an example, this can be set on payments involving alcohol or gambling products.
The recommendation is to only set restrictions on payments where it is required to do so by the law or in case there are some business related reasons for restricting the payment.
