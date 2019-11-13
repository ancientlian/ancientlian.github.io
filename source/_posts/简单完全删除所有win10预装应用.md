---
title: 卸载win10预装
date: 2018-10-13 21:36:53
tags: win10
categories: 电脑技巧
---


一行命令删除所有win10预装应用，解决win10烦恼！！

方法和步骤：
1、搜索栏中输入“PowerShell”，在搜索结果中右键选择“以管理员身份运行”PowerShell。
2、在PowerShell窗口中，输入“`Get-AppXPackage | Remove-AppxPackage`”回车确认，然后等待系统自动将当前账户中的所有预装应用都删除即可。
3、如果要卸载某个应用，只需输入对应命令，按下回车键即可删除。