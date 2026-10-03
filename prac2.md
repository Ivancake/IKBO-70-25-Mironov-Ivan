# Решения задач практической работы 2
## Задача 1
```bash
apk update
apk add py3-pip py3-matplotlib

# Вывод служебной информации о пакете
pip show matplotlib

ls /usr/lib/python3.8/site-packages | grep -i matplotlib

cat /usr/lib/python3.8/site-packages/matplotlib-3.2.1-py3.8.egg-info/PKG-INFO | head -40

# Получение пакета без менеджера пакетов: скачивание напрямую с PyPI
wget https://files.pythonhosted.org/packages/source/m/matplotlib/matplotlib-3.2.1.tar.gz

tar -xzf matplotlib-3.2.1.tar.gz
head -20 matplotlib-3.2.1/PKG-INFO
```
## Задача 2
```bash
apk add nodejs npm
npm view express

# Получение пакета без менеджера пакетов: скачивание архива напрямую из репозитория npm
wget https://registry.npmjs.org/express/-/express-4.18.2.tgz

tar -xzf express-4.18.2.tgz

cat package/package.json
```
## Задача 3
```bash
apk add graphviz

cat > matplotlib.dot << 'EOF'
digraph matplotlib {
    rankdir=LR;
    node [shape=box, style=filled, fillcolor=lightblue];
    matplotlib [fillcolor=orange];
    matplotlib -> cycler;
    matplotlib -> kiwisolver;
    matplotlib -> numpy;
    matplotlib -> pyparsing;
    matplotlib -> "python-dateutil";
}
EOF

cat > express.dot << 'EOF'
digraph express {
    rankdir=LR;
    node [shape=box, style=filled, fillcolor=lightgreen];
    express [fillcolor=orange];
    express -> accepts;
    express -> "array-flatten";
    express -> "body-parser";
    express -> "content-disposition";
    express -> "content-type";
    express -> cookie;
    express -> "cookie-signature";
    express -> debug;
    express -> depd;
    express -> encodeurl;
    express -> "escape-html";
    express -> etag;
    express -> finalhandler;
    express -> fresh;
    express -> "http-errors";
    express -> "merge-descriptors";
    express -> methods;
    express -> "on-finished";
    express -> parseurl;
    express -> "path-to-regexp";
    express -> "proxy-addr";
    express -> qs;
    express -> "range-parser";
    express -> "safe-buffer";
    express -> send;
    express -> "serve-static";
    express -> setprototypeof;
    express -> statuses;
    express -> "type-is";
    express -> "utils-merge";
    express -> vary;
}
EOF

dot -Tpng matplotlib.dot -o matplotlib.png
dot -Tpng express.dot -o express.png
```
