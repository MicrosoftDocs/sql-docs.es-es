---
title: Guía de uso del cifrado de datos transparente (TDE) en reposo de Clústeres de macrodatos de SQL Server
titleSuffix: SQL Server Big Data Clusters
description: En este artículo se muestra cómo usar la característica Cifrado de TDE en reposo de SQL Server del BDC
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7d249f9b70479603ad66e6b68cec784b1476324a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489319"
---
# <a name="sql-server-big-data-clusters-transparent-data-encryption-tde-at-rest-usage-guide"></a>Guía de uso del cifrado de datos transparente (TDE) en reposo de Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En esta guía se muestra cómo usar las funcionalidades de cifrado en reposo de Clústeres de macrodatos de SQL Server para cifrar las bases de datos.

En general, la experiencia es igual que en SQL Server en Linux y se aplica la [documentación de TDE estándar](../relational-databases/security/encryption/transparent-data-encryption.md), excepto donde se indique. A fin de supervisar el estado del cifrado en la instancia maestra, siga los modelos de consulta de DMV estándar en [`sys.dm_database_encryption_keys`](../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) y [`sys.certificates`](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).

__Características no admitidas:__
* Cifrado de grupo de datos

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Clústeres de macrodatos de SQL Server CU8+](release-notes-big-data-cluster.md)
- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **Azure Data Studio**
- Usuario de SQL Server con privilegios de administración.

## <a name="query-the-installed-certificates"></a>Consulta de los certificados instalados

1. En Azure Data Studio, conéctese a la instancia maestra de SQL Server del clúster de macrodatos. Para obtener más información, vea [Conectarse a una instancia maestra de SQL Server](connect-to-big-data-cluster.md#master).

1. Haga doble clic en la conexión de la ventana **Servidores** para mostrar el panel del servidor de la instancia maestra de SQL Server. Seleccione **Nueva consulta**.

   ![Consultar una instancia maestra de SQL Server](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Ejecute el comando de Transact-SQL siguiente para cambiar el contexto de la base de datos **maestra** de la instancia maestra.

   ```sql
   USE master
   GO
   ```

1. Consulte los certificados administrados por el sistema instalados. 

   ```sql
    SELECT TOP 1 name FROM sys.certificates WHERE name LIKE 'TDECertificate%' ORDER BY name DESC
   ```

    Use criterios de consulta diferentes según sea necesario.

    El nombre del certificado se mostrará como "TDECertificate{marca de tiempo}". Cuando vea el prefijo TDECertificate seguido de una marca de tiempo, será el certificado proporcionado por la característica administrada por el sistema.

## <a name="encrypt-a-database-using-the-system-provided-certificate"></a>Cifrado de una base de datos mediante el certificado proporcionado por el sistema

En los ejemplos siguientes, considere una base de datos denominada __userdb__ como el destino del cifrado y un certificado proporcionado por el sistema denominado __TDECertificate2020_09_15_22_46_27__ por cada salida de la sección anterior.

1. Use el patrón siguiente para generar una clave de cifrado de base de datos mediante el certificado de sistema proporcionado.

   ```sql
    USE userdb; 
    GO
    CREATE DATABASE ENCRYPTION KEY WITH ALGORITHM = AES_256 ENCRYPTION BY SERVER CERTIFICATE TDECertificate2020_09_15_22_46_27;
    GO
   ```

1. Cifre la base de datos __userdb__ con el comando siguiente.

   ```sql
    ALTER DATABASE userdb SET ENCRYPTION ON;
    GO
   ```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el cifrado en reposo para HDFS:
> [!div class="nextstepaction"]
> [Zonas de cifrado de HDFS](encryption-at-rest-hdfs-encryption-zones.md)
