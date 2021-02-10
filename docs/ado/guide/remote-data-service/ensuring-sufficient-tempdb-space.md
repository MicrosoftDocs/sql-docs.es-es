---
description: Garantía de espacio suficiente de TempDB
title: Garantizar suficiente espacio de TempDB | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- TempDB space in RDS [ADO]
ms.assetid: 09130db1-6248-4234-a1e5-a9c8e1622c06
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f2ea2da9100b117a1526d7a36c6d1cc51a7a636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036495"
---
# <a name="ensuring-sufficient-tempdb-space"></a>Garantía de espacio suficiente de TempDB
Si se producen errores al controlar objetos de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) que necesitan espacio de procesamiento en Microsoft SQL Server 6,5, puede que necesite aumentar el tamaño de tempdb. (Algunas consultas requieren un espacio de procesamiento temporal; por ejemplo, una consulta con una cláusula ORDER BY requiere una ordenación del **conjunto de registros**, que requiere cierto espacio temporal).  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
> [!IMPORTANT]
>  Lea este procedimiento antes de realizar las acciones, ya que no es tan fácil reducir un dispositivo una vez que se expande.  
  
> [!NOTE]
>  De forma predeterminada, en Microsoft SQL Server 7,0 y versiones posteriores, TempDB se establece para que crezca automáticamente según sea necesario. Por lo tanto, este procedimiento solo es necesario en los servidores que ejecutan versiones anteriores a 7,0.  
  
### <a name="to-increase-the-tempdb-space-on-sql-server-65"></a>Para aumentar el espacio de TempDB en SQL Server 6,5  
  
1.  Inicie el administrador de Microsoft SQL Server Enterprise, abra el árbol del servidor y, a continuación, abra el árbol dispositivos de base de datos.  
  
2.  Seleccione un dispositivo (físico) para expandir, como Master, y haga doble clic en el dispositivo para abrir el cuadro de diálogo **editar dispositivo de base de datos** .  
  
     Este cuadro de diálogo muestra la cantidad de espacio que utilizan las bases de datos actuales.  
  
3.  En el cuadro **tamaño** , aumente el dispositivo a la cantidad deseada (por ejemplo, 50 megabytes (MB) de espacio en disco duro).  
  
4.  Haga clic en **cambiar ahora** para aumentar la cantidad de espacio al que se puede expandir la tempdb (lógica).  
  
5.  Abra el árbol de bases de datos en el servidor y, a continuación, haga doble clic en **tempdb** para abrir el cuadro de diálogo **editar base de datos** . En la pestaña **base** de datos se muestra la cantidad de espacio asignado actualmente a tempdb (**tamaño de datos**). De forma predeterminada, es 2 MB.  
  
6.  En el grupo **tamaño** , haga clic en **expandir**. Los gráficos muestran el espacio disponible y asignado en cada uno de los dispositivos físicos. Las barras del color granate representan el espacio disponible.  
  
7.  Seleccione un **dispositivo de registro**, como Master, para mostrar el tamaño disponible en el cuadro **tamaño (MB)** .  
  
8.  Haga clic en **expandir ahora** para asignar ese espacio a la base de datos tempdb.  
  
     En el cuadro de diálogo **editar base de datos** se muestra el nuevo tamaño asignado para tempdb.  
  
 Para obtener más información acerca de este tema, busque el archivo de ayuda del administrador de Microsoft SQL Server Enterprise para "expandir base de datos (cuadro de diálogo)".  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)