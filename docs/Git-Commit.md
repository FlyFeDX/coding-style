# git 提交规范

按照如下整体风格: type(Scope): message

## type

用于说明 commit的类别，只允许使用下面几个标识。

* feat：新功能（feature）
* fix：修补bug（后续考虑加入hotfix：紧急修复bug）
* test：增加测试
* docs：文档
* style： 格式（不影响代码运行的变动）
* refactor：重构（即不是新增功能，也不是修改bug的代码变动）
* chore：构建过程或辅助工具的变动
* revert:   执行 git revert 打印的message

## Scope（可选）

用于说明 代码影响范围

## message

清晰描述提交内容

举个例子，新增了用户登录功能，那么提交信息应该这样写：
git commit -m "feat(user): 新增用户功能"
