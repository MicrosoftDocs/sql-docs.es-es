---
description: sp_copysubscription (Transact-SQL)
title: sp_copysubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysubscription
- sp_copysubscription_TSQL
helpviewer_keywords:
- sp_copysubscription
ms.assetid: 3c56cd62-2966-4e87-a986-44cb3fd0b760
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7377048df6d32715b8f91e26b5b21617c22a0d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192014"
---
# <a name="sp_copysubscription-transact-sql"></a>sp_copysubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

    
> [!IMPORTANT]  
>  La característica de suscripciones adjuntables ha quedado desusada y se retirará en versiones posteriores. Esta característica no se debe utilizar en nuevos trabajos de desarrollo. En las publicaciones de combinación en las que se han creado particiones mediante filtros con parámetros, se recomienda utilizar las nuevas características de las instantáneas con particiones, que simplifican la inicialización de un gran número de suscripciones. Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). En las publicaciones que no están divididas en particiones, puede inicializar una suscripción con una copia de seguridad. Para obtener más información, consulte [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 Copia una base de datos de suscripciones que tiene suscripciones de extracción, pero no de inserción. Solo se pueden copiar bases de datos de un único archivo. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_copysubscription [ @filename = ] 'file_name'  
    [ , [ @temp_dir = ] 'temp_dir' ]  
    [ , [ @overwrite_existing_file = ] overwrite_existing_file]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @filename = ] 'file_name'` Es la cadena que especifica la ruta de acceso completa, incluido el nombre de archivo, en la que se guarda una copia del archivo de datos (. MDF). *el nombre de archivo* es **nvarchar (260)** y no tiene ningún valor predeterminado.  
  
`[ @temp_dir = ] 'temp_dir'` Es el nombre del directorio que contiene los archivos temporales. *temp_dir* es de tipo **nvarchar (260)** y su valor predeterminado es NULL. Si es NULL, se [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizará el directorio de datos predeterminado. El directorio debe tener espacio suficiente para contener un archivo que tenga el tamaño de todos los archivos de la base de datos del suscriptor combinados.  
  
`[ @overwrite_existing_file = ] 'overwrite_existing_file'`Es una marca booleana opcional que especifica si se sobrescribirá o no un archivo existente con el mismo nombre especificado en **\@ filename**. *overwrite_existing_file* es de **bit** y su valor predeterminado es **0**. Si es **1**, sobrescribe el archivo especificado por **\@ filename**, si existe. Si es **0**, se produce un error en el procedimiento almacenado si el archivo existe y el archivo no se sobrescribe.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_copysubscription** se utiliza en todos los tipos de replicación para copiar una base de datos de suscripciones en un archivo como alternativa a la aplicación de una instantánea en el suscriptor. La base de datos se debe configurar para que solo admita suscripciones de extracción. Los usuarios que tengan los permisos apropiados pueden realizar copias de la base de datos de suscripciones y, a continuación, enviar por correo electrónico, copiar o transportar el archivo de suscripción (.msf) a otro suscriptor, donde se puede adjuntar como una suscripción.  
  
 El tamaño de la base de datos de suscripciones que se va a copiar debe ser inferior a 2 gigabytes (GB).  
  
 **sp_copysubscription** solo se admite para bases de datos con suscripciones de cliente y no se puede ejecutar cuando la base de datos tiene suscripciones de servidor.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_copysubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Ubicaciones de carpeta de instantáneas alternativas](../../relational-databases/replication/snapshot-options.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
