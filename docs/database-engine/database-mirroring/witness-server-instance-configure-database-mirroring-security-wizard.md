---
title: 'Asistente para configuración de seguridad: Instancia del servidor testigo'
description: 'Describe la página "Instancia de servidor testigo" del "Asistente para configuración de seguridad de la creación de reflejo de bases de datos". '
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.witnsrvr.f1
ms.assetid: b5763663-984a-473b-93a3-6cd3322ad41c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6cde151af5dffa136c494a8f5e266e2e4af87d91
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352152"
---
# <a name="witness-server-instance-configure-database-mirroring-security-wizard"></a>Instancia de servidor testigo (Asistente para la configuración de seguridad de la creación de reflejo de bases de datos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilice esta página para especificar información sobre la instancia de servidor que actuará como testigo de la sesión.  
  
> [!NOTE]
>  Una instancia de servidor testigo no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **Para configurar la creación de reflejo de la base de datos mediante SQL Server Management Studio**  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opciones  
 **Instancia de servidor testigo**  
 Si ya se ha especificado una instancia de servidor testigo (en la página **Creación de reflejo** del cuadro de diálogo **Propiedades de la base de datos** ), se muestra dicha instancia. Para obtener más información, vea [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md).  
  
 En caso contrario, este cuadro de lista muestra el nombre del servidor actual. Tenga en cuenta que la instancia de servidor testigo no puede ser la misma que las instancias de servidor principal o de servidor reflejado.  
  
 **Conexión**  
 Si no se ha especificado una instancia de servidor testigo, haga clic en **Conectar**. Aparecerá el cuadro de diálogo **Conectar al servidor** , donde puede especificar la instancia de servidor y establecer una conexión.  
  
 Si se ha especificado la instancia, pero el asistente no tiene una conexión con los permisos suficientes para comprobar la existencia del extremo, haga clic en **Conectar**. Aparecerá el cuadro de diálogo **Conectar al servidor** con la instancia de servidor preseleccionada y sin cambios. Especifique una cuenta de dominio con permisos suficientes y conéctese a la instancia de servidor.  
  
> [!NOTE]  
>  Al conectarse a la instancia del servidor, el Asistente para la configuración de seguridad de la creación de reflejo de bases de datos utiliza las credenciales proporcionadas en el cuadro de diálogo **Conectar al servidor** . Éstas son diferentes de las credenciales de una sesión de creación de reflejo, que utiliza las credenciales de la cuenta de inicio y cuya instancia del servidor se ejecuta como un servicio.  
  
 **Listener Port** (Puerto de escucha)  
 El comportamiento de esta opción depende de la existencia del extremo de reflejo para la instancia de servidor, del modo siguiente:  
  
-   Si el puerto de escucha no existe para la instancia de servidor, aparecerá el número de puerto 5022 en el cuadro de texto **Puerto** . Puede escribir cualquier número de puerto disponible, como el 7022.  
  
-   Cuando ya existe el extremo de reflejo, aparecerá el número de puerto del extremo. Si debe cambiar este puerto, utilice una instrucción ALTER ENDPOINT. Para obtener más información, vea [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
    > [!NOTE]  
    >  Se requiere un número de puerto.  
  
 **Nombre del extremo**  
 Si el extremo de reflejo existe para esta instancia de servidor, el nombre del extremo aparecerá aquí. Si el extremo no existe, puede especificar el nombre del extremo.  
  
 **Cifrar datos enviados a través de este extremo**  
 El cifrado está habilitado de forma predeterminada. Cuando se habilita, el cifrado no solo se admite, sino que es necesario y utiliza los valores predeterminados para todas las opciones de cifrado. Para obtener más información, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Para deshabilitar el cifrado, quite la marca de la casilla. Para volver a habilitar el cifrado, active la casilla.  
  
## <a name="see-also"></a>Consulte también  
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
