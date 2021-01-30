---
description: sp_tableoption (Transact-SQL)
title: sp_tableoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tableoption_TSQL
- sp_tableoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tableoption
ms.assetid: 0a57462c-1057-4c7d-bce3-852cc898341d
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d2846b289c31777adec6d548addba11a9c7e463d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204336"
---
# <a name="sp_tableoption-transact-sql"></a>sp_tableoption (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Establece los valores de las opciones de las tablas definidas por el usuario. sp_tableoption puede usarse para controlar el comportamiento consecutivo de las tablas con columnas **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)**, **XML**, **Text**, **ntext**, **Image** o Large-defined.  
  
> [!IMPORTANT]  
>  La característica text in row se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para almacenar datos de valores grandes, se recomienda usar los tipos de datos **VARCHAR (Max)**, **nvarchar (Max)** y **varbinary (Max** ).  
  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_tableoption [ @TableNamePattern = ] 'table'   
     , [ @OptionName = ] 'option_name'   
     ,[ @OptionValue =] 'value'  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @TableNamePattern =] '*tabla*'  
 Es el nombre completo o no completo de una tabla de base de datos definida por un usuario. Si se proporciona un nombre de tabla completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el nombre de la base de datos actual. No se pueden establecer opciones para varias tablas al mismo tiempo. *TABLE* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
 [ @OptionName =] '*option_name*'  
 Es un nombre de opción de tabla. *option_name* es de tipo **VARCHAR (35)** y no tiene ningún valor predeterminado de NULL. *option_name* puede ser uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|table lock on bulk load|Cuando está deshabilitado (valor predeterminado), hace que los procesos de carga masiva en tablas definidas por el usuario obtengan bloqueos de fila. Cuando está habilitado, hace que los procesos de carga masiva en tablas definidas por el usuario obtengan un bloqueo de actualización masiva.|  
|insert row lock|Ya no se admite.<br /><br /> Esta opción no afecta al comportamiento de bloqueo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y solo está incluida para mantener la compatibilidad con los scripts y procedimientos existentes.|  
|text in row|Cuando es OFF o 0 (deshabilitado, valor predeterminado), no cambia el comportamiento actual, y no existe ningún BLOB almacenado en fila de manera consecutiva.<br /><br /> Cuando se especifica y @OptionValue está activado (habilitado) o un valor entero comprendido entre 24 y 7000, las nuevas cadenas **Text**, **ntext** o **Image** se almacenan directamente en la fila de datos. Todos los BLOBs existentes (objetos binarios grandes: datos **Text**, **ntext** o **Image** ) se cambiarán al formato text in row cuando se actualice el valor del BLOB. Para obtener más información, vea la sección Comentarios.|  
|large value types out of row|1 = **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)**, **XML** y las columnas de tipo definido por el usuario (UDT) grandes de la tabla se almacenan de una fila, con un puntero de 16 bytes a la raíz.<br /><br /> 0 = **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)**, **XML** y valores UDT grandes se almacenan directamente en la fila de datos, hasta un límite de 8000 bytes y siempre que el valor quepa en el registro. Si el valor no cabe en el registro, se almacena un puntero en la fila de manera consecutiva y el resto se almacena de forma no consecutiva en el espacio de almacenamiento de LOB. El valor predeterminado es 0.<br /><br /> El tipo definido por el usuario (UDT) de gran tamaño se aplica a: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores. <br /><br /> Utilice la opción TEXTIMAGE_ON de [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) para especificar una ubicación para el almacenamiento de tipos de datos de gran tamaño. |  
|vardecimal storage format|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Cuando es TRUE, ON o 1, la tabla designada se habilita para el formato de almacenamiento vardecimal. Cuando es FALSE, OFF o 0, la tabla no se habilita para el formato de almacenamiento vardecimal. El formato de almacenamiento vardecimal solo se puede habilitar cuando la base de datos se ha habilitado para el formato de almacenamiento vardecimal mediante [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md). En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, el formato de almacenamiento **vardecimal** está en desuso. En su lugar, use la compresión de fila. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md). El valor predeterminado es 0.|  
  
 [ @OptionValue =] '*valor*'  
 Indica si el *option_name* está habilitado (true, on o 1) o deshabilitado (false, OFF o 0). el *valor* es **VARCHAR (12)** y no tiene ningún valor predeterminado. el *valor* no distingue mayúsculas de minúsculas.  
  
 Para la opción text in row, los valores válidos son 0, ON, OFF o un entero comprendido entre 24 y 7000. Cuando el *valor* es on, el valor predeterminado del límite es 256 bytes.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o número de error (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_tableoption se puede utilizar únicamente para definir valores de opción de tablas definidas por el usuario. Para mostrar las propiedades de la tabla, use OBJECTPROPERTY o consulte sys. tables.  
  
 La opción text in row en sp_tableoption puede habilitarse o deshabilitarse solo en tablas que contengan columnas de texto. Si la tabla no contiene una columna de texto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un error.  
  
 Cuando la opción text in row está habilitada, el @OptionValue parámetro permite a los usuarios especificar el tamaño máximo que se va a almacenar en una fila de un BLOB. El valor predeterminado es 256 bytes, pero los valores pueden oscilar entre 24 y 7.000 bytes.  
  
 las cadenas **Text**, **ntext** o **Image** se almacenan en la fila de datos si se cumplen las condiciones siguientes:  
  
-   La opción text in row está habilitada.  
  
-   La longitud de la cadena es menor que el límite especificado en @OptionValue  
  
-   Hay suficiente espacio disponible en la fila de datos.  
  
 Cuando se almacenan cadenas BLOB en la fila de datos, leer y escribir las cadenas **Text**, **ntext** o **Image** puede ser tan rápido como leer o escribir cadenas binarias y de caracteres. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no necesita obtener acceso a páginas independientes para leer o escribir la cadena BLOB.  
  
 Si una cadena de tipo **Text**, **ntext** o **Image** es mayor que el límite especificado o el espacio disponible en la fila, los punteros se almacenan en la fila en su lugar. Las condiciones para almacenar las cadenas BLOB en la fila siguen siendo válidas aunque debe haber espacio suficiente para almacenar los punteros en la fila de datos.  
  
 Los punteros y cadenas BLOB almacenados en la fila de una tabla se tratan de forma parecida a las cadenas de longitud variable. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo utiliza el número de bytes necesario para almacenar la cadena o el puntero.  
  
 Las cadenas BLOB existentes no se convierten inmediatamente cuando text in row se habilita por primera vez. Las cadenas solo se convierten cuando se actualizan. Del mismo modo, cuando se aumenta el límite de la opción text in row, las cadenas **Text**, **ntext** o **Image** que ya están en la fila de datos no se convertirán para adherirse al nuevo límite hasta el momento en que se actualicen.  
  
> [!NOTE]  
>  Para deshabilitar la opción text in row o reducir el límite de la opción será necesario realizar la conversión de todos los BLOB; el proceso puede ser largo en función del número de cadenas BLOB que deban convertirse. La tabla se bloquea durante el proceso de conversión.  
  
 Una variable de tabla, incluida una función que devuelve una variable de tabla, tiene habilitada de forma automática la opción text in row con un valor predeterminado para inline limit de 256. Esta opción no puede modificarse.  
  
 La opción text in row admite las funciones TEXTPTR, WRITETEXT, UPDATETEXT y READTEXT. Los usuarios pueden leer partes de un BLOB con la función SUBSTRING(), pero debe recordarse que los punteros de texto consecutivos tienen límites de duración y de número distintos del resto de punteros de texto.  
  
 Para cambiar una tabla de un formato de almacenamiento vardecimal de nuevo al formato de almacenamiento decimal normal, la base de datos debe estar en modo de recuperación SIMPLE. Si se cambia el modo de recuperación, se interrumpirá la cadena de registro para las copias de seguridad; por lo tanto, debe crear una copia de seguridad completa de la base de datos después de quitar el formato de almacenamiento vardecimal de una tabla.  
  
 Si va a convertir una columna de tipo de datos LOB existente (Text, ntext o Image) en tipos de valores pequeños a medianos (varchar (Max), nvarchar (Max) o varbinary (Max)) y la mayoría de las instrucciones no hacen referencia a las columnas de tipo de valor grande en su entorno, considere la posibilidad de cambiar **large_value_types_out_of_row** a 1 para obtener **un** rendimiento óptimo. Cuando se cambia el valor de la opción de **large_value_types_out_of_row** , los valores VARCHAR (Max), nvarchar (Max), varbinary (Max) y XML existentes no se convierten inmediatamente. El almacenamiento de las cadenas se cambia cuando se actualizan ulteriormente. Los valores nuevos que se inserten en una tabla se almacenan de acuerdo a la opción de tabla vigente. Para obtener resultados inmediatos, haga una copia de los datos y, a continuación, vuelva a rellenar la tabla después de cambiar el valor de la **large_value_types_out_of_row** o actualizar cada columna de tipos de valor grandes de pequeño a mediano a sí misma para que el almacenamiento de las cadenas se cambie con la opción de tabla en vigor. Conviene regenerar los índices en la tabla después de la actualización o de volver a rellenar para condensar la tabla. 
    
  
## <a name="permissions"></a>Permisos  
 Para ejecutar sp_tableoption se requiere el permiso ALTER en la tabla.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-storing-xml-data-out-of-the-row"></a>A. Almacenar datos xml fuera de la fila  
 En el ejemplo siguiente se especifica que los datos **XML** de la `HumanResources.JobCandidate` tabla se almacenen fuera de la fila.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'HumanResources.JobCandidate', 'large value types out of row', 1;  
```  
  
### <a name="b-enabling-vardecimal-storage-format-on-a-table"></a>B. Habilitar el formato de almacenamiento vardecimal en una tabla  
 En el ejemplo siguiente se modifica la `Production.WorkOrderRouting` tabla para almacenar el `decimal` tipo de datos en el `vardecimal` formato de almacenamiento.  

```sql  
USE master;  
GO  
-- The database must be enabled for vardecimal storage format  
-- before a table can be enabled for vardecimal storage format  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'Production.WorkOrderRouting',   
   'vardecimal storage format', 'ON';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
