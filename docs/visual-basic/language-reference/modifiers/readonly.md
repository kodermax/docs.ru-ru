---
title: "ReadOnly (Visual Basic) | Microsoft Docs"
ms.custom: ""
ms.date: "11/24/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "vb.ReadOnly"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "свойства [Visual Basic], только для чтения"
  - "ReadOnly - ключевое слово"
  - "ReadOnly - свойство"
  - "переменные только для чтения"
  - "переменные [Visual Basic], только для чтения"
ms.assetid: e868185d-6142-4359-a2fd-a7965cadfce8
caps.latest.revision: 15
caps.handback.revision: 15
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
translationtype: Human Translation
---
# ReadOnly (Visual Basic)
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

Указывает на то, что переменная или свойство может быть использована для чтения, но не для записи.  
  
## Заметки  
  
## Правила  
  
-   **Контекст объявления.** Можно использовать зарезервированное слово `ReadOnly` только на уровне модуля.  Это означает, что элемент с модификатором `ReadOnly` должен объявляться в контексте класса, структуры или модуля, но не исходного файла, пространства имен или процедуры.  
  
-   **Комбинированные модификаторы.** Использовать модификаторы `ReadOnly` и `Static` в одном объявлении нельзя.  
  
-   **Присвоение значения.** Код, использующий свойство с модификатором `ReadOnly`, не может задать его значение.  Тем не менее, код, имеющий доступ к используемой свойством памяти, может задать или изменить значение в любое время.  
  
     Значение переменной с модификатором `ReadOnly` можно присваивать только при объявлении или в конструкторе класса или структуры.  
  
## Когда следует использовать переменные с модификатором ReadOnly  
 Существуют ситуации, в которых нельзя использовать [Оператор Const](../../../visual-basic/language-reference/statements/const-statement.md) для объявления и присвоения константных значений.  Например, инструкция `Const` может не поддерживать тип данных, который требуется применить, или может отсутствовать возможность вычисления значения во время компиляции с помощью константного выражения.  Значение даже может быть неизвестно во время компиляции.  В этих случаях для хранения постоянного значения можно использовать переменную с модификатором `ReadOnly`.  
  
> [!IMPORTANT]
>  Если тип данных переменной является ссылочным \(например, это массив или экземпляр класса\), то его члены могут быть изменены, даже если сама переменная имеет модификатор `ReadOnly`.  Это показано в приведенном ниже примере.  
  
 `ReadOnly characterArray() As Char = {"x"c, "y"c, "z"c}`  
  
 `Sub changeArrayElement()`  
  
 `characterArray(1) = "M"c`  
  
 `End Sub`  
  
 При инициализации массив, на который указывает `characterArray()`, содержит "х", "y" и "z".  Поскольку переменная `characterArray` имеет модификатор `ReadOnly`, ее значение после инициализации изменить нельзя; это значит, что ей нельзя присвоить новый массив.  Тем не менее, можно изменить значение одного или нескольких элементов массива.  После вызова процедуры `changeArrayElement` массив, на который указывает `characterArray()`, содержит "х", "M" и "z".  
  
 Следует заметить, что это похоже на объявление параметра процедуры с модификатором [ByVal](../../../visual-basic/language-reference/modifiers/byval.md), который запрещает процедуре изменять аргумент вызова, но позволяет ей изменять его члены.  
  
## Пример  
 В следующем примере определяется свойство с модификатором `ReadOnly`, возвращающее дату приема сотрудника на работу.  Класс хранит значение свойства в переменной с модификатором `Private`, и только код внутри класса можно изменить ее значение.  Тем не менее, свойство имеет модификатор `Public`, и любой код, имеющий доступ к классу, можно получить его значение.  
  
 [!CODE [VbVbalrKeywords#4](../CodeSnippet/VS_Snippets_VBCSharp/VbVbalrKeywords#4)]  
  
 Модификатор `ReadOnly` можно использовать в следующих контекстах:  
  
 [Оператор Dim](../../../visual-basic/language-reference/statements/dim-statement.md)  
  
 [Оператор Property](../../../visual-basic/language-reference/statements/property-statement.md)  
  
## См. также  
 [WriteOnly](../../../visual-basic/language-reference/modifiers/writeonly.md)   
 [Ключевые слова](../../../visual-basic/language-reference/keywords/index.md)