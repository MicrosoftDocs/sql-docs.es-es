---
description: 'Apéndice F: Biblioteca de cursores ODBC'
title: 'Apéndice F: biblioteca de cursores ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- cursor library [ODBC]
ms.assetid: a03084df-4e48-48ef-917d-4a3fae48a605
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44fcba1e4c4b2b7f82c99c82399884d7c9bdad85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212525"
---
# <a name="appendix-f-odbc-cursor-library"></a>Apéndice F: Biblioteca de cursores ODBC
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores ODBC (Odbccr32.dll) admite cursores desplazables en bloque para cualquier controlador que cumpla con el nivel de conformidad de la API de nivel 1 y que los desarrolladores puedan redistribuir con sus aplicaciones o controladores. La biblioteca de cursores también admite instrucciones Update y DELETE posicionadas para los conjuntos de resultados generados por las instrucciones **Select** . Aunque solo admite cursores estáticos y de solo avance, la biblioteca de cursores satisface las necesidades de muchas aplicaciones. Además, puede proporcionar un buen rendimiento, especialmente para los conjuntos de resultados de tamaño pequeño a medio, y para las aplicaciones que no tienen una buena compatibilidad con los cursores.  
  
 La biblioteca de cursores es una biblioteca de vínculos dinámicos (DLL) que reside entre el administrador de controladores y el controlador. Cuando una aplicación llama a una función, el administrador de controladores llama a la función en la biblioteca de cursores, que ejecuta la función o la llama en el controlador especificado. Para una conexión determinada, una aplicación especifica si se utiliza siempre la biblioteca de cursores, que se usa si el controlador no admite cursores desplazables, o si no se usa nunca.  
  
 La biblioteca de cursores aparece como un controlador para el administrador de controladores. Si la biblioteca de cursores reside entre el administrador de controladores y un controlador ODBC *2. x* , la biblioteca de cursores aparece como un controlador ODBC *2. x* . Si la biblioteca de cursores reside entre el administrador de controladores y un controlador ODBC *3. x* , la biblioteca de cursores aparece como un controlador ODBC *3. x* . El comportamiento que exhibe la biblioteca de cursores depende de la versión del controlador con el que está trabajando, con la excepción de los desplazamientos de enlace, que es compatible con los controladores ODBC *2. x* y ODBC *3. x* .  
  
 Para implementar cursores de bloque en **SQLFetch** y **SQLFetchScroll**, la biblioteca de cursores llama repetidamente a **SQLFetch** en el controlador. Para implementar el desplazamiento, almacena en caché los datos que se han recuperado en la memoria y en los archivos de disco. Cuando una aplicación solicita un conjunto de filas nuevo, la biblioteca de cursores lo recupera según sea necesario desde el controlador o la memoria caché.  
  
 Para implementar instrucciones Update y DELETE posicionadas, la biblioteca de cursores crea una instrucción **Update** o **Delete** con una cláusula **Where** que especifica el valor almacenado en caché de cada columna enlazada en la fila. Cuando ejecuta una instrucción UPDATE posicionada, la biblioteca de cursores actualiza su memoria caché a partir de los valores de los búferes del conjunto de filas.  
  
 Para obtener más información acerca de la biblioteca de cursores ODBC, consulte las siguientes secciones de este apéndice:  
  
-   [Uso de la biblioteca de cursores ODBC](../../../odbc/reference/appendixes/using-the-odbc-cursor-library.md)  
  
-   [Ejecución de instrucciones de eliminación y actualización Positioned](../../../odbc/reference/appendixes/executing-positioned-update-and-delete-statements.md)  
  
-   [Ejemplo de código de la biblioteca de cursores](../../../odbc/reference/appendixes/cursor-library-code-example.md)  
  
-   [Notas de implementación](../../../odbc/reference/appendixes/implementation-notes.md)  
  
-   [Códigos de error de la biblioteca de cursores ODBC](../../../odbc/reference/appendixes/odbc-cursor-library-error-codes.md)
