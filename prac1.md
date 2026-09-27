# Решения задач практической работы 1

## Задача 1.
```bash
cut -d: -f1 /etc/passwd | sort
```
## Задача 2.
```bash
cat /etc/protocols | sort -k2 -n -r | head -5 | awk '{print $2, $1}'
```
## Задача 3.
```bash
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 \"текст\"" >&2
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
```
## Задача 4
```bash
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 <файл>" >&2
    exit 1
fi
 
file="$1"
 
if [ ! -f "$file" ]; then
    echo "Файл не найден: $file" >&2
    exit 1
fi
 
grep -oE '\b[A-Za-z_][A-Za-z0-9_]*\b' "$file" | sort -u | tr '\n' ' '
echo
```
## Задача 5
```bash
#!/bin/bash
 
if [ "$#" -eq 0 ]; then
    echo "Использование: $0 <файл>" >&2
    exit 1
fi
 
file="$1"
 
if [ ! -f "$file" ]; then
    echo "Файл не найден: $file" >&2
    exit 1
fi
 
# Дать права на выполнение
chmod +x "$file"
 
sudo cp "$file" /usr/local/bin/
echo "Команда '$file' зарегистрирована"
```
## Задача 6
``` bash                                                                                                               
#!/bin/bash

for file in *.c *.js *.py; do
    [ -f "$file" ] || continue
    first=$(head -n 1 "$file")
    
   case "$file" in 
        *.c|*.js)
            if [[ "$first" =~ ^[[:space:]]*(//|/\*) ]]; then
                echo "$file: there is comment"
            else
                echo "$file: there is no comment"
            fi
            ;;
        *.py)
            if [[ "$first" =~ ^[[:space:]]*# ]]; then
                echo "$file: there is comment"
            else
                echo "$file: there is no comment"
            fi
            ;;
    esac
done
```
## Задача 7
``` bash                                                  
#!/bin/bash

path="${1:-.}"

find "$path" -type f -print0 | xargs -0 md5sum | sort | awk '   
    {
        hash = $1
        file = substr($0, length($1) + 3)
        if (hash in seen) {
            if (!(hash in header_shown)) {
                print "Group of duplicates (md5) "hash "):"                   
                print "  " seen[hash]
                header_shown[hash] = 1
            }
            print "  " file 
        } else {
            seen[hash] = file
        }
    }
'
```
# Задача 8
``` bash
#!/bin/bash

dir="${1:-.}"
ext="$2"

if [ -z "$ext" ]; then
    echo "Использование: $0 <каталог> <расширение>" >&2
    exit 1
fi

archive="${ext}_files.tar"

mapfile -d '' files < <(find "$dir" -type f -name "*.${ext}" -print0)

if [ "${#files[@]}" -eq 0 ]; then
    echo "Файлы с расширением .$ext не найдены"
    exit 0
fi

tar -cf "$archive" "${files[@]}"
echo "Создан архив: $archive (файлов: ${#files[@]})"
``` 
# Задача 9
``` bash
#!/bin/bash

input="$1"
output="$2"

if [ -z "$input" ] || [ -z "$output" ]; then
    echo "Использование: $0 <входной_файл> <выходной_файл>" >&2
    exit 1
fi

if [ ! -f "$input" ]; then
    echo "Файл '$input' не найден" >&2
    exit 1
fi

tab=$(printf '\t')
sed "s/    /${tab}/g" "$input" > "$output"

echo "Готово: $output"
```
# Задача 10
``` bash
#!/bin/bash

dir="$1"

if [ -z "$dir" ]; then
    echo "Использование: $0 <директория>" >&2
    exit 1
fi

if [ ! -d "$dir" ]; then
    echo "Директория '$dir' не найдена" >&2
    exit 1
fi

find "$dir" -maxdepth 1 -type f -name "*.txt" -empty -exec basename {} \;
```
