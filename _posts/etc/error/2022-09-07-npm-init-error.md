---
title:  "[error: npm init]"
categories: [etc, error] 
tags: [npm]
toc: true
toc_sticky: true
date: 2022-09-07
---

<br>
<br>
<br>
<br>

> # π¨λ¬Έμ  λ°μ &nbsp;
> * μ¬μ§ μ°Έκ³ 

<br>
<br>

```console
npm init
```
μλ ₯νλλ

![Desktop View](/assets/img/error/npm/1.PNG)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


> # πλ΄κ° ν΄κ²°ν λ°©λ² 

## 1  &nbsp; `windowspowershell`μ μλ ₯νμ¬ κ΄λ¦¬μλ‘ μ€νν©μλ€
<img src="/assets/img/error/npm/2.PNG" width="500" height="500">

<br>
<br>
<br>
<br>
<br>



<br>
<br>
<br>

## 2  &nbsp; κΆνμν νμΈ

<br>

<img src="/assets/img/error/npm/3.PNG" width="700" height="600">

```console
 get-ExecutionPolicy
 ```
 μ΄κ±Έ μλ ₯ν΄λ³΄λ©΄ Restricted λΌκ³  λμ¬κ²λλ€

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## 3  &nbsp; κΆνλ³κ²½ λ° λ³κ²½μ΄ μ λμλμ§ νμΈ

<br>

![Desktop View](/assets/img/error/npm/4.PNG)

<br>
<br>
<br>

```
- κΆν μνκ° μ°Έκ³ 

 Restricted : defaultμ€μ κ°μΌλ‘, μ€ν¬λ¦½νΈ νμΌμ μ€νν  μ μμ΅λλ€.

 AllSigned : μ λ’°ν  μ μλ(μλͺλ) μ€ν¬λ¦½νΈ νμΌλ§ μ€νν  μ μμ΅λλ€.

 RemoteSigned : λ‘μ»¬μμ λ³ΈμΈμ΄ μμ±ν μ€ν¬λ¦½νΈμ, μ λ’°ν  μ μλ(μλͺλ) μ€ν¬λ¦½νΈ νμΌ μ€νν  μ μμ΅λλ€.

 Unrestricted : λͺ¨λ  μ€ν¬λ¦½νΈ μ€νκ°λ₯

 ByPass : κ²½κ³ /μ°¨λ¨ μμ΄ λͺ¨λ  κ²μ μ€νκ°λ₯νλλ‘ν¨

 Undefined : κΆνμ μ€μ νμ§ μκ² μ
```

<br>
<br>
<br>

```console
Set-ExecutionPolicy RemoteSigned
```
μλ ₯ν y μλ ₯νκ³  μν° 

```console
 get-ExecutionPolicy
 ```
κ·Έλ¦¬κ³  `RemoteSigned`λ‘ λ³κ²½μ΄ μ λμλμ§ νμΈν΄λ΄μλ€\
λ€ λμλ€λ©΄ vscodeλ cmdμμ npm init μλ ₯νμμλ μ μλν κ²λλ€!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## λ³΄μκ³  λ―Έν‘ν λΆλΆμ΄ μλ€λ©΄ νΌλλ°±μ μΈμ λ νμν©λλ€!

<br>
<br>
μλ μ¬μ§μ ν΄λ¦­νλ©΄ `μ  μ·¨λ―Έκ³΅κ°`μΌλ‘ μ΄μ΄μ§λλ€ γγ μ¬κΈ°μμ λ©λͺ¨ λ° μΌμμ κΈ°λ‘νλλ° λλ¬μ€μ€ λΆλ€μ μΈμ λ νμν©λλ€!

<br>

# λ§ν¬λ‘ μ΄λνλ €λ©΄ μ¬μ§μ ν΄λ¦­

[![μ΄μμ€μ γ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.com/channels/976352361142452234/976352361142452239)



---
# μ°Έκ³ 
---
 'μ§μ₯μλΆκ΅¬λ©μ΄' &nbsp;&nbsp;&nbsp;&nbsp;   [VSCode μ€λ₯ : μ΄ μμ€νμμ μ€ν¬λ¦½νΈλ₯Ό μ€νν  μ μμΌλ―λ‘ ...](https://hellcoding.tistory.com/entry/VSCode-%EC%98%A4%EB%A5%98-%EC%9D%B4-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%97%90%EC%84%9C-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%95%A0-%EC%88%98-%EC%97%86%EC%9C%BC%EB%AF%80%EB%A1%9C)