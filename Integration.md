# Sender Wallet Extention Integration Tutorial

## Initial the wallet

##### Method: Init

```javascript
/**
* Initial the wallet. If current account has already sign in this contract, init() will auto signin
* @param {*} contractId contract account id
* @returns { accessKey } accessKey is empty string("") if has not request sign in before.
*/
init({ contractId })
```

##### Example

```javascript
const contractId = 'wrap.testnear';
const res = await window.wallet.init({ contractId });
```

## Get current account id

##### Method: getAccountId

```javascript
/**
* 
* @returns current account id
*/
getAccountId()
```

##### Example

```javascript
const accuntId = window.wallet.getAccountId();
```

## Request sign in

##### Method: requestSignIn

```javascript
/**
* 
* @param {*} contractId contract account id
* @param {*} methodNames the method names on the contract that should be allowed to be called. Pass null for no method names and '' or [] for any method names.
* @returns { accessKey } signed in access key
*/
requestSignIn({ contractId, methodNames })
```

##### Example

```javascript
const contractId = 'xxx';
const methodNames = [''];
const res = await window.wallet.requestSignIn({ contractId, methodNames });
```

## Sign out

##### Method: signOut

```javascript
// Clean the signed in accessKey from storage
signOut()
```

##### Example

```javascript
window.wallet.signOut();
```

## Check the account is signed in

##### Method: isSignedIn

```javascript
/**
* Check the current account is signed in with initial contract
* @returns true or false
*/
isSignedIn()
```

##### Example

```javascript
if (window.wallet.isSignedIn()) {
  // TODO
}
```

## Listen the current accmount changed

##### Method: onAccountChanged

```javascript
/**
* Listen the current account changed
* @param {*} callback (accountId) => { "TODO if account has changed" }
*/
onAccountChanged(callback)
```

##### Example

```javascript
window.wallet.onAccountChanged((changedAccountId) => {
  // TODO if account has changed
});
```

## Sign and send transaction

##### Method: signAndSendTransaction

```javascript
/**
* 
* @param {*} contractId contract account id
* @param {*} methodName contract method name
* @param {*} receiverId receiver account id
* @param {*} amount transfer near amount
* @param {*} params function call arguments
* @param {*} gas transaction gas
* @param {*} deposit transaction attached deposit
* @param {*} usingAccessKey If 'true', will using access key to make function call and no need to request user to sign this transaction. Set 'false' will popup a notification window to request user to sign this transaction.
* 
* @returns the result of send transaction
*/
signAndSendTransaction({ contractId, methodName, receiverId, amount, params, gas, deposit, usingAccessKey })
```

##### Examples
```javascript
// Normal transfer
// Transfer NEAR to others
const options = {
  receiverId: 'xxxx.testnet',
  amount: parseNearAmount('1'),
  usingAccessKey: false,
}
const res = await window.wallet.signAndSendTransaction(options);
```

```javascript
// Contract method function call
// Say hi to the contract by using access key
const options = {
  contractId: 'dev-1635836502908-29682237937904',
  methodName: 'sayHi',
  usingAccessKey: true,
}
const res = await window.wallet.signAndSendTransaction(options);
```

```javascript
// Contract method function call
// Deposit storage to wrap.testnet
const options = {
  contractId: 'wrap.testnet',
  methodName: 'storage_deposit',
  params: {
    account_id: window.wallet.accountId,
    registration_only: true,
  },
  gas: parseNearAmount('0.00000000003'),
  deposit: parseNearAmount('0.00125'),
}
const res = await window.wallet.signAndSendTransaction(options);
```


```javascript
// Contract method function call
// Swap NEAR to wNEAR
const options = {
  contractId: 'wrap.testnet',
  methodName: 'near_deposit',
  deposit: parseNearAmount('1'),
  gas, // optional
  deposit, // optional,
  usingAccessKey: false,
}
const res = await window.wallet.signAndSendTransaction(options);
```

```javascript
// Contract method function call
// Send wNEAR to others
const options = {
  contractId: 'wrap.testnet',
  methodName: 'ft_transfer',
  params: {
    received_id: 'xxxx.testnet',
    amount: '1000000000000000000',
  },
  gas, // optional
  deposit, // optional,
  usingAccessKey: false,
}
const res = await window.wallet.signAndSendTransaction(options);
```