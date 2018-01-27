主要讲解三种场景。
1. 跳转到普通的Activity
2. 跳转到透明的Activity
3. 跳转到FragmentActivity

![](https://github.com/teletian/Android/blob/master/LifeCycle/img/lifecycle.png)  
  
## 普通Activity

```
首先一个Activity启动会依次调用  
onCreate -> onContentChanged -> onStart -> onPostCreate ->  
onResume -> onPostResume -> onAttachedToWindow -> onWindowFocusChanged
```

#### onContentChanged
当Activity的布局改动时会调用onContentChanged。  
setContentView()或者addContentView()方法执行完毕时就会调用该方法。

#### onPostCreate、onPostResume
onCreate，onResume彻底执行完毕会调用。一般用不到。  
onPostCreate在onStart或者onRestoreInstanceState之后执行。

#### onAttachedToWindow
这个是View被附加到窗体上的时候调用

#### onWindowFocusChanged
得到和失去焦点的时候调用，这个回掉一般也用来判断对用户是否可见。

```
按Home键之后，依次调用  
onWindowFocusChanged -> onPause -> onSaveInstanceState -> onStop  
```

#### onSaveInstanceState
保存实例状态。  
当一个Activity"容易"被系统销毁时，这个方法就会执行。比如按Home之后，跳转到其他Activity之后，之后的某个时间点是有可能被系统销毁的。  
用户有意识的操作不会调用此方法，比如按了Back键。  
onSaveInstanceState会在onStop之前执行，但是有可能在onPause之前也有可能在onPause之后。

```
按Home之后，再次打开App，依次调用
onRestart -> onStart -> onResume -> onPostResume -> onWindowFocusChanged
```

```
Activity A 跳转到 Activity B，依次调用
A的onWindowFocusChanged -> A的onPause -> B的onCreate -> B的onContentChanged ->  
B的onStart -> B的onPostCreate -> B的onResume -> B的onPostResume ->  
B的onAttachedToWindow -> B的onWindowFocusChanged -> A的onSaveInstanceState -> A的onStop
```

```
Activity B 返回到 Activity A，依次调用
B的onPause -> A的onRestart -> A的onStart ->  
A的onResume -> A的onPostResume -> A的onWindowFocusChanged ->  
B的onWindowFocusChanged -> B的onStop -> B的onDestroy -> 
```
