# What is this?

This is Laravel's Collection helper class (PHP arrays on steroids) that has been pulled from Laravel that can be used all by itself. It doesn't contain any unneeded helper functions, extra classes or service providers. It is just the Collection class. In addition, it doesn't use the `Illuminate` namespace. Other than that, everything else is pretty much the same:

    $collection = collect(['taylor', 'abigail', null])->map(function ($name) {
        return strtoupper($name);
    })
    ->reject(function ($name) {
        return empty($name);
    });

## Installing

Install `stillat/collection` using Composer.

```
$ composer require stillat/collection
```

After Composer does its thing, you should now have the collection helpers available for your project! Continue on to learn all about it.

# Collections

>Note: This documentation comes directly from the [Laravel Docs](https://github.com/laravel/docs) repository. Some tables have been reformatted to make it nicer to look at in GitHub, but that is about it.

- [Introduction](#introduction)
- [Creating Collections](#creating-collections)
- [Available Methods](#available-methods)
- [When will updates be made to this library?](#updates)
- [Can I contribute improvements/fixes?](#howtocontribute)
- [Motivation/Why is this a thing?](#motivation)

<a name="introduction"></a>
## Introduction

The `Collection\Collection` class provides a fluent, convenient wrapper for working with arrays of data. For example, check out the following code. We'll use the `collect` helper to create a new collection instance from the array, run the `strtoupper` function on each element, and then remove all empty elements:

    $collection = collect(['taylor', 'abigail', null])->map(function ($name) {
        return strtoupper($name);
    })
    ->reject(function ($name) {
        return empty($name);
    });


As you can see, the `Collection` class allows you to chain its methods to perform fluent mapping and reducing of the underlying array. In general, every `Collection` method returns an entirely new `Collection` instance.

<a name="creating-collections"></a>
## Creating Collections

As mentioned above, the `collect` helper returns a new `Collection\Collection` instance for the given array. So, creating a collection is as simple as:

    $collection = collect([1, 2, 3]);

<a name="available-methods"></a>
## Available Methods

For the remainder of this documentation, we'll discuss each method available on the `Collection` class. Remember, all of these methods may be chained for fluently manipulating the underlying array. Furthermore, almost every method returns a new `Collection` instance, allowing you to preserve the original copy of the collection when necessary.

You may select any method from this table to see an example of its usage:

| Method | ... | ... |
|---|---|---|
| [all](#method-all) | [intersect](#method-intersect) | [search](#method-search) |
| [avg](#method-avg) | [isEmpty](#method-isempty) | [shift](#method-shift) |
| [chunk](#method-chunk) | [keyBy](#method-keyby) | [shuffle](#method-shuffle) |
| [collapse](#method-collapse) | [keys](#method-keys) | [slice](#method-slice) |
| [contains](#method-contains) | [last](#method-last) | [sort](#method-sort) |
| [count](#method-count) | [map](#method-map) | [sortBy](#method-sortby) |
| [diff](#method-diff) | [max](#method-max) | [sortByDesc](#method-sortbydesc) |
| [each](#method-each) | [merge](#method-merge) | [splice](#method-splice) |
| [every](#method-every) | [min](#method-min) | [sum](#method-sum) |
| [except](#method-except) | [only](#method-only) | [take](#method-take) |
| [filter](#method-filter) | [pluck](#method-pluck) | [toArray](#method-toarray) |
| [first](#method-first) | [pop](#method-pop) | [toJson](#method-tojson) |
| [flatten](#method-flatten) | [prepend](#method-prepend) | [transform](#method-transform) |
| [flip](#method-flip) | [pull](#method-pull) | [unique](#method-unique) |
| [forget](#method-forget) | [push](#method-push) | [values](#method-values) |
| [forPage](#method-forpage) | [put](#method-put) | [where](#method-where) |
| [get](#method-get) | [random](#method-random) | [whereLoose](#method-whereloose) |
| [groupBy](#method-groupby) | [reduce](#method-reduce) | [zip](#method-zip) |
| [has](#method-has) | [reject](#method-reject) | |
| [implode](#method-implode) | [reverse](#method-reverse) | |


<a name="method-listing"></a>
## Method Listing

<a name="method-all"></a>
#### `all()`

The `all` method simply returns the underlying array represented by the collection:

    collect([1, 2, 3])->all();

    // [1, 2, 3]

<a name="method-avg"></a>
#### `avg()`

The `avg` method returns the average of all items in the collection:

    collect([1, 2, 3, 4, 5])->avg();

    // 3

If the collection contains nested arrays or objects, you should pass a key to use for determining which values to calculate the average:

    $collection = collect([
        ['name' => 'JavaScript: The Good Parts', 'pages' => 176],
        ['name' => 'JavaScript: The Definitive Guide', 'pages' => 1096],
    ]);

    $collection->avg('pages');

    // 636

<a name="method-chunk"></a>
#### `chunk()`

The `chunk` method breaks the collection into multiple, smaller collections of a given size:

    $collection = collect([1, 2, 3, 4, 5, 6, 7]);

    $chunks = $collection->chunk(4);

    $chunks->toArray();

    // [[1, 2, 3, 4], [5, 6, 7]]


<a name="method-collapse"></a>
#### `collapse()`

The `collapse` method collapses a collection of arrays into a flat collection:

    $collection = collect([[1, 2, 3], [4, 5, 6], [7, 8, 9]]);

    $collapsed = $collection->collapse();

    $collapsed->all();

    // [1, 2, 3, 4, 5, 6, 7, 8, 9]

<a name="method-contains"></a>
#### `contains()`

The `contains` method determines whether the collection contains a given item:

    $collection = collect(['name' => 'Desk', 'price' => 100]);

    $collection->contains('Desk');

    // true

    $collection->contains('New York');

    // false

You may also pass a key / value pair to the `contains` method, which will determine if the given pair exists in the collection:

    $collection = collect([
        ['product' => 'Desk', 'price' => 200],
        ['product' => 'Chair', 'price' => 100],
    ]);

    $collection->contains('product', 'Bookcase');

    // false

Finally, you may also pass a callback to the `contains` method to perform your own truth test:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->contains(function ($key, $value) {
        return $value > 5;
    });

    // false

<a name="method-count"></a>
#### `count()`

The `count` method returns the total number of items in the collection:

    $collection = collect([1, 2, 3, 4]);

    $collection->count();

    // 4

<a name="method-diff"></a>
#### `diff()`

The `diff` method compares the collection against another collection or a plain PHP `array`:

    $collection = collect([1, 2, 3, 4, 5]);

    $diff = $collection->diff([2, 4, 6, 8]);

    $diff->all();

    // [1, 3, 5]

<a name="method-each"></a>
#### `each()`

The `each` method iterates over the items in the collection and passes each item to a given callback:

    $collection = $collection->each(function ($item, $key) {
        //
    });

Return `false` from your callback to break out of the loop:

    $collection = $collection->each(function ($item, $key) {
        if (/* some condition */) {
            return false;
        }
    });

<a name="method-every"></a>
#### `every()`

The `every` method creates a new collection consisting of every n-th element:

    $collection = collect(['a', 'b', 'c', 'd', 'e', 'f']);

    $collection->every(4);

    // ['a', 'e']

You may optionally pass offset as the second argument:

    $collection->every(4, 1);

    // ['b', 'f']

<a name="method-except"></a>
#### `except()`

The `except` method returns all items in the collection except for those with the specified keys:

    $collection = collect(['product_id' => 1, 'name' => 'Desk', 'price' => 100, 'discount' => false]);

    $filtered = $collection->except(['price', 'discount']);

    $filtered->all();

    // ['product_id' => 1, 'name' => 'Desk']

For the inverse of `except`, see the [only](#method-only) method.

<a name="method-filter"></a>
#### `filter()`

The `filter` method filters the collection by a given callback, keeping only those items that pass a given truth test:

    $collection = collect([1, 2, 3, 4]);

    $filtered = $collection->filter(function ($item) {
        return $item > 2;
    });

    $filtered->all();

    // [3, 4]

For the inverse of `filter`, see the [reject](#method-reject) method.

<a name="method-first"></a>
#### `first()`

The `first` method returns the first element in the collection that passes a given truth test:

    collect([1, 2, 3, 4])->first(function ($key, $value) {
        return $value > 2;
    });

    // 3

You may also call the `first` method with no arguments to get the first element in the collection. If the collection is empty, `null` is returned:

    collect([1, 2, 3, 4])->first();

    // 1

<a name="method-flatten"></a>
#### `flatten()`

The `flatten` method flattens a multi-dimensional collection into a single dimension:

    $collection = collect(['name' => 'taylor', 'languages' => ['php', 'javascript']]);

    $flattened = $collection->flatten();

    $flattened->all();

    // ['taylor', 'php', 'javascript'];

<a name="method-flip"></a>
#### `flip()`

The `flip` method swaps the collection's keys with their corresponding values:

    $collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

    $flipped = $collection->flip();

    $flipped->all();

    // ['taylor' => 'name', 'laravel' => 'framework']

<a name="method-forget"></a>
#### `forget()`

The `forget` method removes an item from the collection by its key:

    $collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

    $collection->forget('name');

    $collection->all();

    // [framework' => 'laravel']

> **Note:** Unlike most other collection methods, `forget` does not return a new modified collection; it modifies the collection it is called on.

<a name="method-forpage"></a>
#### `forPage()`

The `forPage` method returns a new collection containing the items that would be present on a given page number:

    $collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);

    $chunk = $collection->forPage(2, 3);

    $chunk->all();

    // [4, 5, 6]

The method requires the page number and the number of items to show per page, respectively.

<a name="method-get"></a>
#### `get()`

The `get` method returns the item at a given key. If the key does not exist, `null` is returned:

    $collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

    $value = $collection->get('name');

    // taylor

You may optionally pass a default value as the second argument:

    $collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

    $value = $collection->get('foo', 'default-value');

    // default-value

You may even pass a callback as the default value. The result of the callback will be returned if the specified key does not exist:

    $collection->get('email', function () {
        return 'default-value';
    });

    // default-value

<a name="method-groupby"></a>
#### `groupBy()`

The `groupBy` method groups the collection's items by a given key:

    $collection = collect([
        ['account_id' => 'account-x10', 'product' => 'Chair'],
        ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ['account_id' => 'account-x11', 'product' => 'Desk'],
    ]);

    $grouped = $collection->groupBy('account_id');

    $grouped->toArray();

    /*
        [
            'account-x10' => [
                ['account_id' => 'account-x10', 'product' => 'Chair'],
                ['account_id' => 'account-x10', 'product' => 'Bookcase'],
            ],
            'account-x11' => [
                ['account_id' => 'account-x11', 'product' => 'Desk'],
            ],
        ]
    */

In addition to passing a string `key`, you may also pass a callback. The callback should return the value you wish to key the group by:

    $grouped = $collection->groupBy(function ($item, $key) {
        return substr($item['account_id'], -3);
    });

    $grouped->toArray();

    /*
        [
            'x10' => [
                ['account_id' => 'account-x10', 'product' => 'Chair'],
                ['account_id' => 'account-x10', 'product' => 'Bookcase'],
            ],
            'x11' => [
                ['account_id' => 'account-x11', 'product' => 'Desk'],
            ],
        ]
    */

<a name="method-has"></a>
#### `has()`

The `has` method determines if a given key exists in the collection:

    $collection = collect(['account_id' => 1, 'product' => 'Desk']);

    $collection->has('email');

    // false

<a name="method-implode"></a>
#### `implode()`

The `implode` method joins the items in a collection. Its arguments depend on the type of items in the collection.

If the collection contains arrays or objects, you should pass the key of the attributes you wish to join, and the "glue" string you wish to place between the values:

    $collection = collect([
        ['account_id' => 1, 'product' => 'Desk'],
        ['account_id' => 2, 'product' => 'Chair'],
    ]);

    $collection->implode('product', ', ');

    // Desk, Chair

If the collection contains simple strings or numeric values, simply pass the "glue" as the only argument to the method:

    collect([1, 2, 3, 4, 5])->implode('-');

    // '1-2-3-4-5'

<a name="method-intersect"></a>
#### `intersect()`

The `intersect` method removes any values that are not present in the given `array` or collection:

    $collection = collect(['Desk', 'Sofa', 'Chair']);

    $intersect = $collection->intersect(['Desk', 'Chair', 'Bookcase']);

    $intersect->all();

    // [0 => 'Desk', 2 => 'Chair']

As you can see, the resulting collection will preserve the original collection's keys.

<a name="method-isempty"></a>
#### `isEmpty()`

The `isEmpty` method returns `true` if the collection is empty; otherwise, `false` is returned:

    collect([])->isEmpty();

    // true

<a name="method-keyby"></a>
#### `keyBy()`

Keys the collection by the given key:

    $collection = collect([
        ['product_id' => 'prod-100', 'name' => 'desk'],
        ['product_id' => 'prod-200', 'name' => 'chair'],
    ]);

    $keyed = $collection->keyBy('product_id');

    $keyed->all();

    /*
        [
            'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
            'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
        ]
    */

If multiple items have the same key, only the last one will appear in the new collection.

You may also pass your own callback, which should return the value to key the collection by:

    $keyed = $collection->keyBy(function ($item) {
        return strtoupper($item['product_id']);
    });

    $keyed->all();

    /*
        [
            'PROD-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
            'PROD-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
        ]
    */


<a name="method-keys"></a>
#### `keys()`

The `keys` method returns all of the collection's keys:

    $collection = collect([
        'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]);

    $keys = $collection->keys();

    $keys->all();

    // ['prod-100', 'prod-200']

<a name="method-last"></a>
#### `last()`

The `last` method returns the last element in the collection that passes a given truth test:

    collect([1, 2, 3, 4])->last(function ($key, $value) {
        return $value < 3;
    });

    // 2

You may also call the `last` method with no arguments to get the last element in the collection. If the collection is empty, `null` is returned:

    collect([1, 2, 3, 4])->last();

    // 4

<a name="method-map"></a>
#### `map()`

The `map` method iterates through the collection and passes each value to the given callback. The callback is free to modify the item and return it, thus forming a new collection of modified items:

    $collection = collect([1, 2, 3, 4, 5]);

    $multiplied = $collection->map(function ($item, $key) {
        return $item * 2;
    });

    $multiplied->all();

    // [2, 4, 6, 8, 10]

> **Note:** Like most other collection methods, `map` returns a new collection instance; it does not modify the collection it is called on. If you want to transform the original collection, use the [`transform`](#method-transform) method.

<a name="method-max"></a>
#### `max()`

The `max` method return the maximum value of a given key:

    $max = collect([['foo' => 10], ['foo' => 20]])->max('foo');

    // 20

    $max = collect([1, 2, 3, 4, 5])->max();

    // 5

<a name="method-merge"></a>
#### `merge()`

The `merge` method merges the given array into the collection. Any string key in the array matching a string key in the collection will overwrite the value in the collection:

    $collection = collect(['product_id' => 1, 'name' => 'Desk']);

    $merged = $collection->merge(['price' => 100, 'discount' => false]);

    $merged->all();

    // ['product_id' => 1, 'name' => 'Desk', 'price' => 100, 'discount' => false]

If the given array's keys are numeric, the values will be appended to the end of the collection:

    $collection = collect(['Desk', 'Chair']);

    $merged = $collection->merge(['Bookcase', 'Door']);

    $merged->all();

    // ['Desk', 'Chair', 'Bookcase', 'Door']

<a name="method-min"></a>
#### `min()`

The `min` method return the minimum value of a given key:

    $min = collect([['foo' => 10], ['foo' => 20]])->min('foo');

    // 10

    $min = collect([1, 2, 3, 4, 5])->min();

    // 1

<a name="method-only"></a>
#### `only()`

The `only` method returns the items in the collection with the specified keys:

    $collection = collect(['product_id' => 1, 'name' => 'Desk', 'price' => 100, 'discount' => false]);

    $filtered = $collection->only(['product_id', 'name']);

    $filtered->all();

    // ['product_id' => 1, 'name' => 'Desk']

For the inverse of `only`, see the [except](#method-except) method.

<a name="method-pluck"></a>
#### `pluck()`

The `pluck` method retrieves all of the collection values for a given key:

    $collection = collect([
        ['product_id' => 'prod-100', 'name' => 'Desk'],
        ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]);

    $plucked = $collection->pluck('name');

    $plucked->all();

    // ['Desk', 'Chair']

You may also specify how you wish the resulting collection to be keyed:

    $plucked = $collection->pluck('name', 'product_id');

    $plucked->all();

    // ['prod-100' => 'Desk', 'prod-200' => 'Chair']

<a name="method-pop"></a>
#### `pop()`

The `pop` method removes and returns the last item from the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->pop();

    // 5

    $collection->all();

    // [1, 2, 3, 4]

<a name="method-prepend"></a>
#### `prepend()`

The `prepend` method adds an item to the beginning of the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->prepend(0);

    $collection->all();

    // [0, 1, 2, 3, 4, 5]

You can optionally pass a second argument to set the key of the prepended item:

    $collection = collect(['one' => 1, 'two', => 2]);

    $collection->prepend(0, 'zero');

    $collection->all();

    // ['zero' => 0, 'one' => 1, 'two', => 2]

<a name="method-pull"></a>
#### `pull()`

The `pull` method removes and returns an item from the collection by its key:

    $collection = collect(['product_id' => 'prod-100', 'name' => 'Desk']);

    $collection->pull('name');

    // 'Desk'

    $collection->all();

    // ['product_id' => 'prod-100']

<a name="method-push"></a>
#### `push()`

The `push` method appends an item to the end of the collection:

    $collection = collect([1, 2, 3, 4]);

    $collection->push(5);

    $collection->all();

    // [1, 2, 3, 4, 5]

<a name="method-put"></a>
#### `put()`

The `put` method sets the given key and value in the collection:

    $collection = collect(['product_id' => 1, 'name' => 'Desk']);

    $collection->put('price', 100);

    $collection->all();

    // ['product_id' => 1, 'name' => 'Desk', 'price' => 100]

<a name="method-random"></a>
#### `random()`

The `random` method returns a random item from the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->random();

    // 4 - (retrieved randomly)

You may optionally pass an integer to `random`. If that integer is more than `1`, a collection of items is returned:

    $random = $collection->random(3);

    $random->all();

    // [2, 4, 5] - (retrieved randomly)

<a name="method-reduce"></a>
#### `reduce()`

The `reduce` method reduces the collection to a single value, passing the result of each iteration into the subsequent iteration:

    $collection = collect([1, 2, 3]);

    $total = $collection->reduce(function ($carry, $item) {
        return $carry + $item;
    });

    // 6

The value for `$carry` on the first iteration is `null`; however, you may specify its initial value by passing a second argument to `reduce`:

    $collection->reduce(function ($carry, $item) {
        return $carry + $item;
    }, 4);

    // 10

<a name="method-reject"></a>
#### `reject()`

The `reject` method filters the collection using the given callback. The callback should return `true` for any items it wishes to remove from the resulting collection:

    $collection = collect([1, 2, 3, 4]);

    $filtered = $collection->reject(function ($item) {
        return $item > 2;
    });

    $filtered->all();

    // [1, 2]

For the inverse of the `reject` method, see the [`filter`](#method-filter) method.

<a name="method-reverse"></a>
#### `reverse()`

The `reverse` method reverses the order of the collection's items:

    $collection = collect([1, 2, 3, 4, 5]);

    $reversed = $collection->reverse();

    $reversed->all();

    // [5, 4, 3, 2, 1]

<a name="method-search"></a>
#### `search()`

The `search` method searches the collection for the given value and returns its key if found. If the item is not found, `false` is returned.

    $collection = collect([2, 4, 6, 8]);

    $collection->search(4);

    // 1

The search is done using a "loose" comparison. To use strict comparison, pass `true` as the second argument to the method:

    $collection->search('4', true);

    // false

Alternatively, you may pass in your own callback to search for the first item that passes your truth test:

    $collection->search(function ($item, $key) {
        return $item > 5;
    });

    // 2

<a name="method-shift"></a>
#### `shift()`

The `shift` method removes and returns the first item from the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->shift();

    // 1

    $collection->all();

    // [2, 3, 4, 5]

<a name="method-shuffle"></a>
#### `shuffle()`

The `shuffle` method randomly shuffles the items in the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $shuffled = $collection->shuffle();

    $shuffled->all();

    // [3, 2, 5, 1, 4] // (generated randomly)

<a name="method-slice"></a>
#### `slice()`

The `slice` method returns a slice of the collection starting at the given index:

    $collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

    $slice = $collection->slice(4);

    $slice->all();

    // [5, 6, 7, 8, 9, 10]

If you would like to limit the size of the returned slice, pass the desired size as the second argument to the method:

    $slice = $collection->slice(4, 2);

    $slice->all();

    // [5, 6]

The returned slice will have new, numerically indexed keys. If you wish to preserve the original keys, pass `true` as the third argument to the method.

<a name="method-sort"></a>
#### `sort()`

The `sort` method sorts the collection:

    $collection = collect([5, 3, 1, 2, 4]);

    $sorted = $collection->sort();

    $sorted->values()->all();

    // [1, 2, 3, 4, 5]

The sorted collection keeps the original array keys. In this example we used the [`values`](#method-values) method to reset the keys to consecutively numbered indexes.

For sorting a collection of nested arrays or objects, see the [`sortBy`](#method-sortby) and [`sortByDesc`](#method-sortbydesc) methods.

If your sorting needs are more advanced, you may pass a callback to `sort` with your own algorithm. Refer to the PHP documentation on [`usort`](http://php.net/manual/en/function.usort.php#refsect1-function.usort-parameters), which is what the collection's `sort` method calls under the hood.

<a name="method-sortby"></a>
#### `sortBy()`

The `sortBy` method sorts the collection by the given key:

    $collection = collect([
        ['name' => 'Desk', 'price' => 200],
        ['name' => 'Chair', 'price' => 100],
        ['name' => 'Bookcase', 'price' => 150],
    ]);

    $sorted = $collection->sortBy('price');

    $sorted->values()->all();

    /*
        [
            ['name' => 'Chair', 'price' => 100],
            ['name' => 'Bookcase', 'price' => 150],
            ['name' => 'Desk', 'price' => 200],
        ]
    */

The sorted collection keeps the original array keys. In this example we used the [`values`](#method-values) method to reset the keys to consecutively numbered indexes.

You can also pass your own callback to determine how to sort the collection values:

    $collection = collect([
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]);

    $sorted = $collection->sortBy(function ($product, $key) {
        return count($product['colors']);
    });

    $sorted->values()->all();

    /*
        [
            ['name' => 'Chair', 'colors' => ['Black']],
            ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
            ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
        ]
    */

<a name="method-sortbydesc"></a>
#### `sortByDesc()`

This method has the same signature as the [`sortBy`](#method-sortby) method, but will sort the collection in the opposite order.

<a name="method-splice"></a>
#### `splice()`

The `splice` method removes and returns a slice of items starting at the specified index:

    $collection = collect([1, 2, 3, 4, 5]);

    $chunk = $collection->splice(2);

    $chunk->all();

    // [3, 4, 5]

    $collection->all();

    // [1, 2]

You may pass a second argument to limit the size of the resulting chunk:

    $collection = collect([1, 2, 3, 4, 5]);

    $chunk = $collection->splice(2, 1);

    $chunk->all();

    // [3]

    $collection->all();

    // [1, 2, 4, 5]

In addition, you can pass a third argument containing the new items to replace the items removed from the collection:

    $collection = collect([1, 2, 3, 4, 5]);

    $chunk = $collection->splice(2, 1, [10, 11]);

    $chunk->all();

    // [3]

    $collection->all();

    // [1, 2, 10, 11, 4, 5]

<a name="method-sum"></a>
#### `sum()`

The `sum` method returns the sum of all items in the collection:

    collect([1, 2, 3, 4, 5])->sum();

    // 15

If the collection contains nested arrays or objects, you should pass a key to use for determining which values to sum:

    $collection = collect([
        ['name' => 'JavaScript: The Good Parts', 'pages' => 176],
        ['name' => 'JavaScript: The Definitive Guide', 'pages' => 1096],
    ]);

    $collection->sum('pages');

    // 1272

In addition, you may pass your own callback to determine which values of the collection to sum:

    $collection = collect([
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]);

    $collection->sum(function ($product) {
        return count($product['colors']);
    });

    // 6

<a name="method-take"></a>
#### `take()`

The `take` method returns a new collection with the specified number of items:

    $collection = collect([0, 1, 2, 3, 4, 5]);

    $chunk = $collection->take(3);

    $chunk->all();

    // [0, 1, 2]

You may also pass a negative integer to take the specified amount of items from the end of the collection:

    $collection = collect([0, 1, 2, 3, 4, 5]);

    $chunk = $collection->take(-2);

    $chunk->all();

    // [4, 5]

<a name="method-toarray"></a>
#### `toArray()`

The `toArray` method converts the collection into a plain PHP `array`:

    $collection = collect(['name' => 'Desk', 'price' => 200]);

    $collection->toArray();

    /*
        [
            ['name' => 'Desk', 'price' => 200],
        ]
    */

> **Note:** `toArray` also converts all of its nested objects to an array. If you want to get the underlying array as is, use the [`all`](#method-all) method instead.

<a name="method-tojson"></a>
#### `toJson()`

The `toJson` method converts the collection into JSON:

    $collection = collect(['name' => 'Desk', 'price' => 200]);

    $collection->toJson();

    // '{"name":"Desk","price":200}'

<a name="method-transform"></a>
#### `transform()`

The `transform` method iterates over the collection and calls the given callback with each item in the collection. The items in the collection will be replaced by the values returned by the callback:

    $collection = collect([1, 2, 3, 4, 5]);

    $collection->transform(function ($item, $key) {
        return $item * 2;
    });

    $collection->all();

    // [2, 4, 6, 8, 10]

> **Note:** Unlike most other collection methods, `transform` modifies the collection itself. If you wish to create a new collection instead, use the [`map`](#method-map) method.

<a name="method-unique"></a>
#### `unique()`

The `unique` method returns all of the unique items in the collection:

    $collection = collect([1, 1, 2, 2, 3, 4, 2]);

    $unique = $collection->unique();

    $unique->values()->all();

    // [1, 2, 3, 4]

The returned collection keeps the original array keys. In this example we used the [`values`](#method-values) method to reset the keys to consecutively numbered indexes.

When dealing with nested arrays or objects, you may specify the key used to determine uniqueness:

    $collection = collect([
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'iPhone 5', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
        ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
    ]);

    $unique = $collection->unique('brand');

    $unique->values()->all();

    /*
        [
            ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
            ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
        ]
    */

You may also pass your own callback to determine item uniqueness:

    $unique = $collection->unique(function ($item) {
        return $item['brand'].$item['type'];
    });

    $unique->values()->all();

    /*
        [
            ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
            ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
            ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
            ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
        ]
    */

<a name="method-values"></a>
#### `values()`

The `values` method returns a new collection with the keys reset to consecutive integers:

    $collection = collect([
        10 => ['product' => 'Desk', 'price' => 200],
        11 => ['product' => 'Desk', 'price' => 200]
    ]);

    $values = $collection->values();

    $values->all();

    /*
        [
            0 => ['product' => 'Desk', 'price' => 200],
            1 => ['product' => 'Desk', 'price' => 200],
        ]
    */
<a name="method-where"></a>
#### `where()`

The `where` method filters the collection by a given key / value pair:

    $collection = collect([
        ['product' => 'Desk', 'price' => 200],
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Bookcase', 'price' => 150],
        ['product' => 'Door', 'price' => 100],
    ]);

    $filtered = $collection->where('price', 100);

    $filtered->all();

    /*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
    */

The `where` method uses strict comparisons when checking item values. Use the [`whereLoose`](#where-loose) method to filter using "loose" comparisons.

<a name="method-whereloose"></a>
#### `whereLoose()`

This method has the same signature as the [`where`](#method-where) method; however, all values are compared using "loose" comparisons.

<a name="method-zip"></a>
#### `zip()`

The `zip` method merges together the values of the given array with the values of the collection at the corresponding index:

    $collection = collect(['Chair', 'Desk']);

    $zipped = $collection->zip([100, 200]);

    $zipped->all();

    // [['Chair', 100], ['Desk', 200]]

<a name="updates"></a>
## When will updates be made?

This repository will be updated when significant/major changes have been made to Laravel's collection classes. Think it should be updated? Open an issue.

<a name="howtocontribute"></a>
## Can I contribute improvements/fixes?

This repository is automagically generated using a script that involves a lot of black magic/sorcery. This script pulls the collection code from the Laravel repository and "rewrites" it so it can be used by itself. Because of this, any modifications made to the code in this repository would be overwritten whenever the script is ran. If you are still interested in contributing, consider contributing directly to [Laravel](https://github.com/laravel/framework).

<a name="motivation"></a>
## Motivation/Why is this a thing?

The `Collection` helper class is *extremely* useful and it might as well be its own Composer package (because why not?). But mainly this was an experiment to see if I could create a little script to automatically generate the code seen in this repository from the code in the Laravel repository.

However, because the `Collection` helper class is so useful, I am sure someone will find value in this little library.