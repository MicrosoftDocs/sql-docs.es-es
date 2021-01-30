---
description: sp_addscriptexec (Transact-SQL)
title: sp_addscriptexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addscriptexec
- sp_addscriptexec_TSQL
helpviewer_keywords:
- sp_addscriptexec
ms.assetid: 1627db41-6a80-45b6-b0b9-c0b7f9a1c886
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6349479d4f3ce3097771516640b0eec258e32f5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207156"
---
# <a name="sp_addscriptexec-transact-sql"></a>sp_addscriptexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Expone un script SQL (archivo .sql) para todos los suscriptores de una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addscriptexec [ @publication = ] publication  
    [ , [ @scriptfile = ] 'scriptfile' ]  
    [ , [ @skiperror = ] 'skiperror' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @scriptfile = ] 'scriptfile'` Es la ruta de acceso completa al archivo de script de SQL. *scriptfile* es de tipo **nvarchar (4000)** y no tiene ningún valor predeterminado.  
  
`[ @skiperror = ] 'skiperror'` Indica si el Agente de distribución o Agente de mezcla debe detenerse cuando se produce un error durante el procesamiento del script. *SkipError* es de **bit** y su valor predeterminado es 0.  
  
 **0** = el agente se detendrá.  
  
 **1** = el agente continúa el script y omite el error.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al publicar desde un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addscriptexec** se utiliza en la replicación transaccional y la replicación de mezcla.  
  
 **sp_addscriptexec** no se utiliza para la replicación de instantáneas.  
  
 Para utilizar **sp_addscriptexec**, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio debe tener permisos de lectura y escritura en la ubicación de la instantánea y permisos de lectura en la ubicación donde se almacenan los scripts.  
  
 La [utilidad SQLCMD](../../tools/sqlcmd-utility.md) se usa para ejecutar el script en el suscriptor y el script se ejecuta en el contexto de seguridad utilizado por el Agente de distribución o agente de mezcla al conectarse a la base de datos de suscripciones. Cuando el agente se ejecuta en una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se utiliza la [utilidad osql](../../tools/osql-utility.md) en lugar de [sqlcmd](../../tools/sqlcmd-utility.md).  
  
 **sp_addscriptexec** es útil para aplicar scripts a los suscriptores y usa [sqlcmd](../../tools/sqlcmd-utility.md) para aplicar el contenido del script al suscriptor. No obstante, dado que las configuraciones del suscriptor pueden variar, los scripts probados antes de enviarlos al publicador también pueden generar errores en un suscriptor. *skiperror* ofrece la posibilidad de que el Agente de distribución o agente de mezcla omitan los errores y continúen. Use [sqlcmd](../../tools/sqlcmd-utility.md) para probar los scripts antes de ejecutar **sp_addscriptexec**.  
  
> [!NOTE]  
>  Los errores omitidos se registran en el historial del agente como referencia.  
  
 El uso de **sp_addscriptexec** para publicar un archivo de script para publicaciones que usan FTP para la entrega de instantáneas solo se admite para los [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suscriptores de.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addscriptexec**.  
  
## <a name="see-also"></a>Consulte también  
 [Ejecutar scripts durante la sincronización &#40;la programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)   
 [Sincronizar datos](../../relational-databases/replication/synchronize-data.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
