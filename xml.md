# XML基础知识

## CData

在标记CDATA下，所有的标记、实体引用都被忽略，而被XML处理程序一视同仁地当做字符数据看待，CDATA的形式如下：
```
<![CDATA[文本内容]]>
```

术语 `CDATA` 指的是*不应由 XML 解析器进行解析的文本数据*（Unparsed Character Data）。
在 XML 元素中，`"<"` 和 `"&"` 是非法的。
`"<"` 会产生错误，因为解析器会把该字符解释为新元素的开始。
`"&"` 也会产生错误，因为解析器会把该字符解释为字符实体的开始。
某些文本，比如 JavaScript 代码，包含大量 "<" 或 "&" 字符。为了避免错误，可以将脚本代码定义为 `CDATA`。
CDATA 部分中的所有内容都会被解析器忽略。

转义字符：

1      | 2  | 3         |
------ |----| --------- |
\&lt;  | <  | 小于号
\&gt;  | >  | 大于号
\&amp; | &  | 与（和）号
\&apos;| '  | 单引号
\&quot;| "  | 双引号


## XPath语法

C#利用XPath表达式读写xml节点内容代码示例：

```C#
XmlDocument doc = new XmlDocument();
string title = File.ReadAllText(pfbFiles[i].FullName, Encoding.Default);
//写入题目信息
string strReadingPath = Path.Combine(strPaperPath, "Answer\\Reading.xml");
if (File.Exists(strReadingPath))
{
    doc.Load(strReadingPath);
    var contentNodes = doc.SelectNodes("/item/question/text");
    foreach (XmlNode contentNode in contentNodes)
    {
        contentNode.InnerXml = new XCData(title.Trim()).ToString();
    }
    doc.Save(strReadingPath);
}
```

