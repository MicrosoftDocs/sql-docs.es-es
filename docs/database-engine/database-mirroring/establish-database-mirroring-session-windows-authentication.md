---
title: Configuración de la creación de reflejos de la base de datos (autenticación de Windows)
description: Obtenga información sobre cómo configurar una sesión de creación de reflejos de la base de datos mediante la autenticación de Windows con SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], sessions
ms.assetid: 7cb418d6-dce1-4a0d-830e-9c5ccfe3bd72
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f09f731b2d8838c5cdcd4ea9861666c9d89cacf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342349"
---
# <a name="establish-database-mirroring-session---windows-authentication"></a>Establecer una sesión de creación de reflejo de la base de datos - Autenticación de Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Se usa [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] en su lugar.  
  
 Para establecer una sesión de creación de reflejo de la base de datos y modificar las propiedades de la creación de reflejo de la base de datos de una base de datos, utilice la página **Creación de reflejo** del cuadro de diálogo de **Propiedades de la base de datos** . Antes de utilizar la página **Creación de reflejo** para configurar la creación de reflejo de la base de datos, asegúrese de que se hayan cumplido los siguientes requisitos:  
  
-   Las instancias del servidor reflejado y principal se deben ejecutar en la misma edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea Standard o Enterprise. Asimismo, se recomienda encarecidamente que se ejecuten en sistemas comparables que puedan administrar cargas de trabajo idénticas.  
  
    > [!NOTE]  
    >  Una instancia de servidor testigo no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Debe existir la base de datos reflejada y estar vigente.  
  
     Crear una base de datos reflejada requiere una copia de seguridad reciente de la base de datos principal (mediante WITH NORECOVERY) en la instancia del servidor reflejado. También se necesita realizar una o más copias de seguridad del registro después de la copia de seguridad completa, y restaurarlos en secuencia en la base de datos reflejada (mediante WITH NORECOVERY). Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Si las instancias del servidor se ejecutan con distintas cuentas de usuario de dominio, cada una requiere un inicio de sesión en la base de datos **master** de las demás. Si el inicio de sesión no existe, deberá crearlo antes de configurar la creación de reflejo. Para obtener más información, vea [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
### <a name="to-configure-database-mirroring"></a>Para configurar la creación de reflejo de la base de datos  
  
1.  Después de conectarse a la instancia del servidor principal, en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol del servidor.  
  
2.  Expanda **Bases de datos** y seleccione la base de datos que se va a reflejar.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, luego, haga clic en **Reflejado**. Así se abre la página **Creación de reflejo** del cuadro de diálogo **Propiedades de la base de datos** .  
  
4.  Para empezar la configuración de la creación de reflejo, haga clic en **Configurar seguridad** para iniciar el Asistente para la configuración de seguridad de la creación de reflejo de bases de datos.  
  
    > [!NOTE]  
    >  Durante una sesión de creación de reflejo de la base de datos, puede utilizar este asistente para agregar o cambiar la instancia del servidor testigo.  
  
5.  El Asistente para la configuración de seguridad de la creación de reflejo de bases de datos crea automáticamente el punto de conexión de creación de reflejo de la base de datos (si no existe ninguno) en cada instancia de servidor y especifica la dirección de red del servidor en el campo correspondiente al rol de la instancia de servidor (**Entidad de seguridad**, **Reflejado** o **Testigo**).  
  
    > [!IMPORTANT]  
    >  Cuando se crea un extremo, el Asistente para la configuración de seguridad de la creación de reflejo de bases de datos siempre utiliza la Autenticación de Windows. Antes de poder usar el asistente con la autenticación basada en certificados, se debe haber configurado el extremo de creación de reflejo para el uso de certificados en cada una de las instancias de servidor. Además, todos los campos del cuadro de diálogo **Cuentas de servicio** deben permanecer vacíos. Para obtener información sobre la creación de un punto de conexión de creación de reflejo de la base de datos, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
6.  Opcionalmente, cambie el modo operativo. La disponibilidad de determinados modos operativos depende de si se ha especificado una dirección TCP para un testigo. Las opciones son las siguientes:  
  
    |Opción|¿Testigo?|Explicación|  
    |------------|--------------|-----------------|  
    |**Rendimiento alto (asincrónico)**|Null (si existe, no se usa excepto cuando la sesión requiere un quórum)|Para maximizar el rendimiento, la base de datos reflejada siempre estará algo detrás de la base de datos principal, nunca acercándose demasiado. Sin embargo, el espacio entre las bases de datos suele ser pequeño. La pérdida de un asociado tiene el siguiente efecto:<br /><br /> Si la instancia de servidor reflejado deja de estar disponible, el principal continúa.<br /><br /> Si la instancia del servidor principal pasa a no estar disponible, el reflejado se detiene; pero si la sesión no tiene testigo (como se recomienda) o el testigo está conectado al servidor reflejado, se puede tener acceso a éste como servidor en espera activa; el propietario de la base de datos puede forzar el servicio a la instancia del servidor reflejado (con posible pérdida de datos).<br /><br /> <br /><br /> Para obtener más información, vea [Conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).|  
    |**Seguridad alta sin conmutación automática por error (sincrónico)**|No|Se garantiza que todas las transacciones confirmadas se escribirán en disco en el servidor reflejado.<br /><br /> La conmutación manual por error es posible siempre que los asociados estén conectados entre ellos y la base de datos esté sincronizada.<br /><br /> La pérdida de un asociado tiene el siguiente efecto:<br /><br /> Si la instancia de servidor reflejado deja de estar disponible, el principal continúa.<br /><br /> Si la instancia del servidor principal deja de estar disponible, el reflejado se detiene, pero es accesible como servidor en espera activa; el propietario de la base de datos puede forzar el servicio a la instancia del servidor reflejado (con una posible pérdida de datos).<br /><br /> <br /><br /> Para obtener más información, vea [Conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).|  
    |**Seguridad alta con conmutación automática por error (sincrónico)**|Sí (obligatorio)|Se garantiza que todas las transacciones confirmadas se escribirán en disco en el servidor reflejado.<br /><br /> Máxima disponibilidad al incluir una instancia del servidor testigo para permitir la conmutación automática por error. Tenga en cuenta que solo puede seleccionar la opción **Seguridad alta con conmutación automática por error (sincrónico)** si antes ha especificado una dirección del servidor testigo.<br /><br /> La conmutación manual por error es posible siempre que los asociados estén conectados entre ellos y la base de datos esté sincronizada.<br /><br /> En la presencia de un testigo, la pérdida de un asociado tiene el siguiente efecto:<br /><br /> Si la instancia del servidor principal deja de estar disponible, se produce una conmutación automática por error. La instancia del servidor reflejado cambia al rol de servidor principal y ofrece su base de datos como base de datos principal.<br /><br /> Si la instancia de servidor reflejado deja de estar disponible, el principal continúa.<br /><br /> <br /><br /> Para obtener más información, vea [Conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> **&#42;&#42; Importante &#42;&#42;** Si el testigo se desconecta, los asociados deben estar conectados entre ellos para que la base de datos esté disponible. Para obtener más información, vea [Cuórum: cómo un testigo afecta a la disponibilidad de la base de datos &#40;reflejo de la base de datos&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).|  
  
7.  Cuando se den todas las condiciones siguientes, haga clic en **Iniciar creación de reflejo** para iniciar la creación de reflejo:  
  
    -   Actualmente está conectado a la instancia del servidor principal.  
  
    -   La seguridad se ha configurado correctamente.  
  
    -   Las direcciones TCP completas de las instancias del servidor principal y reflejado están especificadas (en la sección **Direcciones de red de servidor** ).  
  
    -   Si el modo operativo está establecido en **Seguridad alta con conmutación automática por error (sincrónico)** , también se especifica la dirección TCP completa de la instancia del servidor testigo.  
  
8.  Una vez iniciada la creación de reflejo, puede cambiar el modo operativo y guardar el cambio haciendo clic en **Aceptar**. Tenga en cuenta que solo puede cambiar al modo de alta seguridad con conmutación automática por error si antes ha especificado la dirección de un servidor testigo.  
  
    > [!NOTE]  
    >  Para quitar el testigo, elimine su dirección de red del servidor del campo **Testigo** . Si pasa del modo de alta seguridad con conmutación automática por error al modo de alto rendimiento, el campo **Testigo** se vacía de forma automática.  
  
## <a name="see-also"></a>Consulte también  
 [Conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Pausar o reanudar una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)   
 [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)   
 [Administración de inicios de sesión y trabajos tras la conmutación de roles &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)   
 [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Agregar o reemplazar un testigo de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
  
