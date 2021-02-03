---
description: MSSQLSERVER_41302
title: MSSQLSERVER_41302 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41302 (Database Engine error)
ms.assetid: 01e75618-afec-4232-ba68-93ab7bc31003
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1d01df7afff73d181cd389c417aa2283b8048f35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185158"
---
# <a name="mssqlserver_41302"></a>MSSQLSERVER_41302
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|41302|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|WRITE_WRITE_CONFLICT|  
|Texto del mensaje|La transacción actual intentó actualizar un registro que se ha actualizado desde que esta transacción se inició. Se anuló la transacción.|  
  
## <a name="explanation"></a>Explicación  
La transacción encontró un conflicto de escritura contra escritura y la instrucción se terminó.  
  
## <a name="user-action"></a>Acción del usuario  
Intente de nuevo la operación posterior en otra transacción. Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>Consulte también  
[OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
