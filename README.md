# Selling crypto using Bank Transfer payment method, Automation Bot boilerplate

A sample application that you can use as a boilerplate for writing bot that would automate selling crypto using 
Bank Transfer payment method.

Use it at your own risk!

## Running App

1. Make sure that your API key has `Paxful API` product added
2. Update `.env` file and set the API secret
3. Deploy the app somewhere OR use `ngrok` (for ngrok howto see below). As a result you should have a publicly
   accessible URL. You will need to use it in step 7) below
5. Run `npm i` to install dependencies
6. Run `node app.js` to start the application
7. Create webhook for `trade.started`, `trade.paid`, `trade.chat_message_received` events (scroll to the bottom of
   [Direct access section](https://developers.paxful.com/dashboard/direct-access)). For target URL use one that you
   got in step 4), but append to it `/paxful/webhook` suffix. So if you had `https://example.com`, then when configuring
   webhooks you should use `https://example.com/paxful/webhook`
8. Create a sell offer on Paxful with Bank Transfer payment method, and then initiate a trade upon it from
   another account.
   
Once a trade is started upon an offer, then this application will automatically detect it (thanks to webhooks you setup
earlier) and then handles the trade process. In order to emulate a callback
from your bank (when fiat transfer is received in your bank account), you need to issue a POST request to
`/bank/transaction-arrived` with the following structure:

```json
{
   "reference": "dfwWnKXpZtV",
   "amount": 123,
   "currency": "EUR"
}
```

In real world implementation parameters are very likely to have different names. Whatever names the parameters
have, the payload **must** include a reference that a buyer specified for the payment (in the example above - `reference`)
- the application will use it to cross-reference and detect what trade this bank transfer is related to.

### Using ngrok

The application by default will be listening on http://localhost:3000, so if you haven't changed the port, then
once you have `ngrok` installed on your machine, you can run the following command to receive a publicly accessible
URL that you can use for registering as a webhook target on paxful.com:

```
ngrok http 3000
```