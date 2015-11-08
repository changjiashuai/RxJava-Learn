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
        
        public final Subscription subscribe(final Observer<? super T> observer) {
            if (observer instanceof Subscriber) {
                return subscribe((Subscriber<? super T>)observer);
            }
            return subscribe(new Subscriber<T>() {

                @Override
                public void onCompleted() {
                    observer.onCompleted();
                }

                @Override
                public void onError(Throwable e) {
                    observer.onError(e);
                }

                @Override
                public void onNext(T t) {
                    observer.onNext(t);
                }

            });
        }
        ...
    }
    
    public interface OnSubscribe<T> extends Action1<Subscriber<? super T>> {
        // cover for generics insanity
    }
    
    public interface Action1<T> extends Action {
        void call(T t);
    }

    public abstract class Subscriber<T> implements Observer<T>, Subscription {
    }
    
    public interface Observer<T> {
        void onCompleted();
        void onError(Throwable e);
        void onNext(T t);
    }
    
    1.
    //Observable.create(OnSubscribe<T> f);
    Observable.create(new OnSubscribe<String>() {
        @Override
        public void call(Subscriber<? super String> subscriber) {
            subscriber.onNext("Hello World!");
            subscriber.onCompleted();
        }
    })
    2.  
    //Observable.subscribe(final Observer<? super T> observer);  
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