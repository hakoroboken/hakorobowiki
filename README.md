# hakorobowiki  
https://hakoroboken.github.io/hakorobowiki/

## ローカル環境の作成
### 初回起動
```
git clone https://github.com/hakoroboken/hakorobowiki.git
cd hakorobowiki

python3 -m venv .venv
source .venv/bin/activate

pip install \
  mkdocs-material \
  markdown \
  mdx_truly_sane_lists \
  mkdocs \
  mkdocs-awesome-pages-plugin==2.9.1 \
  mkdocs-exclude \
  mkdocs-macros-plugin \
  mkdocs-static-i18n \
  mike \
  plantuml-markdown \
  pymdown-extensions \
  python-markdown-math

mkdocs build
mkdocs serve
```
### ２度目以降
```
cd ~/hakorobowiki
source .venv/bin/activate
mkdocs serve
```