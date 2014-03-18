# 为editplus添加php代码格式化工具phpcb

EditPlus是一款非常不借的代码编辑工具，从学PHP开始，就一直使用这个工具做“开发”。原因是因为它的小巧，快速，绿色，强大，有很多很 多的优点，让我不得不去使用它。昨天晚上升级了一下EditPlus到最新版，发现在图标和界面上都改变了不少，很是喜欢。EditPlus还有一个很好 的功能就是可以集成插件，那么今天就给大家介绍一款非常不错的，可以集成在EditPlus或其它编辑器中的PHP代码格式化工具：phpCodeBeautifier，简称phpCB，目前最新版本是2007年1月发布的1.0.1版，虽然有很多年没更新了，不过还是可以继续使用的。

我们安装好EditPlusr后，在菜单栏中，有【工具(T)】 -> 【配置用户工具】的菜单，打开过后，我们点击【添加工具组】 -> 【应用程序】

菜单文本写：PHP代码美化；命令：选择 phpCB 的本机保存地址。

参数写： $(FilePath) 或下面的内容，具体请参考 [phpCodeBeautifier User Manual][link01]

	--space-after-if --optimize-eol --space-after-switch --space-after-while --space-before-start-angle-bracket --space-after-end-angle-bracket --extra-padding-for-case-statement --glue-amperscore --change-shell-comment-to-double-slashes-comment --indent-with-tab --force-large-php-code-tag --force-true-false-null-contant-lowercase --comment-rendering-style PEAR --equal-align-position 50 --padding-char-count 1 "$(FilePath)"

初始目录填写：$(FileDir)

最后注意选择：“运行文本过滤”，下拉框中选择“替换”即可，这样，我们的PHP代码美化工具就添加完毕了，在需要使用的时候，只需要在菜单栏里选择PHP代码美化工具，那么程序将会自动整理我们的杂乱代码，让我们的代码更美观，便于阅读和分析。

phpcb的下载地址是: [下载][downlink]


[link01]:http://www.waterproof.fr/products/phpCodeBeautifier/manual.php
[downlink]:http://www.waterproof.fr/products/phpCodeBeautifier/download.php