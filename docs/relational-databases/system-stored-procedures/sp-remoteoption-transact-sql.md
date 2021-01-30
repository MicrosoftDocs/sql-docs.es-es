---
description: sp_remoteoption (Transact-SQL)
title: sp_remoteoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_remoteoption_TSQL
- sp_remoteoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_remoteoption
ms.assetid: c9a7309b-eab7-4192-a414-e282581af4e5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aff4a43e54c6bbb8e6c406d6b48452fc84915e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185647"
---
# <a name="sp_remoteoption-transact-sql"></a>sp_remoteoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra o cambia las opciones de un inicio de sesión remoto definido en el servidor local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)]sp_remoteoption no cambia ninguna opción y devuelve un mensaje de error. Se admite únicamente por razones de compatibilidad con versiones anteriores.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_remoteoption [ [ @remoteserver = ] 'remoteserver' ]   
     [ , [ @loginame = ] 'loginame' ]   
     [ , [ @remotename = ] 'remotename' ]   
     [ , [ @optname = ] 'optname' ]   
     [ , [ @optvalue = ] 'optvalue' ]  
```  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado devuelve el mensaje de error siguiente:  
  
 `The trusted option in remote login mapping is no longer supported.`  
  
## <a name="see-also"></a>Consulte también  
 [Servidores vinculados &#40;motor de base de datos&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)  
  
  
