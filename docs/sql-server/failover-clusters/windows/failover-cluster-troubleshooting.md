---
title: Solucionar problemas de clústeres de conmutación por error | Microsoft Docs
description: Obtenga información sobre la solución de problemas de clústeres de conmutación por error, incluida la recuperación de un error, la resolución de problemas comunes y el uso de procedimientos almacenados extendidos u objetos COM.
ms.custom: ''
ms.date: 10/21/2015
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- troublshooting, failover clustering
- failover clustering, troubleshooting
- cluster troubleshooting
ms.assetid: 84012320-5a7b-45b0-8feb-325bf0e21324
author: cawrites
ms.author: chadam
ms.openlocfilehash: 34fe360248235da3c9a647799a178d9c734ad027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350047"
---
# <a name="failover-cluster-troubleshooting"></a>Solucionar problemas de clústeres de conmutación por error
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Este tema proporciona información acerca de lo siguiente:  
  
-   Pasos básicos en la solución de problemas.  
  
-   Recuperar desde un error de clúster de conmutación por error.  
  
-   Resolver los problemas más habituales de clústeres de conmutación por error.  
  
-   Usar procedimientos almacenados extendidos y objetos COM.  
  
## <a name="basic-troubleshooting-steps"></a>Pasos básicos en la solución de problemas  
 El primer paso de diagnóstico consiste en ejecutar una comprobación de validación de clúster nueva. Para obtener detalles acerca de la validación, consulte [Guía paso a paso de clústeres de conmutación por error: Validación de hardware para un clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)).  Esto se puede completar sin ninguna interrupción del servicio, ya que no afecta a ningún recurso de clúster en línea. La validación se puede llevar a cabo en cualquier momento una vez instalada la característica Clúster de conmutación por error; por ejemplo, antes de implementar el clúster, durante su creación y mientras se esté ejecutando. De hecho, existen pruebas adicionales que se ejecutan cuando el clúster se encuentra en funcionamiento, que comprueban el cumplimiento de las prácticas recomendadas para cargas de trabajo de alta disponibilidad. Entre estas decenas de pruebas, solo unas pocas afectarán a las cargas de trabajo del clúster en ejecución y estas se engloban todas en la categoría de almacenamiento, por lo que la omisión de esta categoría por completo constituye una forma sencilla de evitar pruebas que interrumpan la actividad.  
Clúster de conmutación por error incorpora una medida de seguridad integrada para evitar que el tiempo de inactividad accidental cuando se ejecuten pruebas de almacenamiento durante la validación. Si el clúster tiene algún grupo en línea cuando se inicia la validación, y las pruebas de almacenamiento permanecen seleccionadas, se mostrará un mensaje donde el usuario tendrá que confirmar si desea ejecutar todas las pruebas (y ocasionar tiempo de inactividad) u omitir la comprobación de los discos de los grupos en línea a fin de evitar dicho perjuicio. Si se ha excluido la categoría de almacenamiento en su totalidad de las pruebas, no se mostrará este mensaje. De este modo, se habilitará la validación del clúster sin conllevar tiempo de inactividad.  
  
#### <a name="how-to-revalidate-your-cluster"></a>Cómo volver a validar el clúster  
  
1.  En el complemento de clústeres de conmutación por error, en el árbol de consola, asegúrese de que **Administración de clúster de conmutación por error** está seleccionado y, después, en **Administración**, haga clic en **Validar una configuración**.  
  
2.  Siga las instrucciones del asistente para especificar los servidores y las pruebas, y ejecute estas últimas. Se mostrará la página **Resumen** tras ejecutar las pruebas.  
  
3.  En la página **Resumen** , haga clic en **Ver informe** para consultar los resultados.  
  
     Si quiere ver los resultados de las pruebas después de cerrar el asistente, vea **%SystemRoot%\Cluster\Reports\Validation Report date and time.html** , donde **%SystemRoot%** es la carpeta donde está instalado el sistema operativo (por ejemplo, **C:\Windows**).  
  
4.  Para ver temas de ayuda que le ayudarán a interpretar los resultados, haga clic en **Más información acerca de las pruebas de validación de clústeres**.  
  
 Para ver los temas de ayuda sobre la validación de clústeres después de cerrar el asistente, en el complemento de clústeres de conmutación por error, haga clic en **Ayuda**, en **Temas de Ayuda** y en la pestaña **Contenido** . Expanda el contenido de la ayuda de clústeres de conmutación por error y haga clic en **Validating a Failover Cluster Configuration (Validar una configuración de clúster de conmutación por error)**.  Una vez que el Asistente para validación haya finalizado, los resultados se mostrarán en **Informe de resumen** . Se deben superar todas las pruebas con una marca de verificación verde o, en algunos casos, un triángulo amarillo (advertencia). Cuando busque áreas con problemas (X de color rojo o signos de interrogación amarillos), en la parte del informe donde se resumen los resultados de las pruebas, haga clic en una prueba individual para revisar los detalles. Todos los problemas indicados con una X de color rojo se tendrán que resolver antes de solucionar los problemas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **Instalación de actualizaciones**  
  
 La instalación de las actualizaciones constituye un factor importante para evitar problemas en su sistema. Vínculos útiles:  
  
-   [Revisiones y actualizaciones recomendadas para clústeres de conmutación por error basados en Windows Server 2012 R2](https://support.microsoft.com/kb/2920151)  
  
-   [Revisiones y actualizaciones recomendadas para clústeres de conmutación por error basados en Windows Server 2012](https://support.microsoft.com/kb/2784261)  
  
-   [Revisiones y actualizaciones recomendadas para clústeres de conmutación por error basados en Windows Server 2008 R2](https://support.microsoft.com/kb/980054)  
  
-   [Revisiones y actualizaciones recomendadas para clústeres de conmutación por error basados en Windows Server 2008](https://support.microsoft.com/kb/957311)  
  
## <a name="recovering-from-failover-cluster-failure"></a>Recuperar desde un error de clúster de conmutación por error  
 Normalmente, un error de clúster de conmutación por error se debe a uno de los dos motivos siguientes:  
  
-   Error de hardware en un nodo de un clúster de dos nodos. Este error de hardware podría estar causado por un error de la tarjeta SCSI o del sistema operativo.  
  
     Para solucionar este error, quite el nodo en el que se ha producido el error de la instancia de clúster de conmutación por error mediante el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , solucione el error de hardware con el equipo sin conexión, vuelva a conectar el equipo y, a continuación, agregue el nodo reparado a la instancia de clúster de conmutación por error.  
  
     Para más información, consulte [Crear un nuevo clúster de conmutación por error de SQL Server &#40;Programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) y [Recuperarse de un error en una instancia de clúster de conmutación por error](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md).  
  
-   Error del sistema operativo. En este caso, el nodo se encuentra sin conexión, pero se puede recuperar.  
  
     Para recuperarse de un error del sistema operativo, recupere el nodo y pruebe la conmutación por error. Si la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no realiza la conmutación por error correctamente, debe utilizar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para quitar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] del clúster de conmutación por error, realizar las reparaciones necesarias, volver a conectar el equipo y agregar el nodo reparado a la instancia de clúster de conmutación por error.  
  
     La recuperación de un error del sistema operativo de esta forma puede llevar algún tiempo. Si el error del sistema operativo se puede recuperar fácilmente, evite el uso de esta técnica.  
  
     Para más información, consulte [Crear un nuevo clúster de conmutación por error de SQL Server &#40;Programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) y [Cómo recuperarse de un error en un clúster de conmutación por error en el escenario 2](recover-from-failover-cluster-instance-failure.md).  
  
## <a name="resolving-common-problems"></a>Resolver problemas habituales  
 En la siguiente lista se describen los problemas de uso más comunes y se explica cómo resolverlos.  
  
### <a name="problem-incorrect-use-of-command-prompt-syntax-to-install-sql-server"></a>Problema: uso incorrecto de la sintaxis del símbolo del sistema para instalar SQL Server  
 **Problema 1:** es difícil diagnosticar los problemas del programa de instalación cuando se usa el modificador **/qn** en el símbolo del sistema, ya que el modificador **/qn** quita todos los cuadros de diálogo y los mensajes de error de dicho programa. Si se especifica el modificador **/qn** , todos los mensajes del programa de instalación, incluidos los mensajes de error, quedarán registrados en los archivos de registro de dicho programa. Para más información sobre los archivos de registro, consulte [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
 **Solución 1**: use el modificador **/qb** en lugar del **/qn**. Si usa el modificador **/qb** , se mostrará la interfaz de usuario básica en cada paso, incluidos los mensajes de error.  
  
### <a name="problem-sql-server-cannot-log-on-to-the-network-after-it-migrates-to-another-node"></a>Problema: SQL Server no puede iniciar una sesión en la red después de migrar a otro nodo  
 **Problema 1:** las cuentas de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no pueden ponerse en contacto con un controlador de dominio.  
  
 **Solución 1**: compruebe los registros de eventos para ver si hay problemas de red, como errores de los adaptadores o problemas de DNS. Compruebe que puede hacer ping al controlador de dominio.  
  
 **Problema 2: las contraseñas de las cuentas de servicio de** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no son idénticas en todos los nodos de clúster o el nodo no reinicia un servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se ha migrado desde un nodo con error.  
  
 **Solución 2:** cambie las contraseñas de las cuentas de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si no lo hace y cambia las contraseñas de la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en un nodo, debe cambiar también las contraseñas de los demás nodos. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lo hace automáticamente.  
  
### <a name="problem-sql-server-cannot-access-the-cluster-disks"></a>Problema: SQL Server no tiene acceso a los discos del clúster  
 **Problema 1:** el firmware o los controladores no están actualizados en todos los nodos.  
  
 **Solución 1:** compruebe que todos los nodos utilizan versiones correctas del firmware además de las mismas versiones de los controladores.  
  
 **Problema 2:** un nodo no puede recuperar los discos del clúster que han migrado desde un nodo con error en un disco compartido de clúster con una letra de unidad diferente.  
  
 **Solución 2:** las letras de unidad de los discos del clúster deben ser iguales en ambos servidores. Si no lo son, revise la instalación original del sistema operativo y del Servicio de Cluster Server de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] (MSCS).  
  
### <a name="problem-failure-of-a-sql-server-service-causes-failover"></a>Problema: un error en un servicio de SQL Server provoca una conmutación por error  
 **Solución:** para evitar que errores en servicios concretos hagan que el grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] realice una conmutación por error, configure estos servicios mediante el Administrador de clústeres de Windows, de la forma siguiente:  
  
-   Desactive la casilla **Afectar al grupo** de la pestaña **Avanzadas** , en el cuadro de diálogo **Propiedades de texto completo** . No obstante, si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] causa una conmutación por error, se reiniciará el servicio de búsqueda de texto completo.  
  
### <a name="problem-sql-server-does-not-start-automatically"></a>Problema: SQL Server no se inicia automáticamente  
 **Resolución:** utilice el Administrador de clústeres de MSCS para iniciar automáticamente un clúster de conmutación por error. El servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe establecerse en inicio manual; el Administrador de clústeres debe estar configurado en MSCS para iniciar el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [Administrar servicios](https://msdn.microsoft.com/library/ms178096\(v=sql.105\).aspx).  
  
### <a name="problem-the-network-name-is-offline-and-you-cannot-connect-to-sql-server-using-tcpip"></a>Problema: el nombre de red se encuentra sin conexión y no es posible conectar con SQL Server a través de TCP/IP  
 **Problema 1:** DNS genera un error con el recurso de clúster configurado para exigir DNS.  
  
 **Solución 1:** solucione los problemas de DNS.  
  
 **Problema 2:** hay un nombre duplicado en la red.  
  
 **Solución 2:** utilice NBTSTAT para encontrar el nombre duplicado y, a continuación, solucione el problema.  
  
 **Problema 3:** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no se conecta mediante Canalizaciones con nombre.  
  
 **Solución 3:** para conectarse mediante Canalizaciones con nombre, cree un alias mediante el Administrador de configuración de SQL Server a fin de conectarse al equipo apropiado. Por ejemplo, si tiene un clúster con dos nodos (**Nodo A** y **Nodo B**) y una instancia de clúster de conmutación por error (**Virtsql**) con una instancia predeterminada, puede conectarse al servidor que tiene el recurso sin conexión Nombre de red al hacer lo siguiente:  
  
1.  Determine en qué nodo se ejecuta el grupo que contiene la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante el Administrador de clústeres. En este ejemplo, será **Node A**.  
  
2.  Inicie el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ese equipo mediante **net start**. Para obtener más información sobre cómo utilizar **net start**, consulte [Iniciar SQL Server manualmente](https://msdn.microsoft.com/library/ms191193\(v=sql.105\).aspx).  
  
3.  Inicie el Administrador de configuración de SQL Server de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en **Node A**. Consulte el nombre de la canalización donde escucha el servidor. Debe ser similar a \\\\.\\$$\VIRTSQL\pipe\sql\query.  
  
4.  En el equipo cliente, inicie el Administrador de configuración de SQL Server.  
  
5.  Cree el alias SQLTEST1 para conectarse a esta canalización a través de Canalizaciones con nombre. Para ello, use **Node A** como nombre de servidor y modifique el nombre de la canalización para que sea \\\\.\pipe\\$$\VIRTSQL\sql\query.  
  
6.  Conéctese a esta instancia utilizando el alias SQLTEST1 como nombre de servidor.  
  
### <a name="problem-sql-server-setup-fails-on-a-cluster-with-error-11001"></a>Problema: el programa de instalación de SQL Server genera el error 11001 en un clúster  
 **Problema:** hay una clave del Registro huérfana en [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.X\Cluster]  
  
 **Resolución:** asegúrese de que el subárbol del Registro de MSSQL.X no se está utilizando y, a continuación, elimine la clave del clúster.  
  
### <a name="problem-cluster-setup-error-the-installer-has-insufficient-privileges-to-access-this-directory-drivemicrosoft-sql-server-the-installation-cannot-continue-log-on-as-an-administrator-or-contact-your-system-administrator"></a>Problema: error del programa de instalación del clúster: "El instalador no dispone de privilegios suficientes para acceder a este directorio: \<drive>\Microsoft SQL Server. La instalación no puede continuar. Inicie sesión como administrador o póngase en contacto con el administrador del sistema".  
 **Problema:** este error está provocado por una unidad compartida SCSI cuyas particiones no son correctas.  
  
 **Resolución:** vuelva a crear una única partición en el disco compartido mediante estos pasos:  
  
1.  Elimine el recurso de disco del clúster.  
  
2.  Elimine todas las particiones del disco.  
  
3.  Compruebe en las propiedades del disco que se trata de un disco básico.  
  
4.  Cree una partición en el disco compartido, formatéelo y asígnele una letra de unidad.  
  
5.  Agregue el disco al clúster mediante el Administrador de clústeres (cluadmin).  
  
6.  Ejecute el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="problem-applications-fail-to-enlist-sql-server-resources-in-a-distributed-transaction"></a>Problema: las aplicaciones no consiguen dar de alta los recursos de SQL Server en una transacción distribuida  
 **Problema:** como el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] (MS DTC) no está completamente configurado en Windows, las aplicaciones puede que no consigan dar de alta los recursos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en una transacción distribuida. Este problema puede afectar a servidores vinculados, consultas distribuidas y procedimientos almacenados remotos que utilicen transacciones distribuidas. Para más configuración sobre cómo configurar MS DTC, consulte [Antes de instalar los clústeres de conmutación por error](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
 **Resolución:** para evitar este tipo de problemas, deberá habilitar totalmente los servicios MS DTC en los servidores en que esté instalado [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y esté configurado MS DTC.  
  
 Para habilitar completamente MS DTC, lleve a cabo los siguientes pasos:  
  
1.  En el Panel de control, abra **Herramientas administrativas** y, a continuación, abra **Administración de equipos**.  
  
2.  En el panel izquierdo de Administración de equipos, expanda **Servicios y Aplicaciones** y, a continuación, haga clic en **Servicios**.  
  
3.  En el panel derecho de Administración de equipos, haga clic con el botón derecho en **Coordinador de transacciones distribuidas** y seleccione **Propiedades**.  
  
4.  En la ventana **Coordinador de transacciones distribuidas** , haga clic en la pestaña **General** y, a continuación, haga clic en **Detener** para detener el servicio.  
  
5.  En la ventana **Coordinador de transacciones distribuidas** , haga clic en la pestaña **Iniciar sesión** y establezca la cuenta de inicio de sesión en NT AUTHORITY\NetworkService.  
  
6.  Haga clic en **Aplicar** y en **Aceptar** para cerrar la ventana **Coordinador de transacciones distribuidas** . Cierre la ventana **Administración de equipos** . Cierre la ventana **Herramientas administrativas** .  
  
## <a name="using-extended-stored-procedures-and-com-objects"></a>Usar procedimientos almacenados extendidos y objetos COM  
 Cuando se utilizan procedimientos almacenados extendidos con una configuración de clústeres de conmutación por error, es necesario instalar todos los procedimientos almacenados extendidos en un disco de clúster dependiente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. De esta manera, se asegura de que podrá utilizar los procedimientos almacenados extendidos aunque un nodo conmute en caso de error.  
  
 Si los procedimientos almacenados extendidos utilizan componentes COM, el administrador debe registrarlos en cada nodo del clúster. La información para cargar y ejecutar los componentes COM debe estar en el Registro del nodo activo para que los componentes se puedan crear. En caso contrario, la información permanece en el Registro del equipo donde se registraron por primera vez los componentes COM.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Cómo funcionan los procedimientos almacenados extendidos](../../../relational-databases/extended-stored-procedures-programming/how-extended-stored-procedures-work.md)   
 [Características de ejecución de los procedimientos almacenados extendidos](../../../relational-databases/extended-stored-procedures-programming/execution-characteristics-of-extended-stored-procedures.md)  
  
