## idea java 报错 完全不提示

当IntelliJ IDEA中的Java代码出现报错但不提示时，可能是由于以下几个原因：12

1. 关闭了高亮报错提示：在IDE的设置中，可能不小心关闭了高亮显示报错的功能。可以通过`File -> Settings -> Editor -> General -> Appearance`检查`Show error highlights`选项是否勾选。
2. 开启了节电模式：IDE在开启省电模式时，可能会停止一些不必要的提示，以节省资源。检查IDE的左下角是否有省电模式的标志（通常是一个小勾勾），如果有，取消勾选即可。
3. 没有勾选项目自动编译：如果项目没有被设置为自动编译，那么代码错误可能不会被IDE即时检测。可以在`Build -> Compilation`中设置项目的编译行为。
4. Inspections设置不正确：在IDE的右下角，可能有一个类似侦探帽子的按钮（Inspections设置）。如果箭头是指向Inspections，可能需要将其调整为Syntax或者其他适当的检查级别。
5. 调整了高亮水平：在IDE的设置中，可能调整了高亮水平至None，导致所有错误提示痕迹消失。可以通过`File -> Settings -> Editor -> Color Scheme -> General -> Syntax Highlighting`检查高亮水平设置。

解决方法：

- 确保`Show error highlights`选项是勾选的。
- 取消省电模式的勾选。
- 设置项目为自动编译。
- 调整Inspections设置至正确的检查级别。
- 如果调整了高亮水平，将其调整回默认设置或适当级别。

按照上述步骤操作后，IDEA应该能够正常显示Java代码的报错提示。