---
title: Importación de datos con formato nativo y de caracteres de versiones anteriores de SQL Server
description: En SQL Server 2019, puede usar bcp para importar datos nativos y con formato de caracteres de otras versiones de SQL Server mediante el uso del modificador -V con un calificador.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- earlier versions [SQL Server], import and export data formats
- -V switch
- data formats [SQL Server], earlier versions
- previous versions [SQL Server], import and export data formats
ms.assetid: e644696f-9017-428e-a5b3-d445d1c630b3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: fbc7cfeb79b71c08d94e22245b8a63c4466e4bc9
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552656"
---
# <a name="import-native-and-character-format-data-from-earlier-versions-of-sql-server"></a>Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En [!INCLUDE [sssql14-md](../../includes/sssql14-md.md)] y versiones posteriores, puede usar **bcp** para importar datos con formato nativo y de caracteres desde [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] con el modificador **-V**. El modificador **-V** hace que [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] use los tipos de datos de la versión anterior especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y el formato del archivo de datos es el mismo que el de esa versión anterior.  
  
Para especificar una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para un archivo de datos, use el modificador **-V** con uno de los siguientes calificadores:  
  
|Versión de SQL Server|Qualifier|  
|------------------------|---------------|  
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|**-V80**|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|**-V90**|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|**-V100**|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|**-V 110**|  
  
## <a name="interpretation-of-data-types"></a>Interpretación de los tipos de datos  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores tienen compatibilidad para algunos nuevos tipos. Si desea importar un tipo de datos nuevo en una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe almacenarlo en un formato que pueda ser leído por la versión anterior de los clientes **bcp** . En la tabla siguiente se resume cómo se convierten los tipos de datos nuevos a efectos de compatibilidad con las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nuevos tipos de datos de SQL Server 2005|Tipos de datos compatibles de la versión 6 *x*|Tipos de datos compatibles de la versión 70|Tipos de datos compatibles de la versión 80|  
|---------------------------------------|-------------------------------------------|-----------------------------------------|-----------------------------------------|  
|**bigint**|**decimal**|**decimal**|*|  
|**sql_variant**|**text**|**nvarchar(4000)**|*|  
|**ntext**|**text**|**text**|**text**|  
|**nvarchar(max)**|**ntext**|**ntext**|**ntext**|  
|**varbinary(max)**|**image**|**image**|**image**|  
|XML|**ntext**|**ntext**|**ntext**|  
|UDT**|**image**|**image**|**image**|  
  
 *Este tipo es compatible en modo nativo.  
  
 **UDT indica un tipo definido por el usuario.  
  
## <a name="exporting-using--v-80"></a>Exportación mediante -V 80  
 Si realiza una exportación masiva de datos con el modificador **-V80**, los datos **nvarchar(max)** , **varchar(max)** , **varbinary(max)** , XML y UDT en modo nativo se almacenan con un prefijo de 4 bytes, como los datos **text**, **image** y **ntext**, en lugar de usar un prefijo de 8 bytes, que es el valor predeterminado en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores.  
  
## <a name="copying-date-values"></a>Copiar valores de fecha  
 **bcp** utiliza la API de copia masiva de ODBC. Por tanto, para importar valores de fecha en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **bcp** usa el formato de fecha ODBC (*yyyy-mm-dd hh:mm:ss*[ *.f...* ]).  
  
 El comando **bcp** exporta los archivos de datos con formato de caracteres utilizando el formato predeterminado ODBC para los valores **datetime** y **smalldatetime** . Por ejemplo, una columna **datetime** que contiene la fecha `12 Aug 1998` se copia de forma masiva en un archivo de datos como la cadena de caracteres `1998-08-12 00:00:00.000`.  
  
> [!IMPORTANT]  
>  Cuando importe datos en un campo **smalldatetime** utilizando **bcp**, asegúrese de que el valor de los segundos sea 00.000; de lo contrario, se producirá un error en la operación. El tipo de datos **smalldatetime** contiene únicamente valores hasta el minuto más cercano. BULK INSERT e INSERT ... SELECT * FROM OPENROWSET(BULK...) no darán error en esta instancia, pero truncarán el valor de los segundos.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para usar formatos de datos para la importación o exportación masivas**  
  
-   [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [bcp (utilidad)](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Compatibilidad con versiones anteriores del Motor de base de datos de SQL Server](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
