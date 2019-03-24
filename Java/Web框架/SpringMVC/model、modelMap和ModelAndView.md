ModelMap继承LinkedHashMap，spring框架自动创建实例并作为controller的入参，用户无需自己创建

Model与model用法差不多

ModelAndView指模型和视图的集合，既包含模型 又包含视图；ModelAndView的实例是开发者自己手动创建的
```
ModelAndView view = new ModelAndView("test");
    //将数据放置到ModelAndView对象view中,第二个参数可以是任何java类型
    view.addObject("name",name);
```

## 取值方式
1. request.getSession().getAttribute("test")
2. request.getAttribute("test")
3. ${test}     map.addAttribute("test", "张三"); 
4. ${test}		model.addAttribute("test", "张三")

需要注意${test}这个取值方式对以上四种都适用，但取值的优先级不同，优先取Model和ModelMap的，Model和ModelMap是同一个东西，谁最后赋值的就取谁的，然后是request，最后是从session中获取