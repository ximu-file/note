# 【python黑魔法】描述器（descriptor）（待理解）
#人工智能/python

> Python的黑魔法包括：  
> 1. 装饰器  
> 2. 迭代器  
> 3. 生成器  
> 4. 描述器  

看着篇之前需要先懂这篇文章： [【Python】类变量和实例变量](bear://x-callback-url/open-note?id=FA30D243-3489-4EEF-9636-F2203480F20C-3870-000013FDC6C0798B)

# 什么是描述器
描述器，即实现了描述符协议，即__get__, __set__, 和 __delete__方法的对象。当一个对象实现了描述符协议__get__和__set__，该对象（类也是对象，一切都是对象）即成为了一个资源描述器。








