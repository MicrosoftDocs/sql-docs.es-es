---
title: Implementación de la compresión Unicode | Microsoft Docs
description: SQL Server usa el algoritmo del esquema de compresión estándar para Unicode (SCSU) para comprimir los valores Unicode que están almacenados en objetos comprimidos de fila o página.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Unicode data compression
- compression [SQL Server], Unicode data
ms.assetid: 44e69e60-9b35-43fe-b9c7-8cf34eaea62a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f78e0bbe469861251c95a0d7fc382be4ed333dd0
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251127"
---
# <a name="unicode-compression-implementation"></a>Implementación de la compresión Unicode
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa una implementación del algoritmo del esquema de compresión estándar para Unicode (SCSU) para comprimir los valores Unicode que están almacenados en objetos comprimidos de fila o página. Para estos objetos comprimidos, la compresión Unicode es automática para columnas **nchar(n)** y **nvarchar(n)** . [!INCLUDE[ssDE](../../includes/ssde-md.md)] almacena los datos Unicode como 2 bytes, independientemente de la configuración regional. Esto se denomina codificación UCS-2. Para algunas configuraciones regionales, la implementación de la compresión SCSU en SQL Server puede ahorrar hasta el 50% de espacio de almacenamiento.  
  
## <a name="supported-data-types"></a>Tipos de datos admitidos  
 La compresión Unicode admite los tipos de datos **nchar(n)** y **nvarchar(n)** de longitud fija. Los valores de datos que no están almacenados de forma consecutiva o en columnas **nvarchar(max)** no se comprimen.  
  
> [!NOTE]  
>  La compresión Unicode no se admite para los datos **nvarchar(max)** aunque estén almacenados de forma consecutiva. Sin embargo, este tipo de datos puede seguir beneficiándose de la compresión de página.  
  
## <a name="upgrading-from-earlier-versions-of-sql-server"></a>Actualizar de versiones anteriores de SQL Server  
 Cuando una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se actualiza a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], los cambios relacionados con la compresión Unicode no se llevan a cabo en ningún objeto de base de datos, tanto si está comprimido como si no. Una vez actualizada la base de datos, los objetos se ven afectados de la siguiente forma:  
  
-   Si el objeto no está comprimido, no se realiza ningún cambio y sigue funcionando como lo hacía anteriormente.  
  
-   Los objetos comprimidos de fila o página siguen funcionando como lo hacían anteriormente. Los datos sin comprimir permanecen sin comprimir hasta que se actualiza su valor.  
  
-   Las nuevas filas que se insertan en una tabla comprimida de fila o página se comprimen con la compresión Unicode.  
  
    > [!NOTE]  
    >  Para aprovechar al máximo las ventajas de la compresión Unicode, el objeto se debe regenerar con la compresión de página o fila.  
  
## <a name="how-unicode-compression-affects-data-storage"></a>Cómo afecta la compresión Unicode al almacenamiento de datos  
 Cuando un índice se crea o regenera, o bien cuando un valor se cambia en una tabla que se comprimió con compresión de fila o página, el índice o el valor afectado se almacena comprimido solo si su tamaño comprimido es menor que su tamaño actual. De este modo se evita que las filas de una tabla o índice aumenten de tamaño debido a la compresión Unicode.  
  
 El espacio de almacenamiento que la compresión ahorra depende de las características de los datos que se vayan a comprimir y la configuración regional de dichos datos. En la tabla siguiente se enumeran los ahorros de espacio que se pueden conseguir para diferentes configuraciones regionales.  
  
|Configuración regional|Porcentaje de compresión|  
|------------|-------------------------|  
|Inglés|50 %|  
|Alemán|50 %|  
|Hindi|50 %|  
|Turco|48%|  
|Vietnamita|39 %|  
|Japonés|15 %|  
  
## <a name="see-also"></a>Consulte también  
 [Comprimir datos](../../relational-databases/data-compression/data-compression.md)   
 [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md)   
 [Implementación de la compresión de página](../../relational-databases/data-compression/page-compression-implementation.md)   
 [sys.dm_db_persisted_sku_features &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-persisted-sku-features-transact-sql.md)  
  
  
