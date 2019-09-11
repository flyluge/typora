# SSH细节

## struts2.5

### 1 struts2.5安全机制

- 使用通配符的方式配置方法不能被访问

  - 解决方案

    1. 关闭严格方法调用

       ```xml
        <package name = "default" namespace="/" extends="struts-default" strict-method-invocation="false">
       ```

    2. 将方法加入白名单(设置全局允许通行的方法)

       ```xml
       <global-allowed-methods>m1,m2</global-allowed-methods>
       ```

    3. 在action配置可以访问的方法

       ```xml
       <allowed-methods>method1,method2</allowed-methods>
       ```

       