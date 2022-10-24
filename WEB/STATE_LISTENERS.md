# State Listeners

The Pay Theory SDK provides a set of event listeners that can be used to know certain state values in the hosted fields and when certain payment method fields are valid. 

These listeners are available on the object returned by the `create` function.

Each of these listeners takes a callback function as an argument. Below we will detail the arguments that are passed to each callback function.

## stateObserver

The `stateObserver` will fire when the state of any hosted field changes. You can use this listener to respond when a hosted field is focused, blurred, or when it has been typed into.

```javascript
myPayTheory.stateObserver(state => {
    // Logic to respond to state changes
})
```

### Callback Argument

The callback will be passed a state object. The state object will include an object with all the possible fields that can be used with the PayTheory SDK and consist of 3 pieces of information.
- **isFocused**: boolean indicating if the field is focused
- **isDirty**: boolean indicating if the field currently has text entered
- **errorMessages**: array of error messages if the field is invalid

*Note: If using the combined card field you will receive state updates for number, cvv, and exp separately*

```json
{
  "card-number": {
    "isDirty": false,
    "isFocused": false,
    "errorMessages": []
  },
  "card-cvv": ...,
  "card-exp": ...,
  "card-name": ...,
  "billing-line1": ...,
  "billing-line2": ...,
  "billing-city": ...,
  "billing-state": ...,
  "billing-zip": ...,
  "account-name": ...,
  "account-type": ...,
  "account-number": ...,
  "routing-number": ...,
  "cash-name": ...,
  "cash-contact": ...
}
```

This can be used to help if you want to display field specific error messages or style the fields based on the state.  
This can also be used to help if you are using the Card Billing fields to capture the payor info and want to ensure the fields are filled out.

## validObserver

The `validObserver` will fire when a set of hosted fields are valid. This is where you should add any logic that should be executed when the hosted fields are valid.

```javascript
myPayTheory.validObserver(valid => {
    // Logic to respond when the form is valid
})
```

### Callback Argument

The callback function will be passed a string that will contain the name of the payment type that is valid. This will be one of the following:

- `card`
- `ach`
- `cash`

There is a possibility that if you have multiple payment types mounted, that the string may contain multiple payment types. You could check for a certain payment type by using the `includes` method on the string.

```javascript
myPayTheory.validObserver(valid => {
    if (valid.includes("card")) {
        // Logic to respond when the card is valid
    }
})
```