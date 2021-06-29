## bash脚本

- 文档更新时间：2020-08-17

### 每日日志压缩

这段脚本用于每天压缩前一天的日志文件。

```bash
#!/bin/bash

#获取前一天的yyyy-mm-dd格式的日期
dir_date=$(date -d yesterday +%Y-%m-%d)
#进入日志目录对应日期的目录
cd /tmp/logs/$dir_date/
#查找所有以.log为后缀名的文件，并压缩
find ./ -name '*.log' | xargs -i gzip {}

exit;
```

