---
## Front matter
title: "Отчёт по лабораторной работе №6"
subtitle: "Разложение чисел на множители"
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
def mod(a ,b):
	return a % b


def pollard(n: int, c: int, f):
    d = 1
    cnt = 0
    a, b = c, c
    
    print(f"a = {a}, b = {b}")
    
    while d == 1:
        a = mod(f(a), n)
        b = mod(f(b), n)
        d = np.gcd(a - b, n)
        
        if mod(cnt, 100) == 0 or d != 1:
            print(f"iteration {cnt+1}: a = {a}, b = {b}, d = {d}")

        cnt += 1
        
    if d == n:
        print("Делитель не найден")
        return None
    
    return d


def pollard_test(n, c):
    print(f'Поллард {n}\n---------')
    f = lambda x: np.power(x, 2) + mod(np.random.randint(1, np.floor(np.sqrt(n))), n)
    p = pollard(n, c, f)
    
    if p != None:
        print(f'Нетривиальный делитель {n}: p = {p}')
        
    print(f'---------\n')


def main():
    pollard_test(1359331, 1)
    pollard_test(137, 5)
    pollard_test(322, 12)
    
if __name__ == "__main__":
    main()
```  




![Тестирование](images/7.png){ #fig:002 width=100% }






