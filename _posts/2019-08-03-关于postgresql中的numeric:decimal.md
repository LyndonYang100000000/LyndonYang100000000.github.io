---
layout:     post
title:      关于postgresql中的numeric/decimal
subtitle:   numeric/decimal
date:       2019-08-03
author:     Lyndon
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Postgres
---

​	postgresql中的该类型精度支持到1000位，采用变长方式存储，那么如何通过atttypmod来获取到定义的precision和scale呢？

​	两种方法：

```
1.观察二进制：
numeric(5,4) => 327688   0101 0000 0000 0000 1000
numeric(5,5) => 327689   0101 0000 0000 0000 1001
numeric(2,2) => 393222   0110 0000 0000 0000 0110
numeric(7,2) => 458758   0111 0000 0000 0000 0110
numeric(8,2) => 524294   1000 0000 0000 0000 0110
numeric(9,2) => 589830   1001 0000 0000 0000 0110
第一个字节为 numeric (n,m) 的N， 最后一个字节为 m+4，即precision为第一个字节，scale为最后一个字节-4
 
2.计算公式：
atttypmod=-1表示null
 
precision: ((atttypmod - 4) >> 16) & 65535
scale: (atttypmod - 4) & 65535
 
 
 
SELECT
  CASE atttypid
         WHEN 21 /*int2*/ THEN 16
         WHEN 23 /*int4*/ THEN 32
         WHEN 20 /*int8*/ THEN 64
         WHEN 1700 /*numeric*/ THEN
              CASE WHEN atttypmod = -1
                   THEN null
                   ELSE ((atttypmod - 4) >> 16) & 65535     -- calculate the precision
                   END
         WHEN 700 /*float4*/ THEN 24 /*FLT_MANT_DIG*/
         WHEN 701 /*float8*/ THEN 53 /*DBL_MANT_DIG*/
         ELSE null
  END   AS numeric_precision,
  CASE
    WHEN atttypid IN (21, 23, 20) THEN 0
    WHEN atttypid IN (1700) THEN           
        CASE
            WHEN atttypmod = -1 THEN null      
            ELSE (atttypmod - 4) & 65535            -- calculate the scale 
        END
       ELSE null
  END AS numeric_scale,
  *
FROM
    pg_attribute ;
```

