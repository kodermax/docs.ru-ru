---
title: "Основы сборки мусора"
description: "Основы сборки мусора"
keywords: .NET, .NET Core
author: stevehoag
ms.author: shoag
manager: wpickett
ms.date: 08/16/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 9d5fce64-95a4-4609-8eee-b0ac70078cdb
translationtype: Human Translation
ms.sourcegitcommit: b20713600d7c3ddc31be5885733a1e8910ede8c6
ms.openlocfilehash: 78a2d593329f0703c71df2462cfea30b02adff85

---

# <a name="fundamentals-of-garbage-collection"></a>Основы сборки мусора

В среде CLR сборщик мусора выполняет функции автоматического диспетчера памяти. Это предоставляет следующие преимущества:

* Позволяет разрабатывать приложение без необходимости освобождать память. 

* Эффективно выделяет память для объектов в управляемой куче. 

* Уничтожает объекты, которые больше не используются, очищает их память и сохраняет память доступной для будущих распределений. Содержимое создаваемых управляемых объектов автоматически оказывается очищенным, чтобы их конструкторам не было нужно инициализировать каждое поле данных.

* Обеспечивает безопасность памяти, гарантируя, что объект не сможет использовать содержимое другого объекта.


В этом разделе описаны основные понятия сборки мусора. Он содержит следующие подразделы:

* [Основы работы с памятью](#fundamentals-of-memory)

* [Условия для сборки мусора](#conditions-for-a-garbage-collection)

* [Управляемая куча](#the-managed-heap)

* [Поколения](#generations)

* [Процесс сборки мусора](#what-happens-during-a-garbage-collection)

* [Работа с неуправляемыми ресурсами](#manipulating-unmanaged-resources)

## <a name="fundamentals-of-memory"></a>Основы работы с памятью

В следующем списке перечислены важные понятия памяти среды CLR.

* Каждый процесс имеет свое собственное отдельное виртуальное адресное пространство. Все процессы на одном компьютере совместно используют одну и ту же физическую память и один файл подкачки, если он есть.

* По умолчанию на 32-разрядных компьютерах каждому процессу выделяется 2 Гбайт виртуального адресного пространства в пользовательском режиме.

* Разработчики приложений работают только с виртуальным адресным пространством и никогда не управляют физической памятью напрямую. Сборщик мусора выделяет и освобождает виртуальную память для разработчика в управляемой куче.

* Виртуальная память может находиться в трех состояниях. 

    * Свободная. Ссылки на блок памяти отсутствуют, и он доступен для выделения.

    * Зарезервировано. Блок памяти доступен для использования разработчиком и не может использоваться для какого-либо другого запроса на выделение. Однако сохранение данных в этот блок памяти невозможно, пока он не будет выделен. 

    * Выделена. Блок памяти назначен физическому хранилищу.

* Виртуальное адресное пространство может стать фрагментированным. Это означает, что в адресном пространстве находятся свободные блоки, также известные как пропуски. Когда производится запрос на выделение виртуальной памяти, диспетчер виртуальной памяти должен найти один свободный блок достаточного размера для выполнения этого запроса на выделение. Даже при наличии 2 Гбайт свободного пространства операция выделения, требующая 2 Гбайт, завершится неудачей, если все это пространство не находится в одном адресном блоке.

* Память может закончиться, если закончится виртуальное адресное пространство для резервирования или физическое пространство для выделения.

Файл подкачки используется, даже если нехватка физической памяти (то есть потребность в физической памяти) невелика. При первом возрастании нехватки физической памяти операционная система должна освободить пространство в физической памяти для хранения данных, и она производит резервное копирование некоторых данных, находящихся в физической памяти, в файл подкачки. Эти данные не выгружаются, пока в этом нет необходимости, так что с подкачкой можно столкнуться в ситуациях с очень небольшой нехваткой физической памяти.

## <a name="conditions-for-a-garbage-collection"></a>Условия для сборки мусора

Сборка мусора возникает при выполнении одного из следующих условий:

* Недостаточно физической памяти в системе.

* Память, используемая объектами, выделенными в управляемой куче, превышает допустимый порог. Этот порог непрерывно корректируется во время выполнения процесса.

* Вызывается метод [GC.Collect](xref:System.GC.Collect). Практически во всех случаях вызов этого метода не потребуется, так как сборщик мусора работает непрерывно. Этот метод в основном используется для уникальных ситуаций и тестирования. 

## <a name="the-managed-heap"></a>Управляемая куча

После инициализации средой CLR сборщик мусора выделяет сегмент памяти для хранения объектов и управления ими. Эта память называется управляемой кучей в отличие от собственной кучи операционной системы. 

Управляемая куча создается для каждого управляемого процесса. Все потоки в процессе выделяют память для объектов в одной и той же куче.

> [!IMPORTANT]
> Размер сегментов, выделенных сборщиком мусора, зависит от реализации и может быть изменен в любое время, в том числе при периодических обновлениях. Приложение не должно делать никаких допущений относительно размера определенного сегмента, полагаться на него или пытаться настроить объем памяти, доступный для выделения сегментов. 
 
Чем меньше объектов распределено в куче, чем меньше придется работать сборщику мусора. При размещении объектов не используйте округленные значения, превышающие фактические потребности, например не выделяйте 32 байта, когда необходимо только 15 байтов. 

Сборка мусора, когда она запущена, освобождает память, занятую неиспользуемыми объектами. Процесс освобождения сжимает используемые объекты, чтобы они перемещались вместе, и удаляет пространство, занятое неиспользуемыми объектами, уменьшая, таким образом, кучу. Это гарантирует, что объекты, распределенные совместно, останутся в управляемой куче рядом, чтобы сохранить их локальность.

Степень вмешательства (частота и длительность) сборок мусора зависит от числа распределений и сохранившейся в управляемой куче памяти. 

Кучу можно рассматривать как совокупность двух куч: куча больших объектов и куча маленьких объектов. 

Куча больших объектов содержит очень большие объекты размером от 85 000 байт. Объекты в куче больших объектов обычно являются массивами. Экземпляр объекта редко бывает очень большим. 

## <a name="generations"></a>Поколения

Куча организована в виде поколений, что позволяет ей обрабатывать долгоживущие и короткоживущие объекты. Сборка мусора в основном сводится к уничтожению короткоживущих объектов, которые обычно занимают только небольшую часть кучи. В куче существует три поколения объектов. 

* **Поколение 0**. Это самое молодое поколение содержит короткоживущие объекты. Примером короткоживущего объекта является временная переменная. Сборка мусора чаще всего выполняется в этом поколении. 

  Вновь распределенные объекты образуют новое поколение объектов и неявно являются сборками поколения 0, если они не являются большими объектами, в противном случае они попадают в кучу больших объектов в сборке поколения 2.

  Большинство объектов уничтожаются при сборке мусора для поколения 0 и не доживают до следующего поколения. 

* **Поколение 1**. Это поколение содержит коротко живущие объекты и служит буфером между короткоживущими и долгоживущими объектами. 

* **Поколение 2**. Это поколение содержит долгоживущие объекты. Примером долгоживущих объектов служит объект в серверном приложении, содержащий статические данные, которые существуют в течение длительности процесса.

Сборки мусора выполняются для конкретных поколений при выполнении соответствующих условий. Сборка поколения означает сбор объектов в этом поколении и во всех соответствующих младших поколениях. Сборка мусора поколения 2 также называется полной сборкой мусора, так как она уничтожает все объекты во всех поколениях (то есть все объекты в управляемой куче).

### <a name="survival-and-promotions"></a>Выживание и переходы

Объекты, которые не уничтожаются при сборке мусора, называются выжившими объектами и переходят в следующее поколение. Объекты, пережившие сборку мусора для поколения 0, переходят в поколение 1, объекты, пережившие сборку мусора для поколения 1, переходят в поколение 2, а объекты, пережившие сборку мусора для поколения 2, остаются в поколении 2.

Когда сборщик мусора обнаруживает высокую долю выживания в поколении, он повышает порог распределений для этого поколения, чтобы при следующей сборке мусора освобождалась заметная часть занятой памяти. В среде CLR непрерывно контролируется равновесие двух приоритетов: не позволить рабочему набору приложения стать слишком большим и не позволить сборке мусора занимать слишком много времени.

### <a name="ephemeral-generations-and-segments"></a>Эфемерные поколения и сегменты

Так как объекты в поколениях 0 и 1 являются короткоживущими, эти поколения называются эфемерными поколениями. 

Эфемерные поколения должны распределяться в сегменте памяти, который называется эфемерным сегментом. Каждый новый сегмент, полученный сборщиком мусора, становится новым эфемерным сегментом и содержит объекты, пережившие сборку мусора для поколения 0. Старый эфемерный сегмент становится новым сегментом поколения 2. 


Этот эфемерный сегмент может содержать объекты поколения 2. Объекты поколения 2 могут использовать несколько сегментов (столько, сколько требуется процессу и сколько разрешает память). 

Объем памяти, освобождаемой при эфемерной сборке мусора, ограничен размером эфемерного сегмента. Освобождаемый объем памяти пропорционален пространству, занятому неиспользуемыми объектами.

## <a name="what-happens-during-a-garbage-collection"></a>Процесс сборки мусора

Сборка мусора состоит из следующих этапов: 

* Этап маркировки, выполняющий поиск всех используемых объектов и составляющий их перечень.

* Этап перемещения, обновляющий ссылки на сжимаемые объекты. 

* Этап сжатия, освобождающий пространство, занятое неиспользуемыми объектами и сжимающий выжившие объекты. На этапе сжатия объекты, пережившие сборку мусора, перемещаются к более старому концу сегмента. 

Так как сборки поколения 2 могут занимать несколько сегментов, объекты, перешедшие в поколение 2, могут быть перемещены в более старый сегмент. Выжившие объекты поколений 1 и 2 могут быть перемещены в другой сегмент, так как они перешли в поколение 2. 

Как правило, куча больших объектов не сжимается, поскольку копирование больших объектов приводит к снижению производительности. Однако можно использовать свойство [GCSettings.LargeObjectHeapCompactionMode](xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode) для сжатия большой кучи объектов по требованию. 

Чтобы определить, являются ли объекты используемыми, сборщик мусора задействует следующие сведения. 

* **Корни стека**. Переменные стека, предоставленные JIT-компилятором и средством обхода стека.

* **Дескрипторы сборки мусора**. Дескрипторы, которые указывают на управляемые объекты и могут быть выделены пользовательским кодом или средой CLR.

* **Статические данные**. Статические объекты в доменах приложений, которые могут ссылаться на другие объекты. Каждый домен приложения следит за своими статическими объектами.

Перед запуском сборки мусора все управляемые потоки, кроме потока, запустившего сборку мусора, приостанавливаются.

На следующем рисунке показан поток, запускающий сборку мусора и вызывающий приостановку других потоков.

![Когда поток запускает сборку мусора](./media/fundamentals/393001.png)

Поток, запускающий сборку мусора

## <a name="manipulating-unmanaged-resources"></a>Манипулирование неуправляемыми ресурсами

Если управляемые объекты ссылаются на неуправляемые объекты, используя свои собственные дескрипторы файлов, разработчику необходимо явно освобождать неуправляемые объекты, так как сборщик мусора следит за памятью только в управляемой куче.

Пользователи управляемого объекта не могут удалить неуправляемые ресурсы, используемые объектом. Для выполнения очистки можно сделать управляемый объект подлежащим завершению. Завершение состоит из очищающих действий, выполняемых, когда объект перестает быть нужным. Когда управляемый объект уничтожается, он выполняет очищающие действия, заданные в его методе завершения.

Когда обнаруживается, что подлежащий завершению объект больше не используется, его метод завершения помещается в очередь, чтобы выполнить его очищающие действия, но сам объект переходит в следующее поколение. Следовательно, придется дождаться следующей сборки мусора, выполняемой для этого поколения (которой необязательно будет следующая сборка мусора), чтобы определить, удален ли объект.

## <a name="see-also"></a>См. также

[Сборка мусора в .NET](gc.md)



<!--HONumber=Nov16_HO3-->


