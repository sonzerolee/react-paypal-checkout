# react-paypal-checkout
React component that renders Paypal's check out button

## Install

```bash
npm install --save react-paypal-checkout
```

## Usage

## Simplest Example (with minimum set of parameters, this will use the "sandbox" environment)

```javascript
import PaypalBtn from 'react-paypal-checkout';

export default class MyApp extends React.Component {
	render() {
		const client = {
			sandbox:    'YOUR-SANDBOX-APP-ID',
			production: 'YOUR-PRODUCTION-APP-ID',
		}	
        return (
            <PaypalBtn client={client} currency={'USD'} total={1.00} />
        );
    }
}    
```

### Full Example

```javascript
import React from 'react';
import PaypalBtn from 'react-paypal-checkout';

export default class MyApp extends React.Component {
	render() {		
		const onSuccess = (payment) => {
			// Congratulation, it came here means everything's fine!
			console.log("The payment was succeeded!", payment);
		}		

		const onCancel = (data) => {
			// User pressed "cancel" or close Paypal's popup!
			console.log('The payment was cancelled!', data);
		}	

		const onError = (err) => {
			// The main Paypal's script cannot be loaded or somethings block the loading of that script!
			console.log("Error!", err);		
		}			

		let env = 'sandbox'; // you can set here to 'production' for production
		let currency = 'USD'; // or you can set this value from your props or state  
		let total = 1;  // same as above, this is the total amount (based on currency) to be 
		let locale = 'en_US'; 
		// For Customize Style: https://developer.paypal.com/docs/checkout/how-to/customize-button/
		let style = {
			'label':'pay', 
			'tagline': false, 
			'size':'medium', 
			'shape':'pill', 
			'color':'gold'
		};

		const client = {
		sandbox:    'YOUR-SANDBOX-APP-ID',
		production: 'YOUR-PRODUCTION-APP-ID',
		}
		// In order to get production's app-ID, you will have to send your app to Paypal for approval first
		// For sandbox app-ID (after logging into your developer account, please locate the "REST API apps" section, click "Create App"): 
		//   => https://developer.paypal.com/docs/classic/lifecycle/sb_credentials/
		// For production app-ID:
		//   => https://developer.paypal.com/docs/classic/lifecycle/goingLive/		

		// NB. You can also have many Paypal express checkout buttons on page, just pass in the correct amount and they will work!		  
		return (
			<PaypalBtn 
				env={env} 
				client={client} 
				currency={currency} 
				total={total} 
				locale={locale} 
				style={style}
				onError={onError} 
				onSuccess={onSuccess} 
				onCancel={onCancel} />
		);
	}
}
```

### Props

- `env: String (default: "sandbox")` - You can set this to "production" for production
- `client: Object (with "sandbox" and "production" as keys)` - MUST set, please see the above example
- `currency: String ("USD", "JPY" etc.)` - MUST set, please see the above example
- `total: Number (1.00 or 1 - depends on currency)` - MUST set, please see the above example
- `shipping: Number (1 or 2 or 3)` - Ability to change if a shipping address is included in checkout, 0 is default and means optional, 1 is no shipping address, 2 is shipping is required
- `onError: Callback function (happens when Paypal's main script cannot be loaded)` - If not set, this will take the above function (in example) as a default return
- `onSuccess: Callback function (happens after payment has been finished successfully)` - If not set, this will take the above function (in example) as a default return
- `onCancel: Callback function (happens when users press "cancel" or close Paypal's popup)` - If not set, this will take the above function (in example) as a default return
- `locale: PayPal uses these default locales in this order: en_US, fr_XC, ja_JP ...` - default: en_US
- `style: (optional) change appearance of Paypal button based on their document: size, color, shape, label` - https://developer.paypal.com/docs/checkout/how-to/customize-button/

## Sample Returned Data

- `onSuccess` - the returned `payment` object will look like: 
	+ `{paid: true, cancelled: false, payerID: "H8S4CU73PFRAG", paymentID: "PAY-47J75876PA321622TLESPATA", paymentToken: "EC-8FE085188N269774L", returnUrl: "https://www.sandbox.paypal.com/?paymentId=PAY-47J75876PA321622TLESPATA&token=EC-8FE085188N269774L&PayerID=H8S4CU73PFRAG"}`
- `onCancel` - the returned `data` object will look like: 
	+ `{paymentToken: "EC-42A825696K839141X", cancelUrl: "https://www.sandbox.paypal.com?token=EC-42A825696K839141X"}`

### Reference Document on How to Have Paypal's Developer Account and Merchant + Buyer accounts

- In fact, please just go to Paypal's developer page, login with your "Real" Paypal account: 
	+ Click on the "Sandbox accounts" on the left hand side
	+ You will see Merchant + Buyer accounts have been created automatically
	+ You can then change password, profile, can also clone those accounts, or create more accounts (choose country, default currency, set amount for new accounts)
- You can check balance & transaction history of those testing accounts (the same as real accounts)	by login with the accounts' credentials with:
	+ https://www.sandbox.paypal.com/
- Official documents here: (just be patient and go step-by-step, by clicking their next topic's button on page, you can then understand the whole process!)
	+ https://developer.paypal.com/docs/classic/paypal-payments-standard/integration-guide/create_payment_button/

## TODO

- Upgrade this library into more advanced Paypal button library (with features like recurring, add-to-cart, checkout-now etc.)
- Fork & pull-request are very welcomed!

## Thank you

- [React NPM Boilerplate](https://github.com/juliancwirko/react-npm-boilerplate)
- [React Paypal Express Checkout](https://github.com/thinhvo0108/react-paypal-express-checkout)

## License

MIT
