---
layout: post_layout
title:  「Spring」Java校验API所提供的校验注解
time: 2018年05月14日 星期一
location: 北京
pulished: true
excerpt_separator: "```"
---

Java 校验API （Java Validation API 又称JSR-303）。

<!--more-->

从Spring3.0开始，在Spring中提供了对Java校验API的支持，在springmvc中如果要使用java校验API的话，只需要保证路径底下有这个校验API的实现即可，比如Hibernate Validator。

Java校验API定义了多个注解，这些注解可以放到属性上，从而限制这些属性的值。所有的注解都位于javax.validation.constraints包中。

例如@NotNull:

```java
/*
 * Bean Validation API
 *
 * License: Apache License, Version 2.0
 * See the license.txt file in the root directory or <http://www.apache.org/licenses/LICENSE-2.0>.
 */
package javax.validation.constraints;

import static java.lang.annotation.ElementType.ANNOTATION_TYPE;
import static java.lang.annotation.ElementType.CONSTRUCTOR;
import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.ElementType.METHOD;
import static java.lang.annotation.ElementType.PARAMETER;
import static java.lang.annotation.ElementType.TYPE_USE;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

import java.lang.annotation.Documented;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import javax.validation.Constraint;
import javax.validation.Payload;
import javax.validation.constraints.NotNull.List;

/**
 * The annotated element must not be {@code null}.
 * Accepts any type.
 *
 * @author Emmanuel Bernard
 */
@Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE })
@Retention(RUNTIME)
@Repeatable(List.class)
@Documented
@Constraint(validatedBy = { })
public @interface NotNull {

	String message() default "{javax.validation.constraints.NotNull.message}";

	Class<?>[] groups() default { };

	Class<? extends Payload>[] payload() default { };

	/**
	 * Defines several {@link NotNull} annotations on the same element.
	 *
	 * @see javax.validation.constraints.NotNull
	 */
	@Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE })
	@Retention(RUNTIME)
	@Documented
	@interface List {

		NotNull[] value();
	}
}
```