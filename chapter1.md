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
    |---> abstract Subscriber<T> implements Observer<T>, Subscription
|___Subscription
    |---
        interface Subscription{
            void unsubscribe();
            void isUnsubscribed();
        }
```


```
 Observable.create(new OnSubscribe<String>() {

            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("Hello World!");
                subscriber.onCompleted();
            }

        }).subscribe(new Subscriber<String>() {

            @Override
            public void onCompleted() {
                System.out.println("Done");
            }

            @Override
            public void onError(Throwable e) {
                e.printStackTrace();
            }

            @Override
            public void onNext(String t) {
                System.out.println(t);
            }

        });
```