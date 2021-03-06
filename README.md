[![Latest Stable Version](https://poser.pugx.org/dtkahl/php-array-tools/v/stable)](https://packagist.org/packages/dtkahl/php-array-tools)
[![License](https://poser.pugx.org/dtkahl/php-array-tools/license)](https://packagist.org/packages/dtkahl/php-array-tools)
[![Build Status](https://travis-ci.org/dtkahl/php-array-tools.svg?branch=master)](https://travis-ci.org/dtkahl/php-array-tools)

# PHP array tools

Different tools for arrays in PHP.


#### Dependencies

* `PHP >= 5.6.0`


#### Installation

Install with [Composer](http://getcomposer.org):
```
composer require dtkahl/php-array-tools
```


## Map Class

This class provides a wrapper class for __indexed__ arrays and implements `\ArrayAccess`, `\Countable` and `\Serializable` Interfaces.


### Usage

Refer namespace

```
use Dtkahl\ArrayTools\Map;
```

create new Map

```php
$map = new Map([
    'first_name' => 'Jeffrey',
    'last_name' => 'Clarence',
    'age' =>  24
]);
```

**available options:**

* `recursive` -> create instance recursive (automatically create Map or Collection instance from array values)
* `key_locked` -> only allow keys which are already defined in Map.

### Methods

#### `get($key, $default = null)`
Returns property value by given key or returns `$default` if property does not exist.

#### `has($key)`
Determine if an property with given key exists in the map.

#### `set($key, $value)`
Set property (override if existing). Returns map instance.

#### `remove($key)`
Remove property if existing. Returns map instance.

#### `only()`
returns Map with all items with given keys.

#### `except()`
returns Map with all items except with given keys.

#### `toArray()`
Returns all items as array.

#### `toSerializedArray()`
Returns serialized items (call `$item->toSerializedArray()` if item is object and has this method) as array.

#### `toJson()`
Returns all items of the map as JSON string.

#### `merge(array $data, $prefer_old = false)`
Merge given array (or Map instance) into map data.
If `$prefer_old` is true, the data will be merged in the opposite direction.
Returns map instance.

#### `copy()`
Returns clone of Map instance.

#### `clear()`
Removes all items map and returns instance of map.

#### `each(\Closure $call)`
Walk through map items.

```php
$map->each(function ($key, $value) {
  // do something
});
```

You can break by `return false` in the closure.
Returns map instance.

#### `map(\Closure $call)`
Walk through map items and overrides there values by the return value of the given closure.

```php
$map->map(function ($key, $value) {
  return "Mapped " . $value;
});
```

Returns collection instance.


## Collection Class

This class provides a wrapper class for __indexed__ arrays implements `\ArrayAccess`, `\Countable`, `\Iterator` and `\Serializable` Interfaces.

**available options:**

* `recursive` -> create instance recursive (automatically create Map or Collection instance from array values)


### Usage

Refer namespace

```
use Dtkahl\ArrayTools\Collection;
```

create new Collection

```php
$collection = new Collection([
    [
        'first_name' => 'Jeffrey',
        'last_name' => 'Clarence',
        'age' =>  24
    ],
    [
        'first_name' => 'Neil',
        'last_name' => 'Hiram',
        'age' =>  32
    ],
    [
        'first_name' => 'Derek',
        'last_name' => 'Deon',
        'age' =>  19
    ],
]);
```

### Methods

#### `toArray()`
Returns all items of the collection as indexed array.

#### `toSerializedArray()`
Returns serialized Items (call `$item->toSerializedArray()` if item is object and has this method) as array.

#### `toJson()`
Returns all items of the collection as JSON string.

#### `copy()`
Returns clone of collection instance.

#### `getKeys()`
Returns an array of collection item keys.

#### `hasKey(int $key)`
Determine if an item with given key exists in the collection.

#### `isEmpty()`
Determine if there are no items in the collection.

#### `getValues()`
Returns an array of Collection items. (actually the same like `toArray()` because collection data is always an indexed array)

#### `hasValue(int $value)`
Determine if an item with given value exists in the collection.

#### `count()`
Returns the count of items in the collection.

#### `clear()`
Remove all item from the collection. Returns collection instance.

#### `get(int $key)`
Returns the collection item with the given key.

#### `remove(int $key, bool $do_not_clear = false)`
Remove the collection item with the given key. Returns collection instance.

#### `each(\Closure $call)`
Walk through collection items.

```php
$collection->each(function ($item, $key) {
  // do something
});
```

You can break by `return false` in the closure.
Returns collection instance.

#### `filter(\Closure $call)`
Walk trough collection items and only keep items where closure returns true.

```php
$collection->filter(function ($item, $key) {
  return $item['age'] > 10;
});
```

#### `reverse()`
Reverse the items in the collection. Returns collection instance.

#### `first()`
Returns the first collection item.

#### `last()`
Returns the last collection item.

#### `shift()`
Returns and removes the first collection item.

#### `unshift(mixed $value)`
Push an item onto the beginning of the collection. Returns collection instance.

#### `pop()`
Returns and removes the last collection item.

#### `push(mixed $value)`
Push an item onto the ending of the collection. Returns collection instance.

#### `put(int $key, mixed $value)`
Put an item in the collection by key. (Override if existing)
Returns collection instance.

#### `inject(int $key, mixed $value)`
Put an item in the collection by key. (move items one possition up where key >= given key)
Returns collection instance.

#### `merge(array $array)`
Merge given array (or Collection instance) into collection data.
Returns collection instance.

#### `sort(\Closure $call)`
Sorts the collection items with [usort](http://php.net/manual/en/function.usort.php)

```php
$collection->sort(function ($a, $b) {
  return $a['age'] > $b['age'];
});
```

Returns collection instance.

#### `map(\Closure $call)`
Walk through collection items and overrides them by the return value of the given closure.

```php
$collection->map(function ($item) {
  return $item['first_name] . " " . $item['last_name];
});
```

Returns collection instance.

#### `slice(int $offset, int|null $length = null)`
Slize the collection data with [array_slice](http://php.net/manual/en/function.array-slice.php).
Returns collection instance.

#### `chunk(int $size)`
Returns an array of collections with given chunk size.

#### `current(int $size)`
Returns item on current pointer position or 'null' if there is no item.

#### `next(int $size)`
Increase internal pointer by one and returns the item or 'null' if there is no item on this position.

```php
while ($item = $collection->next()) {
  var_dump($item['age']);
  echo "<br>";
}
```

#### `previous(int $size)`
Decrease internal pointer by one and returns the item or 'null' if there is no item on this position.

#### `setPointer(int $pointer)`
set the internal pointer position to the given value.

#### `lists(array $keys)`
returns an array with given array entries/public properties of collection items.

```php
$collection->lists(['age']) // array (array('age'=>24'), array('age'=>32), array('age'=>19))
```

#### `join(string $glue)`
Join collection items with a given $glue.

#### `unique()`
Remove duplicated entries.
Returns collection instance.
