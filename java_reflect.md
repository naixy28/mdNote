**JAVA笔记初体验**
---
[TOC] 

# java反射
### Class类
:	类的类，属于`java.lang`, 任何类都是Class类的实例对象

#### Class类的三种表示方式
1.任何一个类都隐含的静态成员
```
	Class c1 = Foo.class;
```
2.该类对象`getClass()`方法 
```
	Class c2 = foo1.getClass();
```
3.调用Class的forName方法
```
	c3=Class.forName("com.reflect.Foo");
```
#### 通过Class类的实例获取类的实例
```java
	Foo foo = null;
        try {
            foo = (Foo)c1.newInstance();//要有无参数的构造方法
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
```

### 动态加载类
* 静态加载类即编译时加载，编译时就加载**所有可能**用到的类 (new是静态加载) 
```java
	//例 那两个类并不存在
	class Office{
		public static void main(String[] args){
			if("Word".equals(args[0])){
				Word w = new Word();
				w.start();
			}
			if("Excel".equals(args[0])){
				Excel e = new Excel();
				e.start();
			}
		}
	}
```

* 动态加载类即运行时加载
```java
	//例 
	class OfficeBetter{
		public static void main(String[] args){
			try{
				Class c=Class.forName(args[0]);
				//通过类类型创建类对象 oa为Word与Excel共同继承的接口
				OfficeAble oa = (OfficeAble)c.newInstance();
				oa.start();
			}
			catch(exception e){
				e.printStackTrace();
			}
		}
	}
```

### Method类获取方法信息
* void、基本数据类型都具有class属性
	`void.class`
* 通过Method类获取方法
:	方法对象，通过getMethods获得

```java
	//传入Object对象，获取其类对象
	Class c = obj.getClass();
	//获取类名
	System.out.prinrln(c,getName);
	//Method类
	//getMethods可获取所以public函数，包括父类继承
	Method[] ms = c.getMethods();
	//获取自己声明的方法
	c.getDeclaredMethods();
	for(int i =0;i<ms.length;i++){
	//获取返回值的类类型
		Class returnType = ms[i].getReturnType();
		System.out.println(returnType.getName()+" ");
	//获取方法名称
		System.out.println(ms[i].getName());
	//获取方法参数类类型数组
		Class[] paramTypes = ms[i].getParameterTypes();
		//通过循环遍历输出方法参数类型
		for(Class class1:paramTypes){
			System.out.println(class1.getName()+",");
		}

	}

```
### Field类获取成员变量信息
* Field类
:	成员变量也是对象，java.lang.reflect.Field
		`getFields()`所有public成员变量
		`getDeclaredFields()`所有自己声明的变量
* 获取Field数组
		`Field[] fs = c.getDeclaredFields();`
* 得到成员变量类型的类类型
		```java
			for(Field field:fs){
				Class fieldType = field.getType();
				String typeName = fieldtype.getName();
				//成员变量名
				String fieldName = field.getName();
				System.out.println();
			}
		```
### 获取构造函数的信息Constructor类
* 获取constructor类
	`Constructor[] cs = c.getDeclaredConstructors()`
* 获取参数列表
	`Class[] paramTypes = con.getParameterTypes()`
* 得到参数名字
	`getName()`

### 方法的反射
* 获取Method对象
	`Method m = A.class.getDeclaredMethod("print",int.class,int.class)`
* 反射方法
	```java
		Object o = m.invoke(a1,1,2)`//a1为已经实例化后的A对象
		//效果等同于a1.print(1,2) o为返回值，无则null
	```

### 补充内容
* 泛型是作用在编译时，编译后的泛型是去泛型化的
* 反射是动态加载在编译后的，因此可以通过反射绕过泛型
