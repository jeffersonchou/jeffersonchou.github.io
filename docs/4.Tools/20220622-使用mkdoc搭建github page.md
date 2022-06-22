# 20220622-使用mkdoc搭建github page

## 本地编译环境

1. 安装 python 依赖包

```
mkdocs==1.3.0
mkdocs-material==8.3.6
mkdocs-material-extensions==1.0.3
mike==1.1.2
```

2. 初始化本地项目 blog

```shell
mkdocs new blog
```

3. 关联github page

* 新建gitHub page项目
创建一个名为为username.github.io的代码仓库，其中username为GitHub的账户名称。
注：如果username与账户名称不匹配，将不会起作用

* 修改README.md
README.md需要修改，指向"https://jeffersonchou.github.io/"域名。

* 准备github page项目
Clone github Page项目，将mkdocs初始化的文件，移入移入项目本地文件夹，并 push 到远程仓库main分支。

* 新建 gh-pages 分支，并设置github page的source页面

```
git checkout -b gb-pages
```

如图，设置source页面：

![设置github page source页面](https://cdn.jsdelivr.net/gh/jeffersonchou/cloudimg@master/blog_img/20220622143930-2022-06-22-14-39-31.png)

* 在项目文件夹下编译生成页面，并将页面提交到 gh-pages 分支

```shell
mkdocs build
```

删除除site之外的文件并将site文件夹的文件更新到root中，将项目文件夹更新到 gh-pages 分支，此时查看github page网页地址即可看到生成的页面；

1. 利用 GitHub Action 完成自动化 mkdocs 编译

* 点击 GitHub myblog 的 Action，新建一个 simple workflow

![image](https://user-images.githubusercontent.com/62104945/162753634-b6c86616-3595-4b62-ae54-bf3ac08f4857.png)

* 修改 yaml 文件

```yaml
# This is a basic workflow to help you get started with Actions

name: public site

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3

      - name: Install mkdocs
        run: |
          python -m pip install mkdocs==1.3.0
          python -m pip install mkdocs-material==8.3.6
          python -m pip install mkdocs-material-extensions==1.0.3
          python -m pip install mike==1.1.2
        
      - name: Create html
        run: mkdocs build
        
      - name: Public
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```
