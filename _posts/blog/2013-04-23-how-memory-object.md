---
layout: post
title: How much memory is used of an object in Java?
description: The GC automatically finds the garbage objects and removes them for reuse. However, there is no easy way to find out how much memory is used of an object
category: blog
tags: java, jvm
---

The garbage collection(GC), also known as automatic memory management, allows developers to create new objects without worrying explicitly about memory allocation and deallocation, because  the GC automatically finds the garbage objects and removes them for reuse. However, there is no easy way to find out how much memory is used of an object:

```java
    Object obj = new Object()
```

### Memory in Stack
The new operator instantiates a class by allocating memory in Java Heap for a new object and returning a reference to that memory. Obviously, `obj` is a reference to an `Object`, it is on the method call stack, which is referred to as "Java Virtual Machine Stacks". The size an object reference depends on the JVM and machine architecture. Generally, it is 4 bytes on a 32-bit JVM. For a 64-bit JVM, the size of a reference is 8 bits, unless you have configured the JVM to use [compressed pointers][1].

### Memory in Heap
Based on the concepts of the Object-Oriented Programming (OOP), the objects have data fields and behavior. The `obj`, although has no data fields, contains behavior(12 methods are defined in [`Object`][2]). Since the `obj` is point to the memory in Java Heap after new operation execution, there must be memory allocating in Java Heap.

In [The Java® Virtual Machine Specification][3],there is no relative description about the memory of the object. But we can use the [jvisualvm][4] to analyze the memory in Java Heap. In order to browse in jvisualvm, the `EmptyObject`, a private static inner class, instead of `Object` is defined. Because the definition of `EmptyObject` is empty, it is equal to `Object`.

```java

import java.util.List;
import java.util.ArrayList;
public class EmptyObject {

	public static void main(String[] args) throws InterruptedException{

		List<EmptyObject> emptys = new ArrayList<>();
		for(int i=0;i<10;i++){
		emptys.add(new EmptyObject());
	}

	Thread.sleep(600*1000);
	}
	private static class EmptyObject{}
}

```

**Figure 1. The heap memory in 32-bit JVM**
![](http://gaolei.me/images/new_object_32.png)
**Figure 2. The heap memory in  64-bit JVM**
![](http://gaolei.me/images/new_object_64.png)

As the above two figures show, the memory of each object is 8 bytes in 32-bit JVM. And the memory of each object is 16 bytes in 64-bit JVM.  So much more data is allocated than you might expect when your Java code uses the new operator to create an instance of a Java object. The additional overhead is metadata that the JVM uses to describe the Java object and the amount of object metadata varies by JVM version and vendor, but it typically consists of:

* Class : A pointer to the class information, which describes the object type. In the case of a java.lang.Integer object, for example, 	this is a pointer to the java.lang.Integer class.
* Flags : A collection of flags that describe the state of the object, including the hash code for the object if it has one, and the shape of the object (that is, whether or not the object is an array).
* Lock : The synchronization information for the object — that is, whether the object is currently synchronized.
* Padding: potentially a few "wasted" unused bytes after the object data, to make every object start at an address that is a convenient multiple of bytes and reduce the number of bits required to represent a pointer to an object.
	
[1]: http://docs.oracle.com/javase/7/docs/technotes/guides/vm/performance-enhancements-7.html#compressedOop
[2]: http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html
[3]: http://docs.oracle.com/javase/specs/jvms/se7/html/
[4]: http://docs.oracle.com/javase/7/docs/technotes/tools/share/jvisualvm.html

