---
title: Compatibilidad con UTF-16
description: Obtenga información sobre la compatibilidad con UTF-16 en un búfer de longitud fija en SQL Server Native Client a partir de SQL Server 2012.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: f2520424-8ef4-409f-8147-d83da5076e96
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d6692e0312e3fda68befc21ffc549fc99518a453
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461976"
---
# <a name="utf-16-support-in-sql-server-native-client-110"></a>Compatibilidad con UTF-16 en SQL Server Native Client 11.0
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  A partir de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], si proporciona un búfer de longitud fija al enlazar un parámetro de salida o el resultado de una columna y si el carácter **wchar** escrito en el búfer antes del carácter de terminación es un punto de código que actúa como suplente superior de un par de suplentes y el carácter **wchar** siguiente es un punto de código que actúa como suplente inferior, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client no agregará el punto de código que actúa como suplente superior al búfer.  
  
## <a name="see-also"></a>Consulte también  
 [Características de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
