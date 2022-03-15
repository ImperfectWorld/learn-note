# 介绍
Babel 是现代 JavaScript 语法转换器,核心是AST。

# babel的工作流程

在日常前端项目中，绝大多数时候我们使用babel进行js代码的转化。它的工作流程大概可以概括称为以下三个方面:
- Parse（解析）阶段：这个阶段将我们的js代码(字符串)进行词法分析生成一系列tokens，之后再进行语法分析将tokens组合称为一颗AST抽象语法树。(比如babel-parser它的作用就是这一步)
- Transform（转化）阶段：这个阶段babel通过对于这棵树的遍历，从而对于旧的AST进行增删改查，将新的js语法节点转化称为浏览器兼容的语法节点。(babel/traverse就是在这一步进行遍历这棵树)
- Generator（生成）阶段：这个阶段babel会将新的AST转化同样进行深度遍历从而生成新的代码。(@babel/generator)
