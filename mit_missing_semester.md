# MIT missing semester

## lecute 2 shell and script

shell 参数：

- \$0 表示脚本名称
- \$1 表示第一个参数
- \$? 从上一条命令中获取错误代码
- \$\_ 表示最后一个参数
- \$# 表示参数的数量
- \$\$ 表示脚本线程 ID
- \$@ 表示所有的参数

多条命令可以使用";"串联

`echo "$(pwd)"` 打印命令执行结果

`tldr` 打印命令示例

`history 1 | grep tail` 查找所有执行过的 tail 命令

`broot`命令交互式查找文件

`nnn`文件夹之间快速跳转
