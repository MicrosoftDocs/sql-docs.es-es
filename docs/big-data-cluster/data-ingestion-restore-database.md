---
title: Restaurar una base de datos
titleSuffix: SQL Server big data clusters
description: En este artículo se explica cómo restaurar una base de datos en la instancia maestra de un clúster de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3082c970c648fddf67b635ba7cbae5b19bfc0170
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049145"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>Restauración de una base de datos en la instancia maestra del clúster de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe cómo restaurar una base de datos existente en la instancia maestra de un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. El método recomendado consiste en seguir una estrategia de copia de seguridad, copia y restauración.

## <a name="backup-your-existing-database"></a>Copia de seguridad de la base de datos existente

En primer lugar, realice una copia de seguridad de la base de datos de SQL Server existente desde SQL Server en Windows o Linux. Use las técnicas de copia de seguridad estándar con Transact-SQL o una herramienta como SQL Server Management Studio (SSMS).

En este artículo se muestra cómo restaurar la base de datos AdventureWorks, pero puede usar cualquier copia de seguridad de base de datos. 

> [!TIP]
> Descargue la [copia de seguridad de AdventureWorks](../samples/adventureworks-install-configure.md).

## <a name="copy-the-backup-file"></a>Copia del archivo de copia de seguridad

Copie el archivo de copia de seguridad en el contenedor de SQL Server en el pod de la instancia maestra del clúster de Kubernetes.

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

Ejemplo:

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

Después, compruebe que el archivo de copia de seguridad se ha copiado en el contenedor de pods.

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

Ejemplo:

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>Restauración del archivo de copia de seguridad

Después, restaure la copia de seguridad de base de datos en la instancia maestra de SQL Server.  Si va a restaurar una copia de seguridad de base de datos creada en Windows, deberá obtener los nombres de los archivos.  En Azure Data Studio, conéctese a la instancia maestra y ejecute este script SQL:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

Ejemplo:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![Lista de archivos de copia de seguridad](media/restore-database/database-restore-file-list.png)

Ahora, restaure la base de datos. El siguiente script es un ejemplo. Reemplace los nombres o las rutas de acceso según sea necesario en función de la copia de seguridad de la base de datos.

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>Configuración del grupo de datos y del acceso a HDFS

Ahora, para que la instancia maestra de SQL Server pueda acceder a los grupos de datos y a HDFS, ejecute los procedimientos almacenados del grupo de datos y del bloque de almacenamiento. Ejecute los siguientes scripts de Transact-SQL en la base de datos recién restaurada:

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> Tendrá que ejecutar estos scripts de configuración solo para bases de datos restauradas a partir de versiones anteriores de SQL Server. Si crea una base de datos en la instancia maestra de SQL Server, los procedimientos almacenados del grupo de datos y del bloque de almacenamiento ya estarán configurados.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea la siguiente introducción:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
