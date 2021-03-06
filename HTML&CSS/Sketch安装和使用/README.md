## Sketch

### 获取 Sketch 安装包

寻找设计相关的专门网站，或者向团队中的设计UI 索要 **Sketch** 安装包。（鼓励支持正版）

### Sketch安装包显示文件损坏的解决方法

一般 **Sketch** 更新后，通过各种途径获得资源包后，解压安装，结果打开一看：“Sketch”已损坏，打不开。应该将它移到废纸篓。

其实，是 **Mac** 有了些限制。这是 `OS 10.12` 以后的系统加强了安全机制，默认不允许用户自行下载安装应用程序，只能从**App Store** 里安装应用。

打开 **安全与隐私**<br/>
在 `通用` 的 第二大块 *允许以下位置下载的应用*<br/>
`OS 10.12`之前这里是有第三项“**任何来源**”的，`OS 10.12`之后就默认消失了，这里需要将此勾选项展现。

打开终端，在终端中输入
> sudo spctl --master-disable
根据提示，填写密码

输入密码回车后，再去查看 **安全与隐私**，第三项已显示，**Sketch** 就可以正常打开了。

如果完成操作后不想显示第三项“任何来源”，可以用相同的步骤，在终端中输入
> sudo spctl --master-enable就可以了。



