---
layout: post
title: 动态代理
date: 2018-03-31 00:00:00
tags: designMode
---
动态代理，动态的为代理对象生成代理。静态代理的类都是事先写好的，而动态代理就是根据代理对象实现的接口动态的生成代理类，使用编译器编译后加载至内存中。
动态代理需要逻辑处理类InvocationHandler实现开发者业务的需要，它主要扮演了两种作用。一是它与代理类之间形成了静态代理，代理类含有它的引用，二是它与代理对象之间也形成了静态代理，它含有代理对象的引用。总的来说，动态代理等同于两次的静态代理。
## 处理类

```
package com.vv.proxy;
import java.lang.reflect.Method;
public interface InvocationHandler {
    public void invoke(Object proxy,Method md);
}
```

## 代理类

```
package com.vv.proxy;
import java.io.File;
import java.io.IOException;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import javax.tools.JavaCompiler;
import javax.tools.JavaCompiler.CompilationTask;
import javax.tools.JavaFileObject;
import javax.tools.StandardJavaFileManager;
import javax.tools.ToolProvider;
import org.apache.commons.io.FileUtils;
public class Proxy {
	public static Object newInstance(Class clazz,InvocationHandler handler) throws Exception{
		String rt = "\r\n";
		String methodStr = "";
		Method[] methods = clazz.getMethods();
		for(Method method:methods){
			methodStr += "@Override"+rt+
						"public void "+method.getName()+"() {"+rt+
							"try{"+rt+
							"Method method = "+clazz.getName()+".class.getMethod(\""+method.getName()+"\");"+rt+
							"handler.invoke(this,method);"+rt+
							"}catch(Exception e){ e.printStackTrace();}"+rt+
						"}";
		}
		String string = "package com.vv.proxy;" +rt+
		"import java.lang.reflect.Method;"+rt+
		"public class $Proxy implements "+clazz.getName()+" {"+rt+
			"private InvocationHandler handler;"+rt+
			"public $Proxy(InvocationHandler handler) {"+rt+
				"super();"+rt+
				"this.handler = handler;"+rt+
			"}"+rt+
			methodStr+rt+
		"}";
		System.out.println(System.getProperty("user.dir"));
		String path = System.getProperty("user.dir").toString()+"/target/classes/com/vv/proxy/$Proxy.java";
		File file = new File(path);
		FileUtils.write(file, string);
		//编译器
		JavaCompiler compiler = ToolProvider.getSystemJavaCompiler();
		//文件管理者
		StandardJavaFileManager manager = compiler.getStandardFileManager(null, null, null);
		//获取文件
		Iterable<? extends JavaFileObject> javaFileObjects = manager.getJavaFileObjects(path);
        //编译任务
		CompilationTask task = compiler.getTask(null, manager, null, null, null, javaFileObjects);
		//进行编译
		task.call();
		manager.close();
		//加载到内存
		ClassLoader loader = ClassLoader.getSystemClassLoader();
		Class<?> loadClass = loader.loadClass("com.vv.proxy.$Proxy");
		Constructor<?> constructor = loadClass.getConstructor(InvocationHandler.class);
		return constructor.newInstance(handler);
	}
}
```

**生成计算时间的处理类**
```
package com.vv.proxy;
import java.lang.reflect.Method;
public class TimeHandler implements InvocationHandler {
	private Object target;
	public TimeHandler(Object target) {
		super();
		this.target = target;
	}
	public void invoke(Object proxy, Method md) {
		try {
			System.out.println("start time");
			md.invoke(target);
			System.out.println("end time");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

**测试方法**
```
package com.vv.proxy;
public class Test1 {
	public static void main(String[] args) throws Exception {
	    //汽车行驶的时间
		Movable movable = (Movable) Proxy.newInstance(Movable.class,new TimeHandler(new Car()));
		movable.move();
		
		//火车行驶的时间
		Movable movable = (Movable) Proxy.newInstance(Movable.class,new TimeHandler(new Train()));
		movable.move();
	}
}
```