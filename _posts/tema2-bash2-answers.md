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
- `#!/bin/bash` 指定脚本解释器为 Bash。  
- `"$#"` 表示传入参数的个数；`-ne 3` 表示“不等于 3”。  
- 若参数数目不为 3，使用 `echo` 打印错误信息与用法，并 `exit 1` 非零退出。  
- 条件通过后，`echo "… $@"` 输出三个参数；`$@` 表示“所有位置参数”。

---

## 2) `arg2.sh` — 显示 3 个参数并连接输出

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
echo -n "$1$2$3"
echo
```

### 🗒️ 逐行说明
- `"$#"` 校验恰好 3 个参数，否则提示并以非零状态退出。  
- `echo -n "$1$2$3"` 直接将第 1、2、3 个参数无分隔连接输出；`-n` 取消末尾换行。  
- 末尾单独 `echo` 输出一个换行，确保终端整洁。

---

## 3) `arg3.sh` — 显示任意文件的任意一行

**题目原文**：  
“Escriure un script anomenat **arg3.sh** que mostri per pantalla **una línia qualsevol d’un fitxer qualsevol**. El **número de línia** i el **nom del fitxer** s’hauran d’introduir **com arguments**。”

### ✅ 解法（脚本）
```bash
#!/bin/bash
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

resultat=$(head -n "$linia" "$fitxer" | tail -n 1)
echo "$resultat"
```

### 🗒️ 逐行说明
- 校验参数：需要 **2 个**（行号、文件名）。  
- `-f` 判断文件是否存在；不存在即报错退出。  
- 用 `grep -Eq '^[0-9]+$'` 检查行号为正整数。  
- 利用 `head -n N | tail -n 1` 取第 **N** 行：`head` 取前 N 行，再由 `tail` 提取最后 1 行。  
- 将结果存入 `resultat` 并输出。

---

## 4) `factorial.sh` — 计算阶乘（参数传入）

**题目原文**：  
“Escriure un script anomenat **factorial.sh** que calculi el **factorial** d’un nombre el valor del qual s’ha introduït **com argument** del shell script.”

### ✅ 解法（脚本）
```bash
#!/bin/bash
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
i=1
while [ "$i" -le "$n" ]; do
  fact=$((fact * i))
  i=$((i + 1))
done

echo "$n! = $fact"
```

### 🗒️ 逐行说明
- 参数应为 1 个，表示要计算阶乘的非负整数 `n`。  
- 通过正则 `^[0-9]+$` 验证 `n` 是非负整数。  
- 初始化 `fact=1`，用 `while` 循环配合算术扩展 `$(( ))` 连乘。  
- 循环结束后输出形如 `n! = 结果`。

---

## 5) `combinatori.sh` — 计算组合数 C(m, n)

**题目原文**：  
“Escriure un script anomenat **combinatori.sh** que generi el nombre combinatori \( \binom{m}{n} = \frac{m!}{n!(m-n)!} \)，on els valors **n i m es sol·licitin per teclat**（且 `m > n`）。”

### ✅ 解法（脚本）
```bash
#!/bin/bash

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
- 使用 `factorial()` 函数计算阶乘，`local` 定义局部变量，避免污染全局。  
- 通过 `read` 从键盘读取 `m` 与 `n`。  
- 正则校验 `m`、`n` 为非负整数，并检查 `m > n`。  
- 先分别计算 `m!`、`n!`、`(m-n)!`，再做整数除法得到组合数。  
- 输出格式 `C(m, n) = 结果`。

---

## 6) `menu.sh` — 函数版菜单（仅 4 退出；完成后返回菜单）

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

mitjana() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi
  suma=0
  compta=0
  while read edat; do
    if echo "$edat" | grep -Eq '^[0-9]+$'; then
      suma=$((suma + edat))
      compta=$((compta + 1))
    fi
  done < <(cut -d: -f4 "$FITXER")
  if [ "$compta" -eq 0 ]; then
    echo "No hi ha dades d'edat vàlides."
  else
    echo "Edat mitjana: $((suma / compta))"
  fi
}

llistar_per_club() {
  if [ ! -f "$FITXER" ]; then
    echo "Error: no s'ha trobat el fitxer '$FITXER'."
    return
  fi
  echo -n "Introdueix el nom del club: "
  read club
  while IFS= read -r linia; do
    c=$(echo "$linia" | cut -d: -f6)
    if [ "$c" = "$club" ]; then
      echo "$linia"
    fi
  done < "$FITXER"
}

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
- 常量 `FITXER="esportistes"` 指定数据文件。  
- `mitjana()`：用 `cut -d: -f4` 取第 4 列年龄，循环求和/计数，做**整数**平均。  
- `llistar_per_club()`：读取目标 club，逐行取第 6 列匹配并输出整行。  
- `llistar_per_cognom()`：读取姓氏，逐行取第 2 列匹配并输出整行。  
- 主循环 `while true`：显示菜单并读取选项；`case` 分发；只有选 **4** 才 `break`。

---

## 使用说明（建议）

```bash
chmod u+x arg1.sh arg2.sh arg3.sh factorial.sh combinatori.sh menu.sh
```
- 上述命令给予脚本“用户可执行”权限。  
- 确保存在数据文件 `esportistes`，且每行格式为 `nom:cognom1:cognom2:edat:esport:club`。  
- 运行示例：`./arg1.sh a b c`、`./arg3.sh 5 fitxer.txt`、`./menu.sh`。
