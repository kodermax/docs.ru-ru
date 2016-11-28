---
title: "Практическое руководство. Вызов компилятора командной строки (Visual Basic) | Microsoft Docs"
ms.custom: ""
ms.date: "11/24/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "командная строка, аргументы"
  - "аргументы командной строки"
  - "vbc.exe"
  - "компилятор Visual Basic, запуск"
ms.assetid: 0fd9a8f6-f34e-4c35-a49d-9b9bbd8da4a9
caps.latest.revision: 28
caps.handback.revision: 28
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
translationtype: Human Translation
---
# Практическое руководство. Вызов компилятора командной строки (Visual Basic)
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

Чтобы вызвать компилятор командной строки, можно ввести имя исполняемого файла в командной строке \(сеанс MS\-DOS\).  Если компиляция выполняется из командной строки Windows, необходимо ввести полный путь к исполняемому файлу.  Чтобы переопределить тип выполнения по умолчанию, можно либо использовать командную строку [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)], либо изменить переменную среды PATH.  Оба способа позволяют выполнять компиляцию из любого каталога, просто указывая имя компилятора.  
  
 [!INCLUDE[note_settings_general](../../../csharp/language-reference/compiler-messages/includes/note_settings_general_md.md)]  
  
### Вызов компилятора с помощью командной строки Visual Studio  
  
1.  В группе программ Microsoft Visual Studio откройте папку "Tools" программы Visual Studio.  
  
2.  Можно использовать командную строку [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] для доступа к компилятору из каталога на компьютере при установке Visual Studio.  
  
3.  Вызовите командную строку [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)].  
  
4.  В командной строке введите `vbc.exe` *имя\_файла\_исходного\_кода* и нажмите клавишу ВВОД.  
  
     Например, если исходный код сохранен в каталоге с именем `SourceFiles`, то откройте командную строку и введите `cd SourceFiles`, чтобы перейти в этот каталог.  Если каталог, содержащий исходный файл, называется `Source.vb`, то для его компиляции введите `vbc.exe Source.vb`.  
  
### Задание переменной среды PATH для компилятора при использовании командной строки Windows  
  
1.  Используйте функцию поиска Windows, чтобы найти файл Vbc.exe на локальном диске.  
  
     Точное имя каталога, в котором расположен компилятор, зависит от расположения каталога Windows и версии установленного модуля [!INCLUDE[Compact](../../../visual-basic/reference/command-line-compiler/includes/compact_md.md)].  Если имеется несколько версий [!INCLUDE[Compact](../../../visual-basic/reference/command-line-compiler/includes/compact_md.md)], необходимо определить, какую версию следует использовать \(как правило, это последняя версия\).  
  
2.  В меню **Пуск** щелкните правой кнопкой мыши **Мой компьютер**, затем в контекстном меню выберите команду **Свойства**.  
  
3.  Перейдите на вкладку **Дополнительно** и нажмите кнопку **Переменные среды**.  
  
4.  В области **Системные переменные** выберите **Path** из списка и нажмите кнопку **Изменить**.  
  
5.  В диалоговом окне **Изменение системной переменной** переместите указатель в конец строки в поле **Значение переменной** и введите точку с запятой \(;\), а следом полное имя каталога \(см. действие 1\).  
  
6.  Нажмите кнопку **ОК**, чтобы подтвердить изменения и закрыть диалоговые окна.  
  
     После изменения переменной среды PATH можно запустить компилятор [!INCLUDE[vbprvb](../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] в командной строке Windows из любого каталога на компьютере.  
  
### Вызов компилятора с помощью командной строки Windows  
  
1.  В меню **Пуск** выберите папку **Стандартные**, а затем пункт **Командная строка Windows**.  
  
2.  В командной строке введите `vbc.exe` *имя\_файла\_исходного\_кода* и нажмите клавишу ВВОД.  
  
     Например, если исходный код сохранен в каталоге с именем `SourceFiles`, то откройте командную строку и введите `cd SourceFiles`, чтобы перейти в этот каталог.  Если каталог, содержащий исходный файл, называется `Source.vb`, то для его компиляции введите `vbc.exe Source.vb`.  
  
## См. также  
 [Компилятор Visual Basic с интерфейсом командной строки](../../../visual-basic/reference/command-line-compiler/index.md)   
 [Условная компиляция](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)