# 听力测试App

Data: 2023/3/22\
Description: 听力测试App开发天坑\
Category: Flutter\
Tag: app

## Little Endian 理解
通过维基百科的定义不难发现Little Endian指的是least significant digits
跑到前面去. 我的VS Code上的插件`Hex Editor`可以查看wav文件.

![wav文件2进制信息](https://raw.githubusercontent.com/AllenAnZifeng/blog_content/master/resources/blog12/wav.png)

根据[RIFF header](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format)
的说明,我高亮出来的4个byte就是an unsigned, little-endian 32-bit integer
这个数字表示了data的大小

![计算器](https://raw.githubusercontent.com/AllenAnZifeng/blog_content/master/resources/blog12/cal.png)
- 注意放入计算器的顺序,每一个byte要看成一个整体
- 在vscode的hex插件中,也可以看到uint32是576036

我们看一下windows中wav文件的属性中的大小
![property.png](https://raw.githubusercontent.com/AllenAnZifeng/blog_content/master/resources/blog12/property.png)
数字非常接近,之所以有一些出入是因为uint32只表示了data的大小,windows中的文件还会有一些metadata

## 踩过的坑
- Matlab上生成的Pure Tone音频随着频率的增加,振幅会逐渐减小,
发现是信号发生形变,随后发现是采样频率不够高导致的
- 使用goldwave对音频做不同声道的处理


