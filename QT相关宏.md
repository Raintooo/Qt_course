- [QT相关宏](#qt相关宏)

# QT相关宏

### ```Q_DECLARE_PRIVATE```  

```C
#define Q_DECLARE_PRIVATE(Class) \
    inline Class##Private* d_func() noexcept \
    { Q_CAST_IGNORE_ALIGN(return reinterpret_cast<Class##Private *>(qGetPtrHelper(d_ptr));) } \
    inline const Class##Private* d_func() const noexcept \
    { Q_CAST_IGNORE_ALIGN(return reinterpret_cast<const Class##Private *>(qGetPtrHelper(d_ptr));) } \
    friend class Class##Private;
```

* 这个本质上是定义一个```d_func()```函数返回IMPL类对象指针
* 这里要注意的是 使用这个宏需要先声明IMPL类指针```d_ptr```

> IMPL是一种C++接口设计模式, 本质上就是为了隐藏具体成员以及实现

### ```Q_PRIVATE_SLOT```

如果我们用QT写一个C++接口的库的时候, 有时候会用到IMPL类的槽函数, 那么就可以用这个QT提供的宏, 这样就可以直接用上IMPL类声明的槽

例如:
```Q_PRIVATE_SLOT(d_func(), void onClicked(bool))```

注意: 使用```Q_PRIVATE_SLOT```前需要先声明有IMPL类指针对象, 一般直接声明```Q_DECLARE_PRIVATE(xxx)```

本质上, 这个宏应该是用来告诉QT的moc工具, 在后续生成的moc文件中的 ```metacall```里面由原来的```obj->onClicked```改成 ```obj->d_func()->onClicked```

### ```Q_DECLARE_METATYPE```

这个宏是用来把我们自定义的类型注册到```QVariant```中去  
如果还需要把自定义类型用在信号槽或者```QObject```的属性系统中去, 就要还需要调用```qRegisterMetaType<T>```
