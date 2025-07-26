```bash
# 临时仓库
pip install -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple some-package
# 永久仓库
pip config set global.index-url https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
# 配置多个仓库
pip config set global.extra-index-url "<url1> <url2>..."
# 查看可安装包的版本
pip install package==
# 查看已安装的库
pip list
# 检查包的依赖项
pip check package
# 更新pip版本
python -m pip install --upgrade pip
# 导出依赖库到文件
pip freeze --all > requirements.txt
# 根据文件内容下载库到指定目录
pip download -d libs -r requirements.txt
# 根据依赖文件内容并指定文件目录,批量安装包
pip install --no-index --find-links=d:\libs -r requirements.txt
# 根据依赖文件内容在线安装
pip install -r requirements.txt
```
