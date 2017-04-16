Strategy
========


## Definition

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.


## Summary

The Strategy pattern encapsulates alternative algorithms (or strategies) for a particular task. It allows a method to be swapped out at runtime by any other method (strategy) without the client realizing it. Essentially, Strategy is a group of algorithms that are interchangeable.

Say we like to test the performance of different sorting algorithms to an array of numbers: shell sort, heap sort, bubble sort, quicksort, etc. Applying the Strategy pattern to these algorithms allows the test program to loop through all algorithms, simply by changing them at runtime and test each of these against the array. For Strategy to work all method signatures must be the same so that they can vary without the client program knowing about it.

In JavaScript the Strategy pattern is widely used as a plug-in mechanism when building extensible frameworks. This can be a very effective approach. To learn more check [JavaScript + jQuery Design Pattern Framework](http://www.dofactory.com/products/javascript-jquery-design-pattern-framework).


## Diagram

<img src="./javascript-strategy.jpg" alt="Strategy Diagram">


## Participants

The objects participating in this pattern are:

- **Context** -- In sample code: _Shipping_
    * maintains a reference to the current Strategy object
    * supports interface to allow clients to request Strategy calculations
    * allows clients to change Strategy
- **Strategy** -- In sample code: _UPS, USPS, Fedex_
    * implements the algorithm using the Strategy interface


## Sample code in JavaScript

In this example we have a product order that needs to be shipped from a warehouse to a customer. Different shipping companies are evaluated to determine the best price. This can be useful with shopping carts where customers select their shipping preferences and the selected Strategy returns the estimated cost.

Shipping is the Context and the 3 shipping companies UPS, USPS, and Fedex are the Strategies. The shipping companies (strategies) are changed 3 times and each time we calculate the cost of shipping. In a real-world scenario the calculate methods may call into the shipper's Web service. At the end we display the different costs.

The log function is a helper which collects and displays results.


```javascript
var Shipping = function(){
    this.company = "";
};

Shipping.prototype = {
    setStrategy: function(company){
        this.company = company;
    },

    calculate: function(package){
        return this.company.calculate(package);
    }
};

var UPS = function(){
    this.calculate = function(package){
        // calculations...
        return "$45.95";
    }
};

var USPS = function(){
    this.calculate = function(package){
        // calculations...
        return "$39.40";
    }
};

var Fedex = function(){
    this.calculate = function(package){
        // calculations...
        return "$43.20";
    }
};

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
    var package = {
        from: "76712",
        to: "10012",
        weigth: "lkg"
    };

    // the 3 strategies
    var ups = new UPS();
    var usps = new USPS();
    var fedex = new Fedex();

    var shipping = new Shipping();

    shipping.setStrategy(ups);
    log.add("UPS Strategy: " + shipping.calculate(package));
    shipping.setStrategy(usps);
    log.add("USPS Strategy: " + shipping.calculate(package));
    shipping.setStrategy(fedex);
    log.add("Fedex Strategy: " + shipping.calculate(package));

    log.show();
}
```

Source: [dofactory.com](http://www.dofactory.com/javascript/strategy-design-pattern)