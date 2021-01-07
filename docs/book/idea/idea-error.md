<center>

![Jdk](../../media/jetbrains.svg ':size=10%')

### <font color=red>IDEA Error</font> <!-- {docsify-ignore} -->
</center>

!> **idea启动spring boot程序有时会遇到错误:Command line too long ...,解决办法是在.idea/workspace.xml中的component标签中添加如下内容:**
```xml
  <component name="PropertiesComponent">
    <property name="dynamic.classpath" value="true" />
  </component>
```

