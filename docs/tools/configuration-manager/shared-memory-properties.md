---
title: Propiedades de Memoria compartida
description: Aprenda a habilitar o deshabilitar el protocolo de memoria compartida, que los clientes pueden usar para conectarse a una instancia de SQL Server que se ejecuta en el mismo equipo.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- shared memory [SQL Server]
ms.assetid: dc1704da-eacd-4d26-b529-c996f958ca4b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8f5d913a9517ef441b50ea4bc3e8cbac5791ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349647"
---
# <a name="shared-memory-properties"></a>Propiedades de Memoria compartida
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilice la página **Protocolo** del cuadro de diálogo **Propiedades de Memoria compartida** para habilitar o deshabilitar el protocolo de memoria compartida. El protocolo de memoria compartida es el más sencillo de utilizar y no tiene ningún valor configurable. Dado que los clientes que utilizan el protocolo de memoria compartida solo se pueden conectar a una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecute en el mismo equipo, no es útil para la mayoría de las actividades de la base de datos. Utilice el protocolo de memoria compartida para la solución de problemas cuando sospeche que los demás protocolos no están configurados correctamente.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Opciones  
 **Enabled**  
 Los valores posibles son **Yes** y **No**. El protocolo de memoria compartida está habilitado de manera predeterminada.  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un protocolo de red](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Crear una cadena de conexión válida con el protocolo de memoria compartida](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
