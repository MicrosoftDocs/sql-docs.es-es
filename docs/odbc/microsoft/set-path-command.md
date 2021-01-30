---
description: Comando de ruta de acceso set
title: Comando establecer ruta de acceso | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET PATH command [ODBC]
ms.assetid: db488d1e-0963-4f45-8c76-a23b9bde9e9d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a97ac5a074116fcaad0477625ea683226abaf76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203929"
---
# <a name="set-path-command"></a>Comando de ruta de acceso set
Especifica una ruta de acceso para las búsquedas de archivos. Para obtener información específica del controlador, consulte la sección Comentarios.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET PATH TO [Path]  
```  
  
## <a name="arguments"></a>Argumentos  
 En [ *ruta de acceso*]  
 Especifica los directorios en los que desea que Visual FoxPro busque. Use comas o punto y coma para separar los directorios.  
  
## <a name="remarks"></a>Observaciones  
 ESTABLECER ruta de acceso permite especificar rutas de búsqueda para otros programas de Visual FoxPro a los que se puede llamar en procedimientos almacenados. ESTABLECER ruta de acceso no cambiará la ruta de acceso del origen de datos especificado para la conexión.  
  
 Problema establezca la ruta de acceso en sin *ruta de acceso* para restaurar la ruta de acceso al directorio o la carpeta predeterminados.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Si se emite SET PATH en un procedimiento almacenado, se omitirán con las siguientes funciones y comandos:  
  
-   Las funciones de catálogo como [SQLTables](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md) y [SQLColumns](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md) omitirán la nueva ruta de acceso y seguirán haciendo referencia a la ruta de acceso especificada por el origen de datos en [SQLPrepare](../../odbc/microsoft/sqlprepare-visual-foxpro-odbc-driver.md) o [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md).  
  
-   Los comandos como SELECT, INSERT, UPDATE, DELETE y CREATE TABLE omitirán la nueva ruta de acceso y seguirán haciendo referencia a la ruta de acceso especificada por el origen de datos en **SQLPrepare** o **SQLExecDirect**.  
  
 Si se emite SET PATH en un procedimiento almacenado y no se vuelve a establecer la ruta de acceso en su estado original, otras conexiones a la base de datos usarán la nueva ruta de acceso (porque SET PATH no tiene el ámbito de las sesiones de datos).  
  
 Si desea crear, seleccionar o actualizar tablas en un directorio distinto del especificado por el origen de datos, especifique la ruta de acceso completa del archivo con el comando.  
  
## <a name="see-also"></a>Consulte también  
 [Cuadro de diálogo Configuración de ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)   
 [SQLColumns (controlador ODBC de Visual FoxPro)](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md)   
 [SQLDriverConnect (controlador ODBC de Visual FoxPro)](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md)   
 [SQLTables (controlador ODBC de Visual FoxPro)](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md)
