# pip install

## Pypi source
豆瓣源 https://pypi.douban.com/simple/
清华源 https://pypi.tuna.tsinghua.edu.cn/simple/

使用方法

临时使用
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

设为默认
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
## 缓存
在docker中使用`pip install`, 添加`--no-cache-dir`参数, 禁止缓存
