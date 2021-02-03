---
title: Habilitar y configurar FILESTREAM | Microsoft Docs
description: Para usar FILESTREAM, habilítelo primero en la instancia del Motor de base de datos de SQL Server. Obtenga información sobre cómo habilitar FILESTREAM con el Administrador de configuración de SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], enabling
ms.assetid: 78737e19-c65b-48d9-8fa9-aa6f1e1bce73
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea678a62f4453232fe8442cf2053c3b951e361ef
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250664"
---
# <a name="enable-and-configure-filestream"></a>Habilitar y configurar FILESTREAM

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para empezar a utilizar FILESTREAM, debe habilitarlo en la instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. En este tema se describe cómo habilitar FILESTREAM con el Administrador de configuración de SQL Server.  
  
##  <a name="enabling-filestream"></a><a name="enabling"></a> Habilitar FILESTREAM  
  
#### <a name="to-enable-and-change-filestream-settings"></a>Para habilitar y cambiar la configuración de FILESTREAM  
  
1.  En el menú **Inicio** , elija **Todos los programas**, [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], **Herramientas de configuración** y, por último, **Administrador de configuración de SQL Server**.  
  
2.  En la lista de servicios, haga clic con el botón derecho en **Servicios de SQL Server** y, después, haga clic en **Abrir**.  
  
3.  En el complemento **Administrador de configuración de SQL Server** , busque la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que quiera habilitar FILESTREAM.  
  
4.  Haga clic con el botón derecho en la instancia y, después, haga clic en **Propiedades**.  
  
5.  En el cuadro de diálogo **Propiedades de SQL Server** , haga clic en la pestaña **FILESTREAM** .  
  
6.  Seleccione la casilla **Habilitar FILESTREAM para acceso Transact-SQL** .  
  
7.  Si quiere leer y escribir datos FILESTREAM de Windows, haga clic en **Habilitar FILESTREAM para el acceso de transmisión por secuencias de E/S de archivos**. Escriba el nombre del recurso compartido de Windows en el cuadro **Nombre de recurso compartido de Windows** .  
  
8.  Si los clientes remotos deben tener acceso a los datos FILESTREAM que están almacenados en este recurso compartido, seleccione **Permitir que los clientes remotos tengan acceso de transmisión por secuencias a los datos FILESTREAM**.  
  
9. Haga clic en **Aplicar**.  
  
10. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic en **Nueva consulta** para mostrar el Editor de consultas.  
  
11. En el Editor de consultas, escriba el siguiente código de [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```sql  
    EXEC sp_configure filestream_access_level, 2  
    RECONFIGURE  
    ```  
  
12. Haga clic en **Ejecutar**.  
  
13. Reinicie el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

##  <a name="best-practices"></a><a name="best"></a> Procedimientos recomendados  
  
###  <a name="physical-configuration-and-maintenance"></a><a name="config"></a> Configuración física y mantenimiento  
 Cuando configure volúmenes de almacenamiento FILESTREAM, tenga en cuenta las directrices siguientes:  
  
-   Desactive la opción de nombres cortos de archivo en equipos FILESTREAM. Los nombres cortos de archivo requieren mucho más tiempo para su creación. Para deshabilitar la opción de nombres cortos de archivo, emplee la utilidad **fsutil** de Windows.  
  
-   Desfragmente periódicamente los equipos FILESTREAM.  
  
-   Use clústeres NTFS de 64 kB. Los volúmenes comprimidos deben establecerse en clústeres NTFS de 4 kB.  
  
-   Deshabilite la indexación en volúmenes FILESTREAM y establezca **disablelastaccess**. Para establecer **disablelastaccess**, use la utilidad **fsutil** de Windows.  
  
-   Deshabilite el examen del antivirus de volúmenes FILESTREAM cuando no sea necesario. Cuando el análisis del antivirus sea necesario, evite el establecimiento de directivas que eliminen automáticamente los archivos causantes del problema.  
  
-   Configure y ajuste el nivel RAID que proporcione la tolerancia a errores y el rendimiento requeridos por una aplicación.  
  
|Nivel RAID|Rendimiento de escritura|Rendimiento de lectura|Tolerancia a errores|Observaciones|  
|-|-|-|-|-|   
|RAID 5|Normal|Normal|Excelente|El rendimiento es mejor que en el caso de un disco o JBOD y menor que RAID 0 o RAID 5 con creación de bandas.|  
|RAID 0|Excelente|Excelente|None||  
|RAID 5 con creación de bandas|Excelente|Excelente|Excelente|Opción más cara.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
  
###  <a name="physical-database-design"></a><a name="database"></a> Diseño físico de base de datos  
 Cuando diseñe una base de datos de FILESTREAM, tenga en cuenta las directrices siguientes:  
  
-   Las columnas FILESTREAM deben ir acompañadas de una columna **uniqueidentifier** ROWGUID correspondiente. Estos tipos de tablas también deben ir acompañados de un índice único. Normalmente, este índice no es un índice clúster. Si la lógica de negocios de bases de datos requiere un índice clúster, debe asegurarse de que los valores almacenados en el índice no sean aleatorios. Los valores aleatorios harán que el índice se vuelva a ordenar cada vez que se agregue o se quite una fila en la tabla.  
  
-   Por razones de rendimiento, los contenedores y grupos de archivos FILESTREAM deben residir en volúmenes distintos del sistema operativo, base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tempdb o archivo de paginación.  
  
-   FILESTREAM no admite directamente la aplicación de directivas ni la administración del espacio. Sin embargo, es posible administrar el espacio y aplicar directivas indirectamente mediante la asignación de cada grupo de archivos FILESTREAM a un volumen independiente y usando las características de administración del volumen.  
  
  
