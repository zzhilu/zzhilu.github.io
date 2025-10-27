---
title: "menu.sh：运动员信息菜单脚本（含逐行解释）"
date: 2025-10-27
layout: page
permalink: /notes/menu-sh/
---

# `menu.sh`：运动员信息菜单脚本（含逐行解释）

> 仅使用最基础的 Bash 命令（`cut`, `grep`, `echo`, `while read` 等），无 `awk` / `sed`。  
> 支持：计算平均年龄、按俱乐部筛选、按姓氏筛选、交互式菜单。

---

## 📜 脚本源码：`menu.sh`

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

---

> 需要把它打包为可下载的 `.md` 文件或 `.zip`（含脚本+示例数据）也行，告诉我你的偏好即可。
