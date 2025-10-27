
---
title: "Bash 题目答案与讲解（Tema 2 bash 2）"
date: 2025-10-27
categories: [Linux, 学习笔记]
tags: [linux, exercicis, bash]
---
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

# 运动员数据文件
ESPORTISTES="esportistes.txt"

# 函数1: 计算平均年龄
calcula_mitjana_edat() {
    echo "Calculant l'edat mitjana dels esportistes..."
    echo "==========================================="
    
    # 检查文件是否存在
    if [ ! -f "$ESPORTISTES" ]; then
        echo "Error: El fitxer $ESPORTISTES no existeix"
        return 1
    fi
    
    total_edat=0
    num_esportistes=0
    
    # 逐行读取文件
    while read linea; do
        # 用cut命令分割字段
        edat=$(echo "$linea" | cut -d: -f4)
        
        # 检查年龄是否是数字
        if echo "$edat" | grep -q "^[0-9][0-9]*$"; then
            total_edat=$((total_edat + edat))
            num_esportistes=$((num_esportistes + 1))
        fi
    done < "$ESPORTISTES"
    
    # 计算并显示平均年龄
    if [ $num_esportistes -gt 0 ]; then
        mitjana=$((total_edat / num_esportistes))
        echo "Edat mitjana: $mitjana anys"
        echo "Total esportistes: $num_esportistes"
    else
        echo "No s'han trobat esportistes amb edat vàlida"
    fi
    
    echo ""
}

# 函数2: 按俱乐部筛选
llista_per_club() {
    echo "Llistat d'esportistes per club"
    echo "=============================="
    
    # 检查文件是否存在
    if [ ! -f "$ESPORTISTES" ]; then
        echo "Error: El fitxer $ESPORTISTES no existeix"
        return 1
    fi
    
    # 获取用户输入的俱乐部名称
    echo "Introdueix el nom del club:"
    read club
    
    echo ""
    echo "Esportistes del club '$club':"
    echo "----------------------------"
    
    trobats=0
    # 逐行读取文件
    while read linea; do
        # 提取俱乐部字段（第6个字段）
        club_actual=$(echo "$linea" | cut -d: -f6)
        
        # 比较俱乐部名称
        if [ "$club_actual" = "$club" ]; then
            # 提取并显示运动员信息
            nom=$(echo "$linea" | cut -d: -f1)
            cognom1=$(echo "$linea" | cut -d: -f2)
            cognom2=$(echo "$linea" | cut -d: -f3)
            edat=$(echo "$linea" | cut -d: -f4)
            esport=$(echo "$linea" | cut -d: -f5)
            
            echo "$nom $cognom1 $cognom2 - $edat anys - $esport"
            trobats=$((trobats + 1))
        fi
    done < "$ESPORTISTES"
    
    # 显示结果统计
    if [ $trobats -eq 0 ]; then
        echo "No s'han trobat esportistes del club '$club'"
    else
        echo "Total: $trobats esportistes"
    fi
    
    echo ""
}

# 函数3: 按姓氏筛选
llista_per_cognom() {
    echo "Llistat d'esportistes per cognom"
    echo "================================"
    
    # 检查文件是否存在
    if [ ! -f "$ESPORTISTES" ]; then
        echo "Error: El fitxer $ESPORTISTES no existeix"
        return 1
    fi
    
    # 获取用户输入的姓氏
    echo "Introdueix el cognom a buscar:"
    read cognom
    
    echo ""
    echo "Esportistes amb cognom '$cognom':"
    echo "--------------------------------"
    
    trobats=0
    # 逐行读取文件
    while read linea; do
        # 提取两个姓氏字段
        cognom1=$(echo "$linea" | cut -d: -f2)
        cognom2=$(echo "$linea" | cut -d: -f3)
        
        # 检查是否匹配任一姓氏
        if [ "$cognom1" = "$cognom" ] || [ "$cognom2" = "$cognom" ]; then
            # 提取并显示完整信息
            nom=$(echo "$linea" | cut -d: -f1)
            edat=$(echo "$linea" | cut -d: -f4)
            esport=$(echo "$linea" | cut -d: -f5)
            club=$(echo "$linea" | cut -d: -f6)
            
            echo "$nom $cognom1 $cognom2 - $edat anys - $esport - $club"
            trobats=$((trobats + 1))
        fi
    done < "$ESPORTISTES"
    
    # 显示结果统计
    if [ $trobats -eq 0 ]; then
        echo "No s'han trobat esportistes amb el cognom '$cognom'"
    else
        echo "Total: $trobats esportistes"
    fi
    
    echo ""
}

# 函数4: 显示菜单
mostra_menu() {
    echo "================================"
    echo "             MENU"
    echo "================================"
    echo "1- Calcular l'edat mitjana dels esportistes"
    echo "2- Llistat de tots els esportistes d'un Club"
    echo "3- Llistat de tots els esportistes amb el mateix cognom"
    echo "4- Sortir"
    echo "================================"
    echo -n "Tria una opció (1-4): "
}

# 创建示例数据文件
crea_fitxer_exemple() {
    cat > "$ESPORTISTES" << 'EOF'
joan:garcia:lopez:25:futbol:fcb
maria:martinez:sanchez:30:tennis:rct
carlos:rodriguez:gomez:22:basquet:eba
laura:garcia:fernandez:28:natacio:cnb
anna:lopez:martinez:35:tennis:rct
marc:sanchez:garcia:19:futbol:fcb
EOF
}

# 主程序
main() {
    # 检查数据文件是否存在，如果不存在则创建
    if [ ! -f "$ESPORTISTES" ]; then
        echo "Creant fitxer d'exemple esportistes.txt..."
        crea_fitxer_exemple
        echo "Fitxer d'exemple creat!"
        echo ""
    fi
    
    # 主循环 - 显示菜单直到用户选择退出
    while true; do
        mostra_menu
        read opcio
        
        case $opcio in
            1)
                echo ""
                calcula_mitjana_edat
                ;;
            2)
                echo ""
                llista_per_club
                ;;
            3)
                echo ""
                llista_per_cognom
                ;;
            4)
                echo ""
                echo "Gràcies per utilitzar el programa. Adeu!"
                exit 0
                ;;
            *)
                echo ""
                echo "Opció no vàlida. Si us plau, tria una opció de 1 a 4."
                echo ""
                ;;
        esac
        
        # 暂停，让用户有时间阅读结果
        echo -n "Prem Enter per continuar..."
        read
        echo ""
    done
}

# 启动程序
main
```

---

## ▶️ 使用方法
```bash
# 1) 保存为 menu.sh，并给执行权限
chmod +x menu.sh

# 2) 运行脚本（目录中会生成或读取 esportistes.txt）
./menu.sh
```

---

## 🗂️ 示例数据（`esportistes.txt`）
```text
joan:garcia:lopez:25:futbol:fcb
maria:martinez:sanchez:30:tennis:rct
carlos:rodriguez:gomez:22:basquet:eba
laura:garcia:fernandez:28:natacio:cnb
anna:lopez:martinez:35:tennis:rct
marc:sanchez:garcia:19:futbol:fcb
```

---

## 🧠 功能列表
- **计算平均年龄**：读取第 4 字段（年龄），过滤数字并求平均。
- **按俱乐部筛选**：匹配第 6 字段（俱乐部），输出姓名/年龄/运动项目。
- **按姓氏筛选**：匹配第 2 或第 3 字段（两个姓氏）。
- **交互式菜单**：循环显示菜单，直到选择退出。

---

## 📝 逐段解释（摘选）

### 文件与变量
```bash
#!/bin/bash
ESPORTISTES="esportistes.txt"
```
- `#!/bin/bash`：指定 Bash 解释器。  
- `ESPORTISTES`：数据文件名变量，修改更方便。

### 读取与字段提取
```bash
while read linea; do
  edat=$(echo "$linea" | cut -d: -f4)
done < "$ESPORTISTES"
```
- `while read`：逐行读取。  
- `cut -d: -f4`：以冒号分隔，取第 4 字段（年龄）。

### 数字校验与累加
```bash
if echo "$edat" | grep -q "^[0-9][0-9]*$"; then
  total_edat=$((total_edat + edat))
  num_esportistes=$((num_esportistes + 1))
fi
```
- `grep -q`：安静模式匹配。  
- `^[0-9][0-9]*$`：一个或多个数字。

### 俱乐部筛选
```bash
club_actual=$(echo "$linea" | cut -d: -f6)
if [ "$club_actual" = "$club" ]; then
  # ... 提取姓名/年龄/项目并显示
fi
```
- 比较第 6 字段与用户输入。

### 姓氏筛选（两个字段）
```bash
cognom1=$(echo "$linea" | cut -d: -f2)
cognom2=$(echo "$linea" | cut -d: -f3)
if [ "$cognom1" = "$cognom" ] || [ "$cognom2" = "$cognom" ]; then
  # 命中
fi
```

### 菜单与控制流
```bash
while true; do
  mostra_menu
  read opcio
  case $opcio in
    1) calcula_mitjana_edat ;;
    2) llista_per_club ;;
    3) llista_per_cognom ;;
    4) exit 0 ;;
    *) echo "Opció no vàlida" ;;
  esac
  echo -n "Prem Enter per continuar..."; read
done
```

---

## ✅ 设计取舍 / 关键点
- **仅用基础命令**：字段解析都用 `cut`，不依赖 `awk/sed`。  
- **输入校验**：对年龄字段做数字校验，避免脏数据。  
- **文件健壮性**：运行前检查数据文件，不存在则创建示例。  
- **交互友好**：清晰提示、分页等待。

## 使用说明（建议）

```bash
chmod u+x arg1.sh arg2.sh arg3.sh factorial.sh combinatori.sh menu.sh
```
- 给予脚本执行权限。  
- 确保存在数据文件 `esportistes`，且每行格式为 `nom:cognom1:cognom2:edat:esport:club`。  
- 运行示例：`./arg1.sh a b c`、`./arg3.sh 5 fitxer.txt`、`./menu.sh`。
