## @Scope的value的选择
* singleton
* prototype
* request
* session
* application

无状态的类（成员变量不会变化）使用sington，性能更高

*   `application`作用域是每个`ServletContext`中包含一个，而不是每个Spring`ApplicationContext`之中包含一个（某些应用中可能包含不止一个`ApplicationContext`）。
*   `application`作用域仅仅作为`ServletContext`的属性可见，单例Bean是`ApplicationContext`可见。
