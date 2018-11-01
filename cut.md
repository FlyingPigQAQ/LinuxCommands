## cut
### 参数详解
-d 分割符号(单个字符)
-f 分隔后，list取值为索引，从1开始
### Examples
echo "Hello,world" | cut -d ',' -f1
**注意制表符分割方式**
echo "Hello world" | cut -d $'\t' -f1
