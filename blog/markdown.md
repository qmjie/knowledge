*MarkDown*支持两种语法：**Setext** 和 **atx** 形式

*Setext 形式*是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题），**Atx 形式**在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶。 <!--more-->

#### 修辞和强调

上面两段的代码为：

    *MarkDown*支持两种语法：__Setext__ 和 **atx** 形式

    *Setext 形式*是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题），**Atx 形式**在行首插入 1 到 6 个 # ，对应  到标题 1 到 6 阶。


#### 列表

星号、加号、减号表示无序列表

    * Candy.
    * Gum.
    * Booze.


*   Candy.
*   Gum.
*   Booze.

数字接着一个英文句点表示有序列表

    1. Red
    2. Green
    3. Blue


1.  Red
2.  Green
3.  Blue

#### 代码

    console.log("hello world!");


四个空格缩进代表*代码区*

#### 链接

*行内* 和 *参考* 两种形式，两种都是使用*角括号*来把文字转成连结

    这是秋意浓的 [博客](http://qmjie.sinaapp.com/).


这是秋意浓的 [博客][1].

参考形式的链接让你可以为链接定一个名称，之后你可以在文件的其他地方定义该链接的内容：

    I get 10 times more traffic from [Google][1] than from[Yahoo][2] or [MSN][3].
    [1]: http://google.com/ "Google"
    [2]: http://search.yahoo.com/ "Yahoo Search"
    [3]: http://search.msn.com/ "MSN Search"


I get 10 times more traffic from [Google][2] than from[Yahoo][3] or [MSN][4].

#### 图片

与链接相似

    行内形式（title 是选择性的）：
    ![alt text](http://tp3.sinaimg.cn/2459242834/180/39997373334/1 "Title")


![alt text][5]

    ![alt text][1]
    [1]: http://ww3.sinaimg.cn/thumbnail/92951152jw1efi9mduxhzj20np0hs0t6.jpg "Title"


![alt text][2]

 [1]: http://qmjie.sinaapp.com/
 [2]: http://ww3.sinaimg.cn/thumbnail/92951152jw1efi9mduxhzj20np0hs0t6.jpg "Title"
 [3]: http://search.yahoo.com/ "Yahoo Search"
 [4]: http://search.msn.com/ "MSN Search"
 [5]: http://tp3.sinaimg.cn/2459242834/180/39997373334/1 "Title"
