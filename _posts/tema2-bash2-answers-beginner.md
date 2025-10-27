# Bash 题目答案与讲解（Tema 2 bash 2）— 新手友好版（不使用 IFS= 与 sed）

> 说明：本版尽量使用最基础命令与写法：`for`、`while read`、`head`、`tail`、`cut`、`grep`、`echo`、`read`、`seq` 等；**不使用 `IFS=` 与 `sed`**。解释均放在代码块之外，便于阅读与打印。

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

## 2) `arg2.sh` — 显示 3 个参数并连接输出（用 `for` 拼接）

**题目原文**：  
“Escriure un script anomenat **arg2.sh** al qual li passem **3 arguments** i **els mostra per pantalla de forma concatenada**.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
if [ "$#" -ne 3 ]; then
  echo "Error: calen 3 arguments."
  echo "Ús: $0 a b c"
  exit 1
fi

out=""
for x in "$@"; do
  out="${out}${x}"
done
printf "%s
" "$out"
```

### 🗒️ 逐行说明
- 校验恰好 3 个参数。  
- 用 `for x in "$@"` 逐个拼接到 `out`，最后一次性输出。

---

## 3) `arg3.sh` — 显示任意文件的任意一行（用 `for + seq`）

**题目原文**：  
“Escriure un script anomenat **arg3.sh** que mostri per pantalla **una línia qualsevol d’un fitxer qualsevol**. El **número de línia** i el **nom del fitxer** s’hauran d’introduir **com arguments**。”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# Ús: ./arg3.sh <numero_linia> <fitxer>
if [ "$#" -ne 2 ]; then
  echo "Ús: $0 <numero_linia> <fitxer>"
  exit 1
fi

linia="$1"
fitxer="$2"

if [ ! -f "$fitxer" ]; then
  echo "Error: fitxer inexistent: $fitxer"
  exit 1
fi

if ! echo "$linia" | grep -Eq '^[0-9]+$'; then
  echo "Error: el número de línia ha de ser enter positiu."
  exit 1
fi

line=""
for i in $(seq 1 "$linia"); do
  line=$(head -n "$i" "$fitxer" | tail -n 1)
done

printf "%s
" "$line"
```

### 🗒️ 逐行说明
- 用 `seq 1 N` 结合 `for` 迭代 1..N；每次用 `head|tail` 取第 `i` 行并覆盖到 `line`。  
- 循环结束时 `line` 就是第 `N` 行内容。

---

## 4) `factorial.sh` — 计算阶乘（用 `for`）

**题目原文**：  
“Escriure un script anomenat **factorial.sh** que calculi el **factorial** d’un nombre el valor del qual s’ha introduït **com argument** del shell script.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# Ús: ./factorial.sh <n>
if [ "$#" -ne 1 ]; then
  echo "Ús: $0 <n_enter_no_negatiu>"
  exit 1
fi

n="$1"
if ! echo "$n" | grep -Eq '^[0-9]+$'; then
  echo "Error: n ha de ser un enter no negatiu."
  exit 1
fi

fact=1
for i in $(seq 1 "$n"); do
  fact=$((fact * i))
done

echo "$n! = $fact"
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

factorial() {
  local x="$1"
  local res=1
  for i in $(seq 1 "$x"); do
    res=$((res * i))
  done
  echo "$res"
}

echo -n "Introdueix m: "
read m
echo -n "Introdueix n: "
read n

if ! echo "$m" | grep -Eq '^[0-9]+$' || ! echo "$n" | grep -Eq '^[0-9]+$'; then
  echo "Error: m i n han de ser enters no negatius."
  exit 1
fi
if [ "$m" -le "$n" ]; then
  echo "Error: es requereix m > n."
  exit 1
fi

mf=$(factorial "$m")
nf=$(factorial "$n")
m_nf=$(factorial $((m - n)))

result=$(( mf / (nf * m_nf) ))
echo "C($m, $n) = $result"
```

### 🗒️ 逐行说明
- `factorial()` 用 `for + seq` 计算阶乘；仅用到基础命令。  
- 读取输入并做整数校验与不等关系校验，最后做整数运算得到结果。

---

## 6) `menu.sh` — 函数版菜单（不使用 `IFS=`，用最基础逐行读取）

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

# 选项 1：平均年龄（逐行 read，不用 IFS= 与 sed）
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

# 选项 2：按 Club 列表（逐行 read + cut）
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
- **逐行读取**：`while read line; do ... done < "$FITXER"` 最基础、易懂，不需要设置 `IFS=`。  
- **字段解析**：每行用 `cut -d: -fN` 提取所需列。  
- **整型处理**：平均值做整数除法，符合题目常规要求。

---

## 使用说明（建议）

```bash
chmod u+x arg1.sh arg2.sh arg3.sh factorial.sh combinatori.sh menu.sh
```
- 给予脚本执行权限。  
- 确保存在数据文件 `esportistes`，且每行格式为 `nom:cognom1:cognom2:edat:esport:club`。  
- 运行示例：`./arg1.sh a b c`、`./arg3.sh 5 fitxer.txt`、`./menu.sh`。
