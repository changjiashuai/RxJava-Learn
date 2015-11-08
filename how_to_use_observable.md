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
        
        public final Subscription subscribe(Subscriber<? super T> subscriber) {
            return Observable.subscribe(subscriber, this);
        }
        
        //订阅方法内部实现
        private static <T> Subscription subscribe(Subscriber<? super T> subscriber, Observable<T> observable) {
            if (subscriber == null) {
                //没有订阅者
                throw new IllegalArgumentException("observer can not be null");
            }
            if (observable.onSubscribe == null) {
                //订阅事件不能为空
                throw new IllegalStateException("onSubscribe function can not be null.");
            }
            //开始订阅
            subscriber.onStart();
            if (!(subscriber instanceof SafeSubscriber)) {
                subscriber = new SafeSubscriber<T>(subscriber);
            }
            try {
                hook.onSubscribeStart(observable, observable.onSubscribe).call(subscriber);
                return hook.onSubscribeReturn(subscriber);
            } catch (Throwable e) {
                Exceptions.throwIfFatal(e);
                try {
                    //出错处理
                    subscriber.onError(hook.onSubscribeError(e));
                } catch (OnErrorNotImplementedException e2) {
                    throw e2;
                } catch (Throwable e2) {
                    RuntimeException r = new RuntimeException("Error occurred attempting to subscribe [" + e.getMessage() + "] and then again while trying to pass to onError.", e2);
                    hook.onSubscribeError(r);
                    throw r;
                }
                return Subscriptions.unsubscribed();
            }
        }
        ...
    }
    
    public interface OnSubscribe<T> extends Action1<Subscriber<? super T>> {
        // cover for generics insanity
    }
    
    public interface Action1<T> extends Action {
        void call(T t);
    }

    //订阅者 实现了 观察者接口
    public abstract class Subscriber<T> implements Observer<T>, Subscription {
    }
    
    public interface Observer<T> {
        void onCompleted();
        void onError(Throwable e);
        void onNext(T t);
    }
    
    //Observable.create(...).subscribe(...);
    
    1.创建
    //Observable.create(OnSubscribe<T> f);
    Observable.create(/*订阅事件*/new OnSubscribe<String>() {
        @Override
        public void call(Subscriber<? super String> subscriber) {
            subscriber.onNext("Hello World!");
            subscriber.onCompleted();
        }
    })
    2.订阅
    //Observable.subscribe(final Observer<? super T> observer);  
    //Observable.subscribe(Subscriber<? super T> subscriber);
    .subscribe(/*订阅者*/new Subscriber<String>() {
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