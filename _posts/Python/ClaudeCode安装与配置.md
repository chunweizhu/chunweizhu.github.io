---
categories:
  - Python   #注意一定要大写
published: false
toc: ture   #给文章自动加上目录
toc_label: "目录"
title: ClaudeCode CC的安装指南
---

# ClaudeCode CC的安装指南

我用了魔法，所以就没有更改镜像源。
- claude code  2.0.76 (Claude Code)
- npm 11.6.2
- brew 5.0.7

[下载nodejs](https://nodejs.org/zh-cn/download)
~~~sh
# 下载并安装 Homebrew
curl -o- https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh | bash

# 下载并安装 Node.js：
brew install node@22

# 验证 Node.js 版本：
node -v # Should print "v22.21.1".

# 验证 npm 版本：
npm -v # Should print "10.9.4".
~~~
claude code 安装
~~~sh
brew install --cask claude-code
#或者npm安装
npm install -g @anthropic-ai/claude-code
~~~

当我要配置Minmax模型api时，会跳转到官网，而且不能注册，解决方案如下：

在~目录找到.claude.json，在最后一行添加："hasCompletedOnboarding":true
重启终端即可

小程序
CC Switch
