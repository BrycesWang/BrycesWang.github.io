---
title: Hexo博客编写流程
date: 2023-12-11 21:11:19
categories: Blog
keywords:
- Blog
- Hexo
tags:
- Blog
- Workflow
- Hexo
---

# Workflow

编写一篇博客的workflow，主要有三个步骤

1. 创建博客：`hexo new post <blog_title>`
2. 编写博客：博客的目录在`~/blog_home/source/_posts/blog_title.md`
2. 清楚缓存：`hexo clean`
3. 生成静态页面：`hexo g`
4. 发布：`hexo d`

## 自动化脚本

每次提交时，都需要输入大量命令，因此可以通过脚本来完成操作。由于每次部署时，都会提示`.deploy_git/*`不为空，因此这里每次部署时，先使用`rm -rf .deploy_git`命令删除该文件夹。因此，每次部署时分为两步：

1. `./cmd clean`
2. `./cmd build`

具体的bash脚本如下：

```bash
#!/bin/bash
pushd $(dirname "${0}") > /dev/null
DIR=$(pwd -L)
popd > /dev/null
DATE=$(date +"%Y-%m-%d %H:%M")

# get action
ACTION=$1

# help
usage() {
  echo "Usage: ./cmd {commit|build|clean}"
  exit 1;
}

# start app
commit() {
    git add .
    git commit -m 'Post Auto Commit'
    git push
}

build() {
    hexo d -g
}

# stop app
clean() {
    rm -rf .deploy_git
    rm -rf public
}

case "$ACTION" in
  commit)
    commit
  ;;
  build)
    build
  ;;
  clean)
    clean
  ;;
  *)
    usage
  ;;
esac
```



# 个性化

## 应用

```bash
# hexo clean //执行此命令后继续下一条
hexo g //生成博客目录
hexo s //本地预览
hexo d //部署
```

## 新建文章文章头设置

新建文章时，都会自动生成文章头方便个性化设置，可以通过修改`/scaffolds/page.md`文件，自定义新建文章时的文章头

```md
---
title: {{ title }}
date: {{ date }}
tags: 
categories: 
keywords:
tags:
toc: true
---
```

# 参考

1. [快速搭建个人博客 —— 保姆级教程](https://pdpeng.github.io/2022/01/19/setup-personal-blog/#%E6%9C%AC%E5%9C%B0%E7%8E%AF%E5%A2%83)

2. [超详细Hexo+Github博客搭建小白教程](https://godweiyang.com/2018/04/13/hexo-blog/#toc-heading-10)

3. [Hexo搭建静态博客](https://leader.js.cool/basic/md/hexo/#1-%E6%96%B0%E5%BB%BA%E4%BB%A3%E7%A0%81%E4%BB%93%E5%BA%93)
4. [Hexo 你的专属博客](https://dc1y.github.io/hexo-your-blog/)
