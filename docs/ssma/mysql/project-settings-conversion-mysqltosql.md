---
description: Configuración del proyecto (conversión) (MySQLToSQL)
title: Configuración del proyecto (conversión) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7ad5fe44-6445-4ba8-a457-5af792631f11
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7cdbc6ac65fa24f8130bc92c03b62145370bedea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017425"
---
# <a name="project-settings-conversion-mysqltosql"></a>Configuración del proyecto (conversión) (MySQLToSQL)
La página conversión del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan cómo SSMA convierte la sintaxis de MySQL en SQL Server o SQL Azure sintaxis.  
  
El panel conversión está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** .  
  
-   Utilice el cuadro de diálogo **configuración predeterminada del proyecto** para establecer las opciones de configuración de todos los proyectos. Para obtener acceso a la configuración de conversión, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione el tipo de proyecto de migración para el que se deben ver los valores de/Changed en la lista desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione **conversión**.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , haga clic en **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **conversión**.  
  
## <a name="options"></a>Opciones  
  
### <a name="collate-clause"></a>Cláusula COLLATE  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Conversión de la cláusula de intercalación explícita**|La opción de conversión de la cláusula de intercalación explícita especifica cómo convertir cláusulas de intercalación explícitas en el código de MySQL. Posibles opciones: omitir y marcar con una advertencia/generar un error<br /><br />**Modo predeterminado**: omitir y marcar con una advertencia<br /><br />**Modo optimista**: omitir y marcar con una advertencia<br /><br />**Modo completo**: omitir y marcar con una advertencia|  
  
### <a name="column-constraints"></a>Restricciones de columna  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Generar restricción para columnas de tipo de datos ENUM**|Genera una restricción para las columnas de tipo de datos ENUM en el SQL Server o SQL Azure tabla, si no está presente en la tabla MySQL. En caso afirmativo, todas las columnas convertidas de tipo de datos ENUM irán acompañadas de la restricción CHECK que controla el valor.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Generar restricción para columnas de tipo de datos SET**|Genera una restricción para las columnas de tipo de datos SET en el SQL Server o SQL Azure tabla, si no está presente en la tabla MySQL. En caso afirmativo, todas las columnas convertidas de tipo de datos SET irán acompañadas de la restricción CHECK que controla el valor.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Generar restricción para columnas de columnas de tipos de datos numéricos sin signo**|Agregue una comprobación de valor no negativo a las columnas de tipos de datos numéricos sin signo.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Generar restricción para columnas de tipo de datos YEAR**|Genera una restricción para las columnas de tipo de datos YEAR en el SQL Server o SQL Azure tabla, si no está presente en la tabla MySQL. En caso afirmativo, todas las columnas convertidas del tipo de datos YEAR irán acompañadas de la restricción CHECK que controla el valor.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
  
### <a name="data-types"></a>Tipo de datos  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Conversión de tipos de datos de ENUMERAción**|Especifica cómo se debe convertir el tipo de datos de ENUMERAción de MySQL como convertir en NVARCHAR o convertir en numérico.<br /><br />**Modo predeterminado**: convertir a nvarchar<br /><br />**Modo optimista**: convertir a nvarchar<br /><br />**Modo completo**: convertir a nvarchar|  
|**ESTABLECER conversión de tipos de datos**|Especifica cómo se debe convertir el tipo de datos SET de MySQL, convertir a NVARCHAR (L)/Convert en BINARIo (L)<br /><br />**Modo predeterminado**: convertir a nvarchar (L)<br /><br />**Modo optimista**: convertir a nvarchar (L)<br /><br />**Modo completo**: convertir a nvarchar (L)|  
  
### <a name="generic"></a>Genérico  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Columnas sin valor predeterminado en INSERT y Replace**|Si es "Yes", todas las instrucciones que hacen referencia a tablas que usan motores almacenados distintos de MyISAM y InnoDb deben marcarse con mensajes de conversión de advertencia.<br /><br />**Modo predeterminado**: agregar a lista de columnas<br /><br />**Modo optimista**: agregar a lista de columnas<br /><br />**Modo completo**: agregar a lista de columnas|  
|**La conversión de división por cero produce**|Especifica si se emula MySQL sin ERROR_FOR_DIVISION_BY_ZERO comportamiento.<br /><br />**Modo predeterminado**: error<br /><br />**Modo optimista**: error<br /><br />**Modo completo**: null|  
|**IN (operador)**|Especifica cómo convertir MySQL en el operador.<br /><br />**Modo predeterminado**: convertir siempre en en<br /><br />**Modo optimista**: convertir siempre en en<br /><br />**Modo completo**: expandir si es necesario|  
|**Conversión de funciones de MySQL**|Especifica cómo convertir las funciones estándar de MySQL.<br /><br />**Modo predeterminado**: optimista<br /><br />**Modo optimista**: optimista<br /><br />**Modo completo**: preciso|  
|**Motores de almacenamiento no admitidos**|Si es "Yes", todas las instrucciones que hacen referencia a tablas que usan motores almacenados distintos de MyISAM y InnoDb deben marcarse con mensajes de conversión de advertencia.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Suprimir la generación de columna auxiliar ROWID**|En caso afirmativo, prohíbe la creación de la creación de la columna auxiliar ROWD en las tablas de destino. Puede afectar a la migración de algunas estructuras.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: no|  
|**TRUNCAte (conversión de instrucción)**|Especifica cómo convertir instrucciones TRUNCAte.<br /><br />**Modo predeterminado**: TRUNCATE<br /><br />**Modo optimista**: TRUNCATE<br /><br />**Modo completo**: TRUNCATE|  
  
### <a name="misc"></a>Varios  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Asignación de esquemas predeterminada**|Especifica cómo asignar las bases de datos MySQL a esquemas de SQL Server.<br /><br />**Modo predeterminado**: base de datos en base de datos<br /><br />**Modo optimista**: base de datos en base de datos<br /><br />**Modo completo**: base de datos en base de datos|  
  
### <a name="procedures-and-functions"></a>Procedimientos y funciones  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Conversión de función predeterminada**|Especifica si las funciones deben convertirse de forma predeterminada en funciones de T-SQL o en procedimientos almacenados.<br /><br />**Modo predeterminado**: convertir a función<br /><br />**Modo optimista**: convertir a función<br /><br />**Modo completo**: convertir a función|  
|**Generar XACT_ABORT SET activado**|Especifica si se debe agregar o no XACT_ABORT del conjunto al principio del procedimiento o desencadenador convertido.<br /><br />**Modo predeterminado**: sí<br /><br />**Modo optimista**: sí<br /><br />**Modo completo**: sí|  
|**Generar SET NOCOUNT ON**|Especifica si se debe agregar o no SET NOCOUNT ON al principio del procedimiento o desencadenador convertido.<br /><br />**Modo predeterminado**: sí<br /><br />**Modo optimista**: sí<br /><br />**Modo completo**: sí|  
  
### <a name="spatial-data-types"></a>Tipos de datos espaciales  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Cuadro de límite predeterminado {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} para los índices espaciales**|Define el valor predeterminado para el parámetro {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} del cuadro de límite usado en los índices espaciales.<br /><br />**Modo predeterminado**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0<br /><br />**Modo optimista**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0<br /><br />**Modo completo**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0|  
|**Densidad de cuadrícula predeterminada para los índices espaciales**|Define el valor predeterminado para LEVEL_1, LEVEL_2, LEVEL_3 y LEVEL_4 de la densidad de la cuadrícula utilizada en los índices espaciales.<br /><br />**Modo predeterminado**<br /><br />LEVEL_1: valor predeterminado<br /><br />LEVEL_2: valor predeterminado<br /><br />LEVEL_3: valor predeterminado<br /><br />LEVEL_4: valor predeterminado<br /><br />**Modo optimista**<br /><br />LEVEL_1: valor predeterminado<br /><br />LEVEL_2: valor predeterminado<br /><br />LEVEL_3: valor predeterminado<br /><br />LEVEL_4: valor predeterminado<br /><br />**Modo completo**<br /><br />LEVEL_1: valor predeterminado<br /><br />LEVEL_2: valor predeterminado<br /><br />LEVEL_3: valor predeterminado<br /><br />LEVEL_4: valor predeterminado|  
  
### <a name="transactions"></a>Transacciones  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Tablas no transaccionales**|Especifica si todas las referencias a la tabla que no admiten transacciones deben marcarse con mensajes de conversión de advertencia.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Nivel de aislamiento de transacción**|Especifica el nivel de aislamiento de transacción que se debe usar para las nuevas transacciones.<br /><br />**Modo predeterminado**: predeterminado<br /><br />**Modo optimista**: predeterminado<br /><br />**Modo completo**: lectura repetible|  
  
### <a name="value-control"></a>Control de valor  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Conversión de caracteres a numéricos**|Especifica cómo controlar la conversión implícita y explícita del tipo de datos de caracteres a tipos de datos numéricos.<br /><br />**Modo predeterminado**: optimista<br /><br />**Modo optimista**: optimista<br /><br />**Modo completo**: preciso|  
|**Controlar valores numéricos sin signo**|Controlar la asignación de valores a variables numéricas y parámetros sin signo.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: sí|  
|**Control de resta sin signo**|Modifique los valores negativos insertados en columnas de tabla de tipo de valor sin signo.<br /><br />**Modo predeterminado**: convertir ' as-is '<br /><br />**Modo optimista**: convertir "tal cual"<br /><br />**Modo completo**: marca con una advertencia|  
|**Conversión a y desde el tipo de datos binary**|Especifica cómo controlar la conversión implícita y explícita del tipo de datos binario.<br /><br />**Modo predeterminado**: optimista<br /><br />**Modo optimista**: optimista<br /><br />**Modo completo**: preciso|  
|**Conversión al tipo de datos de fecha y hora**|Especifica cómo controlar la conversión implícita y explícita al tipo de datos de fecha y hora.<br /><br />**Modo predeterminado**: emular formato MySQL<br /><br />**Modo optimista**: usar formato de SQL Server<br /><br />**Modo completo**: emular el formato MySQL|  
|**Literales numéricos con una precisión superior a 38**|Especifica cómo convertir literales numéricos con una precisión superior a 38.<br /><br />**Modo predeterminado**: Round si es posible<br /><br />**Modo optimista**: Round si es posible<br /><br />**Modo completo**: Round si es posible|  
|**Fecha cero en columnas NOT NULL**|Especifica cómo controlar la asignación a columnas NOT NULL de valores de fecha y hora de cero, cero o no válidos.<br /><br />**Modo predeterminado**: getDate ()<br /><br />**Modo optimista**: getDate ()<br /><br />**Modo completo**: getDate ()|  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
  
