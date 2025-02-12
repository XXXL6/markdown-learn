



# 帮助命令

可以通过以下命令查看更多选项帮助

```shell
pandoc --help
# 或
man pandoc
```



可以通过指令查看支持的格式

```shell
pandoc --list-input-formats
pandoc --list-output-formats
```



# 处理文件格式转换

基本使用方法的命令： 

```shell
# 读取文件
pandoc -f 输入格式 输入文件名 -t 输出格式 -o 输出文件名 输入文件
#输入文件名前无标识符，输出文件名前需加标识符-o
```





例如，将一个 TXT 文件转换为 HTML 文件：

```shell
pandoc -f markdown input.txt -t html -o output.html
```

上面这行命令中，`-f markdown` [表示](https://sspai.com/link?target=https%3A%2F%2Fpandoc.org%2FMANUAL.html%23option--from) 输入文件的格式为 Markdown，也可以写作 `--from=markdown`、`-r markdown` 或 `--read=markdown`。`-t html` [表示](https://sspai.com/link?target=https%3A%2F%2Fpandoc.org%2FMANUAL.html%23option--to) 输出文件格式为 HTML，也可以写作 `--to=html`、`-w html` 或 `--write=html`。输入文件为 `input.txt`，`-o output.html` 也可以写作 `--output=output.html`，[表示](https://sspai.com/link?target=https%3A%2F%2Fpandoc.org%2FMANUAL.html%23option--output) 将输出写入到一个 HTML 文件中，命名为 `output.html`。



需要注意的是，在命令行中明确指出输入或输出的文件格式不是必须的，因为 Pandoc 可以根据文件扩展名 [推测出](https://sspai.com/link?target=https%3A%2F%2Fpandoc.org%2FMANUAL.html%23specifying-formats) 文件格式，例如，它会将 `.txt`、`.md`、`.markdown` 等扩展名视为 Markdown，将 `.html` 视为 HTML。而如果输入文件没有扩展名，则会被当作 Markdown，如果输出文件没有扩展名，则会被当作 HTML。因此，上面这行命令可以简写为：

```shell
pandoc input.txt -o output.html
```

---



```shell
# -s标记
-s ,--standalone
```



生成包含适当页眉和页脚的输出（例如独立的 HTML、LaTeX、TEI 或 RTF 文件，而不是片段）。此选项对于 pdf、epub、epub3、fb2、docx 和 odt 输出会自动设置。对于原生输出，此选项会导致包含元数据；否则，将抑制元数据。

# 处理标准输入输出

与大部分命令行工具一样，Pandoc 的输入和输出也可以是`stdin `（标准输入）或`stdout`（标准输出），而不只是文件。

如果没有指定输入文件，Pandoc 会从 `stdin` 读入，如果没有指定输出文件，则输出为 `stdout`，也就是直接显示在终端中，例如执行下面这行简单的命令：

```shell
echo 'hello world' | pandoc
```

通过 `管道操作` | 将 `echo` 命令的输出结果传递给 `pandoc`。由于这里没有指定输入和输出文件，Pandoc 默认会将输入当作 Markdown，将输出当作 HTML



意识到 Pandoc 不只能处理文件，我们就可以让 Pandoc 不仅限用于「转换文档格式」，还可以实现一些「处理文本」的需求。除此之外，Pandoc 还可以读取网页内容，并将其转换为其他格式：

```shell
pandoc -f html https://pandoc.org -t commonmark-raw_html -o pandoc.md
```

这行命令将 Pandoc 官网主页从 HTML 转换为 Markdown，并关闭 `raw_tml` [扩展](https://sspai.com/link?target=https%3A%2F%2Fpandoc.org%2FMANUAL.html%23extension-raw_html)（`-extenson` 表示关闭扩展），避免转换后的 Markdown 中出现很多 HTML 语法。需要指出的是，[CommonMark](https://sspai.com/link?target=https%3A%2F%2Fcommonmark.org%2F) 是一套针对 [标准 Markdown 语法](https://sspai.com/link?target=https%3A%2F%2Fdaringfireball.net%2Fprojects%2Fmarkdown%2Fsyntax) 进行严格定义并与之高度兼容的规范，也是由 John MacFarlane 教授主导开发的。





# 文本处理操作



##  1. 处理文本与媒体文件

Pandoc支持在文档中嵌入和处理媒体文件，如图片、音频和视频。用户可以通过简单的Markdown语法嵌入这些媒体文件，Pandoc在转换过程中会自动处理这些文件。



**示例：将包含图片的Markdown文档转换为HTML**
假设你有一个包含图片的Markdown文档 input.md，你可以使用以下命令将其转换为HTML格式，并确保图片在转换后的文档中正确显示：

```shell
pandoc input.md -o output.html --extract-media=media
```



在这个命令中，`--extract-media=media` 选项告诉Pandoc将所有媒体文件提取到名为 `media` 的目录中，并在输出文档中引用这些文件。

---



## 2. 转换参考文献

Pandoc支持通过BibTeX文件进行参考文献的转换。用户可以在Markdown文档中使用 `@citekey` 的方式引用参考文献，Pandoc会自动解析这些引用，并在生成的文档中插入相应的参考文献列表。



示例：将包含引用的Markdown文档转换为Word文档
假设你有一个包含引用的Markdown文档 input.md，并且引文数据存储在一个BibTeX文件 references.bib 中，你可以使用以下命令将其转换为Word文档：



```shell
pandoc --citeproc --bibliography=test.bib -M reference-section-title="参考文献" --csl=chinese-gb7714-2005-numeric.csl test.md -o result.docx
```



这里解释一下这些参数：

- --citeproc：处理文献引用，这样才能识别文中的`[@]`的cite key，也可以使用`-C`代替；
- --bibliography：`bib`文件的路径，这里因为已经`cd`到了`test.bib`文件所在的目录下，所以直接使用了文件名；
- -o：output，导出命令；
- -M reference-section-title="参考文献"：设置参考文献表的标题为「参考文献」，不编号

---



## 3. 交叉引用（Pandoc-crossref的使用）

### 3.1. 图片引用

```
{#fig:id}`和`[@fig:id]
```

- `{#fig:id}`：它其实就是相当于告诉`pandoc-crsooref`此时这个地方被我标记为需要引用的图片啦，但是引用归引用，每一张图片肯定不能重复呀，所以就额外让你设置一个唯一的id来保证引用的准确性，这里我的建议是使用这张图片的名称或者简写，不建议使用1-1、12之类的数字，避免后期对图文的顺序修改时出现混乱；
- `[@fig:id]`：这里和上述一样，只不过换成了告诉`pandoc-crsooref`该引用被标记为`id`的图片啦。 这里不知道大家注意到没有，标记图片时使用的是`{}`花括号加上`#`，`#`一般就是标记的意思，你看markdown的一级标题、二级标题等都是使用了`#`，而使用引用的时候则是`[]`方括号加上`@`，`@`一般就有引用的意思，这里就容易记忆一下。

### 3.2. 表格引用

表格的引用和图片几乎一样，只是把`{#fig:id}`中的`fig`改为了`tbl`，其它都是一样的。

### 3.3. 图表按章节引用

默认情况下pandoc在过滤器的帮助下也只能形成`fig.1 ... fig.12`这种，可是长篇论文或者书中都是使用的`fig.2-3`这种按照章节引用的。这里我们需要在命令中添加参数`-M chapters`：

```shell
pandoc --filter pandoc-crossref -M chapters input.md -o output.docx
```

### 3.4.公式引用

同理，只是改成 `{#eq：id}` 



**完整命令样式如下：**

```shell
pandoc --filter pandoc-crossref --citeproc --bibliography=myref.bib -M reference-section-title="参考文献" --csl=chinese-gb7714-2005-numeric.csl -s demo-figref.md -o demo-figref.docx
```

转为word时使用特定模版

```shell
--reference-doc templates.docx
```

