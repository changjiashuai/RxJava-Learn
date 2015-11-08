# First Chapter

```
rx
|___annotations
    |___...
|___exceptions
    |___...
|___functions
    |___...
|___internal
    |___...
|___observables
    |___...
|___observers
    |___...
|___plugins
    |___...
|___schedulers
    |___...
|___subscriptions
    |___...
|___Notification
    |>>>final class Notification<T>
    
|___Observable
    |>>>>
|___Observer
    |---
        interface Observer<T>{
            void onCompleted();
            void onError(Throwable e);
            void onNext(T t);
        }
|___Producer
|___Scheduler
|___Single
|___SingleSubscriber
|___Subscriber
|___Subscription
```


```
Observable<String> myObservable = Observable.create(  
    new Observable.OnSubscribe<String>() {  
        @Override  
        public void call(Subscriber<? super String> sub) {  
            sub.onNext("Hello, world!");  
            sub.onCompleted();  
        }  
    }  
);
```