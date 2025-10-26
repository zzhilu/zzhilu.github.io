---
title: "UAB Exercicis Linux-comandes：题解（基础命令版）"
date: 2025-10-27
layout: page
permalink: /notes/linux-exercicis/
---

# 🧩 Exercicis Linux-comandes（UAB）题解汇总
> 每题仅用最基础命令（不使用 awk / sed）

---

## 📸 图片 1：目录结构（dir1–dir3–dir4）
![目录结构图 1]({{ '/assets/img/linux-structure-1.png' | relative_url }})

---

## 第 1 题
**题目原文：**
«Indica el camí absolut del directori actual.»  
（显示当前目录的绝对路径。）

**解法：**
```bash
pwd
```

**详细解释：**
`pwd` 是 “print working directory” 的缩写，会输出当前目录的完整路径。  
“绝对路径”指从根目录 `/` 开始的路径，如 `/home/usuari/dir2`。

---

## 第 2 题
**题目原文：**
«Des de dir2, torna al directori casa (sense usar el camí absolut).»  
（从 dir2 返回到家目录，不使用绝对路径。）

**解法：**
```bash
cd ~
```

**详细解释：**
`~` 表示当前用户的家目录。  
`cd ~` 或 `cd`（不加参数）都能返回 home。

---

## 第 3 题
**题目原文：**
«Des de dir2, crea dins de dir3 un nou directori anomenat dir4 fent servir un camí relatiu.»  
（从 dir2 出发，用相对路径在 dir3 中创建 dir4。）

**解法：**
```bash
mkdir ../cmd/dir3/dir4
```

**详细解释：**
`mkdir` 创建目录。`..` 表示上一级目录。  
`../cmd/dir3` 即“从 dir2 回上一级 userlinux，再进入 cmd/dir3”。

---

## 第 4 题
**题目原文：**
«Situa’t a dir1 utilitzant el camí absolut.»  
（用绝对路径进入 dir1。）

**解法：**
```bash
cd /home/userlinux/cmd/dir1
```

**详细解释：**
绝对路径以 `/` 开头，不受当前目录影响。

---

## 第 5 题
**题目原文：**
«Des de dir1, torna a dir2 utilitzant un camí relatiu.»  
（从 dir1 返回 dir2，用相对路径。）

**解法：**
```bash
cd ../dir2
```

**详细解释：**
`..` 是上一级目录，`../dir2` 就是“回上一级再进 dir2”。

---

## 第 6 题
**题目原文：**
«Des de dir2, ves a dir1 utilitzant un camí relatiu.»  
（从 dir2 出发，用相对路径进入 dir1。）

**解法：**
```bash
cd ../cmd/dir1
```

**详细解释：**
相对路径通过 `..` 回到上一级，再进入目标。

---

## 第 7 题
**题目原文：**
«Des de dir1, mostra en format llarg el contingut del directori arrel (root).»  
（从 dir1 出发，用长格式列出根目录内容。）

**解法：**
```bash
ls -l /
```

**详细解释：**
`ls` 列出目录，`-l` 显示权限、大小等详细信息。`/` 表示根目录。

---

## 第 8 题
**题目原文：**
«Situa’t a userlinux.»  
（进入 userlinux 目录。）

**解法：**
```bash
cd ../..
```

**详细解释：**
`..` 表示上一级，两次表示上上级。

---

## 第 9 题
**题目原文：**
«Mostra el contingut de userlinux incloent fitxers ocults.»  
（显示 userlinux 的所有文件，包括隐藏文件。）

**解法：**
```bash
ls -la
```

**详细解释：**
`-a` 显示隐藏文件，`-l` 长格式输出。

---

## 第 10 题
**题目原文：**
«Des de userlinux, situa’t a dir2 fent servir el camí absolut.»  
（从 userlinux 用绝对路径进入 dir2。）

**解法：**
```bash
cd /home/userlinux/dir2
```

**详细解释：**
绝对路径始终从根 `/` 开始，精确定位。

---

## 第 11 题
**题目原文：**
«Mostra en format llarg el contingut del directori arrel fent servir el camí absolut.»  
（用绝对路径长格式列出根目录内容。）

**解法：**
```bash
ls -l /
```

**详细解释：**
同第 7 题。

---

## 第 12 题
**题目原文：**
«Torna al directori casa (utilitza una forma diferent de la pregunta 2).»  
（回到 home，用不同方式。）

**解法：**
```bash
cd
```

**详细解释：**
`cd` 不带参数自动回 home，等价于 `cd ~`。

---

## 第 13 题
**题目原文：**
«Mostra la diferència entre les ordres cp practica1 practica2 i cp -r practica1 practica2.»  
（比较 cp 与 cp -r 的区别。）

**解法：**
```bash
cp practica1 practica2
cp -r practica1 practica2
```

**详细解释：**
`cp` 复制文件。  
普通文件可直接复制；目录需加 `-r`（递归）。

---

## 第 14 题
**题目原文：**
«Quina ordre serveix per netejar la pantalla?»  
（清屏命令。）

**解法：**
```bash
clear
```

**详细解释：**
`clear` 清除终端显示，不删历史或文件。快捷键：`Ctrl + L`。

---

## 第 15 题
**题目原文：**
«Explica la diferència entre rmdir practica, rm practica i rm -r practica.»  
（解释三种删除命令区别。）

**解法：**
```bash
rmdir practica
rm practica
rm -r practica
```

**详细解释：**
- `rmdir` 仅删空目录；  
- `rm` 删文件；  
- `rm -r` 递归删除目录及内容（危险操作）。

---



## 第 16 题
**题目原文：**
«Indicar el nombre de línies del fitxer file1 que comencen per la lletra ‘a’.»  
（统计 file1 中以字母 a 开头的行数。）

**解法：**
```bash
cd ~
grep "^a" file1 > linies_a
wc -l linies_a
```

**详细解释：**
- `grep "^a" file1`：筛选 file1 中所有以 a 开头的行。  
  `^` 表示“行首”，所以 `^a` 即“行首是 a”。  
- `> linies_a`：把这些行输出到新文件 linies_a。  
- `wc -l linies_a`：统计该文件中的行数。  
最终结果就是 file1 里以 a 开头的行数。

---

## 第 17 题
**题目原文：**
«Crea el fitxer file1bis igual que file1, però amb els dos primers camps intercanviats.»  
（创建 file1bis，使其与 file1 相同，但前两个字段互换。）

**解法（不使用 awk/sed）：**
```bash
cd ~
cut -d: -f1 file1 > col1
cut -d: -f2- file1 > rest
paste -d: <(cut -d: -f2 file1) <(cut -d: -f1,3- file1) > file1bis
```

**详细解释：**
在基础命令下，这题若完全实现，需要文本处理工具（如 awk/sed）。  
理解思路即可：  
- `cut -d:`：以 : 为分隔符。  
- `-f1` 取第 1 字段，`-f2` 取第 2 字段。  
若交换字段顺序并重写输出，即得到 file1bis。  
（在只用基础命令的前提下，此题仅能说明思路。）

---

## 第 18 题
**题目原文：**
«Modifica la primera columna de la línia 24 del fitxer file1bis, substituint-la per “AAA”.»  
（修改 file1bis 的第 24 行，把第 1 个字段改成 AAA。）

**解法（基础方法）：**
```bash
cd ~
cp file1bis file1bis_nou
head -n 23 file1bis > part1
echo "AAA:..." >> part1
tail -n +25 file1bis >> part1
mv part1 file1bis
```

**详细解释：**
由于不能用 sed/awk，只能手动分割：  
- `head -n 23`：取前 23 行。  
- `echo`：写入新第 24 行（改掉首列）。  
- `tail -n +25`：取第 25 行起的剩余部分并拼接。  
最后合并成新的 file1bis。

---

## 第 19 题
**题目原文：**
（从 Carol 目录开始的一系列操作，涉及切换与复制。）

![目录结构图 2]({{ '/assets/img/linux-structure-2.png' | relative_url }})


**解法与解释（主要步骤）：**

1️⃣ 返回家目录（不使用绝对路径）  
```bash
cd ..
```  
回到上一级目录 userlinux。

2️⃣ 显示当前目录路径  
```bash
pwd
```  
确认当前路径为 `/home/userlinux`。

3️⃣ 创建目录 practica  
```bash
mkdir practica
```  
在家目录下新建文件夹。

4️⃣ 进入 Shell-Project （绝对路径）  
```bash
cd /home/userlinux/Shell-Project
```

5️⃣ 创建文件 examen.txt  
```bash
echo "examen" > examen.txt
```  
`>` 把文字输出写入文件。

6️⃣ 把 esportistes 内容追加到 examen.txt  
```bash
cat esportistes >> examen.txt
```  
`>>` 表示追加，不覆盖。

7️⃣ 回到 Carol  
```bash
cd ../Carol
```

8️⃣ 复制 Carol 中所有内容到 exemples  
```bash
cp -r . ../Shell-Project/exemples/
```  
`-r` 递归复制当前目录下所有内容。

9️⃣ 回到 Shell-Project  
```bash
cd ../Shell-Project
```

🔟 长格式列出 exemples  
```bash
ls -l exemples
```

11️⃣ 统计普通文件数量  
```bash
ls -l | grep "^-“ | wc -l
```  
以 `-` 开头的行代表普通文件。

12️⃣ 显示以 “Barcelona” 结尾的行  
```bash
grep "Barcelona$" esportistes
```

13️⃣ 返回上一级 (userlinux)  
```bash
cd ..
```

14️⃣ 按名字排序显示目录内容  
```bash
ls | sort
```

15️⃣ 进入 Carol  
```bash
cd Carol
```

16️⃣ 给 mitjana.sh 文件追加 “FINAL”  
```bash
echo "FINAL" >> ../Shell-Project/mitjana.sh
```

17️⃣ 长格式显示根目录  
```bash
ls -l /
```

18️⃣ 显示 personatges 文件中包含 “Iron Man” 的行  
```bash
grep "Iron Man" personatges
```

19️⃣ 创建 versio2 ，去掉含 “Iron Man” 的行  
```bash
grep -v "Iron Man" personatges > versio2
```

20️⃣ 回到家目录  
```bash
cd
```

---

## 第 20 题（选择题形式）

a) 显示不以 A 开头的文件名  
```bash
ls | grep -v "^A"
```  
`-v` 表示“反选”，排除匹配项。

b) 显示第 5 到第 10 行  
```bash
head -n 10 fitxer | tail -n 6
```  
先取前 10 行，再保留最后 6 行，即第 5 至 10 行。

c) 把文件的行数存入变量  
```bash
lin=$(cat dades | wc -l)
```  
`wc -l` 统计行数，结果存入变量 `lin`。

d) 切到当前目录  
```bash
cd ./
```  
`.` 表示当前目录，等同 `cd .`。

e) 显示空行  
```bash
grep "^$" fitxer
```  
`^$` 匹配行首紧跟行尾，代表空行。

f) 显示两个文件的内容  
```bash
cat fitxer1 fitxer2
```  
`cat` 按顺序输出两个文件的内容。

---
