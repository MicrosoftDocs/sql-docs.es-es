---
title: Agregar características a una instancia de SQL Server (programa de instalación)
description: En este artículo se proporciona un procedimiento paso a paso para agregar características dependientes de la instancia a una instancia de SQL Server 2019.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- " SQL Server, features"
- adding features to  SQL Server
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 02/05/2021
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 40f6618c8564f0d264076316038274b401fb650a
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514902"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>Agregar características a una instancia de SQL Server (programa de instalación)

[!INCLUDE [ SQL Server - Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

En este artículo se proporciona un procedimiento paso a paso para agregar características a una instancia de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Algunos componentes o servicios de SQL Server son específicos de una instancia de SQL Server. También se denominan dependientes de la instancia. Comparten la misma versión que la instancia que los hospeda y se usan exclusivamente para dicha instancia. Puede agregar los componentes dependientes de la instancia a una instancia de SQL Server, junto con los componentes compartidos si no están instalados. Para obtener una lista de las características admitidas por las diferentes ediciones de SQL Server, consulte [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) o [Ediciones y características admitidas de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

Para agregar características a una instancia de SQL Server desde el símbolo del sistema, vea [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md).

> [!CAUTION]
> Al agregar características a una instalación existente de SQL Server, se instalará la característica en el nivel de versión del disco de instalación, que puede estar detrás del nivel de versión de otras características de SQL Server. Esto puede producir un comportamiento o errores inesperados. Siga siempre la realización correcta de la configuración de SQL Server mediante la incorporación de la nueva característica al mismo nivel de versión. Instale los Service Pack, las actualizaciones acumulativas o las versiones de distribución general según sea necesario. Para determinar la versión de las características agregadas a una instalación de SQL Server, consulte [Determinación del nivel de versión, edición y actualización de SQL Server y sus componentes](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level).

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, lea los artículos de [Planear una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).

> [!NOTE]
> En instalaciones locales, debe ejecutar el programa de instalación como administrador. Si instala SQL Server desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura para dicho recurso.  
  
> [!NOTE]
> Al agregar las características a una instancia de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la configuración del informe de uso existente se aplica a las características agregadas recientemente. Para cambiar estos valores, utilice la herramienta **Informes de uso y errores de SQL Server** en el menú de **Herramientas de configuración** de SQL Server.

## <a name="procedures"></a>Procedimientos

#### <a name="to-add-features-to-an-instance-of-sscurrent"></a>Para agregar características a una instancia de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

1. Inserte el disco de instalación de SQL Server. En la carpeta raíz, haga doble clic en setup.exe. Para realizar la instalación desde un recurso compartido de red, navegue hasta la carpeta raíz de dicho recurso y, a continuación, haga doble clic en setup.exe. Si aparece el cuadro de diálogo Programa de instalación de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], seleccione **Aceptar** para instalar los requisitos previos y, a continuación, seleccione **Cancelar** para salir de la instalación de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  

2. El Asistente para la instalación iniciará el Centro de instalación de SQL Server. Para agregar una nueva característica a una instancia existente de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], seleccione **Instalación** en el área de navegación del lado izquierdo y, a continuación, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

3. El Comprobador de configuración del sistema ejecutará una operación de detección en su equipo. Para ver los detalles de la comprobación, seleccione **Ver detalles**. Para continuar, seleccione **Aceptar**.

4. En la página Actualizaciones del producto, se muestran las actualizaciones del producto SQL Server más recientes disponibles. Si no desea incluir las actualizaciones, desactive la casilla **Incluir actualizaciones del producto SQL Server**. Si no se detectan actualizaciones de producto, el programa de instalación de SQL Server no muestra esta página y pasa automáticamente a la página **Instalar archivos de configuración**.

5. En la página Instalar archivos de instalación, el programa de instalación proporciona el progreso de descarga, extracción e instalación de los archivos de instalación. Si se encuentra una actualización para la instalación de SQL Server, y se especifica que debe incluirse, esa actualización también se instalará. Seleccione **Instalar** para instalar los archivos auxiliares del programa de instalación.  

6. El Comprobador de configuración del sistema comprobará el estado del sistema de su equipo antes de seguir con la instalación.  

7. En la página Tipo de instalación, seleccione la opción **Agregar características a una instancia existente de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]** y seleccione la instancia que quiera actualizar.

8. En la página Selección de características, seleccione los componentes de la instalación. Después de seleccionar el nombre de la característica, la descripción de cada grupo de componentes aparece en el panel derecho. Puede activar cualquier combinación de casillas. Para obtener más información, consulte [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) y [Ediciones y características admitidas de SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md). Cada componente solo se puede instalar una vez en cada instancia de SQL Server. Para instalar varios componentes, deberá instalar una instancia adicional de SQL Server.

    Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. La instalación de SQL Server instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.

    El Comprobador de configuración del sistema comprobará el estado del sistema de su equipo antes de seguir con la instalación. Seleccione **Siguiente** para continuar.

9. La página Requisitos de espacio en disco calcula el espacio en disco necesario para las características especificadas, y compara los requisitos con el espacio en disco disponible en el equipo donde se ejecuta el programa de instalación.

10. El flujo de trabajo en el resto de este artículo depende de las características que haya especificado en la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas.

11. En la página Configuración del servidor - Cuentas de servicio, especifique las cuentas de inicio de sesión para los servicios de SQL Server. Los servicios reales que se configuran en esta página dependen de las características que se van a instalar.

    Puede asignar la misma cuenta de inicio de sesión a todos los servicios de SQL Server, o configurar cada cuenta de servicio individualmente. También puede especificar si los servicios se inician de forma automática o manual, o si están deshabilitados. Microsoft recomienda configurar las cuentas de servicio de forma individual para proporcionar a cada servicio los privilegios mínimos; así, los servicios de SQL Server obtendrán los permisos mínimos que necesitan para completar sus tareas. Para obtener más información, vea [Configuración del servidor - Cuentas de servicio](./install-sql-server.md) y [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

    Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio en esta instancia de SQL Server, se han de proporcionar credenciales en los campos de la parte inferior de la página.

    **Nota de seguridad** : [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]

    Cuando termine de especificar la información de inicio de sesión para los servicios de SQL Server, seleccione **Siguiente**.

12. Use la pestaña **Configuración del servidor - Intercalación** para especificar intercalaciones no predeterminadas para Database Engine y Analysis Services. Para obtener más información, vea [Configuración del servidor - Intercalación](./install-sql-server.md).

13. Use la página Configuración del motor de base de datos - Aprovisionamiento de cuentas para especificar lo siguiente:  

    - Modo de seguridad: seleccione la autenticación de Windows o la autenticación de modo mixto para su instancia de SQL Server. Si selecciona Autenticación de modo mixto, debe proporcionar una contraseña segura para la cuenta de administrador del sistema de SQL Server integrada.

        Una vez que un dispositivo establece una conexión correcta con SQL Server, el mecanismo de seguridad es el mismo para la autenticación de Windows y para el modo mixto. Para obtener más información, vea [Configuración del motor de base de datos: configuración del servidor](./install-sql-server.md).  

    - Los administradores de SQL Server deben especificar por lo menos un administrador del sistema para la instancia de SQL Server. Para agregar la cuenta en la que se ejecuta el programa de instalación de SQL Server, seleccione **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, seleccione **Agregar** o **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de SQL Server. Para obtener más información, vea [Configuración del motor de base de datos: configuración del servidor](./install-sql-server.md).

    Cuando haya terminado de editar la lista, seleccione **Aceptar**. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, seleccione **Siguiente**.

14. Use la página Configuración del Motor de base de datos - Directorios de datos para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.  

    > [!IMPORTANT]
    > Si especifica los directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de SQL Server. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de SQL Server.

    Para obtener más información, vea [Configuración del motor de base de datos - Directorios de datos](./install-sql-server.md).

15. Use la página Configuración de Motor de base de datos - FILESTREAM para habilitar FILESTREAM para la instancia de SQL Server. Para obtener más información sobre FILESTREAM, vea [Configuración del motor de base de datos - Secuencia de archivo](./install-sql-server.md). Para continuar, seleccione Siguiente.

16. Use la página Configuración de Analysis Services - Aprovisionamiento de cuentas para especificar el modo de servidor y los usuarios o las cuentas que tendrán permisos de administrador para Analysis Services. El modo servidor determina los subsistemas de memoria y de almacenamiento que se utilizan en el servidor. Tipos diferentes de solución se ejecutan en modos servidor diferentes. Si tiene previsto ejecutar bases de datos multidimensionales de cubo en el servidor, elija la opción predeterminada, el modo servidor multidimensional y de minería de datos. En lo que respecta a los permisos de administrador, debe especificar al menos un administrador del sistema para Analysis Services. Para agregar la cuenta en la que se ejecuta el programa de instalación de SQL Server, seleccione **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, seleccione **Agregar** o **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de Analysis Services. Para obtener más información sobre los permisos de administrador y de modo de servidor, vea [Configuración de Analysis Services - Aprovisionamiento de cuentas](./install-sql-server.md).

    Cuando haya terminado de editar la lista, seleccione **Aceptar**. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, seleccione **Siguiente**.

17. Use la página Configuración de Analysis Services - Directorios de datos para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.

    > [!IMPORTANT]
    > Si especifica los directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de SQL Server. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de SQL Server.

    Para obtener más información, vea [Configuración de Analysis Services - Directorios de datos](./install-sql-server.md).

18. Use la página de configuración de Reporting Services para especificar el tipo de instalación de Reporting Services que se debe crear. Para obtener más información sobre los modos de configuración de Reporting Services, vea [Opciones de configuración de Reporting Services &#40;SSRS&#41;](./install-sql-server.md).

19. Use la página Configuración de Distributed Replay Controller para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Controller. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Controlador de Distributed Replay.

    Seleccione **Agregar usuario actual** para agregar los usuarios a los que desee conceder permisos de acceso para el servicio Distributed Replay Controller. Seleccione el botón **Agregar** para agregar permisos de acceso para el servicio Distributed Replay Controller. Seleccione el botón **Quitar** para quitar los permisos de acceso del servicio de controlador Distributed Replay.

    Para continuar, seleccione **Siguiente**.

20. Use la página Configuración de Distributed Replay Client para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Client. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Distributed Replay Client.

    **Nombre del controlador** es un parámetro opcional y el valor predeterminado es \<*blank*>. Escriba el nombre del controlador con el que se comunicará el equipo cliente para el servicio Distributed Replay Client. Tenga en cuenta lo siguiente:

    - Si ya ha configurado un controlador, escriba el nombre del controlador mientras configura cada cliente.

    - Si aún no ha configurado ningún controlador, puede dejar el nombre del controlador en blanco. Sin embargo, debe escribir manualmente el nombre del controlador en el archivo de **configuración de cliente** .

    Especifique el **Directorio de trabajo** para el servicio Distributed Replay Client. El directorio de trabajo predeterminado es \<*drive letter*>:\Archivos de programa\Microsoft\SQL Server\DReplayClient\WorkingDir.

    Especifique el **Directorio de resultados** para el servicio Distributed Replay Client. El directorio de resultados predeterminado es \<*drive letter*>:\Archivos de programa\Microsoft\SQL Server\DReplayClient\ResultDir.

    Para continuar, seleccione **Siguiente**.

21. En la página Informes de errores, especifique la información que desea enviar a Microsoft y que ayudará a mejorar SQL Server. De forma predeterminada, se habilitan las opciones de informes de errores.

22. El Comprobador de configuración del sistema ejecutará uno o varios conjuntos de reglas para validar la configuración del equipo con las características de SQL Server que ha especificado.  

23. La página Listo para instalar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. En esta página, el programa de instalación indica si la característica de actualización de producto está habilitada o deshabilitada y la versión final de actualización. Después de seleccionar la opción de instalación, el Programa de instalación de SQL Server instalará primero los requisitos previos necesarios para las características seleccionadas y, después, instalará las características.  

24. La página Progreso de la instalación muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  

25. Después de la instalación, la página Operación completada proporciona un vínculo al archivo de registro de resumen para la instalación y otras notas importantes. Para completar el proceso de instalación de SQL Server, seleccione **Cerrar**.

26. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras completar el programa de instalación. Para obtener más información sobre los archivos de registro de instalación, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

> [!CAUTION]
> Aplicación de actualizaciones de mantenimiento
>
> Al agregar características a una instalación existente de SQL Server, se instalará la característica en el nivel de versión del disco de instalación, que puede estar detrás del nivel de versión de otras características de SQL Server. Esto puede producir un comportamiento o errores inesperados. Realice siempre un seguimiento de la característica nueva mediante la incorporación de la nueva característica al mismo nivel de versión. Instale los Service Pack, las actualizaciones acumulativas o las versiones de distribución general según sea necesario. Para determinar la versión del servidor y las características nuevas, consulte [Determinación del nivel de versión, edición y actualización de SQL Server y sus componentes](/troubleshoot/sql/general/determine-version-edition-update-level).

## <a name="next-steps"></a>Pasos siguientes

Configuración de la instalación de SQL Server

- Para reducir el área expuesta del sistema susceptible de recibir ataques, SQL Server instala y activa de manera selectiva los servicios y las características clave. Para obtener más información, vea [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).

## <a name="see-also"></a>Consulte también
- [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Validar una instalación de SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)
- [Reparar una instalación de SQL Server 2016 con errores](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
- [Upgrade SQL Server Using the Installation Wizard &#40;Setup&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md) (Actualización de SQL Server mediante el Asistente para instalación [programa de instalación])
- [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)
- [Últimas actualizaciones de SQL Server](latest-updates-for-microsoft-sql-server.md)
