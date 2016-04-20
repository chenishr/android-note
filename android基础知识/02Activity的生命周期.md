##### 学会查看帮助文档

##### 安卓 Activity 的生命周期

```
    graph TD
    
    开始 --> onCreate
    
    onCreate --> onStrat
    
    onStrat --> onResume
    
    onResume --> running 

    running --> onPasue
    
    onPasue --> onStop
    
    onStop --> onDestroy
    
    onPasue --> onResume
    
    onStop --> |被隐藏| onRestart
    
    onRestart --> onStrat
    
    onStop --> |从内存擦除| onCreate
    
    onDestroy --> 结束
```

##### Activity 之间跳转时的生命周期

##### 跳转到半透明 Activity 之间的生命周期
