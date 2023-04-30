# Lazy-Javascript
 

Lazy-javascript is a JavaScript library that provides a collection of methods to manipulate arrays and json data in a functional and lazy way. The Lazy class defined in the above code allows for chaining of array operations in a deferred manner. This means that the operations are not immediately performed on the original array but rather they are executed when the final result is requested. It also offers parallel processing of arrays using the mapAsync and filterAsync methods. The JsonHandler class provides a simple interface for working with JSON data that abstracts away many of the details of caching and sharding. When combined with the chainable methods provided by lazyjs, working with JSON data can become even easier and more intuitive.

## Installation

Lazy.js can be installed using npm:

```bash

npm i lazy-javascript

```

## Why use Lazy.js?
Lazy evaluation: lazyjs only evaluates the results of operations when necessary, which can help to reduce the amount of memory used when working with large datasets.

Parallel processing: lazyjs provides several methods for performing parallel processing on arrays, such as mapAsync and filterAsync, which can help to speed up operations that would otherwise be performed serially.

Code simplicity: The chainable methods provided by lazyjs can help to simplify code by allowing complex operations to be expressed as a sequence of simple method calls.

Flexibility: Because lazyjs operates on arrays, it can be used with any data source that can be converted to an array, such as JSON data retrieved from a REST API or a local file.

Ease of use: The JsonHandler class provides a simple interface for working with JSON data that abstracts away many of the details of caching and sharding. When combined with the chainable methods provided by lazyjs, working with JSON data can become even easier and more intuitive.

## Usage

Example using JsonHandler class to query data and using lazy class to chain array operations:

```javascript
  const queryData = () => {
    console.log('Querying data from API...');
    return Promise.resolve(jsonData);
  };
  
  // Use the JsonHandler to query the data
  jsonHandler.query(queryData)
    .then((data) => {
      console.log('Retrieved data:', data);
      
      // Use Lazy.js to chain array operations
      const lazy = new Lazy(data);
      const result = lazy
        .filter((person) => person.age > 30)
        .sort((person) => person.name)
        .map((person) => person.name)
        .join(', ');
      
      console.log('Result:', result);
    })
    .catch((error) => {
      console.error('Error:', error);
    });

    /*
      Output:
      Retrieved data: [
     { id: 1, name: 'Alice', age: 25 },
     { id: 2, name: 'Bob', age: 32 },
     { id: 3, name: 'Charlie', age: 18 },
     { id: 4, name: 'David', age: 43 },
     { id: 5, name: 'Emily', age: 28 },
    { id: 6, name: 'Frank', age: 39 },
    { id: 7, name: 'Grace', age: 21 },
    { id: 8, name: 'Henry', age: 37 }
    ]
    Result: Bob, David, Frank, Henry
    */

```

# Example using parralel caching and sharding

```js
const handler = new JsonHandler();

// Define the query functions
const queryFn1 = async () => {
  // Some expensive database query or other operation
  return { id: 1, name: "John Doe", age: 30 };
};

const queryFn2 = async () => {
  // Another expensive operation
  return { id: 2, name: "Jane Doe", age: 28 };
};

const queryFn3 = async () => {
  // Yet another expensive operation
  return { id: 3, name: "Bob Smith", age: 45 };
};

// Parallel caching
handler.queryParallel([queryFn1, queryFn2, queryFn3]).then((results) => {
  console.log(results);
});

// Sharding
handler.queryWithSharding([queryFn1, queryFn2, queryFn3], 2).then((results) => {
  console.log(results);
});

/*
  Output:
  [
  { id: 1, name: 'John Doe', age: 30 },
  { id: 2, name: 'Jane Doe', age: 28 },
  { id: 3, name: 'Bob Smith', age: 45 }
]
[ { queries: [ [Object], [Object] ] }, { queries: [ [Object] ] } ]
[
  { id: 1, name: 'John Doe', age: 30 },
  { id: 2, name: 'Jane Doe', age: 28 },
  { id: 3, name: 'Bob Smith', age: 45 }
]
 
 as you can see we have our queries for each shard and the outputted data 
*/

```

the use of sharding and caching is very useful when you have a large amount of data to query and you want to speed up the process of querying the data. this can be used for large scale applications!


## License

## Lazy class Api

constructor(arr): Initializes a new instance of the Lazy class with an array as its input.

map(fn): Returns a new Lazy instance with the result of applying the given function fn to each element in the array.

filter(fn): Returns a new Lazy instance with only the elements in the array that satisfy the given predicate function fn.

reduce(fn, initialValue): Returns the accumulated result of applying the binary operator function fn to the elements of the array, starting with the given initialValue.

subscribe(subscriber): Adds a subscriber function to the Lazy instance, which will be called whenever an array operation is performed.

setStore(key, value): Adds the given value to the Lazy instance's internal store under the given key.

getStore(key): Returns the value associated with the given key in the Lazy instance's internal store.

sort(comparator): Returns a new Lazy instance with the elements in the array sorted according to the given comparator function.

reverse(): Returns a new Lazy instance with the elements in the array reversed.

find(fn): Returns the first element in the array that satisfies the given predicate function fn.

some(fn): Returns true if at least one element in the array satisfies the given predicate function fn, otherwise false.

every(fn): Returns true if all elements in the array satisfy the given predicate function fn, otherwise false.

concat(...args): Returns a new Lazy instance with the elements of the original array concatenated with the given arrays.

slice(start, end): Returns a new Lazy instance with a portion of the original array specified by the start and end indices.

splice(start, deleteCount, ...items): Modifies the original array by removing or replacing elements and/or inserting new elements.

push(...items): Adds one or more elements to the end of the original array and returns its new length.

pop(): Removes the last element from the original array and returns it.

shift(): Removes the first element from the original array and returns it.

unshift(...items): Adds one or more elements to the beginning of the original array and returns its new length.

forEach(fn): Executes the given function fn for each element in the original array.

includes(item): Returns true if the given item is found in the original array, otherwise false.

indexOf(item): Returns the first index at which the given item can be found in the original array, or -1 if it is not present.

lastIndexOf(item): Returns the last index at which the given item can be found in the original array, or -1 if it is not present.

join(separator): Returns a string representing the original array, with the elements separated by the given separator.

toString(): Returns a string representing the original array.

notifySubscribers(fnName, args): Notifies all subscribers of the Lazy instance that an array operation has been performed with the given function name fnName and arguments args.

saveToDisk(cache): Saves the given cache object to disk as a

## JsonHandler class Api

query(queryFn: function): Promise<any> - Executes the specified query function and returns its result. If the result is already cached, returns the cached result instead. If an error occurs while executing the query, it is logged to the console and re-thrown.

subscribe(subscriber: function): void - Adds a subscriber function to the set of subscribers. Subscribers are notified whenever a query result is cached.

unsubscribe(subscriber: function): void - Removes a subscriber function from the set of subscribers.

getCacheSizePerNode(): object - Returns an object containing the size of the cache for each node.

notifySubscribers(fnName: string, args: any[], result: any): void - Notifies all subscribers with the specified function name, arguments, and result.

queryParallel(queryFns: function[]): Promise<any[]> - Executes multiple query functions in parallel and returns an array of their results.

queryWithSharding(queryFns: function[], shardCount: number): Promise<any[]> - Executes the specified query functions with sharding and returns an array of their results. The shardCount parameter specifies the number of shards to create.

saveCache(saveFn: function): void - Saves the cache to a storage location specified by the provided save function.

saveShardSessions(filename: string): void - Saves the shard sessions to a file with the specified filename.

saveCacheToFile(filename: string): void - Saves the cache to a file with the specified filename.

loadCacheFromFile(filename: string): Promise<void> - Loads the cache from a file with the specified filename.

reloadShards(): Promise<any[]> - Reloads the results for all shards and returns an array of the combined result

## Maintainers

- [Malik](https://github.com/MalikWhitten67)


## Contributing

See [the contributing file](CONTRIBUTING.md)!
PRs accepted.
