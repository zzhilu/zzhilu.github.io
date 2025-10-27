# Bash 题目答案与讲解（Tema 2 bash 2）


---

## 1) `arg1.sh` — 仅允许 3 个参数

**题目原文**：  
“Escriure un script anomenat **arg1.sh** al qual només puguem passar-li **tres arguments** qualsevols i en cas de passar-li un nombre diferent d’arguments mostri un **missatge d’error** per pantalla.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
if [ "$#" -ne 3 ]; then
  echo "Error: s'han de passar exactament 3 arguments."
  echo "Ús: $0 arg1 arg2 arg3"
  exit 1
fi
echo "Correcte. Has passat 3 arguments: $@"
```

### 🗒️ 逐行说明（解释不放在代码块里）
- `"$#"` 表示参数个数；不等于 3 就报错退出。  
- 通过后输出三个参数（`$@` 表示所有参数）。

---

## 2) `arg2.sh` — 显示 3 个参数并连接输出

**题目原文**：  
“Escriure un script anomenat **arg2.sh** al qual li passem **3 arguments** i **els mostra per pantalla de forma concatenada**.”

### ✅ 解法（脚本）
```bash
#!/bin/bash

# 检查是否有3个参数
if [ $# -ne 3 ]; then
    echo "Error: Has de passar exactament 3 arguments"
    echo "Ús: ./arg2.sh arg1 arg2 arg3"
    exit 1
fi

# 获取三个参数
arg1=$1
arg2=$2
arg3=$3

# 连接并显示
resultat="$arg1$arg2$arg3"
echo "$resultat"
```

### 🗒️ 逐行说明
- 校验恰好 3 个参数。  
-$1：第一个参数

-$2：第二个参数

-$3：第三个参数

-$#：参数总个数。

---

## 3) `arg3.sh` — 显示任意文件的任意一行

**题目原文**：  
“Escriure un script anomenat **arg3.sh** que mostri per pantalla **una línia qualsevol d’un fitxer qualsevol**. El **número de línia** i el **nom del fitxer** s’hauran d’introduir **com arguments**。”

### ✅ 解法（脚本）
```bash
#!/bin/bash

# 检查参数数量
if [ $# -ne 2 ]; then
    echo "Error: Has de passar exactament 2 arguments"
    echo "Ús: ./arg3.sh <nom_fitxer> <numero_linia>"
    exit 1
fi

fitxer=$1
linia=$2

# 检查文件是否存在
if [ ! -f "$fitxer" ]; then
    echo "Error: El fitxer '$fitxer' no existeix"
    exit 1
fi

# 检查行号是否为正整数
if ! [[ "$linia" =~ ^[0-9]+$ ]] || [ "$linia" -lt 1 ]; then
    echo "Error: El número de línia ha de ser un nombre enter positiu"
    exit 1
fi

# 获取文件总行数
total_linies=$(wc -l < "$fitxer")

# 检查行号是否超出范围
if [ "$linia" -gt "$total_linies" ]; then
    echo "Error: El fitxer només té $total_linies línies. No pots demanar la línia $linia."
    exit 1
fi

# 显示指定行
head -n "$linia" "$fitxer" | tail -n 1
```

### 🗒️ 逐行说明

- exit 1：以错误状态退出（1表示错误）。
- ne 2：不等于2（not equal to 2）。  
- `[ ! -f "$fitxer" ]`：如果文件不存在
-  `=~ ^[0-9]+$`：检查是否只包含数字，防止用户输入字母、负数或零
-  `head -n "$linia" "$fitxer"`：显示文件的前 $linia 行
- 循环结束时 `line` 就是第 `N` 行内容。

---

## 4) `factorial.sh` — 计算阶乘（用 `for`）

**题目原文**：  
“Escriure un script anomenat **factorial.sh** que calculi el **factorial** d’un nombre el valor del qual s’ha introduït **com argument** del shell script.”

### ✅ 解法（脚本）
```bash
#!/bin/bash

# 检查参数数量
if [ $# -ne 1 ]; then
    echo "Error: Has de passar exactament 1 argument"
    echo "Ús: ./factorial.sh <nombre>"
    exit 1
fi

# 获取参数
n=$1

# 检查是否为正整数
if ! [[ "$n" =~ ^[0-9]+$ ]] || [ "$n" -lt 0 ]; then
    echo "Error: El nombre ha de ser un enter positiu"
    exit 1
fi

# 计算阶乘
factorial=1

if [ $n -eq 0 ]; then
    # 0! = 1
    factorial=1
else
    for ((i=1; i<=n; i++)); do
        factorial=$((factorial * i))
    done
fi

echo "$n! = $factorial"
```

### 🗒️ 逐行说明
- `for i in $(seq 1 "$n")` 迭代 1..n，每步累乘。  
- 只用到基础算术扩展 `$(( ))`。

---

## 5) `combinatori.sh` — 计算组合数 C(m, n)（用 `for`）

**题目原文**：  
“Escriure un script anomenat **combinatori.sh** que generi el nombre combinatori \( \binom{m}{n} = \frac{m!}{n!(m-n)!} \)，on els valors **n i m es sol·licitin per teclat**（且 `m > n`）。”

### ✅ 解法（脚本）
```bash
#!/bin/bash

# 计算阶乘的函数
factorial() {
    local num=$1
    local result=1
    
    for ((i=1; i<=num; i++)); do
        result=$((result * i))
    done
    
    echo $result
}

# 输入 m 和 n
echo "Introdueix el valor de m:"
read m
echo "Introdueix el valor de n:"
read n

# 检查 m > n
if [ $m -le $n ]; then
    echo "Error: m ha de ser major que n"
    exit 1
fi

# 计算组合数
fact_m=$(factorial $m)
fact_n=$(factorial $n)
fact_mn=$(factorial $((m - n)))

combinatori=$((fact_m / (fact_n * fact_mn)))

echo "El nombre combinatori C($n,$m) = $combinatori"
```



### 🗒️ 逐行说明
- `factorial()` 用 `for ` 计算阶乘；仅用到基础命令。  
- 读取输入并做整数校验与不等关系校验，最后做整数运算得到结果。

---

## 6) `menu.sh` — 函数版菜单）

**题目原文（摘要）**：  
“Implementar un **script anomenat menu.sh** que mostri el menú amb 4 opcions：  
1. **Calcular l’edat mitjana** dels esportistes  
2. **Llistat de tots els esportistes d’un Club**  
3. **Llistat de tots els esportistes amb el mateix cognom**  
4. **Sortir**  
（仅 **4** 退出；其他操作完成后**返回菜单**。此题**要求使用函数**组织解法。）

> 数据文件：`esportistes`，每行结构 `nom:cognom1:cognom2:edat:esport:club`。

### ✅ 解法（脚本）
```bash
#!/bin/bash
FITXER="esportistes"

# 选项 1：平均年龄
mitjana() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi
  suma=0
  compta=0
  while read line; do
    edat=$(echo "$line" | cut -d: -f4)
    if echo "$edat" | grep -Eq '^[0-9]+$'; then
      suma=$((suma + edat))
      compta=$((compta + 1))
    fi
  done < "$FITXER"
  if [ "$compta" -eq 0 ]; then
    echo "No hi ha dades d'edat vàlides."
  else
    echo "Edat mitjana: $((suma / compta))"
  fi
}

# 选项 2：按 Club 列表（ read + cut）
llistar_per_club() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi
  echo -n "Introdueix el nom del club: "
  read club

  while read line; do
    c=$(echo "$line" | cut -d: -f6)
    if [ "$c" = "$club" ]; then
      echo "$line"
    fi
  done < "$FITXER"
}

# 选项 3：按姓氏列表（逐行 read + cut）
llistar_per_cognom() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi
  echo -n "Introdueix el cognom: "
  read cognom

  while read line; do
    c1=$(echo "$line" | cut -d: -f2)
    if [ "$c1" = "$cognom" ]; then
      echo "$line"
    fi
  done < "$FITXER"
}

# 主菜单循环
while true; do
  echo "MENU"
  echo "1- Calcular l'edat mitjana dels esportistes"
  echo "2- Llistat de tots els esportistes d'un Club"
  echo "3- Llistat de tots els esportistes amb el mateix cognom"
  echo "4- Sortir"
  echo -n "Opció? "
  read op
  case "$op" in
    1) mitjana ;;
    2) llistar_per_club ;;
    3) llistar_per_cognom ;;
    4) echo "Fins aviat!"; break ;;
    *) echo "Opció no vàlida." ;;
  esac
  echo
done
```

### 🗒️ 逐行说明
- **逐行读取**：`while read line; do ... done < "$FITXER"` 。  
- **字段解析**：每行用 `cut -d: -fN` 提取所需列。  
- **整型处理**：平均值做整数除法。

---

## 使用说明（建议）

```bash
chmod u+x arg1.sh arg2.sh arg3.sh factorial.sh combinatori.sh menu.sh
```
- 给予脚本执行权限。  
- 确保存在数据文件 `esportistes`，且每行格式为 `nom:cognom1:cognom2:edat:esport:club`。  
- 运行示例：`./arg1.sh a b c`、`./arg3.sh 5 fitxer.txt`、`./menu.sh`。
