---
title: ISSCommandWithParameters (controlador OLE DB)
description: Obtenga información sobre cómo la interfaz ISSCommandWithParameters admite SQL Server XML y tipos definidos por el usuario en OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a30947176efb820a7105f005e3d372f090d4190
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172168"
---
# <a name="isscommandwithparameters-ole-db"></a>ISSCommandWithParameters (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La interfaz **ISSCommandWithParameters** expone la compatibilidad con los tipos XML y definidos por el usuario (UDT) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se trata de una interfaz opcional que hereda de la interfaz OLE DB **ICommandWithParameters** básica. Además de los tres métodos heredados de **ICommandWithParameters**; **GetParameterInfo**, **MapParameterNames** y **SetParameterInfo**; **ISSCommandWithParameters** proporciona dos nuevos métodos que se utilizan para administrar tipos de datos específicos de servidor.  
  
> [!NOTE]  
>  La interfaz **ISSCommandWithParameters** se puede utilizar cuando se usan componentes de servicio, pero los componentes de servicio no utilizarán esta interfaz.  
  
|Método|Descripción|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Devuelve la estructura del conjunto de propiedades **SSPARAMPROPS** en la matriz de cada parámetro UDT o XML pasado al comando, pero no devuelve nada en los demás tipos de parámetros.|  
|[ISSCommandWithParameters::SetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Establece las propiedades de parámetro por cada parámetro por ordinal o establece propiedades masivas de parámetro mediante la especificación de una matriz de estructuras **SSPARAMPROPS** .|  
  
## <a name="see-also"></a>Consulte también  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Utilizar tipos de datos XML](../../oledb/features/using-xml-data-types.md)   
 [Usar tipos definidos por el usuario](../../oledb/features/using-user-defined-types.md)  
  
  
