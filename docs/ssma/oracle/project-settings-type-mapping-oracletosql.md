---
description: Configuración del proyecto (asignación de tipo) (OracleToSQL)
title: Configuración del proyecto (asignación de tipo) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4bb8466e-2199-4f00-8513-b04e9586723d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: cbb47d74535af0dea97842bdf46680a4376c2ba7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067760"
---
# <a name="project-settings-type-mapping-oracletosql"></a>Configuración del proyecto (asignación de tipo) (OracleToSQL)
La página asignación de tipos del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan el modo en que SSMA convierte los tipos de datos de Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de datos.  
  
La página asignación de tipos está disponible en los cuadros de diálogo **configuración del proyecto** y **configuración predeterminada del proyecto** .  
  
-   Para especificar la configuración de todos los proyectos de SSMA futuros, en el menú **herramientas** , haga clic en **configuración de proyecto predeterminada**, seleccione tipo de proyecto de migración para el que es necesario ver o cambiar la configuración en la lista desplegable de la **versión de destino** de la migración y, a continuación, haga clic en **asignación de tipo** en la parte inferior del panel izquierdo.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , haga clic en **configuración del proyecto** y, a continuación, haga clic en asignación de **tipos** en la parte inferior del panel izquierdo.  
  
Para especificar la configuración para el objeto o la clase actual de objetos, utilice la pestaña **asignación de tipos** de la ventana de SSMA primaria.  
  
## <a name="options"></a>Opciones  
En la tabla siguiente se muestran las opciones de la pestaña **asignación de tipos** :  
  
**Tipo de origen**  
El tipo de datos de Oracle asignado.  
  
**Tipo de destino**  
El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino para el tipo de datos de Oracle especificado.  
  
Vea las tablas de la sección siguiente para obtener las asignaciones predeterminadas de SSMA para Oracle Type.  
  
**Add (Agregar)**  
Haga clic para agregar un tipo de datos a la lista de asignaciones.  
  
**Editar**  
Haga clic en esta opción para modificar el tipo de datos seleccionado en la lista asignación.  
  
**Quitar**  
Haga clic en esta opción para quitar la asignación de tipos de datos seleccionada de la lista de asignaciones.  
  
**Valores predeterminados**  
Haga clic en esta opción para restablecer la lista asignación de tipos a los valores predeterminados de SSMA.  
  
## <a name="default-type-mappings"></a>Asignaciones de tipos predeterminadas  
En SSMA para Oracle, puede establecer asignaciones de tipos personalizadas para argumentos, columnas, variables locales y valores devueltos. La asignación predeterminada para los argumentos y los tipos de valor devuelto es casi idéntica.  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>Tipo de argumento predeterminado y asignación de tipo de valor devuelto  
La tabla siguiente contiene la asignación de tipo de datos predeterminada para los argumentos y los valores devueltos.  
  
|Tipo de datos de Oracle|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo de datos predeterminado|  
|--------------------|-------------------------------------------------------------------------|  
|sobre|varbinary(max)|  
|binary_double|Float [53]|  
|binary_float|Float [53]|  
|binary_integer|int|  
|blob|varbinary(max)|  
|boolean|bit|  
|char|ntext|  
|char varying|ntext|  
|character|ntext|  
|character varying|ntext|  
|CLOB|ntext|  
|date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Decimal|Float [53]|  
|double precision|Float [53]|  
|FLOAT|Float [53]|  
|int|int|  
|integer|int|  
|long|ntext|  
|Long RAW|varbinary(max)|  
|Long RAW [ \* .. 8000]<sup>*</sup>|varbinary [*]|  
|Long RAW [8001.. \* ]<sup>*</sup>|varbinary(max)|  
|carácter nacional|nvarchar(max)|  
|National char varying|nvarchar(max)|  
|carácter nacional|nvarchar(max)|  
|variable de carácter nacional<sup>**</sup>|nvarchar(max)|  
|variable de carácter nacional<sup>*</sup>|nvarchar(max)|  
|NCHAR|nvarchar(max)|  
|NClob|nvarchar(max)|  
|número|Float [53]|  
|NUMERIC|Float [53]|  
|nvarchar2|nvarchar(max)|  
|pls_integer|int|  
|raw|varbinary(max)|  
|real|Float [53]|  
|pseudocolumna|UNIQUEIDENTIFIER|  
|signtype|SMALLINT|  
|SMALLINT|SMALLINT|  
|string|ntext|  
|timestamp|datetime2|  
|marca de tiempo con zona horaria local|datetimeoffset|  
|marca de tiempo con zona horaria|datetimeoffset|  
|urowid|UNIQUEIDENTIFIER|  
|varchar|ntext|  
|VARCHAR2|ntext|  
|XmlType|Xml|  
  
<sup>*</sup> Solo se aplica a la asignación de tipo de valor devuelto.  
  
<sup>**</sup> Solo se aplica a la asignación de tipos de argumento.  
  
### <a name="default-column-type-mapping"></a>Asignación de tipo de columna predeterminada  
La tabla siguiente contiene la asignación de tipos predeterminada para las columnas.  
  
|Tipo de datos de Oracle|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo de datos predeterminado|  
|--------------------|-------------------------------------------------------------------------|  
|sobre|varbinary(max)|  
|binary_double|Float [53]|  
|binary_float|Float [53]|  
|blob|varbinary(max)|  
|char|char|  
|char varying [*.. \* ]|VARCHAR [*]|  
|Char [*.. \* ]|Char [*]|  
|character|char|  
|variación de caracteres [*.. \* ]|VARCHAR [*]|  
|carácter [*.. \* ]|Char [*]|  
|CLOB|ntext|  
|date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [*.. \* ]|Dec [*] [0]|  
|Dec [*.. \* ] [\*..\*]|Dec [*] [ \* ]|  
|Decimal|decimal [38] [0]|  
|decimal [*.. \* ]|decimal [*] [0]|  
|decimal [*.. \* ] [\*..\*]|decimal [*] [ \* ]|  
|double precision|Float [53]|  
|FLOAT|Float [53]|  
|Float [*.. 53]|Float [*]|  
|Float [54.. *]|Float [53]|  
|int|int|  
|integer|int|  
|long|ntext|  
|Long RAW|varbinary(max)|  
|Long RAW [*.. 8000]|varbinary [*]|  
|Long RAW [8001.. *]|varbinary(max)|  
|long varchar|ntext|  
|Long [*.. 8000]|VARCHAR [*]|  
|Long [8001.. *]|ntext|  
|carácter nacional|NCHAR|  
|National char varying [*.. \* ]|nvarchar [*]|  
|National Char [*.. \* ]|nchar [*]|  
|carácter nacional|NCHAR|  
|variable Nacional de caracteres [*.. \* ]|nvarchar [*]|  
|carácter nacional [*.. \* ]|nchar [*]|  
|NCHAR|NCHAR|  
|nchar [*]|nchar [*]|  
|NClob|nvarchar(max)|  
|número|Float [53]|  
|número [*.. \* ]|Numeric [*]|  
|número [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|NUMERIC|NUMERIC|  
|Numeric [*.. \* ]|Numeric [*]|  
|Numeric [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|NVARCHAR2 [*... \* ]|nvarchar [*]|  
|sin formato [*.. \* ]|varbinary [*]|  
|real|Float [53]|  
|pseudocolumna|UNIQUEIDENTIFIER|  
|SMALLINT|SMALLINT|  
|timestamp|datetime2|  
|marca de tiempo con zona horaria local|datetimeoffset|  
|marca de tiempo con zona horaria local [*.. \* ]|DateTimeOffset [*]|  
|marca de tiempo con zona horaria|datetimeoffset|  
|marca de tiempo con zona horaria [*.. \* ]|DateTimeOffset [*]|  
|marca de tiempo [*.. \* ]|datetime2 [*]|  
|Urowid|UNIQUEIDENTIFIER|  
|urowid [*... \* ]|UNIQUEIDENTIFIER|  
|VARCHAR [*.. \* ]|VARCHAR [*]|  
|VARCHAR2 [*.. \* ]|VARCHAR [*]|  
|XmlType|Xml|  
  
### <a name="default-local-variable-type-mapping"></a>Asignación de tipo de variable local predeterminada  
La tabla siguiente contiene la asignación de tipos predeterminada para las variables locales.  
  
|Tipo de datos de Oracle|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Tipo de datos predeterminado|  
|--------------------|-------------------------------------------------------------------------|  
|Bfile|varbinary(max)|  
|binary_double|Float [53]|  
|binary_float|Float [53]|  
|binary_interger|int|  
|Blob|varbinary(max)|  
|Boolean|bit|  
|Char|char|  
|char varying [*.. 8000]|VARCHAR [*]|  
|char varying [8001.. *]|ntext|  
|Char [*.. 8000]|Char [*]|  
|Char [8001.. *]|ntext|  
|Carácter|char|  
|variable de carácter [*.. 8000]|VARCHAR [*]|  
|variación de caracteres [8001.. *]|ntext|  
|carácter [*.. 8000]|Char [*]|  
|carácter [8001.. *]|ntext|  
|CLOB|ntext|  
|date|datetime2 [0]|  
|dec|Dec [38] [0]|  
|Dec [*.. \* ]|Dec [*] [0]|  
|Dec [*.. \* ] [\*..\*]|Dec [*] [ \* ]|  
|Decimal|decimal [38] [0]|  
|decimal [*.. \* ]|decimal [*] [0]|  
|decimal [*.. \* ] [\*..\*]|decimal [*] [ \* ]|  
|double precision|Float [53]|  
|Float|Float [53]|  
|Float [*.. 53]|Float [*]|  
|Float [54.. *]|Float [53]|  
|Int|int|  
|Entero|int|  
|entero [*.. \* ]|Numeric [*] [0]|  
|long|ntext|  
|Long RAW|varbinary(max)|  
|Long RAW [*.. 8000]|varbinary [*]|  
|Long RAW [8001.. *]|varbinary(max)|  
|carácter nacional|NCHAR|  
|National char varying [*.. 4000]|nvarchar [*]|  
|National char varying [4001.. *]|nvarchar(max)|  
|National Char [*.. 4000]|nchar [*]|  
|National Char [4001.. *]|nvarchar(max)|  
|carácter nacional|NCHAR|  
|carácter nacional [*.. 4000]|nvarchar [*]|  
|carácter nacional [4001.. *]|nvarchar(max)|  
|variable Nacional de caracteres [*.. 4000]|nvarchar [*]|  
|variación de carácter nacional [4001.. *]|nvarchar(max)|  
|Nchar|NCHAR|  
|nchar [*.. 4000]|nchar [*]|  
|nchar [4001.. *]|nvarchar(max)|  
|nchar varying [*.. 4000]|nvarchar [*]|  
|nchar varying [4001.. *]|nvarchar(max)|  
|NClob|nvarchar(max)|  
|Number|Float [53]|  
|número [*.. \* ]|Numeric [*]|  
|número [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|Numeric|Numeric [38] [0]|  
|Numeric [*.. \* ]|Numeric [*]|  
|Numeric [*.. \* ] [\*..\*]|Numeric [*] [ \* ]|  
|nvarchar2[*.. 4000]|nvarchar [*]|  
|NVARCHAR2 [4001.. *]|nvarchar(max)|  
|pls_integer|int|  
|sin formato [*.. 8000]|varbinary [*]|  
|sin formato [8001.. *]|varbinary(max)|  
|Real|Float [53]|  
|Rowid|UNIQUEIDENTIFIER|  
|Signtype|SMALLINT|  
|Smallint|SMALLINT|  
|cadena [*.. 8000]|VARCHAR [*]|  
|cadena [8001.. *]|ntext|  
|timestamp|datetime2|  
|marca de tiempo con zona horaria local|datetimeoffset|  
|marca de tiempo con zona horaria|datetimeoffset|  
|marca de tiempo con zona horaria local [*.. \* ]|DateTimeOffset [*]|  
|marca de tiempo con zona horaria [*.. \* ]|DateTimeOffset [*]|  
|marca de tiempo [*.. \* ]|datetime2 [*]|  
|Urowid|UNIQUEIDENTIFIER|  
|urowid [*... \* ]|UNIQUEIDENTIFIER|  
|VARCHAR [*.. 8000]|VARCHAR [*]|  
|VARCHAR [8001.. *]|ntext|  
|VARCHAR2 [*.. 8000]|VARCHAR [*]|  
|VARCHAR2 [8001.. *]|varcha (Max)|  
|XmlType|Xml|  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
