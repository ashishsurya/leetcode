# Cache With Time Limit

Write a class that allows getting and setting key-value pairs, however a time until expiration is associated with each key.

The class has three public methods:

**set(key, value, duration)**: accepts an integer key, an integer value, and a duration in milliseconds. Once the duration has elapsed, the key should be inaccessible. The method should return true if the same un-expired key already exists and false otherwise. Both the value and duration should be overwritten if the key already exists.

**get(key)**: if an un-expired key exists, it should return the associated value. Otherwise it should return -1.


**count()**: returns the count of un-expired keys.


## Approach 1: setTimeout + clearTimeout + Class Syntax

```ts
type MapValue = {value : number, timeout : NodeJS.Timeout}

class TimeLimitedCache {
    cache = new Map<number, MapValue>()

    set(key: number, value: number, duration: number): boolean {
        const valueInCache = this.cache.get(key);
        if(valueInCache) {
            clearTimeout(valueInCache.timeout)
        }

        const timeout = setTimeout(() => this.cache.delete(key),duration);
        this.cache.set(key, {value , timeout})

        return Boolean(valueInCache);
    }

    get(key: number): number {
        return this.cache.has(key) ? this.cache.get(key).value : -1;
    }

	count(): number {
        return this.cache.size;
    }
}

/**
 * Your TimeLimitedCache object will be instantiated and called as such:
 * var obj = new TimeLimitedCache()
 * obj.set(1, 42, 1000); // false
 * obj.get(1) // 42
 * obj.count() // 1
 */
```







