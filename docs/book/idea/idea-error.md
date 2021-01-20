<center>

![logo](../../media/idea/logo.svg ':size=10%')

### <font color=red>IDEA Error</font> <!-- {docsify-ignore} -->
</center>

!> **idea 启动 spring boot 程序有时会遇到错误: Command line too long ..., 解决办法是在 .idea/workspace.xml
中的 component 标签中添加如下内容:**
```xml
  <component name="PropertiesComponent">
    <property name="dynamic.classpath" value="true" />
  </component>
```

