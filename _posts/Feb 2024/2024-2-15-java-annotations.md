---
layout: post
title: "Annotations in Java"
categories: Java
permalink: :title
---

<img src="/assets/Feb 2024/Java annotations/annotation.gif" alt="Annotate this" class="blog-gif-top">


## What are Java annotations and how can we customize them?

I recently began working on a Spring Boot project where Java annotations are extensively utilized. Realizing my superficial familiarity with annotations, I decided to delve deeper into the subject. This blog aims to enhance my comprehension and to captivate those who are yet to explore the realm of Java annotations. If you're in the same boat or just curious, stick around. Let's figure this out. 😊

### Introduction

**Definition:** Metadata that provides information about the program but doesn't affect its execution.
**Usage:** Compiler instructions, **build-time processing**, **runtime processing**. (This is important!)
**Common annotations:** @Override, @Deprecated, @SuppressWarnings.

#### Hierarchy of Java annotations
<img src="/assets/Feb 2024/Java annotations/JavaAnnotationshierarchy.jpg" alt="Annotate hierarchy" class="blog-gif-top">


### Types
#### Standard Annotations in Java SE

* @Override: Indicates that a method is intended to override a method declared in a superclass.

* @Deprecated: Marks a program element (class, method, field, etc.) as no longer recommended for use.

* @FunctionalInterface: Indicates that the type declaration is intended to be a functional interface, as defined by the Java Language Specification.

*And more..*
#### Meta-Annotations (Annotations that Apply to Other Annotations)

* @Retention: Specifies how long annotations with the annotated type are to be retained.

* @Documented: Indicates that elements annotated with the annotated type should be documented by javadoc and similar tools by default.

* @Target: Marks another annotation to restrict the kinds of Java elements to which the annotation can be applied.

*And more..*
#### Annotations for Special Purposes

* Annotations in Java EE: Java EE provides a set of annotations for simplifying enterprise application development, such as `@Entity`, `@Stateless`, `@WebService`, etc., used for ORM, EJBs, and web services, respectively.

* JUnit Annotations: Used for writing test cases. Common annotations include `@Test`, `@Before`, `@After`, `@BeforeEach`, `@AfterEach`, `@BeforeClass`, `@AfterClass`, and `@RunWith`.

* Spring Framework Annotations: Widely used in developing Spring applications, such as `@Component`, `@Controller`, `@Service`, `@Repository`, `@Autowired`, `@RequestMapping`, etc., for component scanning, dependency injection, and request mapping.

*And more..*

#### Custom Annotations
Developers can create their own annotations to express metadata that their application should process. Custom annotations can be designed to provide configuration data, runtime information, or compile-time instructions, among other uses.

Each type of annotation serves a specific purpose, from providing compiler instructions, generating documentation, or offering runtime information for frameworks and applications.
### Categories

#### Marker Annotations

Marker annotations in Java are annotations that do not contain any elements. They are used simply to mark a declaration, providing metadata without any additional configuration.

```java
// CustomMarkerAnnotation file
package org.keshavblogs.annotations;  
  
public @interface CustomMarkerAnnotation {  
}
```

```java
// Main.java
package org.keshavblogs;  
  
import org.keshavblogs.annotations.CustomMarkerAnnotation;  
  
@CustomMarkerAnnotation  
public class Main {  
    public static void main(String[] args) {  
        System.out.println("Hello world!");  
    }  
}
```

```java
public class ChildClass extends ParentClass {
    @Override
    public void someMethod() {
        // overridden method implementation
    }
}
```


#### Single-Value Annotations

Single-value annotations are annotations with only one element. These elements can have default values, simplifying their usage.

```java
package org.keshavblogs.annotations;  
  
public @interface CustomSingleValueAnnotation {  
    String value() default "My Default Value";  
}
```

```java
// Main.java
package org.keshavblogs;  
  
import org.keshavblogs.annotations.CustomSingleValueAnnotation;  
  
@CustomSingleValueAnnotation("Updated value")  
public class Main {  
    public static void main(String[] args) {  
        System.out.println("Hello world!");  
    }  
}
```

```java
// SuppressWarnings Annotation

// Class 1
class DeprecatedExample {
    @Deprecated
    public void display() {
        System.out.println("Deprecated example display()");
    }
}

// Class 2
public class SuppressWarningExample {
    // If we remove the following annotation, the program will generate warnings
    @SuppressWarnings({"checked", "deprecation"})
    public static void main(String args[]) {
        DeprecatedExample d1 = new DeprecatedExample();
        d1.display();
    }
}
```

#### Full Annotations

Full annotations in Java are annotations that have multiple elements, and when you use them, you need to provide values for all the elements. These annotations allow developers to configure and customize their usage in a more detailed manner compared to marker or single-value annotations.

```java
package org.keshavblogs;  
  
import org.keshavblogs.annotations.CustomFullAnnotation;  
  
@CustomFullAnnotation(myString = "Updated value", myInt = 5)  
public class Main {  
    public static void main(String[] args) {  
        System.out.println("Hello world!");  
    }  
}
```

```java
package org.keshavblogs.annotations;  
  
public @interface CustomFullAnnotation {  
    String myString() default "My default value";  
    int myInt() default 1;  
}
```


#### Repeatable Annotations 
Repeatable annotations in Java, introduced in Java 8, allow an annotation to be applied more than once to the same declaration or type use. Before Java 8, an annotation could be used only once on a given element. Repeatable annotations simplify code and make it more readable when multiple occurrences of the same annotation are needed.

- Annotations that can be used more than once on the same declaration or type use.
- Introduced in Java 8 to enhance code readability and flexibility.

```java
// CustomRepeatableAnnotationContainer.java
package org.keshavblogs.annotations;  
  
public @interface CustomRepeatableAnnotationContainer {  
    CustomRepeatableAnnotation[] value();  
}
```

```java
// CustomRepeatableAnnotation.java
package org.keshavblogs.annotations;  
  
import java.lang.annotation.Repeatable;  
  
@Repeatable(CustomRepeatableAnnotationContainer.class)  
public @interface CustomRepeatableAnnotation {  
    String myString() default "Default String";  
  
    int myInt() default 2;  
}
```

```java
package org.keshavblogs;  
  
import org.keshavblogs.annotations.CustomRepeatableAnnotation;  
  
@CustomRepeatableAnnotation  
@CustomRepeatableAnnotation(myInt = 10, myString = "Updated String")  
public class Main {  
    public static void main(String[] args) {  
        System.out.println("Hello world!");  
    }  
}
```

- Have to use @Repeatable annotation to make this work, otherwise we get an error.


#### Container Annotation
- A container annotation in Java is an annotation that holds an array of repeated annotations. Container annotations were introduced along with repeatable annotations in Java 8 to allow an annotation to be applied more than once to the same declaration or type use. 
- Container annotations provide a way to group multiple occurrences of the same annotation under a single container.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface RolesAllowedContainer {
    RolesAllowed[] value();
}
```

```java
@RolesAllowedContainer({@RolesAllowed("ADMIN"), @RolesAllowed({"USER", "MANAGER"})})
public class SecureResource {
    // Class implementation
}
```
- Also refer the previous example.

#### Type Annotations 

Type annotations in Java, introduced in Java 8, allow you to apply annotations to types. Types in Java include classes, interfaces, enums, and type parameters. Type annotations provide a more fine-grained way to express metadata about types, enabling developers to specify additional information at the level of types rather than just declarations or elements within the code.

See [@Target](https://docs.oracle.com/javase/tutorial/java/annotations/predefined.html)

```java
package org.keshavblogs.support;  
  
import org.keshavblogs.annotations.IsCustomTypeAnnotation;  

// Generic class with Type annotation  
public class  {  
    public T work() {  
        // Do something  
        return null;  
    }  
}
```

```java
package org.keshavblogs.annotations;  

import java.lang.annotation.*;  
@Target(ElementType.TYPE_USE) TypeAnnotationExample <@IsCustomTypeAnnotation(myString = "Parent class", myInt = 2) T>
public @interface IsCustomTypeAnnotation {  
    String myString() default "Custom Type String";  
    int myInt() default 1;  
}
```

- Please look up more on this only if you need it.
- Refer this [GFG - Annotations in Java](https://www.geeksforgeeks.org/annotations-in-java/#:~:text=Category%204%3A%20Type%20Annotations%C2%A0)

### Important Methods


1. annotationType() Method 
   - This method is available in the `Annotation` interface.
   - It returns the `Class` object corresponding to the annotation type.

   **Example:**
   ```java
   // " ? " meanswe are not sure what it would return. 
   // extends Annotation, means, it would definitely extend Annotation class. 
   Class<? extends Annotation> annotationType = myAnnotation.annotationType();
   ```


2. getDeclaredMethods() Method 
   - Available in the `Class` class.
   - Returns an array of `Method` objects reflecting all the declared methods of a class.

   **Example:**
   ```java
   Method[] methods = CustomClass.class.getDeclaredMethods();
   ```


3. Reflection and Annotations 
   - The `java.lang.reflect` package provides classes like `Method`, `Field`, and `Constructor` that allow you to inspect and interact with annotated elements.

   **Example (using reflection to get annotations on a method):**
   ```java
   Method method = CustomClass.class.getDeclaredMethod("someMethod");
   MyAnnotation annotation = method.getAnnotation(CustomAnnotation.class);
   ```


4. @Inherited Meta-Annotation
   - Indicates that an annotation type should be inherited by subclasses of annotated classes.

   **Example:**
   ```java
   @Inherited
   public @interface InheritedAnnotation {
       // Annotation elements, if any
   }
   ```



The end!

### References
1. [Coding with John](https://www.youtube.com/watch?v=DkZr7_c9ry8)
2. [Java docs](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)
