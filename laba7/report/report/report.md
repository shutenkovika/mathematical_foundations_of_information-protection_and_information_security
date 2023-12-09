---
## Front matter
title: "Отчёт по лабораторной работе №7"
subtitle: "Дискретное логарифмирование в конечном поле"
author: "Шутенко Виктория Михайловна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---


# Цель рабoты

Приoбрести практические навыки рабoты с разложением чисел на множители.

# Хoд рабoты 

## Алгоритм, реализующий р-метод Полларда


```Python
# Laboratory Work
# Theme: Discrete logarthmification
# Author: Vladimir Doborschuk

# --- Modules ---

import numpy as np

# --- Functions ---

# --- mod(a, b) ---

def mod(a ,b):
	return a % b

# --- find mod order ---

def order(a, p):
    x = 1
    while mod(a**x - 1, p) != 0:
        x += 1
        
    return x

# --- Pollard's P-method for Log ---

'''
a - основание
b - значение остатка
p - простое число
'''
def po_method(a: int, b: int, p: int):
    print(f"\n{a}^(x) = {b} mod {p}")
    print("-----------------------------------------------------------------")
    print('|\tc\t|\tlog c\t|\td\t|\tlog d\t|')
    print("-----------------------------------------------------------------")
    
    u = np.random.randint(4)
    v = np.random.randint(4)
    r = order(a, p)
    
    c = mod(np.power(a, u) * np.power(b, v), p)
    d = c
    
    u_c, u_d = u, u
    v_c, v_d = v, v
    
    print(f'|\t{c}\t|\t{u_c}+{v_c}x\t|\t{d}\t|\t{u_d}+{v_d}x\t|')
    
    def f(x, u_x, v_x):
        if x < r:
            return mod(a*x, p), u_x + 1, v_x
        else:
            return mod(b*x, p), u_x, v_x + 1            

    c, u_c, v_c = f(c, u_c, v_c)
    tmp_d = f(d, u_d, v_d)
    d, u_d, v_d = f(tmp_d[0], tmp_d[1], tmp_d[2])
    
    while mod(c, p) != mod(d, p):
        print(f'|\t{c}\t|\t{u_c}+{v_c}x\t|\t{d}\t|\t{u_d}+{v_d}x\t|')
        c, u_c, v_c = f(c, u_c, v_c)
        tmp_d = f(d, u_d, v_d)
        d, u_d, v_d = f(tmp_d[0], tmp_d[1], tmp_d[2])
        
    print(f'|\t{c}\t|\t{u_c}+{v_c}x\t|\t{d}\t|\t{u_d}+{v_d}x\t|')
    print("-----------------------------------------------------------------")
    
    x = 1
    # print(v_c - v_d, u_d - u_c)
    while mod((v_c - v_d)*x, r) != mod(u_d - u_c, r):
        x += 1
        
    print(f"x = {x}")
    print(f"\n{a}^({x}) = {b} mod {p}")
    print("-----------------------------------------------------------------")
    return x

# --- Main ---

def main():
    po_method(10, 64, 107)
    po_method(2, 1, 15)
    
    
if __name__ == "__main__":
    main()
```  




![Тестирование](images/1.png){ #fig:001 width=100% }






