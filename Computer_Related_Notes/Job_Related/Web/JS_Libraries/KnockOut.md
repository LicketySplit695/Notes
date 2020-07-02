# KnockOut

[toc]

# Introduction

* Knockout is a JavaScript library that helps you to create rich, responsive display and editor user interfaces with a clean underlying data model. 
* Any time you have sections of UI that update dynamically (e.g., changing depending on the user’s actions or when an external data source changes).
* Uses Declarative bindings to connect parts of UI to the data model.
* KnockOut is built around
  1. Observables and Dependency Tracking
  2. Declarative Bindings
  3. Templating



### KnockOut vs jQuery

* jQuery is a replacement for the clunky and inconsistent DOM API.

* Its a low-level way to manipulate elements and event handlers in a web page.

* KnockOut provides a complementary, high-level way to link a data model to a UI.

  

### The M-V-VM Architecture

 We have 3 parts:

1. **A Model**: ( Reusable Code – DATA )

   Objects that represent the structure of Data.

2. **A ViewModel**: ( Reusable Code – LOGIC )

   Link between Model and View OR It Retrieves data from Model and exposes it to the View. This is the model specifically designed for the View.

3. **A View**:  ( Platform Specific Code – USER INTERFACE )

   Interactive UI representing the state of the view model. It displays information from the view model, sends commands to the view model (e.g., when the user clicks buttons), and updates whenever the state of the view model changes.

   <img src="JS_Lib_Images/KO_MVVMPattern.png" alt="MVVM Patter" style="zoom:85%;" />

---

#### Points on M-V-VM Pattern

1. We are on the client side so we can hold on to objects and do a lot more logic in a non-disconnected
   state.
2. Whereas MVC is typically used when things are transactional and disconnected as is the case with server-side web.
3. Debugging M-V-VM would be a bit difficult when we have complex data bindings.

---



#### Activating KnockOut

A simple view model is as follows:

```javascript
let myViewModel = {
    personName: "Bob";
    personAge: 23
};
```

The view model could be used as:

```html
<p>
    The name is <span data-bind="text: personName"></span>
</p>
```

To activate the `data-bind` attribute we need to use:

```javascript
ko.applyBindings(myViewModel);
```

`applyBindings(...)` takes the following parameters:

1. view model object we want to use with the declarative bindings it is supposed to activate

2. Optionally, we can pass a second parameter to define which part of the document we want to search for `data-bind` attributes. For ex:

   ```javascript
   ko.applyBindings(myViewModel, document.getElementById('someElementId'))
   ```

   This restricts the activation to the element with ID `someElementId` and its descendants which is useful if we have *multiple view models*.

   

# Observables

To update the UI *automatically* when the view model changes, the associated *view model* properties must be declared as *observables*. 

Doing so we create JavaScript objects that can notify subscribers about changes, and can automatically detect dependencies.

```javascript
var myViewModel = {
    personName: ko.observable('Bob'),
    personAge: ko.observable(123)
};
```



### Reading & Writing Observables

`ko.observable` objects are actually *functions* and not JS properties for browser compatibility. Thus

* To **read** the current value:

```javascript
myViewModel.personName()	// Gives -> 'Bob'
```

* To **write** a new value to the observable

```javascript
myViewModel.personName('Mary')	// The value changes to 'Mary'
```

* To write values to **multiple observable properties** on a model object, you can use *chaining syntax*.

```javascript
myViewModel.personName('Mary').personAge(50)
```

So, when we wrote `data-bind="text: personName"`, the `text` binding registered itself to be notified when `personName` changes.



### Explicitly Subscribing to Observables

To be notified when an observable change:

```javascript
let subscription = myViewModel.personName.subscribe(function(newValue) {
    alert("The person's new name is " + newValue);
});
```

`subscribe(...)` accepts 3 parameters:

1. `callback` is the function that is called whenever the notification happens
2. `target` (optional) defines the value of `this` in the callback function
3. `event` (optional; default is `"change"`) is the name of the event to receive notification for

To terminate a subscription:

```javascript
subscription.dispose();
```



#### Reacting to a specific observable event [^1]

We can do that using `ko.when()`

```javascript
ko.when(function () {
    return myViewModel.personName() !== undefined;
}, function (result) {
    myViewModel.isInitialized(true);
});
```

* `ko.when` waits until the first function (`predicate`) returns `true` or a true-ish value, at which time it runs the second function (`callback`), passing the `predicate` result.
* optionally a third parameter (`context`) can be passed in that defines the value of `this` for the predicate and callback functions. `ko.when` returns a `subscription` object that you can use the cancel the action.

* `ko.when` can also be called with just the `predicate` function. In that case, it returns a `Promise` that will be resolved with the `predicate` result once the `predicate` returns a true-ish value.



#### Using the `extend(...)` method

* When writing to an observable that contains a primitive value (a number, string, boolean, or null), the dependencies of the observable are normally only notified if the value actually changed.

* We can use the built-in `notify` [extender](https://knockoutjs.com/documentation/extenders.html) to ensure that an observable’s subscribers are always notified on a write, even if the value is the same.

  ```javascript
  myViewModel.personName.extend({ notify: 'always' });
  ```

* To limit or delay the observable’s change notifications

  ```javascript
  // Ensure it notifies about changes no more than once per 50-millisecond period
  myViewModel.personName.extend({ rateLimit: 50 });
  ```



## Observable Arrays

* To detect and respond to changes of a *collection of things*, we use an `observableArray`. 

* Useful while displaying or editing multiple values and need repeated sections of UI to appear and disappear as items are added and removed.

```javascript
var myObservableArray = ko.observableArray();	// Initially an empty array
myObservableArray.push('Some value');	// Adds the value and notifies observers
```

A live example on on data-binding with observable array can be found [here](https://knockoutjs.com/examples/simpleList.html).

---

#### Note

* An `observableArray` tracks which objects are *in* the array, *not* the state of those objects. Simply putting an object into an `observableArray` doesn’t make all of that object’s properties themselves observable. 

* An `observableArray` just tracks which objects it holds, and notifies listeners when objects are added or removed.

---



### Reading information from an `observableArray`

* an `observableArray` is actually an [observable](https://knockoutjs.com/documentation/observables.html) whose value is an array.

* We can get the underlying JavaScript array by invoking the `observableArray` as a function with no parameters as

  ```javascript
  alert('The length of the array is ' + myObservableArray().length);
  alert('The first element is ' + myObservableArray()[0]);
  ```

  KO’s `observableArray` has JS equivalent functions of its own, and they’re more useful sometimes for example:

  ```javascript
  myObservableArray.indexOf('Blah')
  // -> return the zero-based index of the first array entry that equals Blah, or the value -1 if no matching value was found. Works in all browsers
  ```

  Similarly other set of functions exposed by observableArray can be found [here](https://knockoutjs.com/documentation/observableArrays.html).



### Delaying and/or suppressing change notifications

Normally, an `observableArray` notifies its subscribers immediately, as soon as it’s changed. To limit or delay change notifications use the [`rateLimit` extender](https://knockoutjs.com/documentation/rateLimit-observable.html):

```javascript
// Ensure it notifies about changes no more than once per 50-millisecond period
myViewModel.myObservableArray.extend({ rateLimit: 50 });
```



### Tracking array changes

Knockout also provides a super-fast method to find out how an observable array has changed (i.e., which items were just added, deleted, or moved) by subscribing to array changes:

```javascript
obsArray.subscribe(fn, thisArg, "arrayChange");
```

Advantages:

* Performance is `O(1)` in most cases. Knockout supplies the change log without running any difference algorithm. Knockout only falls back on an algorithm if an arbitrary change is made without using a typical array mutation function.
* The change log just gives the items that actually changed.



## Computed Observables

These are functions that are dependent on one or more other observables, and will automatically update whenever any of these dependencies change. For ex:

```javascript
function AppViewModel() {
    this.firstName = ko.observable('Bob');
    this.lastName = ko.observable('Smith');
    this.fullName = ko.computed(function() {
        return this.firstName() + " " + this.lastName();
    }, this);
}
```

And data binding in the view:

```html
The name is <span data-bind="text: fullName"></span>
```



---

[^1]: This was added in Knockout 3.5.