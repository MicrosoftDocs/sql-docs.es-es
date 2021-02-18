---
title: Ventana de puntos de interrupción
description: Obtenga información sobre cómo usar la ventana Puntos de interrupción del Editor de consultas del motor de base de datos para administrar los puntos de interrupción del depurador de Transact-SQL.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Breakpoints Window [Transact-SQL]
ms.assetid: bad88d10-fdd5-4d3d-b5ea-a4f063847485
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/22/2020
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: a9de728e7b50f27558e53b9b4db2fbcda1ed6e1e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340672"
---
# <a name="transact-sql-debugger---breakpoints-window"></a>Depurador de Transact-SQL: ventana Puntos de interrupción

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La ventana **Puntos de interrupción** muestra todos los puntos de interrupción que se establecen en el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] actual. Para administrar los puntos de interrupción, utilice la barra de herramientas de la ventana **Puntos de interrupción** . Los puntos de interrupción son las ubicaciones del código en las que la ejecución se pausa en modo de depuración para que se puedan ver los datos de depuración.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Lista de tareas

**Para tener acceso a la ventana Puntos de interrupción**

- En el menú **Depurar**, seleccione **Ventanas**,y seleccione **Puntos de interrupción**.

## <a name="breakpoints-window-columns"></a>Columnas de la ventana Puntos de interrupción

De forma predeterminada, la ventana **Puntos de interrupción** muestra las columnas siguientes.  

**Nombre**  
Muestra el nombre del punto de interrupción. El depurador proporciona los nombres para los puntos de interrupción. Este nombre incluye el de la ventana Editor de consultas del motor de base de datos que contiene el punto de interrupción y el número de línea del Editor de consultas en el que está establecido.  

**Condition**  
Muestra **(sin condición)** . El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] no permite establecer las condiciones de punto de interrupción.

**Número de llamadas**  
Muestra **interrumpe siempre**.

Puede agregar y quitar las columnas siguientes seleccionándolos en la lista **Columnas** .  

**Filter**  
Muestra **(ninguno)** . El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] no permite establecer los filtros de punto de interrupción.

**Al visitar**  
Muestra **Interrumpir**.

**Lenguaje**  
Muestra **Transact-SQL** para [!INCLUDE[tsql](../../includes/tsql-md.md)].  

**Function**  
Muestra el número de la línea en la que se establece el punto de interrupción.  

**Archivo**  
Muestra el nombre del archivo de código fuente que contiene el punto de interrupción y el número de la línea en la que se establece.

**Dirección**  
El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] no admite esta característica.  

**Proceso**  
Muestra **[SQL]** para indicar que se trata de un proceso de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . A continuación aparece el nombre de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en la que se ejecuta el código.

## <a name="breakpoints-window-toolbar"></a>Barra de herramientas de la ventana Puntos de interrupción

Cuando la ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] actual tiene puntos de interrupción activos, la ventana **Puntos de interrupción** muestra una barra de herramientas que se puede utilizar para administrarlos.

**Eliminar**  
Elimina el punto de interrupción seleccionado.

**Eliminar todos los puntos de interrupción**  
Elimina todos los puntos de interrupción que se muestran en la ventana **Puntos de interrupción** .  

**Deshabilitar todos los puntos de interrupción**  
Deshabilita todos los puntos de interrupción para que ya no detengan la ejecución del código; sin embargo, permanecen. Cuando todos los puntos de interrupción están deshabilitados, este botón se convierte en **Habilitar todos los puntos de interrupción**.

**Habilitar todos los puntos de interrupción**  
Habilita todos los puntos de interrupción para que detengan la ejecución del código. Cuando todos los puntos de interrupción están habilitados, este botón se convierte en **Deshabilitar todos los puntos de interrupción**.  

**Ir a código fuente**  
Coloca el cursor en la línea del Editor de consultas que contiene el punto de interrupción seleccionado.

**Columnas**  
Muestra todas las columnas que se pueden mostrar en la ventana **Puntos de interrupción** . Una casilla indica las columnas que se muestran. Para agregar o quitar una columna en la ventana **Puntos de interrupción** , seleccione la columna en la lista.

## <a name="see-also"></a>Consulte también

[Depurador de Transact-SQL](./transact-sql-debugger.md)