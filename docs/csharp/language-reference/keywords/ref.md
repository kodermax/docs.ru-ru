---
title: "ref (Справочник по C#) | Microsoft Docs"
ms.custom: ""
ms.date: "11/24/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-csharp"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "ref_CSharpKeyword"
  - "ref"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "параметры [C#], ref"
  - "ref - ключевое слово [C#]"
ms.assetid: b8a5e59c-907d-4065-b41d-95bf4273c0bd
caps.latest.revision: 32
caps.handback.revision: 32
author: "BillWagner"
ms.author: "wiwagn"
manager: "wpickett"
translationtype: Human Translation
---
# ref (Справочник по C#)
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

Ключевое слово `ref` используется для передачи аргумента по ссылке, а не по значению.  Эффект передачи по ссылке в том, что все изменения вызываемого метода отражаются на значении переменной аргумента, используемой в вызове метода.  Например если вызывающий объект передает выражение локальной переменной или выражение доступа к элементу массива и вызванный метод заменяет объект, на который ссылается параметр ref, то локальная переменная или элемент массива взывающего объекта теперь ссылаться на новый объект.  
  
> [!NOTE]
>  Не следует путать понятие передачи по ссылке с понятием ссылочных типов.  Эти два понятия не совпадают.  Параметр метода может быть изменен с помощью `ref` независимо от того, принадлежит ли он к типу значения или ссылочному типу.  При передаче по ссылке упаковка\-преобразование типа значения не производится.  
  
 Для использования параметра `ref` и при определении метода, и при вызове метода следует явно использовать ключевое слово `ref`, как показано в следующем примере.  
  
 [!CODE [csrefKeywordsMethodParams#6](../CodeSnippet/VS_Snippets_VBCSharp/csrefKeywordsMethodParams#6)]  
  
 Аргумент, передаваемый в параметр `ref`, перед передачей должен быть инициализирован.  В этом заключается отличие от параметров `out`, аргументы которых не требуют явной инициализации перед передачей.  Дополнительные сведения см. в разделе [out](../../../csharp/language-reference/keywords/out.md).  
  
 Члены класса не могут иметь сигнатуры, отличие которых заключается только в `ref` и `out`.  Если единственное различие между двумя членами типа состоит в том, что один из них имеет параметр `ref`, а второй имеет параметр `out`, возникает ошибка компилятора.  Например, следующий код не будет компилироваться.  
  
 [!CODE [csrefKeywordsMethodParams#2](../CodeSnippet/VS_Snippets_VBCSharp/csrefKeywordsMethodParams#2)]  
  
 Однако перегрузка может быть выполнена, если один метод принимает параметр `ref` или `out`, а другой принимает параметр по значению, как показано в следующем примере.  
  
 [!CODE [csrefKeywordsMethodParams#7](../CodeSnippet/VS_Snippets_VBCSharp/csrefKeywordsMethodParams#7)]  
  
 В других ситуациях, требующих соответствия сигнатур, таких как скрытие или переопределение, `ref` и `out` являются частью сигнатуры и не соответствуют друг другу.  
  
 Свойства не являются переменными.  Они являются методами и не могут быть переданы в параметр `ref`.  
  
 Дополнительные сведения о передаче массивов см. в разделе [Передача массивов при помощи параметров ref и out](../../../csharp/programming-guide/arrays/passing-arrays-using-ref-and-out.md).  
  
 Ключевые слова `ref` и `out` нельзя использовать для следующих типов методов.  
  
-   Асинхронные методы, которые определяются с помощью модификатора [async](../../../csharp/language-reference/keywords/async.md).  
  
-   Методы итератора, которые включают оператор [yield return](../../../csharp/language-reference/keywords/yield.md) или `yield break`.  
  
## Пример  
 В предыдущих примерах показано, что происходит при передаче типов значений по ссылке.  Можно также использовать ключевое слово `ref` для передачи ссылочных типов.  Передача ссылочного типа по ссылке позволяет вызываемому методу изменять объект, на который указывает ссылочный параметр.  Место хранения объекта передается методу в качестве значения ссылочного параметра.  Если изменить место хранения параметра \(с указанием на новый объект\), необходимо изменить место хранения, на который ссылается вызывающий объект.  В следующем примере экземпляр ссылочного типа передается как параметр `ref`.  Дополнительные сведения о передаче ссылочных типов по значению и по ссылке см. в разделе [Передача параметров ссылочного типа](../../../csharp/programming-guide/classes-and-structs/passing-reference-type-parameters.md).  
  
 [!CODE [csrefKeywordsMethodParams#8](../CodeSnippet/VS_Snippets_VBCSharp/csrefKeywordsMethodParams#8)]  
  
## Спецификация языка C\#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## См. также  
 [Справочник по C\#](../../../csharp/language-reference/index.md)   
 [Руководство по программированию на C\#](../../../csharp/programming-guide/index.md)   
 [Передача параметров](../../../csharp/programming-guide/classes-and-structs/passing-parameters.md)   
 [Параметры методов](../../../csharp/language-reference/keywords/method-parameters.md)   
 [Ключевые слова C\#](../../../csharp/language-reference/keywords/index.md)