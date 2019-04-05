# React Native GPay
[![react-native version](https://img.shields.io/badge/react--native-0.41-0ba7d3.svg?style=flat-square)](http://facebook.github.io/react-native/releases/0.40)
[![npm](https://img.shields.io/npm/v/react-native-payments.svg?style=flat-square)](https://www.npmjs.com/package/react-native-payments)
[![npm](https://img.shields.io/npm/dm/react-native-payments.svg?style=flat-square)](https://www.npmjs.com/package/react-native-payments)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)

Accept Payments with Android Pay using the [Payment Request API](https://paymentrequest.show).

__Features__
- __Simple.__ No more checkout forms.
- __Effective__. Faster checkouts that increase conversion.
- __Future-proof__. Use a W3C Standards API, supported by companies like Google, Firefox and others.
- __Cross-platform__. Share payments code between your iOS, Android, and web apps.
- __Add-ons__. Easily enable support for Stripe or Braintree without add-ons.


<div>
 <img width="280px" src="https://github.com/JadavChirag/react-native-GPay/blob/master/image/Google-Pay-3.jpg" />
<img width="280px" src="https://github.com/JadavChirag/react-native-GPay/blob/master/image/Google-Pay-1.png" />
<img width="280px" src="https://github.com/JadavChirag/react-native-GPay/blob/master/image/Google-Pay-2.png" />
</div>


## Table of Contents
- [Sample](#Sample)
- [Installation](#installation)
- [Usage](#usage)
- [Testing Payments](#testing-payments)
- [Google Pay button](#google-pay-button)
- [API](#api)
- [Resources](#resources)
- [License](#license)

# react-native-gpay


## Sample
You can run the sample by cloning the project and running:

```shell
$ react-native run android
```

## Getting started

`$ npm install react-native-gpay --save`

### Mostly automatic installation

`$ react-native link react-native-gpay`

### Manual installation


#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-gpay` and add `RNGpay.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNGpay.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.reactlibrary.RNGpayPackage;` to the imports at the top of the file
  - Add `new RNGpayPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-gpay'
  	project(':react-native-gpay').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-gpay/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-gpay')
  	```

#### Windows
[Read it!](https://github.com/ReactWindows/react-native)

1. In Visual Studio add the `RNGpay.sln` in `node_modules/react-native-gpay/windows/RNGpay.sln` folder to their solution, reference from their app.
2. Open up your `MainPage.cs` app
  - Add `using Gpay.RNGpay;` to the usings at the top of the file
  - Add `new RNGpayPackage()` to the `List<IReactPackage>` returned by the `Packages` method


## Usage
  
  ▪	Setting up Google Pay/Android Pay
	▪	Importing the Library
	▪	Initializing the Payment Request
	▪	Displaying the Payment Request
	▪	Aborting the Payment Request (Support Only in Andriod)
	▪	Processing Payments
	▪	Dismissing the Payment Request
  
  ##	Setting up Google Pay/Android Pay
	
  Before you can start accepting payments in your App, you'll need to setup Google Pay and/or Android Pay.
	▪	Android Pay

	1.	Add Android Pay and Google Play Services to your dependencies
	2.	Enable Android Pay in your Manifest

	Google has documentation on how to do this in their _[Setup Android Pay]        (https://developers.google.com/pay/api/android/guides/setup)_ guide.
  

##	Importing the Library
```javascript
import GPay, { GooglePayImage } from 'react-native-gpay'
```
##	 Initializing the Payment Request
	To initialize a Payment Request, you'll need to provide Payment Request details.

▪  Card Networks Details (Suported Brands)
 
  ```
    const cardNetworks = ['AMEX', 'JCB', 'MASTERCARD', 'VISA'] // G-PAY SUPPORT CARD.

  ```
  
▪  Payment Request Details
    
    The Payment Request is where you defined the forms of payment that you accept. To enable Android Pay, we'll define a payment gateway of android-pay. We're also required to pass a data object to configures android Pay. This is where we provide our merchant id, define the supported card types and the currency we'll be operating in.
   
   GATEWAY INTEGRATION
   ```
    const paymentRequest = {
      cardPaymentMethodMap: {
        gateway: {
          name: 'GATEWAY_NAME', // Identify your gateway and your app's gateway merchant identifier     https://developers.google.com/pay/api/android/reference/object#PaymentMethodTokenizationSpecification
          merchantId: '055XXXXXXXXXXXXX336',  // YOUR_GATEWAY_MERCHANT_ID
          clientKey: 'sandbox_XXXXXXXXXXXXndxm44jw', // OPTIONAL YOUR_TOKENIZATION_KEY. Need for BRAINTREE & STRIPE GATEWAY.
          sdkVersion: 'client.VERSION' // OPTIONAL YOUR Client.VERSION. Need for BRAINTREE & STRIPE GATEWAY.
        },
        cardNetworks
      },
      transaction: {
        totalPrice: '11',
        totalPriceStatus: 'FINAL', // PAYMENT AMOUNT STATUS 
        currencyCode: 'USD' // CURRENCY CODE
      },
        merchantName: 'XXXXXXXXXXXX'  // MERCHANT NAME Information about the merchant requesting payment information
      }
  ```

  DIRECT 
    ```
    const paymentRequest = {
      cardPaymentMethodMap: {
        gateway: {
          protocolVersion: 'ECv1',
          publicKey: 'BC9u7amr4kFD8qsdxnEfWV7RPDR9v4gLLkx3jfyaGOvxBoEuLZKE0Tt5O2jMMxJ9axHpAZD2Jhi4E74nqxr944=',
        },
        cardNetworks
      },
      transaction: {
        totalPrice: '11',
        totalPriceStatus: 'FINAL', // PAYMENT AMOUNT STATUS 
        currencyCode: 'USD' // CURRENCY CODE
      },
        merchantName: 'XXXXXXXXXXXX'  // MERCHANT NAME Information about the merchant requesting payment information
      }
  ```

▪  Check Google Pay (Android-Pay) Support.

  This function for check GPay Support Your App and Devices?

  ```
  onPressCheck = async () => {
    const isAvailable = await GPay.checkGPayIsEnable(
      GPay.ENVIRONMENT_TEST, // You can change environment here ENVIRONMENT_TEST,ENVIRONMENT_PRODUCTION
      cardNetworks
    ).catch(error => {
      console.warn(error.toString())
      return false
    })
    this.setState({ isAvailable })
  }
  ```
  
Once you've defined your paymentrequest you're ready to initialize your Payment Request.

```es6
  const paymentRequestToken = GPay(GPay.ENVIRONMENT_TEST, paymentRequest);
```

## Displaying the Payment Request

Now that you've setup your Payment Request Token, displaying it is as simple as calling the show method.

```paymentRequestToken.show();```

## Processing Payments

Now that we know how to initialize, display, and dismiss a Payment Request, let's take a look at how to process payments.

When a user accepts to pay, GPay.show will resolve to a Payment Response.	
// You can change environment here ENVIRONMENT_TEST,ENVIRONMENT_PRODUCTION

```
  GPay.show(GPay.ENVIRONMENT_TEST, paymentRequest).then(function(response) {
    console.log("sucess", response);
    // Process response
  }).catch(function(err) {
    console.log("Uh oh, something bad happened", err.message);
    // Handle Error
  });
```

You can learn more about server-side decrypting of Payment Tokens on Google Payment Token Format Reference documentation.

GOOGLE PAY
https://developers.google.com/pay/api/android/guides/resources/payment-data-cryptography#public-key-format
https://developers.google.com/pay/api/android/guides/tutorial

## API

NativePayments
PaymentRequest
PaymentRequestUpdateEvent
PaymentResponse

## Resources

		▪ Payment Request
		▪ Introducing the Payment Request API
		▪ Deep Dive into the Payment Request API
		▪ Web Payments
	 	▪ Android Pay
		▪ Setup Android Pay
		▪ Tutorial
		▪ Brand Guidelines
		▪ Gateway Token Approach
		▪ Network Token Approach

## License

Licensed under the MIT License, Copyright © 2018, Chirac Jadav.

See [LICENSE](https://github.com/JadavChirag/react-native-GPay/blob/master/LICENSE) for more information.


