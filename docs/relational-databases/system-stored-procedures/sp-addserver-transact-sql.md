---
description: sp_addserver (Transact-SQL)
title: sp_addserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addserver
- sp_addserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addserver
- renaming servers
- machine names [SQL Server]
- computer names
ms.assetid: 160a6b29-5e80-44ab-80ec-77d4280f627c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fabe72f253eda808131d876fd180ddb2f7a67ad1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207094"
---
# <a name="sp_addserver-transact-sql"></a>sp_addserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Define el nombre de la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando se cambie el nombre del equipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda, utilice **sp_addserver** para informar a la instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nombre del nuevo equipo. Este procedimiento debe ejecutarse en todas las instancias del [!INCLUDE[ssDE](../../includes/ssde-md.md)] hospedado en el equipo. No se puede cambiar el nombre de instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Para cambiar el nombre de instancia de una instancia con nombre, instale una instancia nueva con el nombre deseado, desasocie los archivos de base de datos de la instancia antigua, asocie las bases de datos a la nueva instancia y quite la antigua instancia. Como alternativa, puede crear un nombre de alias de cliente en el equipo cliente, redirigir la conexión a un servidor y nombre de instancia diferentes o a la combinación **servidor:puerto** sin cambiar el nombre de la instancia en el equipo servidor.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```

sp_addserver [ @server = ] 'server' ,
     [ @local = ] 'local' 
     [ , [ @duplicate_ok = ] 'duplicate_OK' ]
```

## <a name="arguments"></a>Argumentos
`[ @server = ] 'server'` Es el nombre del servidor. Los nombres de los servidores tienen que ser únicos y cumplir las reglas para los nombres de equipo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, aunque no se permiten espacios. *server* es de tipo **sysname** y no tiene ningún valor predeterminado.

 Cuando se instalan varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo, una instancia opera como si estuviera en un servidor independiente. Especifique una instancia con nombre haciendo referencia al *servidor* como *nombredeservidor\nombredeinstancia*.

`[ @local = ] 'LOCAL'` Especifica que el servidor que se va a agregar como servidor local. **\@ local** es **VARCHAR (10)** y su valor predeterminado es NULL. Al especificar **\@ local** como **local** , se define **\@ Server** como el nombre del servidor local y se hace que la @SERVERNAME función @ devuelva el valor de *Server*.

 El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establece esta variable en el nombre del equipo durante la instalación. De manera predeterminada, el nombre del equipo es la forma en que los usuarios se conectan a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sin requerir ninguna configuración adicional.

 La definición local solo surte efecto después de reiniciarse el [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Solo puede definirse un servidor local en cada instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].

`[ @duplicate_ok = ] 'duplicate_OK'` Especifica si se permite un nombre de servidor duplicado. **\@ duplicate_OK** es de tipo **VARCHAR (13)** y su valor predeterminado es NULL. **\@ duplicate_OK** solo puede tener el valor **duplicate_OK** o null. Si se especifica **duplicate_OK** y el nombre del servidor que se va a agregar ya existe, no se genera ningún error. Si no se utilizan parámetros con nombre, se debe especificar **\@ local** .

## <a name="return-code-values"></a>Valores de código de retorno
 0 (correcto) o 1 (error)

## <a name="remarks"></a>Observaciones
 Para establecer o borrar las opciones de servidor, use **sp_serveroption**.

 no se puede usar **sp_addserver** dentro de una transacción definida por el usuario.

 El uso de **sp_addserver** para agregar un servidor remoto es discontinuo.  Use en su lugar [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).

## <a name="permissions"></a>Permisos
 Debe pertenecer al rol fijo de servidor **setupadmin** .

## <a name="examples"></a>Ejemplos
 En el ejemplo siguiente se cambia a [!INCLUDE[ssDE](../../includes/ssde-md.md)] la entrada del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el nombre del equipo que hospeda `ACCOUNTS`.

```
sp_addserver 'ACCOUNTS', 'local';
```

## <a name="see-also"></a>Consulte también
 [Cambiar el nombre de un equipo que hospeda una instancia de Stand-Alone de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) [sp_addlinkedserver &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) SP_DROPSERVER &#40;[Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md) [Sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) [procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) [procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)


