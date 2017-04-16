Visitor
=======


## Definition

Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.


## Summary

The Visitor pattern defines a new operation to a collection of objects without changing the objects themselves. The new logic resides in a separate object called the Visitor.

Visitors are useful when building extensibility in a library or framework. If the objects in your project provide a 'visit' method that accepts a Visitor object which can make changes to the receiving object then you are providing an easy way for clients to implement future extensions.

In most programming languages the Visitor pattern requires that the original developer anticipates functional adjustments in the future. This is done by including methods that accept a Visitor and let it operate on the original collection of objects.

Visitor is not important to JavaScript because it offers far more flexibility by the ability to add and remove methods at runtime. To learn more about this flexibility and how it benefits JavaScript patterns and pattern architectures see [JavaScript + jQuery Design Pattern Framework](http://www.dofactory.com/products/javascript-jquery-design-pattern-framework).


## Diagram

<img src="./javascript-visitor.jpg" alt="Visitor Diagram">


## Participants

The objects participating in this pattern are:

- **ObjectStructure** -- In sample code: _employees array_
    * maintains a collection of Elements which can be iterated over
- **Elements** -- In sample code: _Employee objects_
    * defines an accept method that accepts visitor objects
    * in the accept method the visitor's visit method is invoked with 'this' as a parameter
- **Visitor** -- In sample code: _ExtraSalary, ExtraVacation_
    * implements a visit method. The argument is the Element being visited. This is where the Element's changes are made


## Sample code in JavaScript

In this example three employees are created with the Employee constructor function. Each is getting a 10% salary raise and 2 more vacation days. Two visitor objects, ExtraSalary and ExtraVacation, make the necessary changes to the employee objects.

Note that the visitors, in their visit methods, access the closure variables salary and vacation through a public interface. It is the only way because closures are not accessible from the outside. In fact, salary and vacation are not variables, they are function arguments, but it works because they are also part of the closure.

Notice the self variable. It is used to maintain the current context (stored as a closure variable) and is used in the visit method.

The log function is a helper which collects and displays results.


```javascript
var Employee = function(name, salary, vacation){
    var self = this;

    this.accept = function(visitor){
        visitor.visit(self);
    };

    this.getName = function(){
        return name;
    };

    this.getSalary = function(){
        return salary;
    };

    this.setSalary = function(sal){
        salary = sal;
    };

    this.getVacation = function(){
        return vacation;
    };

    this.setVacation = function(vac){
        vacation = vac;
    };
};

var ExtraSalary = function(){
    this.visit = function(emp){
        emp.setSalary(emp.getSalary() * 1.1);
    };
};

var ExtraVacation = function(){
    this.visit = function(emp){
        emp.setVacation(emp.getVacation() + 2);
    };
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
    var employees = [
        new Employee("John", 10000, 10),
        new Employee("Mary", 20000, 21),
        new Employee("Boss", 250000, 51)
    ];

    var visitorSalary = new ExtraSalary();
    var visitorVacation = new ExtraVacation();

    for (var i = 0, len = employees.length; i < len; i++){
        var emp = employees[i];

        emp.accept(visitorSalary);
        emp.accept(visitorVacation);
        log.add(emp.getName() + ": $" + emp.getSalary() +
            " and " + emp.getVacation() + " vacation days");
    }

    log.show();
}
```

Source: [dofactory.com](http://www.dofactory.com/javascript/visitor-design-pattern)