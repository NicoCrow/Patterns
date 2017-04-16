Adapter
=======


## Definition

Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.


## Summary

The Adapter pattern translates one interface (an object's properties and methods) to another. Adapters allows programming components to work together that otherwise wouldn't because of mismatched interfaces. The Adapter pattern is also referred to as the Wrapper Pattern.

One scenario where Adapters are commonly used is when new components need to be integrated and work together with existing components in the application.

Another scenario is refactoring in which parts of the program are rewritten with an improved interface, but the old code still expects the original interface.


## Diagram

<img src="./javascript-adapter.jpg" alt="Adapter Diagram">


## Participants

The objects participating in this pattern are:

- **Client** -- In sample code: the run() function.
    * calls into Adapter to request a service
- **Adapter** -- In sample code: _ShippingAdapter_
    * implements the interface that the client expects or knows
- **Adaptee** -- In sample code: _AdvancedShipping_
    * the object being adapted
    * has a different interface from what the client expects or knows


## Sample code in JavaScript

The example code below shows an online shopping cart in which a shipping object is used to compute shipping costs. The old Shipping object is replaced by a new and improved Shipping object that is more secure and offers better prices.

The new object is named AdvancedShipping and has a very different interface which the client program does not expect. ShippingAdapter allows the client program to continue functioning without any API changes by mapping (adapting) the old Shipping interface to the new AdvancedShipping interface.

The log function is a helper which collects and displays results.


```javascript
// old interface
function Shipping(){
    this.request = function(zipStart, zipEnd, weight){
        // ...
        return "$49.75";
    }
}

// new interface
function AdvancedShipping(){
    this.login = function(credentials){
        /* ... */
    };
    this.setStart = function(start){
        /* ... */
    };
    this.setDestination = function(destination){
        /* ... */
    };
    this.calculate = function(weight){
        return "$39.50";
    };
}

// adapter interface
function ShippingAdapter(credentials){
    var shipping = new AdvancedShipping();

    shipping.login(credentials);

    return {
        request: function(zipStart, zipEnd, weight){
            shipping.setStart(zipStart);
            shipping.setDestination(zipEnd);
            return shipping.calculate(weight);
        }
    };
}

// log helper
var log = (function(){
    var log = "";

    return {
        add: function(msg){
            log += msg + "\n";
        },
        show: function(){
            alert(log);
            log = "";
        }
    }
})();

function run(){
    var shipping = new Shipping();
    var credentials = {
        token: "30a8-6ee1"
    };
    var adapter = new ShippingAdapter(credentials);

    // original shipping object and interface

    var cost = shipping.request("78701", "10010", "2 lbs");
    log.add("Old cost: " + cost);

    // new shipping object with adapted interface

    cost = adapter.request("78701", "10010", "2 lbs");

    log.add("New cost: " + cost);
    log.show();
}
```

Source: [dofactory.com](http://www.dofactory.com/javascript/adapter-design-pattern)