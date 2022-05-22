```sh
mkdir -p themes/hugo-geekdoc
curl -L https://github.com/thegeeklab/hugo-geekdoc/releases/latest/download/hugo-geekdoc.tar.gz | tar -xz -C themes/hugo-geekdoc/ --strip-components=1

hugo server -D
hugo -D
```
