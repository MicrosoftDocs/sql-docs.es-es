---
description: Procedimientos almacenados de OLE Automation (Transact-SQL)
title: Procedimientos almacenados de automatización OLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system stored procedures [SQL Server], OLE Automation
- OLE Automation [SQL Server], stored procedures
ms.assetid: ff16a833-01fe-4877-8aa6-55b72603ec2e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a143a27fdd38de676359bd49ccfba6d6049fa387
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205272"
---
# <a name="ole-automation-stored-procedures-transact-sql"></a>Procedimientos almacenados de OLE Automation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite los siguientes procedimientos almacenados del sistema que permiten el uso de objetos de OLE Automation en un lote [!INCLUDE[tsql](../../includes/tsql-md.md)]. De forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloquea el acceso a los procedimientos almacenados de OLE Automation porque este componente se desactiva como parte de la configuración de seguridad para este servidor. Un administrador del sistema puede habilitar el acceso a los procedimientos de OLE Automation mediante sp_configure. Para obtener más información, vea [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  

:::row:::
    :::column:::
        [sp_OACreate](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)

        [sp_OADestroy](../../relational-databases/system-stored-procedures/sp-oadestroy-transact-sql.md)

        [sp_OAGetErrorInfo](../../relational-databases/system-stored-procedures/sp-oageterrorinfo-transact-sql.md)

        [sp_OAGetProperty](../../relational-databases/system-stored-procedures/sp-oagetproperty-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_OAMethod](../../relational-databases/system-stored-procedures/sp-oamethod-transact-sql.md)

        [sp_OASetProperty](../../relational-databases/system-stored-procedures/sp-oasetproperty-transact-sql.md)

        [sp_OAStop](../../relational-databases/system-stored-procedures/sp-oastop-transact-sql.md)

        [Sintaxis de jerarquía de objetos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/object-hierarchy-syntax-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Ole Automation Procedures (opción de configuración del servidor)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
  
