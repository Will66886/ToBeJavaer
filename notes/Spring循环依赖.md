# Spring循环依赖

```java
@Component
public class A {
  private B b;
  public void setB(B b) {
    this.b = b;
  }
}
@Component
public class B {
  private A a;
  public void setA(A a) {
    this.a = a;
  }
}
```

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#doCreateBean

```java
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args){
	boolean earlySingletonExposure = (
    mbd.isSingleton() && //是否为单例模式
    this.allowCircularReferences &&//是否允许循环依赖
    isSingletonCurrentlyInCreation(beanName));//是否有标记正在创建中
	if (earlySingletonExposure) {
			addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));//getEarlyBeanReference获取全局变量还没有赋值的实例A，将其放入到三级缓存
	}
    populateBean(beanName, mbd, instanceWrapper);//将全局变量B注入到实例A中
}
```

