### bean实例化

-----

##### bean是如何创建出来的

- bean本质上就是对象，创建bean使用构造方法完成

  ```java
  public BookDaoImpl() {
      System.out.println("book dao constructor is running ... success");
  }
  
  // 该构造方法在bean的对象创建的时候也会使用
  // 且不管该构造方法是公共的还是私有的
  // 如果是私有的，那么使用了反射机制
  ```

  Spring创建bean的时候调用的是**无参的构造方法**

  如果没有提供无参的构造方法（如果自己构造就使用自己写的，如果没有，系统自动分配），将抛出异常：BeanCreationException

##### :star2:spring的报错如何阅读呢

- 拉到最后，查看最后一个异常信息
- 如果处理了最后一个异常信息能够解决问题，则ok，如果不能，就往上再解决倒数第二个
- 以此类推，依次往上解决异常信息