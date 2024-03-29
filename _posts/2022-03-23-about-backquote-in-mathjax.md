---
title: 反单引号「`」被 MathJax 渲染的问题
tags: 记录
---

今天在更新博客时用到了「\`」这个符号，结果上传后发现莫名其妙变成了MathJax 的形状。搜到半夜看见 [gist - Mathjax strangely render back-tick in code blocks - Stack Overflow](https://stackoverflow.com/questions/62111699/mathjax-strangely-render-back-tick-in-code-blocks) 这篇文章才知道 Jekyll 引用的 MathJax 脚本还支持一种 AsciiMath 的分隔符读取方式，而 AsciiMath 的默认分隔符就是反单引号。设置一种叫 `asciimath2jax` 的属性后，终于变正常了。

<!--more-->

```css
<script type="text/x-mathjax-config">
	var _config = { 
		tex2jax: {
			inlineMath: [['$', '$']],
			displayMath: [['$$', '$$']]
		}, 
		asciimath2jax: {
			delimiters: [['$', '$']]
		}
	};
	{%- if _mathjax_autoNumber == true -%}
		_config.TeX = { equationNumbers: { autoNumber: "all" } };
	{%- endif -%}
	MathJax.Hub.Config(_config);
</script>
```



