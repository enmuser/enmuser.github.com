

```java
//JVM为每个加载的class创建Class实例，并在实例中保存该class的所有信息，如果获取了某个Class的实例，就获取了该实例对应的class的所有信息，通过Class实例可以获取class的信息的方法称为反射(Reflection)
Class实例在JVM中是唯一的
    可以用==比较两个Class
Integer n = new Integer(123);
boolean b3 = n instanceof Integer;//true
boolean b3 = n instanceof Number;//true
boolean b1 = n.getClass()==Integer.class;//true
boolean b2 = n.getClass()==Number.class;//false
//获取Class引用的三种方式
Class strClass1 = String.class;

String abc = new String("abc");

Class strClass2 = abc.getClass();

Class strClass3 = Class.forName("java.lang.String");
//从Class实例中获取class的信息
strClass1.getName();//包名+类名
strClass1.getSimpleName();//类名
strClass1.getPackage().getName(); //包名
//从Class实例中判断class的信息
strClass1.isInterface();
strClass1.isEnum();
strClass1.isArray();
strClass1.Primitive();//是不是基本类型
//创建class的实例
strClass1.newInstance();
```



```java
//通过Class获取field信息
//getField(name)  获取某个public的field(包括父类)
//getDeclaredField(name) 获取当前类的某个field(不包括父类)
//getFields()  获取所有public的field(包括父类)
//getDeclaredFields() 获取当前类所有field(不包括父类)
	//Field对象包含field的所有信息
    //getName()
	//getType()
	//getModifiers()
//获得一个实例field的值
// field.get(Object(实例)) 等价于 实例.value
Integer n = new Integer(123);
Class cls = n.getClass();
Field field = cls.getDeclaredField("value");
field.setAccessible(true);
System.out.println(field.get(n));
//设置field的值
//field.set(Object(实例)，Object(值))
	field.set(n,456);

    Student stu = new Student();
    Class cls = stu.getClass();
    Field field = cls.getDeclaredField("name");
    System.out.println(field.getName());//name
    System.out.println(field.getType());//class java.lang.String
    System.out.println(Modifier.isPublic(field.getModifiers()));//true
    System.out.println(Modifier.isPrivate(field.getModifiers()));//false
    System.out.println(Modifier.isStatic(field.getModifiers()));//false

//public static int number = 0;获取静态属性，不需要传递对象
    Student stu = new Student();
    Class cls = stu.getClass();
    Field field = cls.getDeclaredField("number");
    System.out.println(field.get(null));
    field.set(null,2);
    System.out.println(field.get(null));
        
```



```java
//通过Class获取field信息
//getMethod(name,Class...)  获取某个public的method(包括父类)
//getDeclaredMethod(name,Class...) 获取当前类的某个method(不包括父类)
//getMethods()  获取所有public的method(包括父类)
//getDeclaredMethods() 获取当前类所有method(不包括父类)
//Method对象包含了一个方法的所有信息
//getName() getReturnType() getParameterTypes() getModifiers()
            Integer n = new Integer(123);
            Class cls = n.getClass();
            Method[] ms = cls.getMethods();
            for(Method m:ms)
            {
                m.getName();
                Class returnCls = m.getReturnType();
                Class[] paramTypes = m.getParameterTypes();
                m.getModifiers();
            }
//调用无参的Method  Object invoke(Object obj(实例))
            Integer n = new Integer(123);
            Class cls = n.getClass();
            Method m = cls.getMethod("toString");
            String s = (String) m.invoke(n);
            System.out.println(s);
//调用带有参数的Method  Object invoke(Object obj(实例)，Object... arg(参数))
            Integer n = new Integer(123);
            Class cls = n.getClass();
            Method m = cls.getMethod("compareTo", Integer.class);
            Integer r = (Integer) m.invoke(n,456);
            System.out.println(r);
//通过Class.newInstance()只能public的无参构造函数
	        String s = String.class.newInstance();
            Integer i = Integer.class.newInstance();//报错
//Constructor对象包含一个构造方法的所有信息，可以创建一个实例
            Constructor intCon1 = Integer.class.getConstructor(int.class);
            Integer instance1 = (Integer) intCon1.newInstance(123);
            Constructor intCon2 = Integer.class.getConstructor(String.class);
            Integer instance2 = (Integer) intCon2.newInstance("123"); 
//通过Class获取Constructor信息
//getConstructor(Class...)  获取某个public的Constructor
//getDeclaredConstructor(Class...) 获取某个Constructor
//getConstructors()  获取所有public的Constructor
//getDeclaredConstructors() 获取当前类所有Constructor

//获取父类的Class
//Class getSuperClass()
//Object的父类是null
//interface的父类是null
Class sup = Integer.class.getSuperClass();//Number.class
Object.class.getSuperClass();//null
Runnable.class.getSuperClass();//null

//获取当前类直接实现的interface：
//Class[] getInterface()
//不包括间接实现的interface
//没有interface的class返回空数组
//interface返回继承的interface
Class[] ifs = Integer.class.getInterfaces();//[Comparable.class]
Class[] ifs = java.util.ArrayList.class.getInterfaces();
//[List.class,RandomAccess.class,Cloneable.class,Serializable.class]
Class[] ifs = Math.class.getInterfaces();//[]
Class[] ifs = java.util.List.class.getInterfaces();//[Collection.class]
//判断向上转型是否成立
boolean isAssignableFrom(Class cls)
Number.class.isAssignableFrom(Integer.class)//true
Integer.class.isAssignableFrom(Number.class)//false
```



