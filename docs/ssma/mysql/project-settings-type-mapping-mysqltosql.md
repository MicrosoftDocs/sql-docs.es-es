---
description: Configuración del proyecto (asignación de tipo) (MySQLToSQL)
title: Configuración del proyecto (asignación de tipo) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 136fdf6d-657f-447b-af41-49bbc6e0e93e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2f3e077a93d90afccf05023e41f8e5f2ea6fabc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017325"
---
# <a name="project-settings-type-mapping-mysqltosql"></a>Configuración del proyecto (asignación de tipo) (MySQLToSQL)
La configuración del proyecto de asignación de tipos permite establecer asignaciones de tipos predeterminadas para el proyecto SSMA.  

La asignación de tipos está disponible en los cuadros de diálogo Configuración del proyecto y configuración predeterminada del proyecto:  
  
-   Utilice el cuadro de diálogo Configuración del proyecto para establecer las opciones de configuración del proyecto actual. Para obtener acceso a la configuración de asignación de tipos, en el menú herramientas, seleccione Configuración del proyecto y, a continuación, haga clic en tipo de asignación en el panel izquierdo.  
  
-   Utilice el cuadro de diálogo Configuración predeterminada del proyecto para establecer las opciones de configuración de todos los proyectos. Para obtener acceso a la configuración de asignación de tipos, en el menú herramientas, seleccione Configuración predeterminada del proyecto, seleccione tipo de proyecto de migración para el que se deben ver los valores de/Changed en la lista desplegable de la **versión de destino** de la migración y, a continuación, haga clic en tipo de asignación en el panel izquierdo.  
  
## <a name="options"></a>Opciones  
  
##### <a name="source-type"></a>Tipo de origen  
Es el tipo de datos MySQL, que debe asignarse al tipo de datos de la base de datos de destino.  
  
##### <a name="target-type"></a>Tipo de destino  
El tipo de datos de la base de datos de destino para el tipo de datos MySQL especificado.  
  
##### <a name="add"></a>Agregar  
Haga clic para agregar un tipo de datos a la lista de asignaciones.  
  
##### <a name="edit"></a>Editar  
Haga clic en esta opción para modificar el tipo de datos seleccionado en la lista asignación.  
  
##### <a name="remove"></a>Remove  
Haga clic en esta opción para quitar la asignación de tipos de datos seleccionada de la lista de asignaciones.  
  
##### <a name="reset-to-default"></a>Valores predeterminados  
Haga clic en esta opción para restablecer la lista asignación de tipos a los valores predeterminados de SSMA.  
  
## <a name="type-mappings"></a>Asignaciones de tipos  
En la tabla siguiente se muestra la asignación predeterminada entre los tipos de datos de origen y de destino.  
  
|Tipo de datos MySQL|Tipo de datos de SQL Server|  
|-|-|  
|bigint|bigint|  
|BIGINT [*.. 255]|bigint|  
|binary|binario [1]|  
|binario [0.. 1]|binario [1]|  
|binario [2.. 255]|binario [*]|  
|bit|binario [1]|  
|bit [0.. 8]|binario [1]|  
|bit [17.. 24]|binario [3]|  
|bit [25.. 32]|binario [4]|  
|bit [33.. 40]|binario [5]|  
|bit [41.. 48]|binario [6]|  
|bit [49.. 56]|binario [7]|  
|bit [57... 64]|binario [8]|  
|bit [9.. 16]|binario [2]|  
|blob|varbinary(max)|  
|BLOB [0.. 1]|varbinary [1]|  
|BLOB [2.. 8000]|varbinary [*]|  
|BLOB [8001.. *]|varbinary(max)|  
|bool|bit|  
|boolean|bit|  
|char|nchar [1]|  
|byte Char|binario [1]|  
|char Byte [0.. 1]|binario [1]|  
|char Byte [2.. 255]|binario [*]|  
|Char [0.. 1]|nchar [1]|  
|Char [2.. 255]|nchar [*]|  
|character|nchar [1]|  
|variable de carácter [0.. 1]|nvarchar [1]|  
|carácter variable [2.. 255]|NVARCHAR|  
|carácter [0.. 1]|nchar [1]|  
|carácter [2.. 255]|nchar [*]|  
|date|date|  
|datetime|datetime2 [0]|  
|dec|Decimal|  
|Dec [*.. 65]|decimal [*] [0]|  
|Dec [*.. 65] [ \* .. 30|decimal [*] [ \* ]|  
|Decimal|Decimal|  
|decimal [*.. 65]|decimal [*] [0]|  
|decimal [*.. 65] [ \* .. 30|decimal [*] [ \* ]|  
|double|Float [53]|  
|double precision|Float [53]|  
|precisión doble [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|Double [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|fijo|NUMERIC|  
|corregido [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|FLOAT|Float [24]|  
|Float [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|Float [*.. 53]|Float [53]|  
|int|int|  
|int [*.. 255]|int|  
|integer|int|  
|entero [*.. 255]|int|  
|longblob|varbinary(max)|  
|longtext|nvarchar(max)|  
|mediumblob|varbinary(max)|  
|mediumint|int|  
|mediumint[*.. 255]|int|  
|mediumtext|nvarchar(max)|  
|carácter nacional|nchar [1]|  
|National Char [0.. 1]|nchar [1]|  
|National Char [2.. 255]|nchar [*]|  
|carácter nacional|nchar [1]|  
|variable de carácter nacional|nvarchar [1]|  
|carácter nacional variable [0.. 1]|nvarchar [1]|  
|carácter nacional variable [2.. 4000]|nvarchar [*]|  
|carácter nacional variable [4001.. *]|nvarchar(max)|  
|carácter nacional [0.. 1]|nchar [1]|  
|carácter nacional [2.. 255]|nchar [*]|  
|VARCHAR nacional|nvarchar [1]|  
|National VARCHAR [0.. 1]|nvarchar [1]|  
|National VARCHAR [2.. 4000]|nvarchar [*]|  
|National VARCHAR [4001.. *]|nvarchar(max)|  
|NCHAR|nchar [1]|  
|nchar VARCHAR|nvarchar [1]|  
|nchar VARCHAR [0.. 1]|nvarchar [1]|  
|nchar VARCHAR [2.. 4000]|nvarchar [*]|  
|nchar VARCHAR [4001.. *]|nvarchar(max)|  
|nchar [0.. 1]|nchar [1]|  
|nchar [2.. 255]|nchar [*]|  
|NUMERIC|NUMERIC|  
|Numeric [*.. 65]|Numeric [*] [0]|  
|Numeric [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|NVARCHAR|nvarchar [1]|  
|nvarchar [0.. 1]|nvarchar [1]|  
|nvarchar [2.. 4000]|nvarchar [*]|  
|nvarchar [4001.. *]|nvarchar(max)|  
|real|Float [53]|  
|real [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|serial|bigint|  
|SMALLINT|SMALLINT|  
|smallint [*.. 255]|SMALLINT|  
|text|nvarchar(max)|  
|texto [0.. 1]|nvarchar [1]|  
|texto [2.. 4000]|nvarchar [*]|  
|texto [4001.. *]|nvarchar(max)|  
|time|time|  
|timestamp|datetime|  
|tinyblob|varbinary [255]|  
|TINYINT|SMALLINT|  
|tinyint [*.. 255]|SMALLINT|  
|tinytext|nvarchar [255]|  
|BIGINT sin signo|bigint|  
|BIGINT sin signo [*.. 255]|bigint|  
|sin signo Dec|Decimal|  
|sin signo Dec [*.. 65]|decimal [*] [0]|  
|sin signo Dec [*.. 65] [ \* .. 30|decimal [*] [ \* ]|  
|decimal sin signo|Decimal|  
|decimal sin signo [*.. 65]|decimal [*] [0]|  
|decimal sin signo [*.. 65] [ \* .. 30|decimal [*] [ \* ]|  
|unsigned Double|Float [53]|  
|doble precisión sin signo|Float [53]|  
|doble precisión sin signo [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|Double sin signo [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|Fixed sin signo|NUMERIC|  
|sin signo fijo [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|unsigned Float|Float [24]|  
|unsigned Float [*.. 255] [ \* .. 30|Numeric [*] [ \* ]|  
|unsigned Float [*.. 53]|Float [53]|  
|unsigned int|bigint|  
|unsigned int [*.. 255]|bigint|  
|entero sin signo|bigint|  
|entero sin signo [*.. 255]|bigint|  
|unsigned MEDIUMINT|int|  
|unsigned MEDIUMINT [*.. 255]|int|  
|numérico sin signo|NUMERIC|  
|numérico sin signo [*.. 65]|Numeric [*] [0]|  
|numérico sin signo [*.. 65] [ \* .. 30|Numeric [*] [ \* ]|  
|real sin signo|Float [53]|  
|real sin signo [*.. 255 [[ \* .. 30|Numeric [*] [ \* ]|  
|smallint sin signo|int|  
|smallint sin signo [*.. 255]|int|  
|tinyint sin signo|TINYINT|  
|unsigned tinyint [*.. 255]|TINYINT|  
|varbinary [0.. 1]|varbinary [1]|  
|varbinary [2.. 8000]|varbinary [*]|  
|varbinary [8001.. *]|varbinary(max)|  
|VARCHAR [0.. 1]|nvarchar [1]|  
|VARCHAR [2.. 4000]|nvarchar [*]|  
|VARCHAR [4001.. *]|nvarchar(max)|  
|year|SMALLINT|  
|año [2.. 2]|SMALLINT|  
|año [4... 4]|SMALLINT|  
  
##### <a name="add"></a>Agregar  
Haga clic para agregar un tipo de datos a la lista de asignaciones.  
  
##### <a name="edit"></a>Editar  
Haga clic para editar un tipo de datos en la lista asignación.  
  
##### <a name="remove"></a>Remove  
Haga clic en esta opción para quitar la asignación de tipos de datos seleccionada de la lista de asignaciones.  
  
##### <a name="reset-to-default"></a>Valores predeterminados  
Haga clic para restablecer todas las asignaciones de tipos de datos a los valores predeterminados de SSMA.  
  
