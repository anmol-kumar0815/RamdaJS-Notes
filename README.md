Ramda js notes
=> Ramdajs never mutate any data.

1. remove(index, count, array) => remove "count" elements starting from "index" in array.
const ary = [1,2,3,4,5]
const newAry = remove(1,2,ary);
console.log(newAry); //[1,4,5]
=================================================================================================================================================
2. Currying => Pass the properties as the first set of arguments and then we pass towards the end the actual data that we want to manipulate.
=> Currying doesn’t call a function. It just transforms it.
const ary = [1,2,3,4,5]
const removeFirstTwoElement = remove(0, 2);
removeFirstTwoElement(ary) //[3,4,5]
=> now we can use this function many times in order to remove first two element from an array.
remove(0,2)(arr) // same result as before
remove(0)(2)(arr) // same result as before
remove(0, 2, arr) // same result as before
=================================================================================================================================================
3. assoc(key, newValue, object) => It creats shallow copy of an object and changes value of a "key", original object will remains same.
it can also add a key, value pair.
Example: 1
const obj = {
    name: "Anmol",
}
const newObj = assoc("name", "Ranju", obj)
console.log(newObj) // { name: "Ranju" }

Example: 2
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};

const newObj = assoc("name", "Anmol", order);
console.log(newObj)
/*
    {
        id: 1,
        name: "Anmol",  //this name is updated
        quantity: 10,
        customer: {
            name: "John", //but this is same as previous
            address: {
                city: "Miami",
                pin: 111111,
            }
        }
    }
*/
=================================================================================================================================================
4. dissoc(key, object) => it will remove the key value pair from the object.
const obj = { name: "Anmol" }
const newObject = dissoc("name", obj);
console.log(newObject); // {}
=================================================================================================================================================
5. assocPath(["key", "nestedKey"], value, object) => it is same as assos i.e update value of a key inside an object, but it works on nested object.
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
const newOrder = assocPath(["customer", "address", "pin"], 110003, order);
console.log(newOrder);
/*
    {
        id: 1,
        name: "ORD1",
        quantity: 10,
        customer: {
            name: "John",
            address: {
                city: "Miami",
                pin: 110003,    //updated value
            }
        }
    }
*/
note: assoc methods cannot update multiple properties at a time. merge methods i.e mergeLeft/mergeRight can update multiple properties with a single statement.
=================================================================================================================================================
6. dissocPath(["key", "nestedKey"], object) => it is same as dissoc i.e it will remove the key value pair from the object, but it works on nested Object.
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
const newOrder = dissocPath(["customer", "address", "pin"], order);
console.log(newOrder);
/*
    {
        id: 1,
        name: "ORD1",
        quantity: 10,
        customer: {
            name: "John",
            address: {
                city: "Miami",
                                //pin key is removed
            }
        }
    }
*/
=================================================================================================================================================

7. mergeLeft(Object1, object2) => it will merge both objects and create a new one, but if both object has some same key then mergeLeft will take value from object1.
const object1 = { name: "Anmol" }
const object2 = { name: "Ranju", age: 20 }
const newObject = mergeLeft(object1, object2)
console.log(newObject) // { name: "Anmol", age: 20 }
=================================================================================================================================================

8. mergeRight(Object1, object2) => it will merge both objects and create a new one, but if both object has some same key then mergeRight will take value from object2.
const object1 = { name: "Anmol" }
const object2 = { name: "Ranju", age: 20 }
const newObject = mergeRight(object1, object2)
console.log(newObject) // { name: "Ranju", age: 20 }
=================================================================================================================================================

7. mergeDeepLeft({key: value}, object) => works same as mergeLeft but it also works on nested Object
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
const newObject = mergeDeepLeft({ id: 2, friend: "Ranju", customer: { address: { pin: 110003 } } }, order);
console.log(newObject)
/*
    {
        id: 2,   //changes by 1
        friend: "Ranju",    //added a new key value pair
        name: "ORD1",
        quantity: 10,
        customer: {
            name: "John",
            address: {
                city: "Miami",
                pin: 110003,    //changes from 111111
            }
        }
    }
*/
=================================================================================================================================================

8. mergeDeepRight({ key: value }, object) => works same as mergeRight but it also works on nested Object.
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
const newObject = mergeDeepRight({ id: 2, friend: "Ranju", customer: { address: { pin: 110003 } } }, order);
console.log(newObject)
/*
    {
        id: 1,   //remains same
        friend: "Ranju",    //added a new key value pair
        name: "ORD1",
        quantity: 10,
        customer: {
            name: "John",
            address: {
                city: "Miami",
                pin: 110003,    //remains same
            }
        }
    }
*/
Question. Preferring left vs right?
Answer. preferring left would be when we have to make sure a particular set of values as always assigned to certain properties
// We want to ensure every order object will have details of the current user.
const USER_DETAILS = {
    user: currentUser,
    deliveryAddress: currentUser.address
  };

  // main code
  const { orders } = await ordersApi.fetch();
  return orders.map(mergeLeft(USER_DETAILS));
preferring right would be when we have to set default values to certain properties, if they already do not have a value set.
const ORDER_DEFAULTS = {
    orderDispatched: false,
    shippingMode: "free"
};

// main code
const { orders } = await ordersApi.fetch();
return orders.map(mergeRight(ORDER_DEFAULTS));
=================================================================================================================================================

9. modify(key, prevValue => change in prevValue, object) => The assoc and merge methods can only set an external value to the data.
If we want to update the data using previous state of that key, then we can use modify

Example: 1
const person = {name: 'James', age: 20, pets: ['dog', 'cat']};
modify('age', previousAge => previousAge + 1, person); // {name: 'James', age: 21, pets: ['dog', 'cat']}
//the above statement can be written using a ramda helper like so:
modify('age', add(1), person); //=> {name: 'James', age: 21, pets: ['dog', 'cat']}

Example: 2
modify('pets', append('turtle'), person); //=> {name: 'James', age: 20, pets: ['dog', 'cat', 'turtle']}
=================================================================================================================================================

10. modifyPath(["keys", "nested keys"], prevValue=> PrevValue + values, object) => it is same as modify but it can work on nested objects.
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
modifyPath(["customer", "address", "pin"], inc, order); // inc method is from Ramda= => it will do increment in the value that it receive and return that value.
/*
{
  id: 1,
  name: "ORD1",
  quantity: 10,
  customer: {
    name: "John",
    address: {
      city: "Miami",
      pin: 111112, //=> increment by 1
    }
  }
}
*/
=================================================================================================================================================

11. evolve({ nestedObjectKey: prevValue => prevValue + someChange}, object) => it basically update any key value where you can use their previous state values.
It works same as modifyPath but it can update any number of nested keys in one go.
const order = {
    id: 1,
    name: "ORD1",
    quantity: 10,
    customer: {
      name: "John",
      address: {
        city: "Miami",
        pin: 111111,
      }
    }
};
evolve(
    {
        id: prevValue => prevValue + 1,
        customer: {
            name: prevName => prevName + " kumar",
        }
    },
    order
)

/*
{
  id: 2,    //updated id
  name: "ORD1",
  quantity: 10,
  customer: {
    name: "John Kumar",   //updated name
    address: {
      city: "Miami",
      pin: 111112, //=> increment by 1
    }
  }
}
*/
=================================================================================================================================================

12. pipe(function1(), function2()) => it will pass the result from one function to another, in a left to right order.
const positiveCalc = pipe(Math.abs, add(2), multiply(2), subtract(__, 2));
positiveCalc(2); //=> 6

13. fromPairs: Creates a new object from a array of key-value pairs. If a key appears in multiple pairs, then the rightmost pair is included in the object.
fromPairs([["a", 1], ["b", 2], ["c", 3]]) // {"a": 1, "b": 2, "c": 3}

14. toPairs: Converts an object into an array of key, value arrays.
toPairs({ a: 1, b: 2, c:3 }) // [["a", 1], ["b", 2], ["c", 3]]
=================================================================================================================================================

State updates in React
// longer version
setUser(prevUser => {...prevUser, firstName: "Oliver"})

// ramda version of above code
setUser(assoc("firstName", "Oliver")) //=> curries prevUser as 3rd param
=================================================================================================================================================

15. eqProps("key", object1, object2) => compare a key value of two objects.
const o1 = { a: 1, b: 2, c: 3, d: 4 };
const o2 = { a: 10, b: 20, c: 3, d: 40 };
R.eqProps('a', o1, o2); //=> false
R.eqProps('c', o1, o2); //=> true
=================================================================================================================================================

16. forEachObjIndexed => Iterate over an input object, calling a provided function fn for each key and value in the object.
fn receives three argument: (value, key, obj).
const person = { name: "Anmol", age: 22 }
const print = (value, key) => console.log(key, value)
forEachObjIndexed(print, person)
=================================================================================================================================================

17. has("key", object) => return true if object has that key otherwise false.
const person = { name: "Anmol", age: 22 }
console.log(has("name", person))    //true
=================================================================================================================================================

18. hasPath(["key", "nested_key"], object) => retrun true is an object has specified path otherwise false.
hasPath(['a', 'b'], {a: {b: 2}});         // => true
hasPath(['a', 'b'], {a: {b: undefined}}); // => true
hasPath(['a', 'b'], {a: {c: 2}});         // => false
hasPath(['a', 'b'], {});                  // => false
=================================================================================================================================================

19. invert(object) => it will put object key into an array which were having duplicate values.

const persons = {
    first: 'Ranju',
    second: 'Anmol',
    third: 'Anmol',
  };

invert(persons);    // { Ranju: ["first"], Anmol: ["second", "third"] }
=================================================================================================================================================

20. map(callbackFunction, array/object) => it is similar as js map function but it can also run over an Object.

const double = x => x * 2;
map(double, [1, 2, 3]); //=> [2, 4, 6]
map(double, {x: 1, y: 2, z: 3}); //=> {x: 2, y: 4, z: 6}
=================================================================================================================================================

21. mapObjIndexed(callbackFunctin, object) => it is similar as map but it is only for object not for array, Its callback function takes 3 arguments as
values, keys, object
Note: if you are working only with objects value then use map() instead.

const object = { x: 1, y: 2, z: 3 };
const fun = (num, key, obj) => key + " => " + (num * 2);

mapObjIndexed(fun, object); //=> { x: 'x => 2', y: 'y => 4', z: 'z => 6' }
=================================================================================================================================================

22. omit(["key", "key"], object) => Returns a partial copy of an object omitting or excluding the keys specified.
omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
=================================================================================================================================================

23. path(["key", "nested key"], object) => Retrieve the value at a given path inside a nested Object.
path(['a', 'b'], {a: {b: 2}}); //=> 2
path(['a', 'b'], {c: {b: 2}}); //=> undefined
path(['a', 'b', 0], {a: {b: [1, 2, 3]}}); //=> 1
path(['a', 'b', -2], {a: {b: [1, 2, 3]}}); //=> 2
=================================================================================================================================================

24. pathOr(defaultValue, ["path"], object) => it is similar as path, if path exist then return path value else return default values.
pathOr('Not Found', ['a', 'b'], {a: {b: 2}}); //=> 2
pathOr('Not Found', ['a', 'b'], {c: {b: 2}}); //=> 'Not Found'
=================================================================================================================================================

25. props(["key"], object) => array of keys in, array of values out. Preserves order.
const person = { name: "Anmol", age: 22 }
props(["name", "age"], person) => ["Anmol", 22]
=================================================================================================================================================

26. unwind("key on which you want to unwind", object) => Deconstructs an array field from the input documents to output a document for each element.
Each output document is the input document with the value of the array field replaced by the element.
unwind('hobbies', {
    name: 'alice',
    hobbies: ['Golf', 'Hacking'],
    colors: ['red', 'green'],
  });
  // [
  //   { name: 'alice', hobbies: 'Golf', colors: ['red', 'green'] },
  //   { name: 'alice', hobbies: 'Hacking', colors: ['red', 'green'] }
  // ]

=================================================================================================================================================

27. values(object) => it will get an array with the values of an Object.
values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]
=================================================================================================================================================

28. test(RegExp, string) => Determines whether a given string matches a given regular expression.
R.test(/^x/, 'xyz'); //=> true
R.test(/^y/, 'xyz'); //=> false
=================================================================================================================================================

29. filter(callbackFunction, object/map) => return filtered value as per the callback function.
const isEven = n => n % 2 === 0;
filter(isEven, [1, 2, 3, 4]); //=> [2, 4]
filter(isEven, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
=================================================================================================================================================
