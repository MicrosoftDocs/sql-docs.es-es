---
description: MSSQL_ENG018456
title: MSSQL_ENG018456 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018456 error
ms.assetid: 3daa8144-d81f-445a-b6c3-4bb3e9fd1526
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0afdbb3340cb0162da95c654d3d72026256c3a60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205628"
---
# <a name="mssql_eng018456"></a>MSSQL_ENG018456
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|18456|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|Error de inicio de sesión del usuario '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Explicación  
 El error MSSQL_ENG018456 se produce cuando no se puede iniciar sesión. Si el mensaje de error incluye la cuenta **distributor_admin** (Error de inicio de sesión del usuario 'distributor_admin'.), el problema reside en la cuenta utilizada en la replicación. La replicación crea un servidor remoto, **repl_distributor**, que permite la comunicación entre el distribuidor y el publicador. El inicio de sesión **distributor_admin** se asocia con este servidor remoto y debe tener una contraseña válida.  
  
## <a name="user-action"></a>Acción del usuario  
 Asegúrese de que ha especificado una contraseña para esta cuenta. Para más información, vea [Proteger el distribuidor](../../relational-databases/replication/security/secure-the-distributor.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
