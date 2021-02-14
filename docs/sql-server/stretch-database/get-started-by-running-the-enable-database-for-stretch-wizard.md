---
description: Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch
title: Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: quickstart
f1_keywords:
- sql13.swb.stretchwizard.f1
- sql13.swb.stretchwizard.createdatabasecredentials.f1
- sql13.swb.stretchwizard.selectdatabasetables.f1
- sql13.swb.stretchwizard.validatesqlserver.f1
- sql13.swb.stretchwizard.selectazuredeployment.f1
- sql13.swb.stretchwizard.configureazuredeployment.f1
- sql13.swb.stretchwizard.Summary.f1
- sql13.swb.stretchwizard.Results.f1
- sql13.swb.stretchwizard.introduction.f1
helpviewer_keywords:
- Stretch Database, wizard
- Enable Database for Stretch Wizard
ms.assetid: 855dd9fc-f80c-4dbc-bf46-55a9736bfe15
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 74435746bc2ef72741cf25a610cb37d4ac1893b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080996"
---
# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 Si desea configurar una base de datos de Stretch Database, ejecute el Asistente para Habilitar base de datos para Stretch.  En este artículo se describe la información que tendrá que especificar y las decisiones que deberá tomar en el asistente.  
  
 Para obtener más información sobre Stretch Database, consulte [Stretch Database](../../sql-server/stretch-database/stretch-database.md). 
 
 > [!NOTE] 
 > Posteriormente, si deshabilita Stretch Database, recuerde que al deshabilitar Stretch Database para una tabla o una base de datos no se elimina el objeto remoto. Si quiere eliminar la tabla o la base de datos remotas, tiene que quitarlas mediante el Portal de administración de Azure. Los objetos remotos siguen acumulando gastos de Azure hasta que se eliminan manualmente. 
  
## <a name="launch-the-wizard"></a>Inicio del asistente  
  
1.  En SQL Server Management Studio, en el Explorador de objetos, seleccione la base de datos en la que desea habilitar Stretch.  
  
2.  Haga clic con el botón derecho y seleccione **Tareas**; después, seleccione **Stretch** y, por último, **Habilitar** para iniciar el asistente.  
  
##  <a name="introduction"></a><a name="Intro"></a> Introducción  
 Consulte la finalidad del asistente y los requisitos previos.  
 
 Los requisitos previos importantes incluyen los siguientes:
 -   Tiene que ser un administrador para cambiar la configuración de la base de datos.
 -   Tiene que tener una suscripción de Microsoft Azure.
 -   Su SQL Server tiene que poder comunicarse con el servidor remoto de Azure.
  
 ![Página de introducción del Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-1.png "Página de introducción del Asistente para Stretch Database")  
  
##  <a name="select-tables"></a><a name="Tables"></a> Seleccionar tablas  
 Seleccione las tablas que desee habilitar para Stretch.  
 
Las tablas con muchas filas aparecen en la parte superior de la lista ordenada. Antes de que el Asistente muestre la lista de tablas, las analiza para buscar tipos de datos que no sean compatibles en estos momentos con Stretch Database. 
  
 ![Página de selección de tablas del Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-2.png "Página de selección de tablas del Asistente para Stretch Database")  
  
|Columna|Descripción|  
|------------|-----------------|  
|(sin título)|Active la casilla de esta columna para habilitar la tabla seleccionada para Stretch.|  
|**Nombre**|Especifica el nombre de la tabla de la base de datos.|  
|(sin título)|Un símbolo en esta columna puede representar una advertencia que no le impide habilitar la tabla seleccionada para Stretch. También puede representar un problema de bloqueo que le impide habilitar la tabla seleccionada para Stretch; por ejemplo, porque la tabla usa un tipo de datos no compatible. Mantenga el mouse encima del símbolo para que se muestren más detalles al respecto como información sobre herramientas. Para obtener más información, vea [Limitaciones del área expuesta y problemas de bloqueo de Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).|  
|**Stretched (Extendida)**|Indica si ya se ha habilitado la tabla para Stretch.|  
|**Migrar**|Puede migrar una tabla completa (**Toda la tabla**) o puede especificar un filtro en una columna existente de la tabla. Si quiere usar una función de filtro diferente para seleccionar las filas que se van a migrar, ejecute la instrucción ALTER TABLE para especificar la función de filtro después de que salga del asistente. Para obtener más información sobre la función de filtro, vea [Select rows to migrate by using a filter function (Seleccionar las filas que se van a migrar mediante una función de filtro)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Para obtener más información sobre cómo aplicar la función, vea [Enable Stretch Database for a table (Habilitar Stretch Database para una tabla)](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) o [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).|  
|**Filas**|Especifica el número de filas de la tabla.|  
|**Tamaño (KB)**|Especifica el tamaño de la tabla en kB.|  
  
## <a name="optionally-provide-a-row-filter"></a>Proporcionar filtro de fila de manera opcional  
 Si quiere proporcionar una función de filtro para seleccionar las filas que se van a migrar, siga los procedimientos siguientes en la página **Seleccionar tablas** .  
  
1.  En la lista **Seleccione las tablas que quiere ajustar** , haga clic en **Toda la tabla** en la fila de la tabla. Se abre el cuadro de diálogo **Seleccionar filas para ajustar** .  
  
     ![Definición de un predicado de filtro basado en fechas](../../sql-server/stretch-database/media/stretch-wizard-2a.png "Definición de un predicado de filtro basado en fechas")  
  
2.  En el cuadro de diálogo **Seleccionar filas para ajustar** , seleccione **Elegir filas**.  
  
3.  En el **Campo de nombre**, proporcione un nombre para la función de filtro.  
  
4.  Para la cláusula **Where** , elija una columna de la tabla, seleccione un operador y proporcione un valor.  
  
5.  Haga clic en **Comprobar** para probar la función. Si la función devuelve resultados de la tabla, es decir, si existen filas que se van a migrar que cumplan la condición, la prueba notifica **Correcto**.  

> [!NOTE] 
> El cuadro de texto que muestra la consulta de filtro es de solo lectura. No puede editar la consulta en el cuadro de texto.
  
6.  Haga clic en Listo para volver a la página **Seleccionar tablas** .  

La función de filtro se crea en SQL Server solo cuando termina el asistente. Hasta entonces, puede volver a la página **Seleccionar tablas** para modificar o cambiar el nombre de la función de filtro.

![Página de selección de tablas después de definir un predicado de filtro](../../sql-server/stretch-database/media/stretch-wizard-2b.png "Página de selección de tablas después de definir un predicado de filtro")

Si quiere usar un tipo de función de filtro diferente para seleccionar las filas que va a migrar, realice una de las siguientes acciones.  
  
-   Salga del asistente y ejecute la instrucción ALTER TABLE para habilitar Stretch para la tabla y especificar una función de filtro. Para obtener más información, vea [Enable Stretch Database for a table (Habilitar Stretch Database para una tabla)](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).  
  
-   Ejecute la instrucción ALTER TABLE para especificar una función de filtro después de salir del asistente. Para conocer los pasos necesarios, vea [Agregar una función de filtro después de ejecutar el asistente](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
##  <a name="configure-azure"></a><a name="Configure"></a> Configure Azure (Configurar Azure)  
  
1.  Inicie sesión con una cuenta Microsoft en Microsoft Azure.  
  
     ![Inicio de sesión en Azure: Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-3.png "Inicio de sesión en Azure: Asistente para Stretch Database")  
  
2.  Seleccione la suscripción a Azure existente que vaya a usar para Stretch Database. 

> [!NOTE] 
> Para habilitar Stretch en una base de datos debe tener derechos de administrador para la suscripción que está usando. El asistente de Stretch Database solo mostrará las suscripciones donde el usuario tenga derechos de administrador.
  
3.  Seleccione la región de Azure que se utilizará para Stretch Database.
    -   Si crea un nuevo servidor, el servidor se crea en esta región.  
    -   Si tiene servidores existentes en la región seleccionada, el asistente los enumera cuando elija **Servidor existente**.
  
     Para minimizar la latencia, elija la región de Azure en la que se encuentre su servidor de SQL Server. Para obtener más información sobre las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).  
  
4.  Especifique si desea usar un servidor existente o crear un nuevo servidor de Azure.  
  
     Si el Active Directory del servidor de SQL Server está federado con Azure Active Directory, también puede usar una cuenta de servicio federado para que SQL Server se comunique con el servidor remoto de Azure. Para obtener más información sobre los requisitos de esta opción, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
    -   **Creación de un nuevo servidor**  
  
        1.  Cree un inicio de sesión y una contraseña para el administrador del servidor.  
  
        2.  También puede utilizar una cuenta de servicio federado para que SQL Server se comunique con el servidor remoto de Azure.  
  
         ![Creación de un nuevo Asistente para Stretch Database de servidor de Azure](../../relational-databases/tables/media/stretch-wizard-4.png "Creación de un nuevo Asistente para Stretch Server de servidor de Azure")  
  
    -   **Servidor existente**  
  
        1.  Seleccione el servidor de Azure existente.  
  
        2.  Seleccione el método de autenticación.  
  
            -   Si selecciona **Autenticación de SQL Server**, proporcione el inicio de sesión y la contraseña del administrador.  
  
            -   Seleccione **Autenticación integrada de Active Directory** a fin de utilizar una cuenta de servicio federado para que SQL Server se comunique con el servidor remoto de Azure. Si el servidor seleccionado no está integrado en Azure Active Directory, esta opción no aparece.
  
         ![Selección de un servidor de Azure existente: Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-5.png "Selección de un servidor de Azure existente: Asistente para Stretch Database")  
  
##  <a name="secure-credentials"></a><a name="Credentials"></a> Secure credentials (Proteger las credenciales)  
 Tendrá que contar con una clave maestra de base de datos para proteger las credenciales que Stretch Database utiliza para conectarse a la base de datos remota.  
  
 Si ya existe una clave maestra de base de datos, escriba la contraseña para esta.  
  
 ![Captura de pantalla que muestra la página Secure credentials (Proteger las credenciales) del asistente para Stretch Database con el cuadro de texto Contraseña vacío.](../../sql-server/stretch-database/media/stretch-wizard-6b.PNG "Página de credenciales seguras del Asistente para Stretch Database")  
  
 Si la base de datos no tiene una clave maestra existente, escriba una contraseña segura para crear una clave maestra de base de datos.  
  
 ![Captura de pantalla que muestra la página Secure credentials (Proteger las credenciales) del asistente para Stretch Database con los cuadros de texto Nueva contraseña y Confirmar contraseña rellenados.](../../relational-databases/tables/media/stretch-wizard-6.png "Página de credenciales seguras del Asistente para Stretch Database")  
  
 Para obtener más información sobre la clave maestra de base de datos, vea [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) y [Crear la clave maestra de una base de datos](../../relational-databases/security/encryption/create-a-database-master-key.md). Para obtener más información sobre la credencial que crea el asistente, vea [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
##  <a name="select-ip-address"></a><a name="Network"></a> Select IP address (Seleccionar la dirección IP)  
 Use el intervalo de direcciones IP de la subred (recomendado) o la dirección IP pública de SQL Server para crear una regla de firewall en Azure que permita a SQL Server comunicarse con el servidor remoto de Azure.  
  
 Las direcciones IP que proporcione en esta página indican al servidor de Azure que permita que los datos entrantes, las consultas y las operaciones de administración iniciadas por SQL Server pasen por el firewall de Azure. El asistente no cambia nada en la configuración del firewall del servidor de SQL Server.  
  
 ![Página Seleccionar dirección IP del Asistente para Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Página Seleccionar dirección IP del Asistente para Stretch Database")  
  
##  <a name="summary"></a><a name="Summary"></a> Resumen  
 Revise los valores especificados y las opciones seleccionadas en el asistente y los costos estimados en Azure. Después, seleccione **Finalizar** para habilitar Stretch.  
  
 ![Página de resumen del Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-8.png "Página de resumen del Asistente para Stretch Database")  
  
##  <a name="results"></a><a name="Results"></a> Resultados  
 Revise los resultados.  
  
 Para supervisar el estado de la migración de datos, vea [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Supervisar y solucionar problemas de migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 ![Página de resultados del Asistente para Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-9.PNG "Página de resultados del Asistente para Stretch Database")  
  
##  <a name="troubleshooting-the-wizard"></a><a name="KnownIssues"></a> Solución de problemas del asistente  
 **Error en el Asistente para Stretch Database.**  
 Si aún no se ha habilitado Stretch Database en el nivel de servidor y ejecuta el asistente sin los permisos de administrador del sistema necesarios para habilitarlo, se mostrará un error. Pida al administrador del sistema que habilite Stretch Database en la instancia del servidor local y, después, vuelva a ejecutar el asistente. Para obtener más información, vea [Requisito previo: permiso para habilitar Stretch Database en el servidor](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).  
  
## <a name="next-steps"></a>Pasos siguientes  
 Habilitación tablas adicionales para Stretch Database supervisar la migración de datos y administrar las tablas y las bases de datos habilitadas para Stretch.  
  
-   [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) (Habilitación de Stretch Database para una tabla) para habilitar tablas adicionales.  
  
-   [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Supervisar y solucionar problemas de migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) para ver el estado de la migración de datos.  
  
-   [Pause and resume data migration &#40;Stretch Database&#41; (Pausar y reanudar la migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Administración y solución de problemas de Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Copia de seguridad y restauración de bases de datos habilitadas para Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Restauración de bases de datos habilitadas para Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Consulte también  
 [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Habilitación de Stretch Database para una tabla](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
