---
layout: post
title: 静态代理
date: 2018-03-30 00:00:00
tags: designMode
---
静态代理有2种实现方式：聚合和继承

```
package com.vv.proxy;
public interface Moveable {
	void move();
}
```

```
package com.vv.proxy;
public class Car implements Movable {
	public void move() {
		System.out.println("Car run。。。");
	}
}
```

**使用继承方式计算Car行驶的时间**

```
package com.vv.proxy;
public class Car2 extends Car {
	@Override
	public void move() {
		System.out.println("开始时间");
		super.move();
		System.out.println("结束时间");
	}
}
```

**使用继承方式计算Car行驶的距离**

```
package com.vv.proxy;
public class Car3 extends Car {
	@Override
	public void move() {
		System.out.println("起始地");
		super.move();
		System.out.println("目的地");
	}
}
```

**使用继承方式先计算Car行驶的距离再计算行驶的时间**

```
package com.vv.proxy;
public class Car4 extends Car3 {
	@Override
	public void move() {
		System.out.println("开始时间");
		super.move();
		System.out.println("结束时间");
	}
}
```

**使用继承方式先计算Car行驶的时间再计算行驶的距离**

```
package com.vv.proxy;
public class Car5 extends Car2 {
	@Override
	public void move() {
		System.out.println("起始地");
		super.move();
		System.out.println("目的地");
	}
}
```

**总结：**从上面的例子可以每当我们增加一个需求，就会重新生成一个新的类，容易导致类数量增长。

**使用聚合方式计算Car行驶的时间**

```
package com.vv.proxy;
public class CarTimeProxy implements Movable {
	private Movable vehicle;
	public CarTimeProxy(Movable vehicle) {
		super();
		this.vehicle = vehicle;
	}
	public void move() {
		System.out.println("开始时间");
		super.move();
		System.out.println("结束时间");
	}
}
```

**使用聚合方式计算Car行驶的距离**

```
package com.vv.proxy;
public class CarDistanceProxy implements Movable {
	private Movable vehicle;
	public CarDistanceProxy(Movable vehicle) {
		super();
		this.vehicle = vehicle;
	}
	public void move() {
		System.out.println("起始地");
		super.move();
		System.out.println("目的地");
	}
}
```

**总结：**由于代理类含有代理对象的引用，而且同代理对象实现了同一接口，那也就意味着代理类可以含有代理类的引用，从而实现不同顺序的嵌套。聚合测试代码如下：

```
package com.vv.proxy;
public class Test {
	public static void main(String[] args) {
	    //先计算Car行驶的时间再计算行驶的距离
		Movable car = new Car();
		Movable cartimeProxy = new CarTimeProxy(car);
		Movable carlogProxy = new CarDistanceProxy(cartimeProxy);
		carlogProxy.move();
		
		//先计算Car行驶的距离再计算行驶的时间
		Movable car = new Car();
		Movable carlogProxy = new CarDistanceProxy(car);
		Movable cartimeProxy = new CarTimeProxy(carlogProxy);
		carlogProxy.move();
	}
}
```

上面的静态代理计算了汽车的时间和距离，如果想要计算火车的时间和距离、上班的时间和距离，是不是又要重复上述代码，由此想来也会引起类数量爆炸,于是就有了动态代理。