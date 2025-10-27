# Bash Exercicis 1 — Respostes i Explicacions

---

## 1) ex1.sh — Buscar línies amb “un” i “dos”

**题目原文**：  
Escriure un script que mostri les línies d’un fitxer qualsevol que continguin les paraules **un** i **dos** sense importar si estan en majúscules o minúscules.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el nom del fitxer:"
read fitxer
grep -i "un" "$fitxer" | grep -i "dos"
```

### 💬 解释
读取文件名，用 `grep -i`（忽略大小写）先查找含“un”的行，再通过管道筛选出也含“dos”的行。  
**结果**：只显示同时包含“un”和“dos”的行。

---

## 2) ex2.sh — Determinar si un nombre és positiu

**题目原文**：  
Escriure un script al qual li passem un nombre enter per teclat i ens diu si és **positiu**.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix un nombre enter:"
read num
if [ $num -gt 0 ]; then
  echo "És positiu"
fi
```

### 💬 解释
通过 `read` 读取一个整数。  
`[ $num -gt 0 ]` 判断输入是否大于 0。  
如果为真，输出“És positiu”。  
**小结**：只处理正数的情况。

---

## 3) ex3.sh — Repetir lectura de paraules

**题目原文**：  
Escriure un script que llegeixi una paraula per teclat i la mostri per pantalla. Aquesta operació s’haurà de realitzar **10 vegades**.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
for i in $(seq 10); do
  echo "Introdueix una paraula:"
  read paraula
  echo $paraula
done
```

### 💬 解释
`for i in $(seq 10)` 重复 10 次：每次提示输入一个单词并显示。练习循环与 `read`。

---

## 4) ex4.sh — Guardar paraules ordenades en un fitxer

**题目原文**：  
Escriure un script que llegeixi paraules per teclat i les guardi en un fitxer de forma ordenada fins que s’escrigui “q”.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
sortida="paraules.txt"
> "$sortida"
while true; do
  read -p "Introdueix una paraula (q per sortir): " paraula
  if [ "$paraula" = "q" ]; then
    break
  fi
  echo "$paraula" >> "$sortida"
done
sort "$sortida" -o "$sortida"
```

### 💬 解释
清空输出文件后用无限循环不断读入单词；输入 q 时跳出。结束后对文件排序。

---

## 5) ex5.sh — Comprovar si és un fitxer o directori

**题目原文**：  
Escriure un script al qual li introduïm per teclat el nom absolut d’un fitxer i haurà d’indicar si correspon a un directori o a un fitxer ordinari.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el nom absolut del fitxer:"
read nom
if [ -d "$nom" ]; then
  echo "És un directori"
elif [ -f "$nom" ]; then
  echo "És un fitxer ordinari"
else
  echo "No existeix"
fi
```

### 💬 解释
使用 `-d` 判断是否为目录，`-f` 判断是否为普通文件，否则输出不存在。

---

## 6) ex6.sh — Modificar el primer camp d’una línia

**题目原文**：  
Escriure un script que modifiqui el primer camp d’una línia qualsevol d’un fitxer qualsevol per una cadena introduïda per teclat.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el nom del fitxer:"
read fitxer
echo "Introdueix el número de línia:"
read linia
echo "Introdueix la nova cadena pel primer camp:"
read nova_cadena
awk -F: -v linia="$linia" -v nova="$nova_cadena" 'NR==linia {$1=nova} {print}' OFS=":" "$fitxer" > tmp && mv tmp "$fitxer"
```

### 💬 解释
用 `awk` 按冒号分隔字段：当行号等于输入的目标行时，将第 1 列改为新内容，再输出到临时文件覆盖原文件。

---

## 7) ex7.sh — Missatge segons número

**题目原文**：  
Escriure un script que sol·liciti a l’usuari un número de l’1 al 3 i, segons el número introduït tregui per pantalla missatges diferents.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix un número de l'1 al 3:"
read num
case $num in
  1) echo "Ets el primer en arribar" ;;
  2) echo "Subcampió de la temporada" ;;
  3) echo "A la tercera va la venguda" ;;
  *) echo "Número no vàlid" ;;
esac
```

### 💬 解释
用 `case` 匹配输入的 1–3，输出对应信息，否则输出“无效号码”。

---

## 8) ex8.sh — Mostrar el primer camp de cada línia

**题目原文**：  
Escriure un script que per totes les línies del fitxer file1 mostri per pantalla el missatge: El camp 1 de la línia XX és YYY.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
fitxer="file1"
linia_num=1
while read linia; do
  camp1=$(echo "$linia" | cut -d: -f1)
  echo "El camp 1 de la línia $linia_num és $camp1"
  linia_num=$((linia_num + 1))
done < "$fitxer"
```

### 💬 解释
逐行读取文件，用 `cut -d:` 取第一列并输出带行号的结果。

---

## 9) taules.sh — Generar taules de multiplicar

**题目原文**：  
Escriure un script que generi les taules de multiplicar de l’1 al 10 i les guardi en fitxers.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
for i in $(seq 10); do
  fitxer="taula-del-$i"
  > "$fitxer"
  for j in $(seq 10); do
    echo "$i x $j = $((i * j))" >> "$fitxer"
  done
done
```

### 💬 解释
嵌套两层循环：外层控制乘数文件名，内层生成乘法表。

---

## 10) calculadora.sh — Operacions bàsiques

**题目原文**：  
Escriure un script que realitzi sumar, restar, multiplicar i dividir amb dos valors introduïts per teclat.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el primer nombre:"
read a
echo "Introdueix el segon nombre:"
read b
echo "Introdueix l'operació (+, -, x, /):"
read op
case $op in
  "+") echo "$a + $b = $((a + b))" ;;
  "-") echo "$a - $b = $((a - b))" ;;
  "x") echo "$a x $b = $((a * b))" ;;
  "/") echo "$a / $b = $((a / b))" ;;
  *) echo "Operació no vàlida" ;;
esac
```

### 💬 解释
读取两个数和操作符，用 `case` 匹配四种算术操作，利用 `$(( ))` 执行整数计算。

---

## 11) nom.sh — Cercar esportistes pel nom

**题目原文**：  
Implementar un script que mostri tots els esportistes amb un nom introduït per teclat.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix un nom:"
read nom
grep "^$nom:" esportistes
```

### 💬 解释
读取姓名，用 `grep "^$nom:"` 查找文件 `esportistes` 中以该名字开头的行。

---

## 12) FiltrarCognom.sh — Dividir per inicial del cognom

**题目原文**：  
Implementar un script que generi dos fitxers esportistes-AM i esportistes-NZ segons la inicial del primer cognom.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
> esportistes-AM
> esportistes-NZ
while read linia; do
  cognom=$(echo "$linia" | cut -d: -f2)
  primera_lletra=$(echo "$cognom" | cut -c1)
  if [[ "$primera_lletra" =~ [A-Ma-m] ]]; then
    echo "$linia" >> esportistes-AM
  else
    echo "$linia" >> esportistes-NZ
  fi
done < esportistes
```

### 💬 解释
逐行读取 `esportistes`，提取第二字段（姓氏），判断首字母范围分配到不同文件。

---

## 13) FiltrarClub.sh — Filtrar per club

**题目原文**：  
Implementar un script que generi un fitxer amb els esportistes d’un club sol·licitat per teclat.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el nom del club:"
read club
> Club
grep ":$club$" esportistes >> Club
```

### 💬 解释
从键盘读取俱乐部名称，用 `grep` 匹配以该 club 结尾的行并输出到文件 `Club`。

---

## 14) menu.sh — Menú principal

**题目原文**：  
Implementar un script anomenat menu.sh que mostri un menú amb 4 opcions.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
while true; do
  echo "MENU"
  echo "1- Calcular l’edat mitjana dels esportistes"
  echo "2- Llistat de tots els esportistes d’un Club"
  echo "3- Llistat de tots els esportistes amb el mateix cognom"
  echo "4- Sortir"
  read opcio
  case $opcio in
    1) ./mitjana.sh ;;
    2) ./FiltrarClub.sh ;;
    3) ./FiltrarCognom.sh ;;
    4) break ;;
    *) echo "Opció no vàlida" ;;
  esac
done
```

### 💬 解释
通过无限循环显示菜单；使用 `case` 分派选项，1–3 调用其他脚本，4 退出。

---

## 15) ex15.sh — Comprovar si és capicua

**题目原文**：  
Escriure un script que demani a l’usuari un número de tres xifres i digui si és capicua o no.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix un número de tres xifres:"
read num
if [ $(echo $num | rev) = $num ]; then
  echo "És capicua"
else
  echo "No és capicua"
fi
```

### 💬 解释
利用 `rev` 反转字符串；若与原输入相同则为回文数。

---

## 16) ex16.sh — Menú amb select

**题目原文**：  
Escriure un script que permeti escollir una opció (Acudit, Refrany, Poema, Sortir) utilitzant `select`.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
PS3="Tria una opció: "
select opcio in Acudit Refrany Poema Sortir; do
  case $opcio in
    Acudit) echo "Saben aquel que dice" ;;
    Refrany) echo "Com més serem més riurem" ;;
    Poema) echo "Si parlo dels teus ull" ;;
    Sortir) break ;;
    *) echo "Opció no vàlida" ;;
  esac
done
```

### 💬 解释
使用 `select` 自动生成编号菜单，用户选择后通过 `case` 执行对应输出。

---

## 17) ex17.sh — Comprovar tres paraules iguals

**题目原文**：  
Escriure un script que comprovi si les tres primeres paraules d’un fitxer que conté només una línia són iguals o diferents.

### ✅ 解法（你的脚本）
```bash
#!/bin/bash
echo "Introdueix el nom del fitxer:"
read fitxer
paraules=($(head -1 "$fitxer"))
if [ "${paraules[0]}" = "${paraules[1]}" ] && [ "${paraules[1]}" = "${paraules[2]}" ]; then
  echo "Les tres primeres paraules són iguals"
else
  echo "Les tres primeres paraules són diferents"
fi
```

### 💬 解释
从文件第一行读取单词数组，比较前三个元素是否相等。

---

## 🔚 总结

这些练习涵盖了 Bash 编程的核心：  
- 输入输出：`read`, `echo`  
- 条件判断：`if`, `case`  
- 循环结构：`for`, `while`, `select`  
- 文本与文件处理：`grep`, `cut`, `awk`, `rev`, `sort`  
- 逻辑组织：函数调用与脚本组合  

通过这 17 题，你几乎掌握了 Bash 脚本入门所需的所有关键语法。
