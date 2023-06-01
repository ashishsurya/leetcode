# Event Emmiter Implementation
Design an EventEmitter class. This interface is similar (but with some differences) to the one found in Node.js or the Event Target interface of the DOM. The EventEmitter should allow for subscribing to events and emitting them.

Your EventEmitter class should have the following two methods:

- `subscribe` - This method takes in two arguments: the name of an event as a string and a callback function. This callback function will later be called when the event is emitted.
An event should be able to have multiple listeners for the same event. When emitting an event with multiple callbacks, each should be called in the order in which they were subscribed. An array of results should be returned. You can assume no callbacks passed to subscribe are referentially identical.
The subscribe method should also return an object with an unsubscribe method that enables the user to unsubscribe. When it is called, the callback should be removed from the list of subscriptions and undefined should be returned.

- `emit` - This method takes in two arguments: the name of an event as a string and an optional array of arguments that will be passed to the callback(s). If there are no callbacks subscribed to the given event, return an empty array. Otherwise, return an array of the results of all callback calls in the order they were subscribed.

## Solution

```ts
type Callback = (...args: any[]) => any;
type Subscription = {
    unsubscribe: () => void
}

class EventEmitter {
    eventMap = {};
    subscribe(eventName: string, callback: Callback): Subscription {
        if(!this.eventMap.hasOwnProperty(eventName)) {
            this.eventMap[eventName] = new Set();
        }

        this.eventMap[eventName].add(callback);
        return {
        unsubscribe: () => {
            this.eventMap[eventName].delete(callback);
        }
        };
    }

    emit(eventName: string, args: any[] = []): any {
        const res = [];
        if(!this.eventMap.hasOwnProperty(eventName)) {
            return res;
        }

        this.eventMap[eventName].forEach(cb => res.push(cb(...args)));

        return res;

    }
}

/**
 * const emitter = new EventEmitter();
 *
 * // Subscribe to the onClick event with onClickCallback
 * function onClickCallback() { return 99 }
 * const sub = emitter.subscribe('onClick', onClickCallback);
 *
 * emitter.emit('onClick'); // [99]
 * sub.unsubscribe(); // undefined
 * emitter.emit('onClick'); // []
 */
 ```
 
 ## Interview Purpose
- How would you handle events with arguments using the EventEmitter class?
  - When subscribing to an event, the callback function can accept the event arguments as parameters. When emitting the event, you can pass an array of arguments to the emit method, which will be passed to the subscribed callbacks. The callbacks can then access and utilize these arguments in their implementation.
- Can multiple callbacks be subscribed to the same event using the EventEmitter class?
  - Yes, the EventEmitter class allows multiple callbacks to be subscribed to the same event. Each subscribed callback will be called in the order they were subscribed when the event is emitted.
- How can you ensure the order of callback execution in the EventEmitter class?
  - The EventEmitter class maintains the order of callback execution by storing the callbacks in the order they are subscribed. When emitting an event, the class iterates through the list of callbacks in the order they were subscribed and calls each callback function.
- What happens when you emit an event with no subscribed callbacks using the EventEmitter class?
  - If there are no callbacks subscribed to a particular event, emitting that event using the EventEmitter class will return an empty array. This indicates that no callbacks were executed because there were no listeners for the event.
- Can you explain the difference between an EventEmitter and a simple callback function?
  - While a simple callback function allows you to execute a single function when an event occurs, an EventEmitter provides a more structured and scalable way to manage events. With an EventEmitter, you can have multiple callbacks for the same event, handle subscription and unsubscription, control the order of callback execution, pass arguments to callbacks, and have a more decoupled architecture.
 
