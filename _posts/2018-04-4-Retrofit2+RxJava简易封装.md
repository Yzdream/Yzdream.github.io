---
layout: post
title: 'Retrofit2+Rxjava简易封装'
date: 2018-04-4
categories: Android
tags: Android
---

Retrofit作为当下主流网络框架之一，在github上已达到27.2的Star，而在Retrofit2的基础上拓展Rxjava和RxAndroid使得网络处理更加的方便，对于网络设置，数据处理也有了更多了拓展性。

### 添加依赖
```android
compile 'com.squareup.retrofit2:retrofit:2.3.0'         //retrofit网络
compile 'com.squareup.retrofit2:adapter-rxjava:2.3.0'   //RxJava回调适配器
compile 'com.squareup.retrofit2:converter-gson:2.3.0'   //Gson转换器
compile 'io.reactivex:rxandroid:1.2.1'                  //Rxandroid
```

### api接口
```android
public interface RetrofitService {

    @POST("api/user/login")
    Observable<BaseRespond<UserData>> getCode(@Body RequestBody body);
}
```
说明：请求参数这里我是用RequestBody是为了方便加密处理，如不需加密可直接使用Map或单个参数

### BaseRespond返回的实体
这里的BaseRespond使用了泛型，方便数据处理，能力有限不能封装为万能的方法。get和set方法这里就不贴了。
```android
public class BaseRespond<T> {

    private int status;

    private String msg;

    private T data;

    //此处省略get、set方法

}
```
### RetofitHttp
```android
public class RetofitHttp {

    private static RetofitHttp retofitHttp;
    private static RetrofitService retrofitService;
    private OkHttpClient client = new OkHttpClient();

    private RetofitHttp() {
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(RetrofitService.BASE_HTTP)
                .addCallAdapterFactory(RxJavaCallAdapterFactory.create()) // 使用RxJava作为回调适配器
                .addConverterFactory(ResponseConvertFactory.create()) // 使用Gson作为数据转换器
                .client(getClient())
                .build();
        retrofitService = retrofit.create(RetrofitService.class);
    }

    /**
     * 获取单例
     *
     * @return retrofitService
     */
    public static RetrofitService getInstance() {
        if (retofitHttp == null) {
            synchronized (RetofitHttp.class) {
                if (retofitHttp == null)
                    retofitHttp = new RetofitHttp();
            }
        }
        return retrofitService;
    }

    //此处是参数加密
    public static RequestBody geiBody(Map<String, Object> pragma) {
        return RequestBody.create(MediaType.parse("application/json; charset=utf-8"),
                CyptoUtils.encode(new JSONObject(pragma).toString()));
    }


    /**
     * 网络连接设置
     *
     * @return OkHttpClient
     */
    private OkHttpClient getClient() {
        try {
            SSLContext sslContext = SSLContext.getInstance("SSL");
            sslContext.init(null, new TrustManager[]{getTrustManager()}, null);
            SSLSocketFactory sslSocketFactory = sslContext.getSocketFactory();

            client = new OkHttpClient.Builder()
                    .connectTimeout(10, TimeUnit.SECONDS)
                    .writeTimeout(10, TimeUnit.SECONDS)
                    .readTimeout(10, TimeUnit.SECONDS)
                    .sslSocketFactory(sslSocketFactory, getTrustManager())//证书认证 使用HTTPS协议
                    .build();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return client;
    }

    /**
     * 证书认证 使用HTTPS协议
     *
     * @return X509TrustManager
     */
    private X509TrustManager getTrustManager() {
        try {
            TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
            trustManagerFactory.init((KeyStore) null);
            TrustManager[] trustManagers = trustManagerFactory.getTrustManagers();
            if (trustManagers.length != 1 || !(trustManagers[0] instanceof X509TrustManager)) {
                throw new IllegalStateException("Unexpected default trust managers:" + Arrays.toString(trustManagers));
            }
            return (X509TrustManager) trustManagers[0];
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

}

```
说明：ResponseConvertFactory是自定义的转换器，用于查看返回的json和数据解密，如不需要可直接使用GsonConverterFactory作为转换器
### ResponseConvertFactory
```android
public class ResponseConvertFactory extends Converter.Factory {

    /**
     * Create an instance using a default {@link Gson} instance for conversion. Encoding to JSON and
     * decoding from JSON (when no charset is specified by a header) will use UTF-8.
     */
    static ResponseConvertFactory create() {
        return create(new Gson());
    }

    /**
     * Create an instance using {@code gson} for conversion. Encoding to JSON and
     * decoding from JSON (when no charset is specified by a header) will use UTF-8.
     */
    private static ResponseConvertFactory create(Gson gson) {
        return new ResponseConvertFactory(gson);
    }

    private final Gson gson;

    private ResponseConvertFactory(Gson gson) {
        if (gson == null) throw new NullPointerException("gson == null");
        this.gson = gson;
    }

    @Override
    public Converter<ResponseBody, ?> responseBodyConverter(Type type, Annotation[] annotations,
                                                            Retrofit retrofit) {
        return new GsonResponseBodyConverter<>(gson, type);
    }

}
```

### GsonResponseBodyConverter
```android
public class GsonResponseBodyConverter<T> implements Converter<ResponseBody, T> {
    private final Gson gson;
    private final Type type;

    GsonResponseBodyConverter(Gson gson, Type type) {
        this.gson = gson;
        this.type = type;
    }

    @Override
    public T convert(@NonNull ResponseBody value) throws IOException {
        String response = CyptoUtils.decode(value.string());
        Logger.json(response);
        return gson.fromJson(response, type);
    }
}
```

封装到这里网络请求就可以简单的使用了，使用方法：
```android
 RetofitHttp.getInstance().getCode(geiBody(pragma)).
                subscribeOn(Schedulers.io()).   //网络线程
                unsubscribeOn(Schedulers.io()).
                observeOn(AndroidSchedulers.mainThread()) //返回后的线程
                .subscribe(new Observer<BaseRespond<CategoryResultData>>() {
                    @Override
                    public void onCompleted() {
                        //网络请求完成
                    }

                    @Override
                    public void onError(Throwable e) {
						//网络请求失败
                    }

                    @Override
                    public void onNext(BaseRespond<CategoryResultData> categoryResultDataBaseRespond) {
						//返回结果
                    }
                });
```
可以看到这样的使用比较麻烦，每次请求都要切换线程，抛异常，判断状态，所以需要进一步的封装

### FilteredFactory
```android
public class FilteredFactory {

    private final static Observable.Transformer transformer = new SimpleTransformer();

    /**
     * 将Observable<BaseResponse<T>>转化Observable<T>,并处理BaseResponse
     *
     * @return 返回过滤后的Observable.
     */
    @SuppressWarnings("unchecked")
    public static <T> Observable<T> compose(Observable<BaseRespond<T>> observable) {
        return observable.compose(transformer);
    }

    /**
     * 转换Observable.
     */
    private static class SimpleTransformer<T> implements Observable.Transformer<BaseRespond<T>, T> {
        //这里对Observable,进行一般的通用设置.不用每次用Observable都去设置线程以及重连设置
        @Override
        public Observable<T> call(Observable<BaseRespond<T>> observable) {
            return observable.subscribeOn(Schedulers.io())
                    .observeOn(AndroidSchedulers.mainThread())
                    .unsubscribeOn(Schedulers.io())
                    .timeout(5, TimeUnit.SECONDS)//重连间隔时间
                    .retry(5)//重连次数
                    .flatMap(new Func1<BaseRespond<T>, Observable<T>>() {
                        @Override
                        public Observable<T> call(BaseRespond<T> tBaseResponse) {
                              return flatResponse(tBaseResponse);  //返回的结果
                        }
                    });
        }


        /**
         * 处理请求结果,BaseResponse
         *
         * @param response 请求结果
         * @return 过滤处理, 返回只有data数据的Observable
         */
        private Observable<T> flatResponse(final BaseRespond<T> response) throws RuntimeException {
            return Observable.create(new Observable.OnSubscribe<T>() {
                @Override
                public void call(Subscriber<? super T> subscriber) {
                    if (response.getStatus() == 10) {//根据服务器返回的状态进行判断
                        if (!subscriber.isUnsubscribed()) {
                            subscriber.onNext(response.getData());//返回数据成功
                        }
                    } else {//失败
                        if (!subscriber.isUnsubscribed()) {
                            subscriber.onError(new ApiExecption(response.getStatus(), response.getMsg()));
                        }
                        return;
                    }
                    if (!subscriber.isUnsubscribed()) {//请求完成
                        subscriber.onCompleted();
                    }
                }
            });
        }
    }
}

```
### ApiExecption错误拦截
```android

/**
 * 捕获异常
 * 将服务器返回的msg加入ApiExecption的msg中
 * 也可以根据和服务器约定好的errorCode来判断
 * Created by 残梦 on 2018/3/30.
 */

public class ApiExecption extends IOException {

    private int errorCode;

     ApiExecption(int code, String msg) {
        super(msg);
        this.errorCode = code;
    }

    public int getErrorCode() {
        return errorCode;
    }
}

```

### CustomObserver自定义订阅者
```android
public abstract class CustomObserver<T> implements Observer<T> {

    @Override
    public void onCompleted() {

    }

    @Override
    public void onError(Throwable e) {

       if (e instanceof HttpException) {
            HttpException httpException = (HttpException) e;
            switch (httpException.code()) {
                case UNAUTHORIZED:
                    onError("当前请求需要用户验证");
                    break;
                case FORBIDDEN:
                    onError("服务器已经理解请求，但是拒绝执行它");
                    break;
                case NOT_FOUND:
                    onError("服务器异常，请稍后再试");
                    break;
                case REQUEST_TIMEOUT:
                    onError("请求超时");
                    break;
                case GATEWAY_TIMEOUT:
                    onError("作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI 标识出的服务器，例如 HTTP、FTP、LDAP）或者辅助服务器（例如 DNS）收到响应");
                    break;
                case INTERNAL_SERVER_ERROR:
                    onError("服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理");
                    break;
                case BAD_GATEWAY:
                    onError("作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应");
                    break;
                case SERVICE_UNAVAILABLE:
                    onError("由于临时的服务器维护或者过载，服务器当前无法处理请求");
                    break;
                default:
                    onError("网络错误");  //其它均视为网络错误
                    break;
            }
        } else if (e instanceof ApiExecption) {
            ApiExecption execption = (ApiExecption) e;
           onError(execption.getMessage());
        } else if (e instanceof ConnectException) onError("连接失败");
        else if (e instanceof JsonParseException || e instanceof JSONException) onError("解析失败");
        else if (e instanceof SSLHandshakeException) onError("证书验证失败");
        else if (e instanceof UnknownHostException) onError("网络异常");
        else if (e instanceof SocketTimeoutException) onError("连接超时");
        else{
            e.printStackTrace();
            onError("服务器错误");
        }

    }

    @Override
    public void onNext(T response) {
        onSuccess(response);
    }

    public abstract void onSuccess(T response);

    public abstract void onError(String msg);
}

```

### 网络调用
```android
//Model层
public class TestModel implements ITestModel {

    @Override
    public Observable<CategoryResultData> getData(Map<String, Object> pragma) {
        return FilteredFactory.compose(RetofitHttp.getInstance().getCode(geiBody(pragma)));

    }
}
//Presenter层
public class MainPresenter implements IMainPresenter {

    private TestModel testModel;
    private IMainActivity mView;

    private CompositeSubscription compositeSubscription;


    public MainPresenter(IMainActivity mView) {
        this.mView = mView;
        testModel = new TestModel();
        compositeSubscription = new CompositeSubscription();
    }

    @Override
    public void getData() {
        if (mView != null) {
            mView.showLoading();
            Map<String, Object> pragma = new HashMap<>();
            pragma.put("account", "17610069507");
            pragma.put("pwd", "123");
            compositeSubscription.add(testModel.getData(pragma).subscribe(new CustomObserver<CategoryResultData>() {

                @Override
                public void onSuccess(CategoryResultData response) {

                }


                @Override
                public void onError(String msg) {
                    mView.onError(msg);
                }
            }));
        }
    }

    @Override
    public void onDestory() {
        mView = null;
        if (compositeSubscription.hasSubscriptions())
            compositeSubscription.unsubscribe();//结束所有add的subscribe事件
    }

}

```
CompositeSubscription用于管理网络的生命周期，防止有内层泄露。

### 混淆
```android
# Retain generic type information for use by reflection by converters and adapters.
-keepattributes Signature
# Retain service method parameters.
-keepclassmembernames,allowobfuscation interface * {
    @retrofit2.http.* <methods>;
}
# Ignore annotation used for build tooling.
-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequireme
```



<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="MTAyMC8zMjU2Ny85MTI4">
<script type="text/javascript">
   (function(d, s) {
   var j, e = d.getElementsByTagName(s)[0];

   if (typeof LivereTower === 'function') { return; }

   j = d.createElement(s);
   j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
   j.async = true;

   e.parentNode.insertBefore(j, e);
   })(document, 'script');
</script>
<noscript> 为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->