# Решения задач

## Задача 1.
cut -d: -f1 /etc/passwd | sort

## Задача 2.
cat /etc/protocols | sort -k2 -n -r | head -5 | awk '{print $2, $1}'

## Задача 3.
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 \"текст\""
    exit 1
fi
 
text="$1"
len=${#text}
 
line="+"
for ((i = 0; i < len + 2; i++)); do
    line+="-"
done
line+="+"
 
echo "$line"
echo "| $text |"
echo "$line"

## Задача 4
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 <файл>"
    exit 1
fi
 
file="$1"
 
if [ ! -f "$file" ]; then
    echo "Файл не найден: $file"
    exit 1
fi
 
grep -oE '\b[A-Za-z_][A-Za-z0-9_]*\b' "$file" | sort -u | tr '\n' ' '
echo

## Задача 5
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 <файл>"
    exit 1
fi
 
file="$1"
 
if [ ! -f "$file" ]; then
    echo "Файл не найден: $file"
    exit 1
fi
 
### Дать права на выполнение
chmod +x "$file"
 
sudo cp "$file" /usr/local/bin/
echo "Команда '$file' зарегистрирована"

## Задача 6
