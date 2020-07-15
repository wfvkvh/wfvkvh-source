---
layout: post_layout
title: 「Spring」DeclareParents注释声明对象的AOP错误
time: 2018年05月10日 星期四
location: 北京
pulished: true
excerpt_separator: "```"
---

今天在使用Spring的 @DeclareParents注释时，使用AspectJ拓展对象的接口

<!--more-->

```java
public interface Encoreable {
    void nextPerformance();
}

@Component
public class DefaultEncoreablePerform implements Encoreable {

    @Override
    public void nextPerformance() {
        System.out.println("这是一场返场表演");
    }
}

public interface Performance {
    public void perform(int num);
}

@Component
public class MusicPerformance implements Performance {

    @Override
    public void perform(int num) {
        if (num > 0) {
            System.out.println(1 / num);
        } else {
            System.out.println("Concurred Exception ");
            throw new NumberFormatException();
        }

        System.out.println("精彩的表演");
    }
}


@Configuration
@ComponentScan
@EnableAspectJAutoProxy
public class ConfigAop {
}

@Aspect
@Component
public class AddAspectAudience {

    @DeclareParents(value = "Performance+", defaultImpl = DefaultEncoreablePerform.class)
    public static Encoreable encoreable;
}

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = com.aop.ConfigAop.class)
public class AopAspectjTest {

    @Autowired
    private Performance performance;

    @Test
    public void testAop() {
        performance.perform(1);
        Encoreable encoreable = (Encoreable) (performance);
        encoreable.nextPerformance();

    }

}
```
测试时一直提示对象不能强转，找半天找不到原因不知道为什么，暂时记一下
