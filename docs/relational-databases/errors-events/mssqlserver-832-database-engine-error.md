---
description: MSSQLSERVER_832
title: MSSQLSERVER_832
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 832 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ffd0cce51495b497cfb1a3ac2948f01a2a242e52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348686"
---
# <a name="mssqlserver_832"></a>MSSQLSERVER_832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|832|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|B_CONSTPAGECHANGED|
|Texto del mensaje|Ha cambiado una página que debería haber permanecido constante (suma de comprobación esperada: \<expected value>, suma de comprobación real: \<actual value>, base de datos \<dbid>, archivo \'<filename>', página \<pageno>). Esto indica normalmente un error de memoria u otro tipo de daño de hardware o SO.|
||

## <a name="explanation"></a>Explicación

Un factor externo ha provocado que se haya modificado una página de base de datos fuera del código normal del motor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se usa para cambiar las páginas de bases de datos.  Las condiciones podrían ser estas:  

- Un subproceso que se ejecuta en el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha escrito incorrectamente en una página de base de datos. A eso se le suele denominar "scribbler".
- Un problema de hardware o del sistema operativo en el que la memoria de respaldo de la página de base de datos no se modificó correctamente o está dañada.  

Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detecta este comportamiento, se genera el error 832.

## <a name="user-action"></a>Acción del usuario

Para encontrar la causa del error, considere las siguientes opciones:

- Debe ejecutar las comprobaciones normales del hardware o del sistema para determinar si existe un problema relacionado con la memoria, la CPU u otro tipo de hardware. Asegúrese de que se han aplicado al sistema todos los controladores del sistema, las actualizaciones del sistema operativo y las actualizaciones de hardware. Considere la posibilidad de ejecutar los diagnósticos del fabricante del hardware, incluidas pruebas relacionadas con la memoria.
- Evalúe qué archivos .dll "externos" cargados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podrían causar este problema, incluidos los procedimientos almacenados extendidos, los objetos COM u otros archivos .dll que podrían estar modificando incorrectamente la memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reservada para las páginas de bases de datos.  

Siempre que vea este error, debería considerar la posibilidad de ejecutar de inmediato `DBCC CHECKDB` en la base de datos a la que hace referencia \<dbid> en el mensaje de error.

## <a name="more-information"></a>Más información

Este error lo detecta la tarea que se ejecuta en segundo plano y que a menudo se conoce como escritura diferida. (El "comando" para esta tarea se muestra como LAZY WRITER). Por tanto, este error no se devuelve a una aplicación cliente. El error se escribirá en el registro de eventos de la aplicación de Windows como EventID=832.  

Solo se comprueban las páginas que no se han modificado en caché (o no están "desfasadas"). Este es el motivo por el que el mensaje usa el término "constante", porque la página no ha cambiado desde su lectura desde el disco. Además, la página se leyó de forma "correcta" desde el disco porque tiene un valor de suma de comprobación en la página y no ha encontrado ningún error de suma de comprobación (mensaje 824). Pero la página podría modificarse en algún momento después de este error y, posteriormente, podría escribirse en el disco con la modificación incorrecta. Si se produce este error, se calcula una nueva suma de comprobación en función de todas las modificaciones antes de su escritura en el disco. Por lo tanto, la página podría estar dañada en el disco, pero es posible que una lectura posterior desde el disco no desencadene un error de suma de comprobación. Es importante ejecutar `DBCC CHECKDB` en cualquier base de datos a la que se haga referencia en este error.  

Es posible que ni siquiera `DBCC CHECKDB` notifique un error de una página en este estado después de su escritura en el disco. Esto se debe a que la modificación incorrecta podría encontrarse en ubicaciones de la página que no contienen ningún dato ni información importante sobre la estructura de las páginas o las filas, o bien podría tratarse de una modificación en los datos que CHECKDB no puede detectar.  

Puede obtener más información sobre el mensaje 832 en las notas del producto: [Elementos fundamentales de E/S de SQL Server, capítulo 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).