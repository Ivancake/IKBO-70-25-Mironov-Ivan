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
## Задача 4
```minizinc
include "globals.mzn";

array[1..6] of var 0..9: d;
constraint d[1] + d[2] + d[3] = d[4] + d[5] + d[6];
constraint all_different(d);
solve minimize d[1] + d[2] + d[3];
output ["Билет: \(d[1])\(d[2])\(d[3])\(d[4])\(d[5])\(d[6])\n",
        "Сумма трёх цифр: \(d[1] + d[2] + d[3])\n"];
```
## Задача 5
```minizinc
var {0, 100, 110, 120, 130, 140, 150}: menu;
var {0, 180, 200, 210, 220, 230}: dropdown;
var {0, 100, 200}: icons;

% root
constraint menu != 0;
constraint icons = 100;

% menu 1.1.0 - 1.5.0
constraint menu >= 110 -> dropdown >= 200;

% menu 1.0.0
constraint menu = 100 -> dropdown = 180;

% dropdown 2.0.0 - 2.3.0
constraint dropdown >= 200 -> icons = 200;

solve maximize menu + dropdown + icons;

output ["menu: \(menu div 100).\(menu div 10 mod 10).0\n",
        "dropdown: \(dropdown div 100).\(dropdown div 10 mod 10).0\n",
        "icons: \(icons div 100).\(icons div 10 mod 10).0"];
```
## Задача 6
```minizinc
var {0, 100, 110}: foo;
var {0, 100}: left;
var {0, 100}: right;
var {0, 100, 200}: shared;
var {0, 100, 200}: target;

% root 1.0.0
constraint foo >= 100 /\ foo < 200;
constraint target = 200;

% foo 1.1.0
constraint foo = 110 -> (left = 100 /\ right = 100);

% left 1.0.0
constraint left = 100 -> shared >= 100;

% right 1.0.0
constraint right = 100 -> (shared != 0 /\ shared < 200);

% shared 1.0.0
constraint shared = 100 -> (target >= 100 /\ target < 200);

constraint left != 0 -> foo = 110;
constraint right != 0 -> foo = 110;
constraint shared != 0 -> (left != 0 \/ right != 0);

solve maximize foo + left + right + shared + target;

function string: ver(int: v) =
    if v = 0 then "не установлен"
    else show(v div 100) ++ "." ++ show(v div 10 mod 10) ++ ".0" endif;

output ["foo: " ++ ver(fix(foo)) ++ "\n",
        "left: " ++ ver(fix(left)) ++ "\n",
        "right: " ++ ver(fix(right)) ++ "\n",
        "shared: " ++ ver(fix(shared)) ++ "\n",
        "target: " ++ ver(fix(target))];
```
