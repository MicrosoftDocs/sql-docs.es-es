---
title: 'Conmutación por error manual de una FCI: SQL Server en Linux'
description: Aprenda a conmutar por error de forma manual una instancia de clúster de conmutación por error (FCI) en SQL Server en Linux, específicamente Red Hat Enterprise Linux, Ubuntu y SUSE Linux Enterprise Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 93bff6d536a6c1c5ad4eb8d8805fefb706377594
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346297"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>Operar la instancia de clúster de conmutación por error: SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo usar una instancia de clúster de conmutación por error (FCI) de SQL Server en Linux. Si no ha creado una FCI de SQL Server en Linux, consulte [Configurar la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure.md). 

## <a name="failover"></a>Conmutación por error

La conmutación por error de las FCI es similar a un clúster de conmutación por error de Windows Server (WSFC). Si el nodo de clúster que hospeda la FCI experimenta algún tipo de error, la FCI debe conmutar por error automáticamente en otro nodo. A diferencia de un WSFC, no hay forma de establecer los propietarios preferidos, por lo que Pacemaker elige el nodo que será el nuevo host de la FCI.

En ocasiones, puede que quiera conmutar manualmente la FCI en otro nodo. El proceso no es el mismo que con las FCI en un WSFC. En un WSFC, los recursos se conmutan por error en el nivel de rol. En Pacemaker, elegirá un recurso que mover y, suponiendo que todas las restricciones sean correctas, todo lo demás también se moverá. 

La forma de conmutar por error depende de la distribución de Linux. Siga las instrucciones de su distribución de Linux.

- [RHEL o Ubuntu](#manual-failover-rhel-or-ubuntu)
- [SLES](#manual-failover-sles)

## <a name="manual-failover-rhel-or-ubuntu"></a>Conmutación por error manual (RHEL o Ubuntu)

Para realizar una conmutación por error manual, los servidores de Red Hat Enterprise Linux (RHEL) o Ubuntu ejecutan los pasos siguientes.
1.  Emita el comando siguiente: 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName> es el nombre del recurso de Pacemaker para la FCI de SQL Server.

   \<NewHostNode> es el nombre del nodo de clúster en el que quiere hospedar la FCI. 

   No recibirá ninguna confirmación.

2.  Durante una conmutación por error manual, Pacemaker crea una restricción de ubicación en el recurso que se ha elegido para moverse manualmente. Para ver esta restricción, ejecute `sudo pcs constraint`.

3.  Una vez completada la conmutación por error, envíe `sudo pcs resource clear <FCIResourceName>` para quitar la restricción. 

\<FCIResourceName> es el nombre del recurso de Pacemaker para la FCI. 

## <a name="manual-failover-sles"></a>Conmutación por error manual (SLES)


En SUSE Linux Enterprise Server (SLES), use el comando `migrate` para realizar una conmutación por error manual de una FCI de SQL Server. Por ejemplo:

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName> es el nombre del recurso de la instancia de clúster de conmutación por error. 

\<NewHostNode> es el nombre del nuevo host de destino. 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>Pasos siguientes

- [Configurar la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
