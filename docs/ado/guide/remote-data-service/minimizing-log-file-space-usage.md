---
description: Minimización del uso de espacio de archivo de registro
title: Minimizar el uso del espacio del archivo de registro | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- log file space in RDS [ADO]
ms.assetid: 669662a0-e20f-483e-ab28-53f66c524c98
author: rothja
ms.author: jroth
ms.openlocfilehash: c0be8adfc9fdcf85df12768571567ce832c2f69a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036475"
---
# <a name="minimizing-log-file-space-usage"></a>Minimización del uso de espacio de archivo de registro
Un archivo de registro puede llenarse rápidamente (con lo que se detiene el servidor) si hay un gran volumen de actividad en una base de datos de SQL Server. Puede establecer el archivo de registro en **truncar en el punto de comprobación** para ampliar significativamente la duración del archivo de registro de una base de datos.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-65"></a>Para habilitar TRUNCATE on Checkpoint en Microsoft SQL Server 6,5  
  
1.  Inicie el administrador de Microsoft SQL Server Enterprise, abra el árbol del servidor y, a continuación, abra el árbol dispositivos de base de datos.  
  
2.  Haga doble clic en el nombre de la base de datos en la que se habilitará esta característica.  
  
3.  En la pestaña **base de datos** , seleccione **truncar**.  
  
4.  En la pestaña **Opciones** , seleccione **truncar registro en punto de comprobación** y, a continuación, haga clic en **Aceptar**.  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-70"></a>Para habilitar TRUNCATE on Checkpoint en Microsoft SQL Server 7,0  
  
1.  Inicie el administrador de Microsoft SQL Server Enterprise, abra el árbol del servidor y, a continuación, abra el árbol de bases de datos.  
  
2.  Haga clic con el botón secundario en el nombre de la base de datos en la que se habilitará esta característica y elija **propiedades**.  
  
3.  En la pestaña **Opciones** , seleccione **truncar registro en punto de comprobación** y, a continuación, haga clic en **Aceptar**.  
  
 Para obtener más información acerca de la característica **truncar en punto de comprobación** , consulte la documentación de Microsoft SQL Server.  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)