---
## Front matter
title: "Отчёт по лабораторной работе №3"
subtitle: "Шифрование гаммированием"
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

Приoбрести практические навыки рабoты с шифрованием гаммированием.

# Хoд рабoты 

Программа содержит 3 функции get_alpha, gamma_encrypt и test.


```Python
def get_alpha(option):
    if option=='eng':
        return list(map(chr,range(ord('a'), ord('z')+1)))
    elif option=='rus':
        return list(map(chr,range(ord('а'), ord('я')+1)))
    else:
        print('ошибка')
def gamma_encrypt (message: str, gamma: str):
    alph=get_alph('eng')
    if message.lower() not in alph: 
        alph=get_alph('rus')
    print(alph)
    m=len(alph)
    def encrypt(letters_pair: tuple):
        idx=(letters_pair[0]+1)+(letters_pair[1]+1)%m 
        if idx>m:
            idx=idx-m
        return idx-1
    message_clear=list(filter(lambda s: s.lower() in alph,message)) 
    gamma_clear=list(filter(lambda s: s.lower() in alph, gamma))
    message_ind=list(map(lambda s: alph.index(s.lower()),message_clear)) 
    gamma_ind=list(map (lambda s: alph.index(s.lower()),gamma_clear)) 
    for i in range(len(message_ind)-len(gamma_ind)):
        gamma_ind.append(gamma_ind[i])
    print(f'{message.upper()} -> {message_ind}\n{gamma.upper()} -> {gamma_ind}') 
    encrypted_ind=list(map(lambda s: encrypt(s),zip(message_ind,gamma_ind)))
    print(f'encrypted form: {encrypted_ind}\n')
    return ''.join(list(map(lambda s: alph[s],encrypted_ind))).upper()
def test(message: str, gamma: str):
    print(f'encryption result: {gamma_encrypt(message, gamma)}')
message='приказ'
gamma='гамма'
test(message, gamma)
```  

![Шифрование гаммированием](images/1.png){ #fig:001 width=100% }


