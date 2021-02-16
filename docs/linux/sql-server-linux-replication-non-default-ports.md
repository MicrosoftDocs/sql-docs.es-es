---
title: Configuración de la carpeta de instantáneas de replicación (puertos no predeterminados)
titleSuffix: SQL Server on Linux
description: Obtenga información sobre cómo configurar los recursos compartidos de la carpeta de instantáneas con puertos no predeterminados para la replicación de SQL Server en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f51dd4ebdb3a797a54eb488e8d99c02017dc70e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346485"
---
# <a name="configure-replication-with-non-default-ports-sql-server-linux"></a>Configuración de la replicación con puertos no predeterminados (SQL Server en Linux)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Puede configurar la replicación con SQL Server en instancias de Linux que escuchen en cualquier puerto configurado con la opción network.tcpport mssql-conf. Si se cumplen las condiciones siguientes, es necesario anexar el puerto al nombre del servidor durante la configuración:

1. En la configuración de replicación, se usa una instancia de SQL Server en Linux.
2. Cualquier instancia (Windows o Linux) escucha en un puerto no predeterminado. 

El nombre del servidor de una instancia se puede averiguar si se ejecuta @@servername en la instancia. No use la dirección IP en lugar del nombre del servidor. El uso de la dirección IP para el publicador, el distribuidor o el suscriptor puede producir un error.

> [!NOTE]
> La creación de la replicación de SQL Server en Linux con un puerto no predeterminado solo funcionará con SQL Server 2019 y versiones posteriores.

## <a name="examples"></a>Ejemplos

“Servidor1” escucha en el puerto 1500 en Linux. Para configurar “Servidor1” para la distribución, ejecute `sp_adddistributor` con `@distributor`. Por ejemplo: 

```sql
exec sp_adddistributor @distributor = 'Server1,1500'
```

“Servidor1” escucha en el puerto 1500 en Linux. Para configurar un publicador para el distribuidor, ejecute `sp_adddistpublisher` con `@publisher`. Por ejemplo:

```sql
exec sp_adddistpublisher @publisher = 'Server1,1500' ,  ,  
```

“Servidor2” escucha en el puerto 6549 en Linux. Para configurar “Servidor2” como un suscriptor, ejecute `sp_addsubscription` con `@subscriber`. Por ejemplo:

```sql
exec sp_addsubscription @subscriber = 'Server2,6549' ,  ,  
```

“Servidor3” escucha en el puerto 6549 en Windows con el nombre de servidor “Servidor3” y el nombre de instancia “MSSQL2017”. Para configurar “Servidor3” como un suscriptor, ejecute `sp_addsubscription` con `@subscriber`. Por ejemplo:

```sql
exec sp_addsubscription @subscriber = 'Server3/MSSQL2017,6549',  ,  
```

## <a name="next-steps"></a>Pasos siguientes

[Conceptos: Replicación de SQL Server en Linux](sql-server-linux-replication.md)

[Procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)

