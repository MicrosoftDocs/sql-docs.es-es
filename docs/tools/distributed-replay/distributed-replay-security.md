---
title: Seguridad de Distributed Replay
description: En este artículo se describen los pasos de configuración de seguridad para Distributed Replay de SQL Server y consideraciones importantes para la protección de datos y los pasos de eliminación.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 907fbe327e712c4e9a6aa989a33822baa6122a07
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836884"
---
# <a name="distributed-replay-security"></a>Seguridad de reproducción distribuida

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Antes de instalar y usar la característica [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay, es importante revisar la información de seguridad en este tema. Aquí se describen los pasos para la configuración de seguridad posteriores a la instalación, necesarios para poder utilizar Distributed Replay. En este tema también se describen consideraciones importantes relacionadas con la protección de datos y pasos importantes de eliminación.  
  
## <a name="user-and-service-accounts"></a>Cuentas de usuario y de servicio  
 En la tabla siguiente se describen las cuentas que se utilizan para Distributed Replay. Después de la instalación de Distributed Replay, debe asignar las entidades de seguridad que se ejecutarán como cuentas de servicio del controlador y del cliente. Por tanto, se recomienda configurar las cuentas de usuario de dominio correspondientes antes de instalar las características de Distributed Replay.  
  
|Cuenta de usuario|Requisitos|  
|------------------|------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller|Puede ser una cuenta de usuario de dominio o una cuenta de usuario local. Si usa una cuenta de usuario local, la herramienta de administración, controlador, y el cliente deben estar ejecutándose en el mismo equipo.<br /><br /> **\*\* Nota de seguridad \*\*** Se recomienda que la cuenta no sea un miembro del grupo local Administradores de Windows.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|Puede ser una cuenta de usuario de dominio o una cuenta de usuario local. Si usa una cuenta de usuario local, el controlador, el cliente, y el destino SQL Server deben ejecutarse en el mismo equipo.<br /><br /> **\*\* Nota de seguridad \*\*** Se recomienda que la cuenta no sea un miembro del grupo local Administradores de Windows.|  
|Cuenta de usuario interactivo que se usa para ejecutar la herramienta de administración de Distributed Replay|Puede ser una cuenta de usuario local o de usuario de dominio. Para utilizar una cuenta de usuario local, la herramienta de administración y el controlador se deben estar ejecutando en el mismo equipo.|  
  
 **Importante**: Al configurar Distributed Replay Controller, puede especificar una o más cuentas de usuario que se usarán para ejecutar los servicios de cliente de Distributed Replay. La lista siguiente es una relación de las cuentas admitidas:  
  
-   Cuenta de usuario de dominio  
  
-   Cuenta de usuario local creada por el usuario  
  
-   Administrador  
  
-   Cuenta virtual y MSA (cuenta de servicio administrada)  
  
-   Network Services, servicios locales y sistema  
  
 No se aceptan cuentas de grupo (locales o de dominio) y otras cuentas integradas (como Todos).  
  
 Para establecer las cuentas de servicio o las contraseñas después de instalar Distributed Replay, puede usar la herramienta Servicios de Windows. Para cambiar las cuentas de servicio asociadas a los servicios de Distributed Replay Controller o Client , siga estos pasos:  
  
1.  Realice una de las operaciones siguientes, en función del sistema operativo:  
  
    -   Haga clic en **Inicio**, escriba **services.msc** en el cuadro **Buscar** y presione ENTRAR.  
  
    -   Haga clic en **Inicio**, haga clic en **Ejecutar**, escriba **services.msc** y, a continuación, presione ENTRAR.  
  
2.  En el cuadro de diálogo **Servicios** , haga clic con el botón derecho en el servicio que quiera configurar y, después, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión** , haga clic en **Esta cuenta**.  
  
4.  Configure la cuenta de usuario que desea utilizar.  
  
## <a name="file-and-folder-permissions"></a>Permisos de carpetas y archivos  
 Después de que se hayan especificado las cuentas de servicio, debe conceder los permisos de carpetas y archivos necesarios para esas cuentas de servicio. Configure los permisos de carpetas y archivos según la tabla siguiente:  
  
|Cuenta|Permisos de carpeta|  
|-------------|------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller|`<Controller_Installation_Path>\DReplayController` (lectura, escritura, eliminación)<br /><br /> `DReplayServer.xml` archivo (lectura, escritura)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|`<Client_Installation_Path>\DReplayClient` (lectura, escritura, eliminación)<br /><br /> `DReplayClient.xml` archivo (lectura, escritura)<br /><br /> Los directorios de trabajo y resultado, como se especifica en el archivo de configuración del cliente mediante los elementos `WorkingDirectory` y `ResultDirectory` , respectivamente. (Lectura, escritura)|  
  
## <a name="dcom-permissions"></a>Permisos DCOM  
 DCOM se usa para la comunicación de llamada a procedimiento remoto (RPC) entre el controlador y la herramienta de administración, y entre el controlador y todos los clientes. Cuando se hayan instalado las características de Distributed Replay, debe configurar los permisos DCOM específicos de la aplicación y de todos los equipos del controlador.  
  
 Para configurar los permisos DCOM del controlador, siga estos pasos.  
  
1.  **Abra dcomcnfg.exe, el complemento de Servicios de componentes**: Esta es la herramienta que se utiliza para configurar los permisos de DCOM.  
  
    1.  En el equipo del controlador, haga clic en **Inicio**.  
  
    2.  Tipo de **dcomcnfg.exe** en el cuadro **Buscar** .  
  
    3.  Presione ENTRAR.  
  
2.  **Configure los permisos DCOM para todo el equipo**: conceda los permisos correspondientes DCOM a todos los equipos para cada cuenta que aparece en la tabla siguiente. Para más información sobre cómo establecer permisos a todos los equipos, consulte [Lista de comprobación: administrar aplicaciones DCOM](/windows/win32/com/setting-machine-wide-security-using-dcomcnfg).  
  
3.  **Configure los permisos DCOM específicos de la aplicación**: concede los permisos DCOM específicos de la aplicación correspondiente para cada cuenta que se muestra en la tabla siguiente. El nombre de aplicación DCOM para el servicio del controlador es **DReplayController**. Para más información sobre cómo establecer permisos específicos de la aplicación, consulte [Lista de comprobación: administrar aplicaciones DCOM](/windows/win32/com/setting-processwide-security-using-dcomcnfg).  
  
 En la tabla siguiente se describen los permisos DCOM necesarios para la cuenta de usuario interactivo de herramienta de administración y las cuentas de servicio de cliente:  
  
|Característica|Cuenta|Permisos DCOM necesarios en el controlador|  
|-------------|-------------|---------------------------------------------|  
|Herramienta de administración Distributed Replay|Cuenta de usuario interactivo|Acceso local<br /><br /> Acceso remoto<br /><br /> Inicio local<br /><br /> Inicio remoto<br /><br /> Activación local<br /><br /> Activación remota|  
|Servicio cliente de Distributed Replay|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|Acceso local<br /><br /> Acceso remoto<br /><br /> Inicio local<br /><br /> Inicio remoto<br /><br /> Activación local<br /><br /> Activación remota|  
  
> [!IMPORTANT]  
>  Para protegerse frente a consultas malintencionadas o ataques de denegación de servicio, asegúrese de que utiliza solo un usuario de confianza para la cuenta de servicio del cliente. Esta cuenta podrá conectarse y reproducir cargas de trabajo en la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="sql-server-permissions"></a>Permisos de SQL Server  
 Las cuentas del servicio de cliente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay se usan para conectarse a la instancia de destino de la carga de trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Solo se admite el modo de autenticación de Windows para estas conexiones.  
  
 Después de instalar el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client en un conjunto de equipos, se debe conceder al rol de servidor sysadmin en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la entidad de seguridad que se ha usado para las cuentas de servicio en las que se quiere reproducir la carga de trabajo de seguimiento. Este paso no se realiza automáticamente durante la instalación de Distributed Replay.  
  
## <a name="data-protection"></a>Protección de datos  
 En el entorno de Distributed Replay, las cuentas de usuario siguientes tienen acceso completo a la instancia del servidor de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a la información de seguimiento de entrada y a los archivos de seguimiento de resultados:  
  
-   La cuenta de usuario interactivo que se usa para ejecutar la herramienta de administración.  
  
-   La cuenta de servicio del controlador.  
  
-   La cuenta de servicio del cliente.  
  
-   Miembros del grupo local de Administradores en el controlador.  
  
-   Miembros del grupo local de Administradores en el cliente.  
  
    > [!IMPORTANT]  
    >  Estas cuentas tienen acceso total a cualquier información de identificación persona (PII) o información confidencial contenida en los archivos de seguimiento, intermedios, de distribución o de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizados por Distributed Replay.  
  
 Recomendamos que realice las siguientes precauciones de seguridad:  
  
-   Almacene la información de seguimiento de entrada, resultados de seguimiento de salida y los archivos de base de datos en una ubicación que use el sistema de archivos NTFS, y aplique las listas de control de acceso (ACL) adecuadas. Si es necesario, cifre los datos almacenados en el equipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tenga en cuenta que las listas ACL no se aplican a los archivos de seguimiento y que no hay ningún tipo de enmascaramiento de datos ni ofuscación. Debe eliminar estos archivos rápidamente después de su uso.  
  
-   Aplique las listas ACL y la directiva de retención correspondientes a todos los archivos intermedios y de distribución generados por Distributed Replay.  
  
-   Use la Seguridad de la capa de transporte (TLS), antes conocida como Capa de sockets seguros (SSL), para ayudar a proteger el transporte de red.  
  
## <a name="important-removal-steps"></a>Pasos importantes de eliminación  
 Se recomienda usar solo Distributed Replay en un entorno de prueba. Una vez que haya completado la comprobación, y antes de que prepare los equipos para otra tarea, asegúrese de realizar lo siguiente:  
  
-   Desinstale las características de Distributed Replay y quite archivos de configuración relacionados del controlador y todos los clientes.  
  
-   Elimine cualquier archivo de seguimiento, intermedio, de distribución y de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se haya utilizado para realizar pruebas. Los archivos intermedios y de distribución se almacenan en el directorio de trabajo del controlador y el cliente, respectivamente.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Install Distributed Replay - Overview (Instalar Distributed Replay: información general)](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
