---
description: Usar certificados para un extremo de creación de reflejo de la base de datos (Transact-SQL)
title: Uso de certificados para un punto de conexión de creación de reflejo de la base de datos
descriptoin: Steps to configure the use of a certificate on both inbound and outbound connections for a SQL Server database mirroring endpoint.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: f7c23cc2-48dc-4b78-b441-89ca29a0bd9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 93d25bca0427698a56b77c8e6024d621ba7114b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352170"
---
# <a name="use-certificates-for-a-database-mirroring-endpoint-transact-sql"></a>Usar certificados para un extremo de creación de reflejo de la base de datos (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para habilitar la autenticación de certificados para la creación de reflejos de la base de datos en una instancia determinada del servidor, el administrador del sistema debe configurar cada instancia del servidor para que utilice certificados en las conexiones de entrada y salida. Las conexiones de salida deben configurarse en primer lugar.  
  
> [!NOTE]  
>  Todas las conexiones de creación de reflejo en una instancia de servidor utilizan un único extremo de reflejo de la base de datos; debe especificar el método de autenticación de la instancia de servidor cuando cree el extremo. Por tanto, en la creación de reflejo de la base de datos solo puede utilizar un formulario de autenticación por cada instancia de servidor.  
  
## <a name="configuring-outbound-connections"></a>Configurar conexiones salientes  
 Siga estos pasos en cada instancia de servidor que configure para la creación de reflejo de la base de datos:  
  
1.  En la base de datos **maestra** , cree una clave maestra de base de datos.  
  
2.  En la base de datos **maestra** , cree un certificado cifrado en la instancia de servidor.  
  
3.  Cree un extremo para la instancia de servidor utilizando su certificado.  
  
4.  Realice una copia de seguridad del certificado en un archivo. A continuación, cópiela de forma segura a los demás sistemas.  
  
 Debe completar estos pasos para cada asociado y el testigo, si existe.  
  
 Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
## <a name="configuring-inbound-connections"></a>Configurar conexiones entrantes  
 A continuación, siga estos pasos para cada asociado que configure para la creación de reflejo de la base de datos. En la base de datos **maestra** :  
  
1.  Cree un inicio de sesión para el otro sistema.  
  
2.  Cree un usuario para dicho inicio de sesión.  
  
3.  Obtenga el certificado para el extremo de reflejo de la otra instancia de servidor.  
  
4.  Asociar el certificado al usuario creado en el paso 2.  
  
5.  Conceda el permiso CONNECT para el inicio de sesión para el extremo de reflejo.  
  
 Si existe un testigo, también debe configurar conexiones de entrada para él. Esto requiere la configuración de inicios de sesión, usuarios y certificados para el testigo en los dos asociados y viceversa.  
  
 Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
## <a name="security"></a>Seguridad  
 A menos que garantice que su red es segura, se recomienda utilizar el cifrado para las conexiones de creación de reflejo de la base de datos. Para obtener más información, vea [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 Cuando copie un certificado en otro sistema, utilice un método de copia seguro. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
## <a name="see-also"></a>Consulte también  
 [Crear la clave maestra de una base de datos](../../relational-databases/security/encryption/create-a-database-master-key.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Centro de seguridad para el motor de base de datos SQL Server y la base de datos SQL Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
