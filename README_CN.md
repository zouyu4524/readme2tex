# Readme2TeX, 支持LaTeX的GitHub Readme

截至目前, GitHub项目页面下的Readme(Markdown)还不支持对TeX语法的渲染, 如果Readme中有嵌入公式的需求, 一般可以通过在线latex编辑器生成相应公式的图片链接, 例如[codecogs](https://www.codecogs.com/latex/eqneditor.php)。但是这种方法有两个弊端: 1. 生成的图片比较模糊; 2. 页面打开时需要时间加载, 偶尔可能出现图片无法加载的情况。  
而[readme2tex](https://github.com/leegao/readme2tex)这个项目满足了我所有的需求, svg矢量图片, 离线生成(相对路径加载), 并且无论是inline或block形式, 显示的大小都完美贴合文本内容（Edge上inline显示有问题, Chrome OK）。

### 项目地址

[leegao/readem2tex](https://github.com/leegao/readme2tex)

### 依赖

- <img alt="$\text{\LaTeX}$" src="https://github.com/zouyu4524/readme2tex/blob/master/svgs/c068b57af6b6fa949824f73dcb828783.svg" align=middle width="42.05817pt" height="22.407pt"/> 编译器, 如果平时有使用<img alt="$\text{\LaTeX}$" src="https://github.com/zouyu4524/readme2tex/blob/master/svgs/c068b57af6b6fa949824f73dcb828783.svg" align=middle width="42.05817pt" height="22.407pt"/>, 那一般是满足的  
- `dvisvgm`, 搜索该关键词下载解压, 并将其路径添加至环境变量`PATH`中即可

### 安装

```
pip install readme2tex
```

**注**: 作者暂时未将`pip`中的版本更新至GitHub项目中的最新版本, 缺失的主要功能是`--pngtrick`, 这一点稍后介绍。

### 语法

```
python -m readme2tex --nocdn --output README.md INPUT.md
```

其中`INPUT.md`是包含LaTeX的readme文件, 而`README.md`是由`readme2tex`工具生成的可在GitHub上显示LaTeX公式的readme。  
`--nocdn`选项目前来看是必须的选项, 其作用是将生成的公式svg图像保存至本地, 并在`README.md`中以相对路径索引对应的公式图片, 而不是上传至`rawgit`。如此处理的原因是`rawgit`的作者因为[安全原因](https://rawgit.com/)已经关闭了该服务。  
而在`INPUT.md`文件中, 通过`$ $`与`$$ $$`分别表示inline和block格式的公式即可, `readme2tex`将为每一个公式生成相应的svg图片, 并存储于`svg/`文件夹下。  
而上述提到的`--pngtrick`方法是将svg图片进一步转化为png格式, 以便在GitHub项目readme页面可以索引。（此前, 处于安全考虑, GitHub不提供svg格式的相对索引, 但目前已取消该限制。因此, 没有必要再将svg图片转化为png格式。）  
此外, 该工具还支持`usepackage`命令, 其对<img alt="$\text{\LaTeX}$" src="https://github.com/zouyu4524/readme2tex/blob/master/svgs/c068b57af6b6fa949824f73dcb828783.svg" align=middle width="42.05817pt" height="22.407pt"/>的支持相当完善。更多的使用说明可以参看项目的readme文档。

### 中文支持

原作者的项目中并未考虑对中文等非ASCII编码的字符集, 导致在使用时若文档中有中文字符则会报错, 报错信息如下:

```
Traceback (most recent call last):
  File "C:\Users\yuze\Anaconda3\envs\r2l\lib\runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "C:\Users\yuze\Anaconda3\envs\r2l\lib\runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "C:\Users\yuze\Anaconda3\envs\r2l\lib\site-packages\readme2tex\__main__.py", line 160, in <module>
    args.bustcache)
  File "C:\Users\yuze\Anaconda3\envs\r2l\lib\site-packages\readme2tex\render.py", line 129, in render
    content = readme_file.read()
  File "C:\Users\yuze\Anaconda3\envs\r2l\lib\encodings\cp1252.py", line 23, in decode
    return codecs.charmap_decode(input,self.errors,decoding_table)[0]
UnicodeDecodeError: 'charmap' codec can't decode byte 0x81 in position 86: character maps to <undefined>
```

原因在于, `render.py`中调用`open()`方法使用了缺省的`encoding`参数, 那么就是使用本机的`encoding`方式, 但是可能默认的编码方式不支持中文, 导致此处报错。相应地, 我们可以修改对应读写部分的代码, 我将其`encoding`参数设置为了`utf-8`, 如此, 只需在编写readme文件时将其保存为`utf-8`即可与之匹配, 相应改动后的项目地址为: [zouyu4524/readme2tex](https://github.com/zouyu4524/readme2tex)。  