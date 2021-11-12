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

## Send NEAR to others

##### Method: sendMoney

```javascript
/**
 * @param {*} receiverId received account id
 * @param {*} amount send near amount
 */
sendMoney = ({ receiverId, amount })
```

##### Example

```javascript
const res = await window.wallet.sendMoney({
  receiverId: 'xxx.testnet',
  amount: parseNearAmount('1'),
})

console.log('send near res: ', res);
```

## Sign and send transaction

##### Method: signAndSendTransaction

```javascript
/**
* 
* @param {*} receiverId receiver account id
* @param {*} actions function call actions { methodName, args, gas, deposit, msg }
* @param {*} usingAccessKey If 'true', will using access key to make function call and no need to request user to sign this transaction. Set 'false' will popup a notification window to request user to sign this transaction.
* 
* @returns the result of send transaction
*/
signAndSendTransaction = ({ receiverId, actions, usingAccessKey = false })
```

##### Examples
```javascript
// Contract method function call
// Say hi to the contract by using access key
const options = {
  receiverId: 'dev-1635836502908-29682237937904',
  actions: [
    {
      methodName: 'sayHi',
      args: {},
      gas: parseNearAmount('0.00000000003'),
      deposit: parseNearAmount('0.00125'),
    },
  ],
  usingAccessKey: true,
}
const res = await window.wallet.signAndSendTransaction(options);
```

```javascript
// Contract method function call
// Deposit storage to wrap.testnet
const options = {
  receiverId: 'wrap.testnet',
  actions: [
    { 
      methodName: 'storage_deposit',
      args: {
        account_id: 'xxx.testnet',
        registration_only: true,
      },
      gas: parseNearAmount('0.00000000003'),
      deposit: parseNearAmount('0.00125'),
    },
  ],
}
const res = await window.wallet.signAndSendTransaction(options);
```


```javascript
// Contract method function call
// Swap NEAR to wNEAR, then transfer wNEAR to others
const options = {
  receiverId: 'wrap.testnet',
  actions: [
    {
      methodName: 'near_deposit',
      args: {},
      deposit: parseNearAmount('1'),
    },
    {
      methodName: 'ft_transfer',
      args: {
        received_id: 'xxxx.testnet',
        amount: '1000000000000000000',
      },
      gas: parseNearAmount('0.00000000003'),
      deposit: parseNearAmount('0.00125'),
    }
  ]
}
const res = await window.wallet.signAndSendTransaction(options);
```
