---
description: MSSQLSERVER_33028
title: MSSQLSERVER_33028 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33028 (Database Engine error)
ms.assetid: c5cec0e4-0bcd-4907-826f-e7d835cfcb37
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 039df95618dd8d6f5402fc6c996a2495ee9991b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190918"
---
# <a name="mssqlserver_33028"></a>MSSQLSERVER_33028
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|33028|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SEC_CRYPTOPROV_CANTOPENSESSION|  
|Texto del mensaje|No se puede abrir la sesión para %S_MSG '%.*ls'. Código de error de proveedor: %d.|  
  
## <a name="explanation"></a>Explicación  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pudo abrir el proveedor de servicios criptográficos enumerado en el mensaje de error. El proveedor de servicios criptográficos proporcionó el código de error enumerado. Puede que tenga que ponerse en contacto con su proveedor de servicios criptográficos para obtener más información sobre el error.  
  
|Código de error|Descripción|  
|--------------|---------------|  
|0|Correcto. Ningún error.|  
|1|Error. Se ha producido un error no especificado o inesperado. No se dispone de información adicional.|  
|2|Búfer insuficiente. No se pudo asignar espacio para el proveedor de servicios criptográficos.|  
|3|No compatible. El proveedor de servicios criptográficos no admite esta versión. Seleccione otro proveedor de servicios criptográficos.|  
|4|Not Found. El proveedor de servicios criptográficos especificado no está presente o no se tiene autorización para el acceso a los archivos.|  
|5|Error de autorización. Puede ser el resultado de proporcionar un nombre de usuario o una contraseña incorrectos al crear la credencial. Puede producirse si la credencial no existe.|  
|6|Argumento no válido. Se ha pasado un argumento no válido al proveedor de servicios criptográficos.|  
  
## <a name="user-action"></a>Acción del usuario  
Resuelva el error o póngase en contacto con el proveedor de servicios criptográficos para obtener más información.  
  
