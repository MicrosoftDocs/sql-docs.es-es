---
title: ICommand (controlador OLE DB) | Microsoft Docs
description: Obtenga información sobre el comportamiento del método ICommand::Execute que es específico de OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ICommand [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7d290800b0f002f2ad7ba1703683648a62f41f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191810"
---
# <a name="icommand-ole-db"></a>ICommand (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  En este artículo se describe el comportamiento de OLE DB específico de OLE DB Driver for SQL Server.  
  
## <a name="icommandexecute"></a>ICommand::Execute  
 Insertar datos que sean mayores que el tamaño de una columna suele producir un error. Sin embargo, hay situaciones en las que se devuelve S_OK y `dwStatus` se establece en DBSTATUS_S_TRUNCATED. Estos son algunos escenarios en los que normalmente se produce:

- Al insertar datos con parámetros  
- Donde la columna no es lo suficientemente grande como para contener los datos  
- Cuando no se ha llamado a `ICommandWithParameters::SetParameterInfo`  
  
## <a name="see-also"></a>Consulte también  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)
  
  
