---
description: Cambios de comportamiento y controladores ODBC 3.x
title: Cambios de comportamiento y controladores ODBC 3. x | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_attr_odbc_version [ODBC]
- backward compatibility [ODBC], behavioral changes
- compatibility [ODBC], behavioral changes
ms.assetid: 88a503cc-bff7-42d9-83ff-8e232109ed06
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a76e950ad8c929d701a3c967b3c5f5422fd3db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212502"
---
# <a name="behavioral-changes-and-odbc-3x-drivers"></a>Cambios de comportamiento y controladores ODBC 3.x
El atributo Environment SQL_ATTR_ODBC_VERSION indica al controlador si necesita mostrar el comportamiento de ODBC *2. x* o el comportamiento de ODBC *3. x* . La forma en que se establece el atributo de entorno de SQL_ATTR_ODBC_VERSION depende de la aplicación. Las aplicaciones ODBC *3. x* deben llamar a **SQLSetEnvAttr** para establecer este atributo después de llamar a **SQLAllocHandle** para asignar un identificador de entorno y antes de llamar a **SQLAllocHandle** para asignar un identificador de conexión. Si no lo hacen, el administrador de controladores devuelve SQLSTATE HY010 (error de secuencia de función) en la última llamada a **SQLAllocHandle**.  
  
> [!NOTE]  
>  Para obtener más información sobre los cambios de comportamiento y cómo actúa una aplicación, vea [cambios de comportamiento](../../../odbc/reference/develop-app/behavioral-changes.md).  
  
 Las aplicaciones ODBC *2. x* y las aplicaciones ODBC *2. x* que se vuelven a compilar con los archivos de encabezado ODBC *3. x* no llaman a **SQLSetEnvAttr**. Sin embargo, llaman a **SQLAllocEnv** en lugar de **SQLAllocHandle** para asignar un identificador de entorno. Por lo tanto, cuando la aplicación llama a **SQLAllocEnv** en el administrador de controladores, el administrador de controladores llama a **SQLAllocHandle** y **SQLSetEnvAttr** en el controlador. Por lo tanto, los controladores ODBC *3. x* siempre pueden contar en este atributo que se establece.  
  
 Si una aplicación compatible con los estándares compilada con la marca de compilar ODBC_STD llama a **SQLAllocEnv** (que puede producirse porque **SQLAllocEnv** no está en desuso en ISO), la llamada se asigna a **SQLAllocHandleStd** en tiempo de compilación. En tiempo de ejecución, la aplicación llama a **SQLAllocHandleStd**. El administrador de controladores establece el atributo de entorno SQL_ATTR_ODBC_VERSION en SQL_OV_ODBC3. Una llamada a **SQLAllocHandleStd** es equivalente a una llamada a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV y una llamada a **SQLSetEnvAttr** para establecer SQL_ATTR_ODBC_VERSION en SQL_OV_ODBC3.  
  
 En algunas arquitecturas de controladores, existe la necesidad de que el controlador aparezca como un controlador ODBC *2. x* o un controlador ODBC *3. x* , dependiendo de la conexión. En este caso, es posible que el controlador no sea realmente un controlador, sino una capa que resida entre el administrador de controladores y otro controlador. Por ejemplo, podría imitar un controlador, como ODBC Spy. En otro ejemplo, podría actuar como puerta de enlace, como EDA/SQL. Para que aparezca como un controlador ODBC *3. x* , este tipo de controlador debe ser capaz de exportar **SQLAllocHandle** y mostrarse como un controlador ODBC *2. x* , debe poder exportar **SQLAllocConnect**, **SQLAllocEnv** y **SQLAllocStmt**. Cuando se asigna un entorno, una conexión o una instrucción, el administrador de controladores comprueba para ver si este controlador exporta **SQLAllocHandle**. Como lo hace el controlador, el administrador de controladores llama a **SQLAllocHandle** en el controlador. Si el controlador está trabajando con un controlador ODBC *2. x* , el controlador debe asignar la llamada a **SQLAllocHandle** a **SQLAllocConnect**, **SQLAllocEnv** o **SQLAllocStmt**, según corresponda. Tampoco debe hacer nada con la llamada de **SQLSetEnvAttr** cuando se comporta como un controlador ODBC *2. x* .  
  
 Esta sección contiene los temas siguientes.  
  
-   [Tipos de datos de fecha y hora](../../../odbc/reference/appendixes/datetime-data-types.md)  
  
-   [Compatibilidad con versiones anteriores de los tipos de datos de C](../../../odbc/reference/appendixes/backward-compatibility-of-c-data-types.md)  
  
-   [Marcadores de longitud fija](../../../odbc/reference/appendixes/fixed-length-bookmarks.md)  
  
-   [Compatibilidad con SQLGetInfo](../../../odbc/reference/appendixes/sqlgetinfo-support.md)  
  
-   [Devuelve SQL_NO_DATA](../../../odbc/reference/appendixes/returning-sql-no-data.md)  
  
-   [Llamar a SQLSetPos para insertar datos](../../../odbc/reference/appendixes/calling-sqlsetpos-to-insert-data.md)  
  
-   [Cargar por ordinal](../../../odbc/reference/appendixes/loading-by-ordinal.md)
