1. 程序出现"1 字节的 UTF-8 序列的字节 1 无效"异常怎么办(idea版)

- 原因：xml文件字符编码与idea编辑器字符编码不一致所导致

​				idea默认xml等文件字符编码集为gbk

- 解决方法：修改idea配置文件的编码格式
  - 点击File->Settings->Editor->File Encodings，Project Encoding中选择编码格式为utf-8

