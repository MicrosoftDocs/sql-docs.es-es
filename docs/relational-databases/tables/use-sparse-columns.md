---
description: Usar columnas dispersas
title: Usar columnas dispersas | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, described
- null columns
- sparse columns
ms.assetid: ea7ddb87-f50b-46b6-9f5a-acab222a2ede
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b2d569c3f37370bd132485e843da005b1769b17
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211537"
---
# <a name="use-sparse-columns"></a>Usar columnas dispersas

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Las columnas dispersas son columnas normales que disponen de un almacenamiento optimizado para los valores NULL. Este tipo de columnas reducen los requisitos de espacio de los valores NULL a costa de una mayor sobrecarga a la hora de recuperar valores no NULL. Considere la posibilidad de utilizar columnas dispersas si el ahorro de espacio se sitúa entre el 20 y el 40 por ciento. Las columnas dispersas y los conjuntos de columnas se definen mediante las instrucciones [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 Las columnas dispersas se pueden utilizar con conjuntos de columnas e índices filtrados:  
  
-   Conjuntos de columnas  
  
     Las instrucciones INSERT, UPDATE y DELETE pueden hacer referencia a las columnas dispersas por nombre. Sin embargo, también es posible ver y usar todas las columnas dispersas de una tabla que se han combinado en una única columna XML. Esta columna se denomina conjunto de columnas. Para obtener más información sobre los conjuntos de columnas, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).  
  
-   Índices filtrados  
  
     Dado que las columnas dispersas tienen muchas filas con valores NULL, son especialmente adecuadas para los índices filtrados. Un índice filtrado en una columna dispersa solo puede indizar las filas que contienen valores. Esto permite crear un índice más pequeño y eficaz. Para obtener más información, consulte [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 Las columnas dispersas y los índices filtrados permiten a las aplicaciones, como [!INCLUDE[winSPServ](../../includes/winspserv-md.md)], almacenar y tener acceso de una forma más eficiente a un gran número de propiedades definidas por el usuario usando [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="properties-of-sparse-columns"></a>Propiedades de las columnas dispersas  
 Las columnas dispersas tienen las características siguientes:  
  
-   [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utiliza la palabra clave SPARSE en una definición de columna para optimizar el almacenamiento de valores en dicha columna. Por consiguiente, cuando el valor de la columna es NULL para cualquier fila de la tabla, los valores no requieren ningún almacenamiento.  
  
-   Las vistas de catálogo para una tabla con columnas dispersas son las mismas que para una tabla típica. La vista de catálogo sys.columns contiene una fila por cada columna de la tabla e incluye un conjunto de columnas si se ha definido alguno.  
  
-   Las columnas dispersas son una propiedad del nivel de almacenamiento, en lugar de la tabla lógica. Por tanto, una instrucción SELECT...INTO no copia la propiedad de columna dispersa a una tabla nueva.  
  
-   La función COLUMNS_UPDATED devuelve un valor **varbinary** para indicar todas las columnas que se actualizaron durante una acción DML. Los bits devueltos por la función COLUMNS_UPDATED son los siguientes:  
  
    -   Cuando una columna dispersa se actualiza de forma explícita, el bit correspondiente para dicha columna se establece en 1 y el bit para el conjunto de columnas se establece en 1.  
  
    -   Cuando un conjunto de columnas se actualiza de forma explícita, el bit para dicho conjunto de columnas se establece en 1 y los bits para todas las columnas dispersas de la tabla se establecen en 1.  
  
    -   En las operaciones de inserción, todos los bits se establecen en 1.  
  
     Para obtener más información sobre los conjuntos de columnas, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).  
  
 Los tipos de datos siguientes no se pueden especificar como SPARSE:  
  
|||  
|-|-|  
|**geography**|**text**|  
|**geometry**|**timestamp**|  
|**image**|**tipos de datos definidos por el usuario**|  
|**ntext**||  
  
## <a name="estimated-space-savings-by-data-type"></a>Ahorro de espacio calculado para cada tipo de datos  
 Las columnas dispersas requieren más espacio de almacenamiento para los valores distintos de NULL que el requerido para datos idénticos no marcados como SPARSE. Las tablas siguientes muestran el uso de espacio para cada tipo de datos. La columna **Porcentaje de NULL** indica qué porcentaje de los datos deben ser NULL para un ahorro de espacio neto de un 40 por ciento.  
  
 **Tipos de datos de longitud fija**  
  
|Tipo de datos|Bytes no dispersos|Bytes dispersos|Porcentaje de NULL|  
|---------------|---------------------|------------------|---------------------|  
|**bit**|0,125|5|98%|  
|**tinyint**|1|5|86%|  
|**smallint**|2|6|76%|  
|**int**|4|8|64%|  
|**bigint**|8|12|52%|  
|**real**|4|8|64%|  
|**float**|8|12|52%|  
|**smallmoney**|4|8|64%|  
|**money**|8|12|52%|  
|**smalldatetime**|4|8|64%|  
|**datetime**|8|12|52%|  
|**uniqueidentifier**|16|20|43%|  
|**date**|3|7|69%|  
  
 **Tipos de datos de longitud dependiente de la precisión**  
  
|Tipo de datos|Bytes no dispersos|Bytes dispersos|Porcentaje de NULL|  
|---------------|---------------------|------------------|---------------------|  
|**datetime2(0)**|6|10|57%|  
|**datetime2(7)**|8|12|52%|  
|**time(0)**|3|7|69%|  
|**time(7)**|5|9|60%|  
|**datetimetoffset(0)**|8|12|52%|  
|**datetimetoffset (7)**|10|14|49%|  
|**decimal/numeric(1,s)**|5|9|60%|  
|**decimal/numeric(38,s)**|17|21|42%|  
|**vardecimal(p,s)**|Utilice el tipo **decimal** como un cálculo moderado.|||  
  
 **Tipos de datos de longitud dependiente de los datos**  
  
|Tipo de datos|Bytes no dispersos|Bytes dispersos|Porcentaje de NULL|  
|---------------|---------------------|------------------|---------------------|  
|**sql_variant**|Varía con el tipo de datos subyacente|||  
|**varchar** o **char**|2*|4*|60%|  
|**nvarchar** o **nchar**|2*|4*+|60%|  
|**varbinary** o **binary**|2*|4*|60%|  
|**xml**|2*|4*|60%|  
|**hierarchyid**|2*|4*|60%|  
  
 *La longitud es igual a la media de los datos incluidos en el tipo, más 2 o 4 bytes.  
  
## <a name="in-memory-overhead-required-for-updates-to-sparse-columns"></a>Sobrecarga en memoria necesaria para las actualizaciones de columnas dispersas  
 A la hora de diseñar tablas con columnas dispersas, tenga en cuenta que se necesita una sobrecarga adicional de 2 bytes para cada columna dispersa que no sea NULL de la tabla cuando se está actualizando una fila. Debido a este requisito de memoria adicional, se puede producir inesperadamente un error 576 en las actualizaciones cuando el tamaño total de fila (incluida esta sobrecarga de memoria) es superior a 8019 y no se puede insertar ninguna columna de manera no consecutiva.  
  
 Considere el ejemplo de una tabla que tiene 600 columnas dispersas de tipo bigint. Si hay 571 columnas no NULL, el tamaño total en disco es de 571 * 12 = 6852 bytes. Después de incluir la sobrecarga de fila adicional y el encabezado de columna dispersa, asciende a unos 6895 bytes. La página todavía tiene 1124 bytes disponibles en disco. Esto puede dar la impresión de que se pueden actualizar correctamente columnas adicionales. Pero durante la actualización hay una sobrecarga adicional en memoria de 2\*(número de columnas dispersas no NULL). En este ejemplo, incluida la sobrecarga adicional (2 \* 571 = 1142 bytes) el tamaño de fila en disco aumenta hasta 8037 bytes. Este tamaño supera el tamaño máximo permitido de 8019 bytes. Puesto que todas las columnas tienen tipos de datos de longitud fija, no se pueden insertar de manera no consecutiva. Por tanto, se producirá el error 576 en la actualización.  
  
## <a name="restrictions-for-using-sparse-columns"></a>Restricciones de uso de las columnas dispersas  
 Las columnas dispersas pueden adoptar cualquier tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y comportarse como cualquier otra columna, con las restricciones siguientes:  
  
-   Deben aceptar valores NULL y no pueden tener las propiedades ROWGUIDCOL ni IDENTITY. No pueden adoptar los tipos de datos siguientes: **text**, **ntext**, **image**, **timestamp**, tipo de datos definido por el usuario, **geometry** ni **geography**; ni tener el atributo FILESTREAM.  
  
-   No pueden tener un valor predeterminado.  
  
-   No se pueden enlazar a una regla.  
  
-   Aunque una columna calculada puede contener una columna dispersa, una columna calculada no se puede marcar como SPARSE.  
  
-   Se puede definir una máscara de datos en una columna dispersa, pero no en una columna dispersa que forma parte de un conjunto de columnas.  
  
-   Las columnas dispersas no pueden formar parte de un índice clúster o de un índice de clave principal único. Sin embargo, tanto las columnas calculadas persistentes como las no persistentes que se definen en columnas dispersas sí pueden formar parte de la clave de un índice clúster.  
  
-   Las columnas dispersas no se pueden utilizar como clave de partición de un índice clúster o montón. Sin embargo, sí se pueden utilizar como la clave de partición de un índice no clúster.  
  
-   Las columnas dispersas no pueden formar parte de los tipos de tabla definidos por el usuario que se utilizan en variables de tabla y parámetros con valores de tabla.  
  
-   Las columnas dispersas son incompatibles con compresión de datos. Por consiguiente las columnas dispersas no se pueden agregar a las tablas comprimidas, ni se puede comprimir ninguna tabla que contenga las columnas dispersas.  
  
-   Para cambiar una columna de dispersa a no dispersa o viceversa es preciso cambiar el formato de almacenamiento de la columna. El motor de base de datos de SQL Server usa el siguiente procedimiento para realizar este cambio:  
  
    1.  Agrega una nueva columna a la tabla con el nuevo tamaño y formato de almacenamiento.  
  
    2.  Para cada fila de la tabla, actualiza y copia el valor almacenado de la columna antigua en la columna nueva.  
  
    3.  Quita la columna antigua del esquema de la tabla.  
  
    4.  Vuelve a compilar la tabla (si no hay índice clúster) o el índice clúster para reclamar el espacio que usa la columna antigua.  
  
    > [!NOTE]  
    >  Pueden producirse errores en el paso 2 si el tamaño de los datos de la fila supera el tamaño máximo de fila permitido. Este tamaño incluye el tamaño de los datos almacenados en la columna antigua y los datos actualizados almacenados en la columna nueva. Este límite es de 8060 bytes para las tablas que no contienen ninguna columna dispersa o de 8018 bytes para las tablas que contienen columnas dispersas. Este error puede producirse aunque todas las columnas coincidentes se hayan insertado de manera no consecutiva.  
  
-   Cuando convierta una columna no dispersa en una columna dispersa, esta consumirá más espacio para los valores distintos de NULL. Cuando una fila está cerca del límite de tamaño máximo, se puede producir un error en la operación.  
  
## <a name="sql-server-technologies-that-support-sparse-columns"></a>Tecnologías de SQL Server que admiten columnas dispersas  
 En esta sección se describe la compatibilidad de las columnas dispersas en las siguientes tecnologías de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Replicación transaccional  
  
     La replicación transaccional admite el uso de columnas dispersas, pero no admite los conjuntos de columnas, que se pueden usar con las columnas dispersas. Para obtener más información sobre los conjuntos de columnas, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).  
  
     La replicación del atributo SPARSE viene determinada por una opción de esquema especificada con [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) o el cuadro de diálogo **Propiedades del artículo** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admiten columnas dispersas. Si tiene que replicar los datos a una versión anterior, no olvide especificar que el atributo SPARSE no se debe replicar.  
  
     En las tablas que se publican, no es posible agregar nuevas columnas dispersas ni cambiar la propiedad SPARSE de una columna existente. Si fuera necesario realizar este tipo de operación, quite la publicación y vuelva a crearla.  
  
-   Replicación de mezcla  
  
     La replicación de mezcla no admite el uso de columnas dispersas ni de conjuntos de columnas.  
  
-   seguimiento de cambios  
  
     El seguimiento de cambios admite el uso de columnas dispersas y de conjuntos de columnas. Cuando se actualiza un conjunto de columnas en una tabla, el seguimiento de cambios lo considera como una actualización de la fila completa. No se proporciona ningún seguimiento de cambios detallado que permita obtener el número exacto de columnas dispersas que se actualizan mediante la operación de actualización del conjunto de columnas. Si las columnas dispersas se actualizan de forma explícita mediante una instrucción DML, el seguimiento de cambios funcionará normalmente en ellas y permitirá identificar el número exacto de columnas modificadas.  
  
-   captura de datos modificados  
  
     La captura de datos modificados admite el uso de columnas dispersas, pero no de conjuntos de columnas.  
  
-   La propiedad sparse de una columna no se conserva al copiar la tabla.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo, una tabla de documentos contiene un conjunto común que tiene las columnas `DocID` y `Title`. El grupo de producción desea tener una columna `ProductionSpecification` y una columna `ProductionLocation` para todos los documentos de producción. El grupo de marketing desea tener una columna `MarketingSurveyGroup` para los documentos de marketing. El código de este ejemplo crea una tabla que usa columnas dispersas, inserta dos filas en dicha tabla y, a continuación, selecciona datos en ella.  
  
> [!NOTE]  
>  Esta tabla solo tiene cinco columnas para facilitar su visualización y lectura. Si se establece la opción ANSI_NULL_DFLT_ON, es opcional declarar las columnas dispersas como columnas que aceptan valores NULL.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStore  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL ) ;  
GO  
  
INSERT DocumentStore(DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStore(DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
 La selección de todas las columnas de la tabla devuelve un conjunto de resultados normal.  
  
```  
SELECT * FROM DocumentStore ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation  MarketingSurveyGroup`  
  
 `1      Tire Spec 1  AXZZ217                  27                  NULL`  
  
 `2      Survey 2142  NULL                     NULL                Men 25-35`  
  
 Dado que el departamento de producción no está interesado en los datos de marketing, desean usar una lista de columnas que devuelva solo las columnas de interés, como se muestra en la consulta siguiente.  
  
```  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStore   
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation`  
  
 `1      Tire Spec 1  AXZZ217                  27`  
  
## <a name="see-also"></a>Consulte también  
 [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
