---
title: Configuración de reglas de firewall antes de ejecutar el depurador de T-SQL
description: Aprenda a configurar las reglas de firewall de Windows antes de ejecutar el depurador de Transact-SQL (T-SQL) al conectarse a SQL Server en SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.error.sqlde_accessdenied
- vs.debug.error.sqlde_register_failed
- vs.debug.error.sqlde_firewall.remotemachines
helpviewer_keywords:
- Transact-SQL debugger, remote connections
- Windows Firewall [Database Engine], Transact-SQL debugger
- Transact-SQL debugger, Windows Firewall
- Transact-SQL debugger, configuring
- ports [SQL Server], Transact-SQL debugger
- TCP/IP [SQL Server], port numbers
ms.assetid: f50e0b0d-eaf0-4f4a-be83-96f5be63e7ea
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 10/20/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a55de0f9f6a6b8ae80161adaa0252b3d5c4a774
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354696"
---
# <a name="configure-firewall-rules-before-running-the-tsql-debugger"></a>Configurar reglas de firewall antes de ejecutar al depurador de TSQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Se deben configurar reglas del Firewall de Windows para habilitar la depuración en [!INCLUDE[tsql](../../includes/tsql-md.md)] cuando se establezca conexión con una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se esté ejecutando en un equipo distinto del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="configuring-the-transact-sql-debugger"></a>Configurar el depurador de Transact-SQL

El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] incluye los componentes tanto del lado servidor como del lado cliente. Los componentes del depurador del lado servidor se instalan con cada instancia del motor de base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) o versiones posteriores. Los componentes del depurador del lado cliente se incluyen:

- Cuando instala las herramientas cliente de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o posterior.

- Al instalar Microsoft Visual Studio 2010 o posterior.

- Al instalar [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] desde una descarga Web.

No existen requisitos de configuración para ejecutar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] cuando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] se ejecute en el mismo equipo que la instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. No obstante, para ejecutar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] al establecer conexión con una instancia remota del [!INCLUDE[ssDE](../../includes/ssde-md.md)], se deben habilitar reglas de programas y puertos en el Firewall de Windows de ambos equipos. Estas reglas las puede crear el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si obtiene errores al intentar abrir una sesión de depuración remota, asegúrese de que las reglas de firewall siguientes estén definidas en el equipo.

Use la aplicación **Firewall de Windows con seguridad avanzada** para administrar las reglas de firewall. Tanto en [!INCLUDE[win7](../../includes/win7-md.md)] como en [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)], abra **Panel de control**, abra **Firewall de Windows** y seleccione **Configuración avanzada**. En [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] también puede abrir **Administrador de servicios**, expandir **Configuración** en el panel izquierdo y expandir **Firewall de Windows con seguridad avanzada**.

> [!CAUTION]
>  Al habilitar reglas en Firewall de Windows, el equipo puede quedar expuesto a amenazas de seguridad que el firewall está diseñado para bloquear. Al habilitar reglas para la depuración remota se desbloquean los puertos y los programas mostrados en este tema.

## <a name="firewall-rules-on-the-server"></a>Reglas de firewall en el servidor
En el equipo en el que se ejecuta la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], use la aplicación **Firewall de Windows con seguridad avanzada** para especificar la siguiente información:

- Agregar una regla de programa de entrada para sqlservr.exe. Debe tener una regla para cada instancia que necesite admitir sesiones de depuración remota.

   1. En la opción **Firewall de Windows con seguridad avanzada** del panel izquierdo, haga clic con el botón derecho en **Reglas de entrada** y, luego, seleccione **Nueva regla** en el panel de acciones.

   2. En el cuadro de diálogo **Tipo de regla** , seleccione **Programa** y, a continuación, haga clic en **Siguiente**.

   3. En el cuadro de diálogo **Programa** , seleccione **Esta ruta de acceso del programa:** y escriba la ruta de acceso completa a sqlservr.exe para esta instancia. De forma predeterminada, sqlservr.exe se instala en C:\Archivos de programa\Microsoft SQL Server\MSSQL13.*nombreDeInstancia*\MSSQL\Binn, donde *nombreDeInstancia* es MSSQLSERVER para la instancia predeterminada y el nombre de la instancia de cualquier instancia con nombre.

   4. En el cuadro de diálogo **Acción** , seleccione **Permitir la conexión** y, a continuación, haga clic en **Siguiente**.

   5. En el cuadro de diálogo **Perfil** , seleccione los perfiles que describan el entorno de conexión del equipo cuando desee abrir una sesión de depuración con la instancia y, a continuación, haga clic en **Siguiente**.

   6. En el cuadro de diálogo **Nombre** , escriba un nombre y una descripción para esta regla, y haga clic en **Finalizar**.

   7. En la lista **Reglas de entrada** , haga clic con el botón secundario en la regla que creó y seleccione **Propiedades** en el panel de acciones.

   8. Seleccione la pestaña **Protocolos y puertos** .

   9. Seleccione **TCP** en el cuadro **Tipo de protocolo:** , seleccione **Puertos dinámicos RPC** en el cuadro **Puerto local:** , haga clic en **Aplicar** y, a continuación, haga clic en **Aceptar**.

- Agregue una regla de programa de entrada para svchost.exe con el fin de habilitar las comunicaciones DCOM desde sesiones remotas del depurador.

   1. En la opción **Firewall de Windows con seguridad avanzada** del panel izquierdo, haga clic con el botón derecho en **Reglas de entrada** y, luego, seleccione **Nueva regla** en el panel de acciones.

   2. En el cuadro de diálogo **Tipo de regla** , seleccione **Programa** y, a continuación, haga clic en **Siguiente**.

   3. En el cuadro de diálogo **Programa** , seleccione **Esta ruta de acceso del programa:** y escriba la ruta de acceso completa a svchost.exe. De forma predeterminada, svchost.exe se instala en %systemroot%\System32\svchost.exe.

   4. En el cuadro de diálogo **Acción** , seleccione **Permitir la conexión** y, a continuación, haga clic en **Siguiente**.

   5. En el cuadro de diálogo **Perfil** , seleccione los perfiles que describan el entorno de conexión del equipo cuando desee abrir una sesión de depuración con la instancia y, a continuación, haga clic en **Siguiente**.

   6. En el cuadro de diálogo **Nombre** , escriba un nombre y una descripción para esta regla, y haga clic en **Finalizar**.

   7. En la lista **Reglas de entrada** , haga clic con el botón secundario en la regla que creó y seleccione **Propiedades** en el panel de acciones.

   8. Seleccione la pestaña **Protocolos y puertos** .

   9. Seleccione **TCP** en el cuadro **Tipo de protocolo:** , seleccione **Asignador de extremos de RPC** en el cuadro **Puerto local:** , haga clic en **Aplicar** y, a continuación, haga clic en **Aceptar**.

- Si la directiva de dominio necesita que las comunicaciones de red se realicen a través de IPsec, también debe agregar reglas de entrada que abran los puertos UDP 4500 y 500.

## <a name="firewall-rules-on-the-client"></a>Reglas de firewall en el cliente

En el equipo que ejecuta el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , el programa de instalación de SQL Server o de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] puede haber configurado Firewall de Windows para permitir la depuración remota.

Si obtiene errores al intentar abrir una sesión de depuración remota, puede configurar manualmente las excepciones de programas y de puertos mediante **Firewall de Windows con seguridad avanzada** para configurar reglas de firewall:

- Agregue una entrada de programa para svchost:

   1. En la opción **Firewall de Windows con seguridad avanzada** del panel izquierdo, haga clic con el botón derecho en **Reglas de entrada** y, luego, seleccione **Nueva regla** en el panel de acciones.

   2. En el cuadro de diálogo **Tipo de regla** , seleccione **Programa** y, a continuación, haga clic en **Siguiente**.

   3. En el cuadro de diálogo **Programa** , seleccione **Esta ruta de acceso del programa:** y escriba la ruta de acceso completa a svchost.exe. De forma predeterminada, svchost.exe se instala en %systemroot%\System32\svchost.exe.

   4. En el cuadro de diálogo **Acción** , seleccione **Permitir la conexión** y, a continuación, haga clic en **Siguiente**.

   5. En el cuadro de diálogo **Perfil** , seleccione los perfiles que describan el entorno de conexión del equipo cuando desee abrir una sesión de depuración con la instancia y, a continuación, haga clic en **Siguiente**.

   6. En el cuadro de diálogo **Nombre** , escriba un nombre y una descripción para esta regla, y haga clic en **Finalizar**.

   7. En la lista **Reglas de entrada** , haga clic con el botón secundario en la regla que creó y seleccione **Propiedades** en el panel de acciones.

   8. Seleccione la pestaña **Protocolos y puertos** .

   9. Seleccione **TCP** en el cuadro **Tipo de protocolo:** , seleccione **Asignador de extremos de RPC** en el cuadro **Puerto local:** , haga clic en **Aplicar** y, a continuación, haga clic en **Aceptar**.

- Agregue una entrada de programa para la aplicación que hospeda el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si necesita abrir sesiones de depuración remota desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] en el mismo equipo, debe agregar una regla de programa para ambos:

   1. En la opción **Firewall de Windows con seguridad avanzada** del panel izquierdo, haga clic con el botón derecho en **Reglas de entrada** y, luego, seleccione **Nueva regla** en el panel de acciones.

   2. En el cuadro de diálogo **Tipo de regla** , seleccione **Programa** y, a continuación, haga clic en **Siguiente**.

   3. En el cuadro de diálogo **Programa** , seleccione **Esta ruta de acceso del programa:** y escriba uno de estos tres valores.

       - Para [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], escriba la ruta de acceso completa a ssms.exe. De forma predeterminada, ssms.exe se instala en C:\Archivos de programa (x86)\Microsoft SQL Server\130\Tools\Binn\Management Studio.

       - Para [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] , escriba la ruta de acceso completa a devenv.exe:

            1. De forma predeterminada, el archivo devenv.exe para Visual Studio 2010 se encuentra en C:\Archivos de programa (x86)\Microsoft Visual Studio 10.0\Common7\IDE.

            2. De forma predeterminada, el archivo devenv.exe para Visual Studio 2012 se encuentra en C:\Archivos de programa (x86)\Microsoft Visual Studio 11.0\Common7\IDE.

            3. Puede encontrar la ruta de acceso a ssms.exe en el acceso directo que emplea para iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puede encontrar la ruta de acceso a devenv.exe en el acceso directo que emplea para iniciar [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Haga clic con el botón secundario en el acceso directo y seleccione **Propiedades**. El archivo ejecutable y la ruta de acceso se muestran en el cuadro **Destino** .

   4. En el cuadro de diálogo **Acción** , seleccione **Permitir la conexión** y, a continuación, haga clic en **Siguiente**.

   5. En el cuadro de diálogo **Perfil** , seleccione los perfiles que describan el entorno de conexión del equipo cuando desee abrir una sesión de depuración con la instancia y, a continuación, haga clic en **Siguiente**.

   6. En el cuadro de diálogo **Nombre** , escriba un nombre y una descripción para esta regla, y haga clic en **Finalizar**.

   7. En la lista **Reglas de entrada** , haga clic con el botón secundario en la regla que creó y seleccione **Propiedades** en el panel de acciones.

   8. Seleccione la pestaña **Protocolos y puertos** .

   9. Seleccione **TCP** en el cuadro **Tipo de protocolo:** , seleccione **Puertos dinámicos RPC** en el cuadro **Puerto local:** , haga clic en **Aplicar** y, a continuación, haga clic en **Aceptar**.

## <a name="requirements-for-starting-the-debugger"></a>Requisitos para iniciar el depurador

Todos los intentos de iniciar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] deben cumplir también los requisitos siguientes:

- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] se debe ejecutar bajo una cuenta de Windows que sea miembro del rol fijo de servidor sysadmin.

- La ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se debe conectar mediante el uso de un inicio de sesión de autenticación de Windows o de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que sea miembro del rol fijo de servidor sysadmin.

- La ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] debe estar conectada a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] desde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) o posterior. No puede ejecutar el depurador cuando la ventana del Editor de consultas esté conectada a una instancia cuyo modo sea de usuario único.

- El servidor necesita comunicarse de nuevo con el cliente a través de RPC. La cuenta bajo la que se ejecuta el servicio SQL Server debe tener permisos autenticados para el cliente.

## <a name="see-also"></a>Consulte también

- [Depurador de Transact-SQL](./transact-sql-debugger.md)
- [Ejecutar el depurador de Transact-SQL](./run-the-transact-sql-debugger.md)
- [Avanzar paso a paso por el código Transact-SQL](./step-through-transact-sql-code.md)
- [Ver información del depurador de Transact-SQL](./transact-sql-debugger-information.md)
- [Editor de consultas del motor de base de datos &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)