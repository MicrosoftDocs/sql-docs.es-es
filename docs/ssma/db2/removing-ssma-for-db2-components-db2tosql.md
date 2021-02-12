---
description: Quitar SSMA para componentes de DB2 (DB2ToSQL)
title: Quitar SSMA para componentes de DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4ee0d698-6246-48eb-b963-d62be81cab6a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 63957996baa6f65b864a3fe74bdb6b5c90aab3a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072009"
---
# <a name="removing-ssma-for-db2-components-db2tosql"></a>Quitar SSMA para componentes de DB2 (DB2ToSQL)
Cuando haya terminado de migrar las bases de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , es posible que desee desinstalar los componentes de SSMA. Puede desinstalar los componentes de cliente en cualquier momento. Sin embargo, no debe desinstalar el paquete de extensiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a menos que las bases de datos migradas dejen de usar funciones en el esquema de **ssma_DB2** de la base de datos **sysdb** .  
  
## <a name="uninstalling-the-ssma-for-db2-client"></a>Desinstalación de SSMA para el cliente DB2  
Puede desinstalar SSMA mediante **Agregar o quitar programas**.  
  
**Para desinstalar SSMA**  
  
1.  En el Panel de control, abra **Agregar o quitar programas**.  
  
2.  Seleccione **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant para DB2** y, a continuación, haga clic en **quitar**.  
  
3.  Para confirmar que desea desinstalar SSMA, haga clic en **sí**.  
  
## <a name="uninstalling-the-extension-pack"></a>Desinstalación del paquete de extensiones  
Si está seguro de que las bases de datos migradas no usan objetos en el esquema de **sysdb.ssma_DB2** , puede quitar el paquete de extensión eliminándolo del esquema: no hay ninguna desinstalación de Windows.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para el cliente DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-client-db2tosql.md)  
[Instalación de componentes de SSMA en SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)  
  
