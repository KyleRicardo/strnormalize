# strnormalize
A lightweight C library aimed to convert string between GBK and UTF-8.

一个轻量级的C语言库，旨在将字符串在GBK与UTF-8编码之间转换。

## 声明

该库非本人开发，感谢Cnangel大佬的辛勤工作。本人只是搬运到GitHub上方便使用。

## 说明

关于GBK和UTF-8之间的转换，很多初学者会很迷茫。

一般来说GBK和UTF-8是文字的编码方式，其对应的内码是不一样的，所以GBK和UTF-8的转换需要对内码进行一一映射，然后进行转换。

对于一般系统上的工程，一般使用libiconv即可，但是对于**嵌入式**或手机操作系统，libiconv显得就有点庞大了。

在这里提供GBK和UTF8转换以及全半角、大小写转换等函数，希望对手机开发的同学有所帮助，特别是在iOS上开发的同学。

## 用法

具体全半角、简繁体转换使用方法见下代码：

```c
 #include "strnormalize.h"
 #include <stdio.h>
 #include <stdlib.h>
 #include <string.h>
 
 int main(int argc, char **argv)
 {
     str_normalize_init();
     unsigned options = SNO_TO_LOWER | SNO_TO_HALF;
     if (argc > 1) options = atoi(argv[1]);
 
     char *buffer = (char *)malloc(65536);
     memset(buffer, 0, 65536);
     while (fgets(buffer, 65536, stdin))
     {   
         str_normalize_utf8(buffer, options);
         printf("%s", buffer);
     }   
     free(buffer);
 
     return 0;
 }
```

UTF-8和GBK转换使用方法如下：

```c
#include "strnormalize.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdint.h>

int main(int argc, char **argv)
{
    str_normalize_init();
    const char *utf8 = "我是utf-8字符！";
    const char *gbk = "����GBK�ַ���";
    uint32_t utf8_len = strlen(utf8);
    uint32_t gbk_len = strlen(utf8);
    uint32_t utf8buffer_len = utf8_len * 3 + 1;
    uint32_t gbkbuffer_len = gbk_len * 2 + 1;
    char *utf8buffer = (char *)malloc(utf8buffer_len);
    char *gbkbuffer = (char *)malloc(gbkbuffer_len);
    memset(utf8buffer, 0, utf8buffer_len);
    memset(gbkbuffer, 0, gbkbuffer_len);
    utf8_to_gbk(utf8, utf8_len, &gbkbuffer, &gbkbuffer_len);
    gbk_to_utf8(gbk, gbk_len, &utf8buffer, &utf8buffer_len);
    printf("utf8: %s<=>%d \t gbkbuffer: %s<=>%d\n", utf8, utf8_len, gbkbuffer, gbkbuffer_len);
    printf("gbk: %s<=>%d \t utf8buffer: %s<=>%d\n", gbk, gbk_len, utf8buffer, utf8buffer_len);
    free(utf8buffer);
    free(gbkbuffer);
    return 0;
}
```

