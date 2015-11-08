# How to use Observable

```
    public class Observable<T>{
        ...
        protected Observable(OnSubscribe<T> f) {
            this.onSubscribe = f;
        }
        
        public final static <T> Observable<T> create(OnSubscribe<T> f) {
            return new Observable<T>(hook.onCreate(f));
        }
        ...
    }
    
    1.Observable.create(new OnSubscribe<String>() {

            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("Hello World!");
                subscriber.onCompleted();
            }

        })
        
        
        .subscribe(new Subscriber<String>() {

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