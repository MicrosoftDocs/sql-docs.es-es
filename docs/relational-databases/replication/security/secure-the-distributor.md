---
description: Proteger el distribuidor
title: Proteger el distribuidor | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server replication], Distributors
- Distributors [SQL Server replication], security
ms.assetid: 76d78229-0ff2-4aa4-9b4e-ad97534c5296
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f8a6ff28260945bc3468d11514eb63e404a38583
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475696"
---
# <a name="secure-the-distributor"></a>Proteger el distribuidor
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Al distribuidor se conectan los siguientes agentes de replicación: el Agente de registro del LOG, el Agente de instantáneas, el Agente de lectura de cola, el Agente de distribución y el Agente de mezcla. Es importante proporcionar un inicio de sesión adecuado para cada uno de estos agentes respetando el principio de conceder los derechos mínimos necesarios y proteger el almacenamiento de todas las contraseñas:  
  
-   Para información sobre la administración de inicios de sesión y contraseñas, vea [Manage Logins and Passwords in Replication](../../../relational-databases/replication/security/identity-and-access-control-replication.md) (Administrar inicios de sesión y contraseñas en la replicación).  
  
-   Para obtener información detallada acerca de los permisos exigidos para cada agente, vea [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 Además de administrar correctamente los inicios de sesión y contraseñas, es importante conocer el rol del vínculo del servidor remoto **repl_distributor** y la cuenta **distributor_admin** .  
  
## <a name="securing-the-connection-from-the-publisher-to-the-distributor"></a>Proteger la conexión del publicador al distribuidor  
 Para permitir la comunicación necesaria cuando los procedimientos almacenados administrativos se ejecutan en el publicador y actualizan información en el distribuidor, la replicación configura automáticamente el servidor remoto **repl_distributor**. La entrada del servidor remoto **repl_distributor** se utiliza para la comunicación con la base de datos de distribución, independientemente de si está incluida en la instancia del publicador (un distribuidor local) o reside en una instancia remota de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (un distribuidor remoto).  
  
 Cuando la base de datos de distribución está incluida en una instancia local, se genera una contraseña aleatoria y se configura automáticamente. Cuando la base de datos de distribución se encuentra en una instancia remota, el administrador configura una contraseña compartida durante la instalación del publicador y el distribuidor; a continuación, esta contraseña se utiliza para proporcionar autenticación del tráfico que pasa por el vínculo **repl_distributor** .  
  
 El distribuidor utiliza la entrada del servidor remoto **repl_distributor** para comprobar que el servidor que llama está configurado como un publicador en el distribuidor, valida la contraseña proporcionada por el publicador y valida que el procedimiento almacenado sea un procedimiento almacenado de replicación durante la ejecución.  
  
 La contraseña configurada para la entrada del servidor remoto **repl_distributor** durante la instalación está asociada a un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , **distributor_admin**, que se agrega al rol fijo de servidor **sysadmin** en el distribuidor. Los procedimientos almacenados de replicación utilizan el inicio de sesión **distributor_admin** al conectarse al distribuidor.  
  
> [!NOTE]  
>  No cambie manualmente la contraseña de **distributor_admin** . Use siempre el procedimiento almacenado **sp_changedistributor_password** o los cuadros de diálogo **Propiedades del distribuidor** o **Actualizar contraseñas de replicación** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], ya que los cambios de contraseña se aplican a las publicaciones locales automáticamente.  
  
## <a name="snapshot-folder-security"></a>Seguridad de la carpeta de instantáneas  
 Asegúrese de que el recurso compartido de instantáneas tiene acceso de lectura a la cuenta con la que se ejecutan el Agente de mezcla (para la replicación de mezcla) o el Agente de distribución (para la replicación transaccional o de instantáneas), así como acceso de escritura a la cuenta con la que se ejecuta el Agente de instantáneas. Para obtener más información sobre la carpeta de instantáneas, vea [Proteger la carpeta de instantáneas](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
## <a name="see-also"></a>Consulte también  
 [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  (Ver y modificar la configuración de seguridad de la replicación)  
 [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Ver y modificar la configuración de seguridad de la replicación](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
