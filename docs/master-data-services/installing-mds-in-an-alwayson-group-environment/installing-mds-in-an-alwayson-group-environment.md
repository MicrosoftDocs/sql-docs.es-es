---
title: Alta disponibilidad y recuperación ante desastres
description: Instale y Configure SQL Master Data Services en un grupo de disponibilidad de Always On para mejorar la alta disponibilidad y la recuperación ante desastres de los datos de back-end.
ms.custom: seo-lt-2019
ms.date: 07/28/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ''
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: a6cc30d907d6b8c0c14d177106da3457eb828bef
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194104"
---
# <a name="high-availability-and-disaster-recovery-for-master-data-services"></a>Alta disponibilidad y recuperación ante desastres para Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En este artículo se describe una solución de Master Data Service (MDS) hospedada en Always On configuración de grupo de disponibilidad. En el artículo se describe cómo instalar y configurar SQL 2016 Master Data Services en un grupo de disponibilidad (AG) de SQL 2016 Always On. El propósito principal de esta solución es mejorar la alta disponibilidad y la recuperación ante desastres de datos de back-end MDS hospedados en una base de datos de SQL Server.

## <a name="introduction"></a>Introducción

En este artículo se describe una solución de Master Data Service (MDS) hospedada en una configuración de grupo de disponibilidad Always On. En el artículo se describe cómo instalar y configurar SQL 2016 MDS en un grupo de disponibilidad (AG) de SQL 2016 Always On. El propósito principal de esta solución es mejorar la alta disponibilidad y la recuperación ante desastres de datos de back-end MDS hospedados en una base de datos de SQL Server.

Para implementar la solución, debe llevar a cabo las tareas que se describen en este artículo.

1. [Instalación y configuración del Clúster de conmutación por error de Windows Server (WSFC)](#windows-server-failover-cluster-wsfc).

2. [Configure un grupo de disponibilidad de Always on](#sql-server-always-on-availability-group).

3. [Configuración de MDS para que se ejecute en un nodo de WSFC](#configure-mds-to-run-on-an-wsfc-node).

En las secciones anteriores se presentan brevemente las tecnologías, seguidas de las correspondientes instrucciones. Para obtener información detallada sobre las tecnologías, consulte los documentos que se indican en cada sección.

Esta solución que se describe en este artículo se basa en un AG, en la que cada base de datos tiene varias réplicas sincrónicas o asincrónicas. Solo una réplica acepta la transacción (acepta las solicitudes del usuario). Se trata de la réplica principal.

Cada réplica tiene su propio almacenamiento, por lo que en esta solución no hay ningún almacenamiento compartido centralizado. Si se produce un error de software o de hardware que afecta a la réplica principal, se puede producir una conmutación por error a una réplica sincrónica o asincrónica, ya sea automática o manualmente en función de la configuración y de las situaciones concretas. De esta manera se garantiza la alta disponibilidad de la base de datos con una interrupción mínima para los usuarios.

Las réplicas asincrónicas se suelen hospedar en un centro de datos ajeno al centro de datos de la réplica principal. En el caso de escenarios de desastre, se puede producir una conmutación por error de la réplica principal a otro centro de datos. De esta manera se garantiza la recuperación ante desastres de la base de datos.

Con fines de demostración, la solución descrita en este artículo usa las siguientes versiones de software. Las versiones anteriores deberían funcionar igual con diferencias, en principio, mínimas.

- Windows Server 2012R2 con clúster de conmutación por error de Windows Server
- SQL Server 2016 con la característica Master Data Services

Además, la solución usa dos máquinas virtuales, **MDS-HA1** y **MDS-HA2**, para hospedar dos réplicas. Siempre que sea compatible con un AG, MDS no limita el número de réplicas que puede usar.

En este artículo se supone que tiene conocimientos básicos sobre Windows Server, el clúster de conmutación por error de Windows Server, AG y SQL Server MDS.

## <a name="what-is-not-covered"></a>Qué no se trata

En este documento no se trata lo siguiente:

- Cómo hacer que IIS, el servidor web que hospeda la interfaz de usuario de Master Data Services, tenga una alta disponibilidad y se pueda recuperar después de un desastre. MDS no impone ningún requisito concreto en IIS, por lo que también pueden funcionar las técnicas habituales para que IIS tenga una alta disponibilidad y un equilibrio de carga.

- Cómo usar un SQL Server Always On instancia de clúster de conmutación por error (FCI) para admitir la alta disponibilidad (HA) en el back-end de MDS. Los clústeres de conmutación por error de SQL Server representan otra solución de alta disponibilidad y son compatibles de forma oficial con SQL Server (y funcionan con MDS).

- Cómo usar una solución híbrida de una FCI y un AG para admitir la alta disponibilidad en el back-end de MDS. La solución híbrida funciona con MDS.

## <a name="design-consideration"></a>Consideraciones de diseño

En la figura 1 se muestra una configuración típica usada principalmente en un AG. En el centro de datos principal hay dos réplicas con una relación de confirmación sincrónica, y ambas réplicas tienen el privilegio VOTE. Se usa básicamente para mejorar la alta disponibilidad en caso de que se produzca un error en la réplica principal.

En el centro de datos de recuperación ante desastres hay una réplica secundaria con una relación de confirmación asincrónica con la principal. Este centro de datos suele estar ubicado en una región geográfica diferente del centro de datos principal. La réplica secundaria no tiene el privilegio VOTE.

Esta configuración se usa para lograr la recuperación en caso de que el centro de datos principal esté en un desastre, como un incendio, un terremoto, etc. La configuración consigue alta disponibilidad y recuperación ante desastres con un costo relativamente bajo.

![Configuración típica de un grupo de disponibilidad de Always On](media/Fig1_TypicalConfig.png)

Figura 1. Una configuración de grupo de disponibilidad de Always On típica

Si no tiene que considerar la posibilidad de implantar la recuperación ante desastres, no es necesario tener una réplica en un segundo centro de datos. Si necesita mejorar la alta disponibilidad, podría tener más réplicas sincrónicas en el mismo centro de datos principal,

por lo que es importante pensar en los escenarios y requisitos, elegir cuántas réplicas sincrónicas y asincrónicas necesitará y en qué centro de datos debería ubicarlas.

## <a name="windows-server-failover-cluster-wsfc"></a>Clúster de conmutación por error de Windows Server (WSFC)

En esta sección se tratan las siguientes tareas.

1. [Instalación de la característica del Clúster de conmutación por error de Windows](#install-failover-cluster-feature).

2. [Cree un clúster de conmutación por error de Windows Server](#create-a-windows-server-failover-cluster).

Como se muestra en la figura 1 de la sección anterior, la solución descrita en este artículo incluye el Clúster de conmutación por error de Windows Server (WSFC). Necesitamos configurar WSFC porque AG depende de WFSC para la detección de errores y la conmutación por error.

WSFC es una característica que sirve para mejorar la alta disponibilidad de aplicaciones y servicios. Consta de un grupo de instancias independientes de Windows Server, donde se ejecuta el Servicio de clúster de conmutación por error de Microsoft. Las instancias de Windows Server (o, como se denominan a veces, "nodos") están conectadas para que se puedan comunicar entre ellas y se puedan detectar errores. WSFC ofrece funcionalidades de detección de errores y de conmutación por error. Si se produce un error en un nodo o servicio del clúster, se detectará el error y otro nodo empezará a proporcionar de forma automática o manual los servicios hospedados en el nodo erróneo. Por lo tanto, los usuarios solo experimentarán una interrupción mínima en los servicios y se mejorará la disponibilidad de estos.  

### <a name="prerequisites"></a>Prerrequisitos

El sistema operativo Windows Server debe estar instalado en todas las instancias y se debe haber revisado todas las actualizaciones.

> [!NOTE]
> Se **recomienda encarecidamente** que instale la misma versión de Windows y el mismo conjunto de características en todas las instancias para evitar posibles problemas de incompatibilidad.

### <a name="install-failover-cluster-feature"></a>Instalar la característica del Clúster de conmutación por error

Siga estos pasos para cada instancia de Windows Server para instalar la característica WSFC en cada instancia. Necesitará permisos de administrador.

1. Abra el **Administrador del servidor** en Windows Server y haga clic en **Agregar roles y características** en el panel derecho. Se iniciará el **Asistente para agregar roles y características**.

2. Haga clic en **Siguiente** hasta que llegue a la página **Características**.

3. Marque la casilla **Clúster de conmutación por error** y haga clic en **Siguiente** para finalizar la instalación. Vea la figura 2.

   Si se le pide confirmación para **Agregar las características necesarias para la agrupación en clústeres de conmutación por error**, haga clic en **Agregar características**. Vea la figura 3.

   ![Asistente para agregar roles y características, clústeres de conmutación por error](media/Fig2_SelectFeatures.png)

   Figura 2

   ![Asistente para agregar roles y características, necesario para los clústeres de conmutación por error](media/Fig3_RequiredFeaturesFailover.png)

   Figura 3

4. En la página **Confirmación**, haga clic en **Instalar** para instalar la característica de clústeres de conmutación por error.

5. En la página **Resultado**, asegúrese de que todo se haya instalado correctamente, sin errores ni advertencias.

### <a name="create-a-windows-server-failover-cluster"></a>Crear un clúster de conmutación por error de Windows Server

Cuando haya instalado la característica WSFC en todas las instancias, podrá configurar WSFC. En principio solo tiene que hacer esto en un nodo.

1. Abra el **Administrador del servidor** en Windows Server y haga clic en **Administrador de clústeres de conmutación por error** en el menú **Herramienta**, situado en la esquina superior derecha, para iniciar el administrador.

2. En **Administrador de clústeres de conmutación por error**, haga clic en **Validar configuración** en el panel derecho. Vea la figura 4.

   ![Administrador de clústeres de conmutación por error, Validar configuración](media/Fig4_ValidateConfig.png)

   Figura 4

3. En el **Asistente para validar una** **configuración**, haga clic en **Siguiente**.

4. En el cuadro de diálogo **Seleccionar servidores o un clúster**, agregue los nombres de los servidores que hospedarán SQL Server y haga clic en **Siguiente**. Vea la figura 5.

   En este ejemplo se han agregado dos instancias, MDS-HA1 y MDS-HA2.

   ![Asistente para validar una configuración, página Seleccionar servidores o un clúster](media/Fig5_AddServer.png)

   Figura 5

5. En la página **Opciones de pruebas**, haga clic en **Ejecutar todas las pruebas** y en **Siguiente**.

6. Haga clic en **Siguiente** para finalizar la validación.

   En la página **Validando** se muestra el progreso, mientras que en la página **Resumen** se muestra el resumen de la validación. Vea las figuras 6 y 7.

7. En la página **Resumen**, busque posibles mensajes de advertencia o de error.

   Los errores se deben corregir, pero las advertencias puede que no supongan ningún problema. Un mensaje de advertencia indica que "el elemento probado podría cumplir el requisito, pero hay algo que se debe revisar". Por ejemplo, en la figura 7 se muestra el mensaje de advertencia "Validar latencia de acceso a disco", que puede deberse a que el disco está ocupado temporalmente con otras tareas, y puede ignorarlo. Debe consultar la documentación en línea de cada mensaje de advertencia y de error para obtener más detalles. Vea la figura 7.
   
   ![Asistente para validar una configuración, página Validando](media/Fig6_ValidationTests.png)

   Ilustración 6.

   ![Asistente para validar una configuración, página Resumen](media/Fig7_ValidationSummary.png)

   Figura 7

8. En la página **Resumen**, compruebe que la casilla **Crear el clúster ahora con los nodos validados…** está seleccionada y, luego, haga clic en **Finalizar** para iniciar el **Asistente para crear** **clústeres**.

9. En el **Asistente para crear** **clústeres**, haga clic en **Siguiente**.

10. En la página **Punto de acceso para administrar el clúster**, escriba el nombre del clúster WSFC y haga clic en **Siguiente**. En este ejemplo se usa "MDS-HA" como nombre del clúster. Vea la figura 8.

   ![Escribir el nombre del clúster](media/Fig8_EnterClusterName.png)

   Figura 8

11. Haga clic en **Siguiente** para terminar de crear el clúster. En la sección **Resumen del clúster MDS-HA** se muestra la información del clúster. Vea la figura 9.

   ![Ver información de resumen del clúster](media/Fig9_ClusterSummary.png)

   Figura 9

   Si más tarde necesita agregar un nodo, haga clic en la acción **Agregar nodo** en el panel derecho de **Administrador de clústeres de conmutación por error**.

Notas:

- Es posible que la característica WSFC no esté disponible en todas las ediciones de Windows Server. Asegúrese de que su edición cuenta con esta característica.

- Asegúrese de que cuenta con los permisos adecuados para configurar WSFC en Active Directory. Si hay algún problema, vea [Failover Cluster Step-by-Step Guide: Configure Accounts in Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002(v=ws.10)) (Guía paso a paso de clústeres de conmutación por error: Configurar cuentas en Active Directory).

Para obtener más información detallada sobre WSFC, vea [Failover Clusters](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732488(v=ws.10)) (Clústeres de conmutación por error).

## <a name="sql-server-always-on-availability-group"></a>SQL Server Always On grupo de disponibilidad

En esta sección se tratan las siguientes tareas.

1. [Habilitar SQL Server Always on grupo de disponibilidad](#enable-sql-server-always-on-availability-groups-on-every-sql-server-instance).

2. [Cree un grupo de disponibilidad](#create-an-availability-group).

3. [Validación y prueba del grupo de disponibilidad](#validation-and-test-the-availability-group).

Always On tiene dos características para proporcionar alta disponibilidad y recuperación ante desastres para MDS, ambas se basan en WSFC.

- Grupo de disponibilidad de Always On (AG)

- Always On instancia de clúster de conmutación por error (FCI).

Un AG proporciona disponibilidad en el nivel de base de datos. Los grupos de disponibilidad (un conjunto de bases de datos de usuario) y su nombre de red virtual se registran como recursos en WSFC.

FCI proporcionan alta disponibilidad en el nivel de instancia. El servicio de SQL Server y sus servicios relacionados se registran como recursos en WSFC. Además, la solución FCI requiere un almacenamiento en disco compartido simétrico, como los recursos compartidos de archivos SAN o SMB, que tienen que estar disponibles para todos los nodos en el clúster de WFC.
   
### <a name="prerequisites"></a>Prerrequisitos

- Instale SQL Server en todos los nodos. Para obtener más información, vea [Instalar SQL Server 2016](../../database-engine/install-windows/install-sql-server.md).

- (Recomendado) Instale exactamente la misma versión y el mismo conjunto de características de SQL Server en todos los nodos. En concreto, MDS debe estar instalado.

- (Recomendado) Use la misma configuración en todas las instancias de SQL Server. En concreto, se debe configurar la misma intercalación del servidor en todas las instancias de SQL Server.

- (Recomendado) Use la misma cuenta de servicio para ejecutar todas las instancias de SQL Server. Si no lo hace, tendrá que conceder permiso en cada instancia de SQL Server para garantizar que las instancias de SQL Server puedan comunicarse entre sí.

- Confirme que la configuración del firewall de Windows permite que las instancias de SQL Server se comuniquen entre sí.

### <a name="enable-sql-server-always-on-availability-groups-on-every-sql-server-instance"></a>Habilitar SQL Server Always On grupos de disponibilidad en cada instancia de SQL Server

1. En el **Administrador de configuración de SQL Server**, haga clic en **Servicio SQL Server** en el panel izquierdo, haga clic con el botón derecho en **SQL Server** en el panel derecho y, luego, haga clic en **Propiedades**. Vea la figura 10.

   ![Ventana Propiedades de SQL Server](media/Fig10_SQLServerProperties.png)

   Figura 10

2. En el cuadro de diálogo **propiedades** de **SQL Server (MSSQLSERVER)** , haga clic en la pestaña **Always on alta disponibilidad** y, a continuación, active la casilla **Habilitar grupos de disponibilidad de Always on** . Cuando se muestre un valor en el cuadro de texto **Nombre del clúster de conmutación por error de Windows**, haga clic en **Aceptar** para continuar. Vea la figura 11.

   ![Habilitar Always On opción de grupos de disponibilidad](media/Fig11_EnableAlwaysOn.png)

   Figura 11

3. Cuando aparezca una página de advertencia, haga clic en **Aceptar** para continuar. Vea la figura 12.

   ![Confirmar para detener y reiniciar el servicio](media/Fig12_WarningServiceStopStart.png)

   Figura 12

4. Haga clic en **Reiniciar** para reiniciar el servicio **SQL Server** y hacer que este cambio surta efecto. Vea la figura 10.

> [!NOTE]
> Puede cambiar la cuenta de servicio que se ejecuta en el servicio SQL Server mediante el **Administrador de configuración de SQL Server**. Haga clic en la pestaña **Iniciar sesión** en el cuadro de diálogo **Propiedades de SQL Server** **(MSSQLSERVER)**. Vea la figura 11.

### <a name="create-an-availability-group"></a>Creación de un grupo de disponibilidad

Una vez habilitada la característica AG en todas las instancias de SQL Server, se crea un nuevo AG que contiene la base de datos de MDS en un nodo.

El grupo de disponibilidad solo se puede crear en bases de datos existentes. Así pues, debe crear una base de datos de MDS en un nodo o una base de datos temporal y luego quitarla. En este ejemplo se crea una base de datos de MDS vacía y un grupo de disponibilidad en esta base de datos de MDS.

1. Inicie **SQL Server Management Studio** (**SSMS**) en un nodo y conéctese a la instancia local de SQL Server con las credenciales adecuadas.

2. En SSMS, abra una ventana de **nueva consulta** y ejecute el siguiente script para crear una base de datos vacía. Reemplace C:\\temp por la ubicación que quiera usar para realizar una copia de seguridad completa.

   ```sql
   CREATE DATABASE MDS\_Sample
   GO
   BACKUP DATABASE MDS\_Sample TO DISK='C:\\temp'
   GO
   ```

   > [!NOTE]
   > Es necesario realizar una copia de seguridad completa de la base de datos para crear el grupo de disponibilidad en esta base de datos.

3. En el **Explorador de objetos**, expanda la carpeta **Always on alta disponibilidad** y haga clic en **Asistente para nuevo grupo** de disponibilidad para iniciar el **Asistente para nuevo grupo de disponibilidad**. Vea la figura 13.

   ![Iniciar el Asistente para nuevo grupo de disponibilidad](media/Fig13_AvailabilityGroupsFolder.png)

   Figura 13

4. En el **Asistente para nuevo grupo de disponibilidad**, haga clic en **Siguiente** y se mostrará la página **Especificar nombre de grupo de disponibilidad**. Escriba un nombre para el grupo de disponibilidad y haga clic en **Siguiente**. Vea la figura 14.

   ![Escribir el nombre del grupo de disponibilidad](media/Fig14_AvailabilityGroupName.png)

   Figura 14

5. Haga clic en la base de datos que acaba de crear en la página **Seleccionar bases de datos** y, luego, haga clic en **Siguiente**. Vea la figura 15.

   ![Seleccione la base de datos](media/Fig15_AvailabilityGroupSelectDatabase.png)

   Figura 15

6. En la página **Especificar réplicas**, agregue otra réplica haciendo clic en **Agregar réplica**. En esta página ya aparecen como réplica las instancias locales actuales de SQL Server. Vea la figura 16.

7. En el cuadro de diálogo **Conectar al servidor**, agregue las credenciales adecuadas y haga clic en **Conectar**.

   ![Conectarse a una instancia de SQL Server](media/Fig16_AddReplicaConnectServer.png)

   Figura 16

   Ahora debería ver dos réplicas en la lista. Repita este paso para agregar otros nodos como réplicas. Vea la figura 17.

   ![Ver la lista de réplicas](media/Fig17_AvailabilityGroupSQLReplicas.png)

   Figura 17

   Para cada réplica, configure las opciones **Confirmación sincrónica**, **Conmutación automática por error** y **Secundaria legible**. Vea la figura 17.

**Confirmación sincrónica**: Garantiza que, si se confirma una transacción en la réplica principal de una base de datos, también se confirme en las demás réplicas sincrónicas. La confirmación asincrónica no lo garantiza y podría ir a la zaga de la réplica principal.

Normalmente debe habilitar la confirmación sincrónica solo si ambos nodos están en el mismo centro de datos. Si se encuentran en centros de datos diferentes, la confirmación sincrónica podría ralentizar el rendimiento de la base de datos. Si no se marca esta casilla, se usará la confirmación asincrónica.

**Conmutación automática por error:** Si la réplica principal está inactiva, el grupo de disponibilidad efectuará automáticamente una conmutación por error a su réplica secundaria cuando se seleccione la conmutación automática por error. Solo se puede habilitar en las réplicas que tienen confirmaciones sincrónicas.

**Secundaria legible:** De forma predeterminada, los usuarios no se pueden conectar a ninguna réplica secundaria. Con esta opción, los usuarios podrán conectarse a la réplica secundaria con acceso de solo lectura.

8. En la página **Especificar réplicas**, haga clic en la pestaña **Agente de escucha** y haga lo siguiente. Vea la figura 18.

   a. Haga clic en **Crear un agente de escucha del grupo de disponibilidad** para configurar un agente de escucha del grupo de disponibilidad para la conexión de base de datos de MDS.

   b. Escriba un **nombre DNS del agente de escucha**, como MDSSQLServer.

   c. Escriba el puerto SQL predeterminado, 1433, en el cuadro de texto **Puerto**.

   d. Escriba DHCP en el cuadro de texto **Modo de red** y haga clic en **Siguiente** para continuar.

   > [!NOTE]
   > Opcionalmente, puede elegir "IP estática" como modo de **red** y especificar una dirección IP estática. También puede especificar un puerto que no sea el puerto 1433.

   ![Configurar el agente de escucha](media/Fig18_AvailabilityGroupCreateListener.png)

   Figura 18

9. En la página **Seleccionar sincronización de datos**, haga clic en **Completa** y especifique un recurso compartido de red al que puedan tener acceso todos los nodos. Haga clic en **Siguiente** para continuar. Vea la figura 19.

   Este recurso compartido de red se usará para almacenar la copia de seguridad de la base de datos para crear réplicas secundarias. Si no está disponible en su organización, elija otra preferencia de sincronización de datos. Consulte [SQL Server 2016 Always on grupo de disponibilidad](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) sobre cómo usar otras opciones para crear réplicas secundarias. En la figura 17 también se muestran otras opciones.

   ![Configurar la sincronización de datos](media/Fig19_AvailabilityGroupDataSync.png)

   Figura 19 

10. En la página **Validación**, asegúrese de que todas las validaciones se efectúen correctamente y corrija los posibles errores. Haga clic en **Siguiente** para continuar.

11. En la página **Resumen**, revise todos los valores de configuración y haga clic en **Finalizar**. De esta forma se creará y configurará el grupo de disponibilidad.

12. En la página **Resultado**, confirme que se han seguido todos los pasos necesarios.

### <a name="validation-and-test-the-availability-group"></a>Validación y prueba del grupo de disponibilidad

1. Abra SSMS y conéctese al nombre DNS del agente de escucha que acaba de crear en la sección [Creación de un grupo de disponibilidad](#create-an-availability-group). En este ejemplo es MDSSQLServer.

2. En **Explorador de objetos**, expanda la carpeta **Always on alta disponibilidad** , haga clic con el botón secundario en el AG que acaba de crear en la sección [creación de un grupo de disponibilidad](#create-an-availability-group) y, a continuación, haga clic en **Mostrar panel**. Vea la figura 20. Aparecerá el estado del grupo de disponibilidad nuevo y sus réplicas.

   ![Ver el panel](media/Fig20_ShowDashboard.png)

   Figura 20 

3. Haga clic en **Conmutación por error** para efectuar una conmutación por error a una réplica sincrónica y a una réplica asincrónica. Esto sirve para comprobar que la conmutación por error se efectúa correctamente sin ningún problema.

   Se completó la instalación de AG.

Para obtener más información sobre los grupos de disponibilidad de Always On, consulte [SQL Server 2016 Always on grupos de disponibilidad](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).

## <a name="configure-mds-to-run-on-an-wsfc-node"></a>Configuración de MDS para que se ejecute en un nodo de WSFC

La solución presentada en este artículo solo requiere que la base de datos de back-end de MDS se ejecute en WSFC. Otros elementos de MDS, como las aplicaciones web y el Administrador de configuración de MDS, se pueden ejecutar en el nodo en WSFC o fuera de WSFC, siempre y cuando MDS se pueda conectar al grupo de disponibilidad.

1. Abra el **Administrador de configuración de Master Data Services** en un nodo, haga clic en **Configuración de base de datos** y, luego, haga clic en **Crear base de datos** para iniciar el **Asistente para crear bases de datos**.

2. En la página **Servidor de bases de datos**, escriba el nombre DNS del agente de escucha del grupo de disponibilidad en el cuadro de texto **Instancia de SQL Server**, haga clic en **Probar conexión** y en **Siguiente**. Vea la figura 21.

   ![Configurar el servidor de bases de datos con el agente de escucha del grupo de disponibilidad](media/Fig21_MDSDatabaseServerListener.png)

   Figura 21

3. En la página **Base de datos**, escriba el nombre de la base de datos que creó en la sección [Creación de un grupo de disponibilidad](#create-an-availability-group) y haga clic en **Siguiente**. Vea la figura 22.

   ![Crear y configurar la base de datos](media/Fig22_MDSCreateDatabase.png)

   Figura 22

4. Siga los pasos del **Asistente para crear** **bases de datos**. Para más información, vea [Instalación y configuración de Master Data Services](../master-data-services-installation-and-configuration.md).

5. Haga clic en **Aplicaciones web** en el **Administrador de configuración de Master Data Services** para configurar la aplicación web y, luego, haga clic en **Aplicar** para aplicar la configuración a MDS. Vea la figura 23. Para más información, vea [Instalación y configuración de Master Data Services](../master-data-services-installation-and-configuration.md).

   ![Configurar la aplicación web](media/Fig23_MDSWebApplication.png)

   Figura 23

   La configuración de MDS ha finalizado. Puede repetir los pasos anteriores para configurar MDS para que se ejecute en todos los nodos. La base de datos back-end es la misma en el mismo grupo de disponibilidad.

6. Si anteriormente creó una base de datos temporal (consulte la sección [creación de un grupo de disponibilidad](#create-an-availability-group) ) para crear un AG, debe quitar la base de datos temporal

   Para más información sobre Master Data Services, vea [Master Data Services](../master-data-services-overview-mds.md).

## <a name="conclusion"></a>Conclusión

En estas notas del producto, hemos aprendido a configurar y configurar la base de datos de back-end de Master Data Services como parte de un AG. Esta configuración ofrece alta disponibilidad y recuperación ante desastres en la base de datos back-end de Master Data Services. Para implementar esta configuración, debe instalar y configurar el clúster de conmutación por error de Windows Server, AG y Master Data Services.

## <a name="feedback-comments"></a>Comentarios sobre comentarios

¿Le ha resultado útil este documento? Envíenos sus comentarios haciendo clic en **Comentarios** en la parte superior del artículo. 

Sus comentarios nos ayudarán a mejorar la calidad de las notas del producto que publiquemos.