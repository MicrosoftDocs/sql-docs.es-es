---
description: Adición de búfer de registro persistente a una base de datos
title: Adición de búfer de registro persistente a una base de datos
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- PMEM
- persistent memory
- persisted log buffer
- add log file
- create log buffer
- remove log buffer
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ab2554561abb1590db33249a0ade4e8458b0fd54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272376"
---
# <a name="add-persisted-log-buffer-to-a-database"></a>Adición de búfer de registro persistente a una base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo agregar un búfer de registro persistente a una base de datos de [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Permisos

Requiere el permiso ALTER en la base de datos.  

## <a name="configure-persistent-memory-device-linux"></a>Configuración de dispositivo de memoria persistente (Linux)

Cómo configurar un dispositivo de memoria persistente en [Linux](../../linux/sql-server-linux-configure-pmem.md).

## <a name="configure-persistent-memory-device-windows"></a>Configuración de dispositivo de memoria persistente (Windows)

Cómo configurar un dispositivo de memoria persistente en [Windows](/windows-server/storage/storage-spaces/deploy-pmem/).
  
## <a name="add-a-persisted-log-buffer-to-a-database"></a>Adición de un búfer de registro persistente a una base de datos  

En los siguientes ejemplos se agrega un búfer de registro persistente.

```sql
ALTER DATABASE <MyDB> 
  ADD LOG FILE 
  (
    NAME = <DAXlog>, 
    FILENAME = '<Filepath to DAX Log File>', 
    SIZE = 20MB
  );
```

Se debe dar formato al volumen o montaje donde se coloque el nuevo archivo de registro con DAX (NTFS) o montarlo con la opción DAX (XFS/EXT4).

## <a name="remove-a-persisted-log-buffer"></a>Eliminación de un búfer de registro persistente

Para quitar de forma segura un búfer de registro persistente, la base de datos debe colocarse en modo de usuario único para purgar el búfer de registro persistente.

En el siguiente ejemplo se quita un búfer de registro persistente.

```sql
ALTER DATABASE <MyDB> SET SINGLE_USER;
ALTER DATABASE <MyDB> REMOVE FILE <DAXlog>;
ALTER DATABASE <MyDB> SET MULTI_USER;
```

## <a name="limitations"></a>Limitaciones

[Cifrado de datos transparente (TDE)](../security/encryption/transparent-data-encryption.md) no es compatible con el búfer de registro persistente.

[Los grupos de disponibilidad](../../t-sql/statements/create-availability-group-transact-sql.md) solo pueden usar esta característica en las réplicas secundarias debido a la necesidad de una semántica de escritura de registro normal en la principal. Sin embargo, el archivo de registro pequeño debe crearse en todos los nodos (de ser posible en los volúmenes o montajes DAX).

## <a name="backup-and-restore-operations"></a>Operaciones de copia de seguridad y restauración

Se aplican las condiciones normales de restauración. Si el búfer de registro persistente se restaura en un volumen o montaje DAX, seguirá funcionando, de lo contrario se puede quitar de forma segura.
  
## <a name="next-steps"></a>Pasos siguientes

- [Cómo funciona (solo se ejecuta más rápido): Almacenamiento en caché del final del registro de SQL Server de memoria no volátil](/archive/blogs/bobsql/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm)
- [Datos expuestos: Latencia y durabilidad con SQL Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/Latency-and-Durability-with-SQL-Server-2016)
- [Aceleración de latencia de confirmación de transacciones con memoria de clase de almacenamiento en Windows Server 2016/SQL Server 2016 SP1](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)