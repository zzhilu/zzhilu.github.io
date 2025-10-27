
# Bash 题目答案与讲解（Tema 2 范围）

> 仅使用 **Tema 2** 讲义中涉及到的 Bash 知识点（变量、参数 `$1`/`$#`/`$@`、条件 `if`/`case`、循环 `while`/`for`/`until`、算术扩展 `$(( ))`、`read`、函数 `function`/`local` 等）完成《Prob-bash-2-2526.pdf》里的所有题目。题目原文摘自 PDF。菜单题使用**函数版**实现；与“esportistes”相关的数据格式为 `nom:cognom1:cognom2:edat:esport:club`。

---

## 1) `arg1.sh` — 仅允许 3 个参数

**题目原文**：
“Escriure un script anomenat **arg1.sh** al qual només puguem passar-li **tres arguments** qualsevols i en cas de passar-li un nombre diferent d’arguments mostri un **missatge d’error** per pantalla.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# arg1.sh — 只接受恰好 3 个参数
if [ "$#" -ne 3 ]; then
  echo "Error: s'han de passar exactament 3 arguments."  # 错误信息
  echo "Ús: $0 arg1 arg2 arg3"
  exit 1
fi

# 到这里说明参数个数正确
echo "Correcte. Has passat 3 arguments: $@"
```

### 💬 解释
- `"$#"` 是参数个数（Tema 2 讲义“Scripts (arguments)”演示了 `$#`、`$@` 等特殊变量）。
- 用 `if [ "$#" -ne 3 ]` 判断是否恰好等于 3，不是则提示并退出。

---

## 2) `arg2.sh` — 显示 3 个参数并**连接输出**

**题目原文**：
“Escriure un script anomenat **arg2.sh** al qual li passem **3 arguments** i **els mostra per pantalla de forma concatenada**.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# arg2.sh — 接 3 个参数并连接打印
if [ "$#" -ne 3 ]; then
  echo "Error: calen 3 arguments."
  echo "Ús: $0 a b c"
  exit 1
fi

# 直接无分隔连接输出（按题意）
echo -n "$1$2$3"
echo
```

### 💬 解释
- 仍用 `$#` 校验参数个数。
- 连接：直接按顺序输出 `$1$2$3`，不加分隔。

---

## 3) `arg3.sh` — 显示任意文件的任意一行

**题目原文**：
“Escriure un script anomenat **arg3.sh** que mostri per pantalla **una línia qualsevol d’un fitxer qualsevol**. El **número de línia** i el **nom del fitxer** s’hauran d’introduir **com arguments**.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# Ús: ./arg3.sh <linia> <fitxer>
if [ "$#" -ne 2 ]; then
  echo "Ús: $0 <numero_linia> <fitxer>"
  exit 1
fi

linia="$1"
fitxer="$2"

# 基本校验
if [ ! -f "$fitxer" ]; then
  echo "Error: fitxer inexistent: $fitxer"
  exit 1
fi
# 行号为正整数
if ! echo "$linia" | grep -Eq '^[0-9]+$'; then
  echo "Error: el número de línia ha de ser enter positiu."
  exit 1
fi

# 仅用已学命令：head + tail 组合取第 N 行
# 若 N 超出文件行数，head 会返回文件全部行，tail 仍尝试输出最后 1 行（即整文件最后一行）
resultat=$(head -n "$linia" "$fitxer" | tail -n 1)
echo "$resultat"
```

### 💬 解释
- 参数 1：行号；参数 2：文件名。
- 使用 `head -n N | tail -n 1` 获取第 N 行，避免引入未讲到的 `sed/awk`。

---

## 4) `factorial.sh` — 计算阶乘（参数传入）

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

# 检查是非负整数
if ! echo "$n" | grep -Eq '^[0-9]+$'; then
  echo "Error: n ha de ser un enter no negatiu."
  exit 1
fi

fact=1
i=1
while [ "$i" -le "$n" ]; do
  fact=$((fact * i))
  i=$((i + 1))
done

echo "$n! = $fact"
```

### 💬 解释
- 用 `while` 循环与算术扩展 `$(( ))`（Tema 2）逐步累乘。

---

## 5) `combinatori.sh` — 计算组合数 C(m, n)

**题目原文**：
“Escriure un script anomenat **combinatori.sh** que generi el nombre combinatori \( \binom{m}{n} = \frac{m!}{n!(m-n)!} \)，on els valors **n i m es sol·licitin per teclat**（且 `m > n`）。”

### ✅ 解法（脚本）
```bash
#!/bin/bash
# combinatori.sh — 读入 m 与 n（通过键盘），计算 C(m,n)

# 封装一个阶乘函数（使用已学函数/局部变量）
factorial() {
  local x="$1"
  local res=1
  local i=1
  while [ "$i" -le "$x" ]; do
    res=$((res * i))
    i=$((i + 1))
  done
  echo "$res"
}

echo -n "Introdueix m: "
read m
echo -n "Introdueix n: "
read n

# 基本校验
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

# 纯整数运算
result=$(( mf / (nf * m_nf) ))

echo "C($m, $n) = $result"
```

### 💬 解释
- 使用 **函数** 与 `local` 局部变量（Tema 2 “Scripts ++” 讲义有函数与局部变量案例）。
- 只用整数算术 `$(( ))` 计算组合数。

---

## 6) `menu.sh` — 函数版菜单（仅 4 退出；完成后返回菜单）

**题目原文**（摘要）：
“Implementar un **script anomenat menu.sh** que mostri el menú amb 4 opcions：  
1. **Calcular l’edat mitjana** dels esportistes  
2. **Llistat de tots els esportistes d’un Club**  
3. **Llistat de tots els esportistes amb el mateix cognom**  
4. **Sortir**  
（仅 **4** 退出；其他操作完成后**返回菜单**。此题**要求使用函数**组织解法。）

> 数据文件：`esportistes`，每行结构  
> `nom:cognom1:cognom2:edat:esport:club`（与 Tema 2 前一讲练习一致）。

### ✅ 解法（脚本）
```bash
#!/bin/bash
# menu.sh — 使用函数的菜单程序
# 依赖数据文件：esportistes
# 每行：nom:cognom1:cognom2:edat:esport:club

FITXER="esportistes"

# 选项 1：平均年龄
mitjana() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi

  suma=0
  compta=0
  # 仅用已学命令：cut 提取第 4 列（edat）
  while read edat; do
    if echo "$edat" | grep -Eq '^[0-9]+$'; then
      suma=$((suma + edat))
      compta=$((compta + 1))
    fi
  done < <(cut -d: -f4 "$FITXER")

  if [ "$compta" -eq 0 ]; then
    echo "No hi ha dades d'edat vàlides."
  else
    # 整数平均（向下取整）。如需小数，可用 bc；此处保持整数避免额外工具。
    echo "Edat mitjana: $((suma / compta))"
  fi
}

# 选项 2：按 Club 列表
llistar_per_club() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi

  echo -n "Introdueix el nom del club: "
  read club

  # 遍历整行，取第 6 列与目标比对
  while IFS= read -r linia; do
    c=$(echo "$linia" | cut -d: -f6)
    if [ "$c" = "$club" ]; then
      echo "$linia"
    fi
  done < "$FITXER"
}

# 选项 3：按姓氏（cognom1）列表
llistar_per_cognom() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi

  echo -n "Introdueix el cognom: "
  read cognom

  while IFS= read -r linia; do
    c1=$(echo "$linia" | cut -d: -f2)
    if [ "$c1" = "$cognom" ]; then
      echo "$linia"
    fi
  done < "$FITXER"
}

# 主菜单循环（仅 4 退出）
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

  echo    # 操作后空行，再次显示菜单
done
```

### 💬 解释
- **函数化**：`mitjana`、`llistar_per_club`、`llistar_per_cognom` 分别对应题目 1、2、3 的子任务；主程序用 `case` 分派。
- **返回菜单**：只有选 4 才 `break` 退出；其余执行后继续循环。
- **字段解析**：用 `cut -d: -fN` 获取指定列（仅使用了课堂常见命令与 Tema 2 的脚本结构）。
- **平均值**：为保持工具最简，只做**整数**平均（如需小数，可把 `echo "scale=2; $suma/$compta" | bc` 换掉该行）。

---

## 使用说明（建议）

- 给脚本加执行权限：
  ```bash
  chmod u+x arg1.sh arg2.sh arg3.sh factorial.sh combinatori.sh menu.sh
  ```
- 检查 `esportistes` 文件是否存在，且结构为 `nom:cognom1:cognom2:edat:esport:club`。

---

（完）
