---
title: Notas de la versión de SQL Server Data Tools (SSDT)
description: Vea las notas de la versión de todas las versiones de SQL Server Data Tools (SSDT) que funcionan con Visual Studio 2017 y versiones anteriores.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b071f8b8-c8e5-44e0-bbb6-04804dd1863a
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/15/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 8a13f35c56ab73e3bb984bbdc2c2e27c2d769356
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236801"
---
# <a name="release-notes-for-sql-server-data-tools-ssdt"></a>Notas de la versión de SQL Server Data Tools (SSDT)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Estas notas de la versión son para [SQL Server Data Tools (SSDT)](download-sql-server-data-tools-ssdt.md) para Visual Studio (VS).

<!--
Hello.  We have switched to a newer standardized format for Release Notes articles.
Basically we have switched from bullet lists to a 2-column table.
And we have shortened the H2 titles, to reduce wrapping in the rightNav.
And we have renamed the .md file to the standard format, from 'changelog-for-sql-server-data-tools-ssdt.md' to 'release-notes-ssdt.md'.
The presently latest H2 section (## 15.9.0) has been converted to the new format.
But the older sections are too numerous and long to warrant conversion.

REQUEST_1:  Please use the newer 2-column table format from now onward, for each new release section.

REQUEST_2:  Please consider whether it is time to erase perhaps the oldest 25% of these H2 sections.
Or maybe move all but the latest 9 (of 25) H2 sections to a new file perhaps named 'release-notes-history-ssdt.md', and link to it from the bottom of this file.

For questions, contact CraigG or SStein or GeneMi.

GeneMi , 2019/03/22.

P.S.  there's no need to keep this large HTML comment indefinitely.
-->

## <a name="1597nbsp-ssdt-for-vs-2017"></a>15.9.7,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 5 de enero de 2021  
_Número de compilación:_ &nbsp; 14.0.16228.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | La creación de SSISDB pasó a ser opcional en el Asistente para la creación de IR. |
| Integration Services (SSIS) | Se solucionó un problema por el cual los elementos del cuadro combinado de suscripción de Azure estaban duplicados en el Asistente para la creación de IR y el Asistente para proyectos habilitados para Azure cuando suscripciones distintas tenían el mismo nombre. |
| Integration Services (SSIS) | Se corrigió un problema que a veces el botón Conectar no se puede habilitar en el Asistente para la creación de IR. |
| Integration Services (SSIS) | Se solucionó un problema por el cual los elementos del cuadro combinado de suscripción de Azure estaban duplicados en el Asistente para la creación de IR y el Asistente para proyectos habilitados para Azure cuando suscripciones distintas tenían el mismo nombre. |
| Integration Services (SSIS) | Se corrigió un problema que hacía que el código generado automáticamente bajo bufferwrapper.cs del componente de script agregaba comillas dobles adicionales cuando la configuración regional actual era Alemania. |
| Integration Services (SSIS) | Se corrigió un problema en que el botón Descargar WSDL no aparecía cuando la versión del servidor de destino era SQL Server 2012, 2014, 2016. |
| Integration Services (SSIS) | Se corrigió un problema en el que se generaba un error en la compilación de proyectos grandes debido a una excepción de memoria insuficiente. |
| Integration Services (SSIS) | Se corrigió un problema en el que el paquete no se cambiaba a la versión de servidor de destino actual del proyecto cuando se guardaba como copia en el sistema de archivos o MSDB en el modelo de implementación de paquetes. |
| Integration Services (SSIS) | Se corrigió un problema en el que el destino de procesamiento de dimensiones no funcionaba debido a un error de tipo "No se admite dicha interfaz". |
| Integration Services (SSIS) | Se corrigieron algunos problemas relacionados con la accesibilidad y la configuración elevada de ppp. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| &nbsp; | &nbsp; |


## <a name="1596nbsp-ssdt-for-vs-2017"></a>15.9.6,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 31 de agosto de 2020  
_Número de compilación:_ &nbsp; 14.0.16222.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Se ha corregido un problema que hacía que el botón **Vista previa** no funcionara en el origen de OLE DB al conectarse a un origen de datos de SQL Server Analysis Services (SSAS). |
| Integration Services (SSIS) | Se ha corregido un problema que hacía que la eliminación de una entrada o salida de un componente de flujo de datos antes de la de la ruta de acceso asociada diera lugar a un error COMException. |
| Integration Services (SSIS) | Se ha corregido un problema que hacía que la tarea de procesamiento de SSAS no se pudiera conectar al área de trabajo de Power BI y actualizar sus modelos. |
| Integration Services (SSIS) | Se ha corregido un problema que hacía que Visual Studio no respondiera al depurar el componente o la tarea de script al usar el tiempo de ejecución x64 y cuando el destino es SQL Server 2017. |
| Integration Services (SSIS) | Se ha corregido un problema que hacía que el asistente para importación y exportación se bloqueara al seleccionar un controlador de MySQL en algunos entornos. |
| Integration Services (SSIS) | Se han corregido algunos problemas relacionados con la accesibilidad y la configuración elevada de ppp. |
| Integration Services (SSIS) | Permita a los usuarios omitir la validación al abrir paquetes, lo que mejora el rendimiento. Para obtener más información, consulte [Accelerate opening SSIS packages in SSDT](https://techcommunity.microsoft.com/t5/sql-server-integration-services/accelerate-the-opening-of-ssis-package-in-ssdt/ba-p/1607099) (Acelerar la apertura de paquetes SSIS en SSDT). |
| Integration Services (SSIS) | Bloquee la implementación en Azure-SSIS cuando la versión del servidor de destino no sea SQL Server 2017. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1595nbsp-ssdt-for-vs-2017"></a>15.9.5,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 27 de mayo de 2020  
_Número de compilación:_ &nbsp; 14.0.16218.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Se ha agregado compatibilidad para buscar tareas y componentes de canalización mediante la adición de un cuadro de búsqueda al cuadro de herramientas de SSIS. |
| Integration Services (SSIS) | Se ha agregado una barra de progreso al cambiar la versión del servidor de destino. |
| Integration Services (SSIS) | Se ha agregado una configuración de nube adicional para el proyecto habilitado para Azure y compatibilidad con la autenticación de Windows para ejecutar paquetes en Azure. |
| Integration Services (SSIS) | Se ha agregado evaluación para los paquetes que se van a ejecutar en Azure en el proyecto habilitado para Azure. |
| Integration Services (SSIS) | Se ha corregido un problema que, en algunos casos, impedía la enumeración de instancias de VS en el instalador |
| Integration Services (SSIS) | Se ha corregido un problema que impedía la desinstalación de esta producción si se ha desinstalado la instancia de VS. |
| Integration Services (SSIS) | Se ha corregido un problema por el que un componente de script copiado de otro en el mismo paquete no se puede cargar correctamente durante la depuración cuando la versión del servidor de destino es inferior a SQL Server 2019. |
| Integration Services (SSIS) | Se ha corregido un problema de accesibilidad por el que las proporciones de luminosidad para las líneas de conector de componentes son inferiores a 3:1 en la ventana del diseñador de paquetes. |
| Integration Services (SSIS) | Se ha corregido un problema de accesibilidad por el que relación de luminosidad es menor a 3:1 para el control "Ajustar la vista a la ventana" presente en la ventana del diseñador de paquetes. |
| Integration Services (SSIS) | Se ha corregido un problema que impide que la tarea Transferir bases de datos funcione cuando una base de datos tiene grupos de archivos que contienen una secuencia de archivos. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que, al usar componentes de ODBC en el componente de bucle Foreach, el componente de ODBC encontrará el "Error en la secuencia de función" en el segundo bucle durante la ejecución del paquete. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que la interfaz de usuario de la tarea Volver a generar índice se recortara en modo de baja resolución. |
| Integration Services (SSIS) | Se ha corregido un problema por el que el botón "Iniciar sesión" no aparece en modo de PPP alto. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que los elementos del administrador de conexiones se muestren demasiado grandes en modo de PPP alto. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que los resultados de la ejecución se apilen entre sí en modo de PPP alto. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado | &nbsp; |
| Cuando el destino es SQL Server 2017 y SxS con SQL Server 2017 revisado con CU19 o una versión posterior, la depuración de paquetes que contienen tareas o componentes de script con puntos de interrupción se bloquea si Run64BitRuntime está establecido en true. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1594nbsp-ssdt-for-vs-2017"></a>15.9.4,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 26 de marzo de 2020  
_Número de compilación:_ &nbsp; 14.0.16214.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Se ha corregido un problema que podía bloquear VS al mover las líneas de restricción de flujo de control dentro de un contenedor. |
| Integration Services (SSIS) | Se ha corregido un problema que impedía que la interfaz de usuario de las tareas del plan de mantenimiento no pudiera enumerar los administradores de conexión de ADO.NET creados fuera de la interfaz de usuario. |
| Integration Services (SSIS) | Se ha corregido un problema que impedía que la página de inicio de sesión interactivo de Azure no apareciera al implementar un proyecto de SSAS, que pertenece a una solución que también tiene proyectos de SSIS cargados. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que al hacer clic en el botón Propiedades del controlador MSOLAP se bloqueara el Asistente de DTS cuando no está instalado SQL Server. |
| Integration Services (SSIS) | Se ha corregido un problema que provocaba que el controlador MSOLEDBSQL no admitiera la autenticación de AAD en el Asistente para DTS. |
| Integration Services (SSIS) | Se ha corregido un problema por el que el origen XML y el destino de ADO.NET no se puedan conservar correctamente cuando el destino es SQL Server 2012. |
| Integration Services (SSIS) | Se ha corregido un problema por el que el botón "Descargar WSDL" del Editor de la tarea Servicio web podría no mostrarse correctamente. |
| Integration Services (SSIS) | Se ha corregido un problema por el que la tabla no se podía seleccionar en la página Administrador de conexiones del Editor de transformación Búsqueda. |
| Integration Services (SSIS) | Se ha corregido un problema por el que el diseño del Editor de transformación Caché podía estar desordenado. |
| Integration Services (SSIS) | Se ha corregido un problema por el que el área "Administradores de conexiones" del editor de paquetes podía no mostrarse correctamente. |
| Integration Services (SSIS) | Se ha corregido un problema por el que era posible que el icono de estado no se mostrase correctamente en el Asistente para convertir al modelo de implementación de paquetes. |
| Integration Services (SSIS) | Se ha cambiado el instalador al instalador completo que no requiere descargar la carga desde Internet. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1593nbsp-ssdt-for-vs-2017"></a>15.9.3,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 3 de enero de 2020  
_Número de compilación:_ &nbsp; 14.0.16203.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Se quitó el componente de bandeja de entrada Origen de Power Query para SQL Server 2017. Ahora anunciamos el Origen de Power Query de SQL Server 2017 y 2019 como componente estándar, que se puede descargar [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=100619). |
| Integration Services (SSIS) | Se quitó el componente de bandeja de entrada Conector de Microsoft para Oracle para SQL Server 2019. Ahora anunciamos el Conector de Microsoft para Oracle para SQL Server 2019 como componente estándar, que se puede descargar [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=58228). |
| Integration Services (SSIS) | Se corrigió un problema por el cual el depurador de SSIS no se podía iniciar en algunas ocasiones, debido a que la interfaz IDtsHost no está registrada cuando la versión del servidor de destino es SQL Server 2017 o 2019. |
| Integration Services (SSIS) | Se corrigieron importantes problemas de diseño de interfaz de usuario en el modo con valores altos de PPP. |
| Integration Services (SSIS) | La versión de .NET Framework se actualizó a 4.7 para la tarea o componente de script cuando la versión del servidor de destino es SQL Server 2019. |
| Integration Services (SSIS) | Se agregó la propiedad ConnectByProxy en el administrador de conexiones ODBC para permitir la habilitación del entorno de ejecución de integración autohospedado como proxy en el administrador de conexiones ODBC. |
| Integration Services (SSIS) | Se corrigió un problema por el cual los usuarios no podían agregar orígenes de datos nuevos en el modo de implementación de paquetes. |
| Integration Services (SSIS) | Se corrigió un problema por el cual los usuarios no podían depurar la tarea o el componente de script si el código usaba cualquier sintaxis nueva que se introdujera después de .NET 4.5. |
| Integration Services (SSIS) | Se corrigió un problema que provocaba un error al crear la primera suscripción de Data Factory en Azure a través del Asistente para la creación de Integration Runtime, debido a que el proveedor de recursos de Data Factory no se está registrando. |
| Integration Services (SSIS) | Se corrigió un problema por el cual el Asistente para la conexión de SSIS en ADF no podía mostrar correctamente la lista de cuentas de almacenamiento de Azure cuando había una cuenta de almacenamiento solo de archivo en la suscripción. |
| Integration Services (SSIS) | Se corrigió un problema que hacía que "ejecutar en Azure" no funcionase cuando el paquete incluía un contenedor. |
| Integration Services (SSIS) | Se corrigió un problema que hacía que char(n char) y varchar2(n char) estuviesen asignados a tipos DTS incorrectos en el conector de Oracle. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1592nbsp-ssdt-for-vs-2017"></a>15.9.2,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 17 de julio de 2019  
_Número de compilación:_ &nbsp; 14.0.16194.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Agregue la característica AzureEnabled. Habilite paquetes del proyecto para que se ejecuten en la plataforma como servicio (PaaS) SSIS en Azure Data Factory. |
| Integration Services (SSIS) | Corrección de un problema en el que no se pueden establecer las propiedades del conector de Oracle desde una expresión variable. |
| Integration Services (SSIS) | Corrección de un problema en el que el conector de Oracle tiene un error VS_NEEDSNEWMETATDATA al depurar los paquetes destinados a los suscriptores anteriores a SQL Server 2019. |
| Integration Services (SSIS) | Corrección de un problema en el que el conector de Oracle no pudo actualizar o cambiar a una versión anterior el paquete o proyecto si el paquete o proyecto utiliza expresiones para las propiedades del administrador de conexiones. |
| Integration Services (SSIS) | Corrección de un problema en el que el botón Descargar WSDL del editor de la tarea Servicio web no admite el protocolo TLS 1.1 y 1.2 (el destino es SQL Server 2019). |
| Integration Services (SSIS) | Se ha corregido un problema por el que los paquetes que contienen el administrador de conexiones de DQS no se pueden volver a cargar después de guardarse. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| No se pueden crear o editar orígenes de datos en el modelo de implementación de paquetes. | No se puede abrir al Asistente para orígenes de datos. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1591nbsp-ssdt-for-vs-2017"></a>15.9.1,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 27 de abril de 2019  
_Número de compilación:_ &nbsp; 14.0.16191.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Se ha corregido un problema que hacía que el elemento de paquete no se pudiera almacenar correctamente cuando se destinaba a una versión anterior de SQL Server. |
| Integration Services (SSIS) | Se ha corregido un problema que impedía agregar expresiones a la restricción de precedencia al usar el elemento de paquete. |
| Integration Services (SSIS) | Corrección de un problema que hacía que el botón "Ayuda" del origen de Power Query y el Administrador de conexiones no se vinculara al documento correcto. |
| Integration Services (SSIS) | Corrección de un problema que hacía que la versión de compilación de SSIS no se mostrara en la ventana de Ayuda de VS. |
| Integration Services (SSIS) | Se agrega la propiedad "ConnectByProxy" para el Administrador de conexiones de Ole DB y de Archivo plano, que puede habilitar el acceso a los datos de forma local con IR autohospedado en Azure-SSIS IR. |
| Integration Services (SSIS) | Corrección de un problema que hacía que los componentes ODBC asignaran los tipos de datos DT_DBDATE de manera incorrecta. |
| Integration Services (SSIS) | Se agrega la propiedad "ConnectUsingManagedIdentity" para el administrador de conexiones de ADO.NET y OLE DB, que permite la autenticación de identidad administrada para conectarse al origen de datos en Azure-SSIS IR. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| No se pueden crear o editar orígenes de datos en el modelo de implementación de paquetes. | No se puede abrir al Asistente para orígenes de datos. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1590nbsp-ssdt-for-vs-2017"></a>15.9.0,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 28 de enero de 2019  
_Número de compilación:_ &nbsp; 14.0.16186.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

| Nuevo elemento | Detalles |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Incorpora el origen de Power Query (versión preliminar) para SSIS en ADF 2017. |
| Integration Services (SSIS) | Vuelve a agregar la compatibilidad con SQL Server 2012. |
| Integration Services (SSIS) | Incorpora origen y destino de Oracle para SQL Server 2019. |
| Integration Services (SSIS) | SSDT ya ha instalado el servidor SQL Server 2019 de origen y destino de Oracle. <br/></br> Para diseñar un paquete dirigido a la versión 2017 o inferior del servidor, descargue la versión correspondiente del conector Oracle del sitio de descargas de Microsoft e instálela en el equipo con SSDT. <br/></br> [Microsoft Connector versión 5.0 para Oracle de Attunity dirigido a SQL Server 2017 ](https://www.microsoft.com/download/details.aspx?id=55179 ) <br/></br> [Microsoft Connector versión 4.0 para Oracle de Attunity dirigido a SQL Server 2016 ](https://www.microsoft.com/download/details.aspx?id=52950 )<br/></br> [Microsoft Connector versión 3.0 para Oracle de Attunity dirigido a SQL Server 2014 ](https://www.microsoft.com/download/details.aspx?id=44582 )<br/></br> [Microsoft Connector versión 2.0 para Oracle de Attunity dirigido a SQL Server 2012 ](https://www.microsoft.com/download/details.aspx?id=29283 ) |
| Integration Services (SSIS) | Se ha corregido un problema por el que no se puede cargar la tarea o el componente de script al migrar desde versiones anteriores de SSIS. |
| Integration Services (SSIS) | Se corrige un problema por el que el visor de datos no funciona en Windows 7 SP1 y Windows 8.1. |
| Integration Services (SSIS) | Se corrige un problema por el que, en algunos casos, al guardar el paquete, Visual Studio se bloquea. |
| Integration Services (SSIS) | Se ha corregido un problema por el que, en algunos casos, no se puede ejecutar el paquete. |
| Integration Services (SSIS) | Este problema se produce si se cumplan las dos condiciones siguientes:< br />< br /> &bull;   El nivel de protección es EncryptSensitiveWithPassword.< br /> &bull;   La versión del servidor de destino es anterior a SQL Server 2017.          |
| Integration Services (SSIS) | Se corrige un problema por el que las anotaciones con fuente predeterminada no se muestran en SSDT. |
| Integration Services (SSIS) | ISDeploymentWizard admite la autenticación de SQL, la autenticación integrada de Azure Active Directory y la autenticación de contraseña de Azure Active Directory en el modo de línea de comandos. |

### <a name="known-issues"></a>Problemas conocidos

| Problema conocido | Detalles |
| :---------- | :------ |
| La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. | Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas. |
| El origen de Power Query puede no admitir OData v4 cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query puede no admitir el empleo de ODBC para conectarse a Oracle cuando SSIS y SSAS se instalan en la misma instancia de Visual Studio. | &nbsp; |
| El origen de Power Query no está localizado. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1582nbsp-ssdt-for-vs-2017"></a>15.8.2,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 5 de noviembre de 2018  
_Número de compilación:_ &nbsp; 14.0.16182.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades
**SSIS:**

Se corrigió un problema que consistía en que al implementar un proyecto de SSIS que contenía paquetes con destino de tarea Script o archivo plano para Azure-SSIS, los paquetes no se ejecutaban en Azure-SSIS. 

### <a name="known-issues"></a>Problemas conocidos:

- La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1581nbsp-ssdt-for-vs-2017"></a>15.8.1,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 27 de septiembre de 2018  
_Número de compilación:_ &nbsp; 14.0.16179.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS:**

1. Se ha agregado compatibilidad con [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].
2. Se ha eliminado compatibilidad con SQL Server 2012.

### <a name="known-issues"></a>Problemas conocidos:

- La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.
- Al implementar un proyecto de SSIS que contiene paquetes con destino de tarea Script o archivo plano para Azure-SSIS, los paquetes no se ejecutan en Azure-SSIS.

## <a name="158nbsp-ssdt-for-vs-2017"></a>15,8,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 5 de septiembre de 2018  
_Número de compilación:_ &nbsp; 14.0.16174.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS:**

1. Se ha corregido la regresión en VS 15.8 por la que al guardar la tarea o componente de script se producía un error de compilación.
1. Se ha corregido la regresión en VS 15.8 por la que el asistente para la implementación no funcionaba.
1. Se ha corregido un problema por el que el administrador de conexiones de ADO.NET no admitía un proveedor de ADO.NET de terceros.

**Instalador:**

- Se ha implementado el reinicio en la mitad al instalar SSDT en Windows 10.


### <a name="known-issues"></a>Problemas conocidos:

- La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1571nbsp-ssdt-for-vs-2017"></a>15.7.1,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 2 de julio de 2018  
_Número de compilación:_ &nbsp; 14.0.16167.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS:**

- Se agrega compatibilidad para la autoridad de AAD de Azure Government (login.microsoftonline.us) para el uso con tareas de AS.
- Se ha corregido un problema en el que la interfaz de usuario de la tarea de procesamiento de AS mostrará "Método no encontrado" cuando la versión del servidor de destino es SQLServer2016.
- Se ha corregido un problema por el que algunos componentes de canalización no se pueden ejecutar cuando la versión del servidor de destino es SQL Server 2012.

**Instalador:**

- La lista de instancias de VS se filtra para excluir las que no pueden instalar SSDT.

### <a name="known-issues"></a>Problemas conocidos:

- La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.
- Al instalar SSDT en Windows 10 y elegir "Instalar la nueva instancia de SQL Server Data Tools para Visual Studio 2017", la instalación generará el error "La operación de metarchivo solicitada no es compatible". Reinicie la máquina y vuelva a iniciar el instalador de SSDT para continuar con la instalación.

## <a name="1570nbsp-ssdt-for-vs-2017"></a>15.7.0,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 4 de junio de 2018  
_Número de compilación:_ &nbsp; 14.0.16165.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS:**

- Se ha corregido un problema que provocaba que la página *Diseñadores de Integration Services* del cuadro de diálogo Opciones no se mostrara correctamente.  
- Se ha corregido un problema que provocaba que apareciera un problema de proporción de luminosidad para texto en el *Editor de transformación Ordenar*.  
- Se ha corregido un problema que provocaba que el cuadro de diálogo *Resolver referencias* desapareciera al intentar editar un cuadro combinado.  
- Se ha corregido un problema que provocaba que el vínculo de ayuda de F1 del *Administrador de conexiones de Hadoop* no funcionara.  
- Se ha corregido un problema que provocaba que el código de la tarea de script se perdiera si estaba en un contenedor cuando el destino es SQL Server 2016.  

**Instalador:**

- Se ha corregido un problema que impedía instalar SSAS antes de instalar SSRS y SSIS en VS 15.7.2.

### <a name="known-issues"></a>Problemas conocidos:

- La tarea Ejecutar paquete de SSIS no admite la depuración si *ExecuteOutofProcess* está establecido en *True*. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1560nbsp-ssdt-for-vs-2017"></a>15.6.0,&nbsp; SSDT para VS 2017

_Publicado:_ &nbsp; 10 de abril de 2018  
_Número de compilación:_ &nbsp; 14.0.16162.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS:**

- Se corrige un problema por el que la tarea de procesamiento de sistema autónomo (AS) no registra ningún paso de procesamiento al establecer SQLServer2016 o SQLServer2017 como destino.
- Se corrige un problema por el que se produce una infracción de acceso al abrir dtsx con nombres de tareas largos y no escritos en inglés en SSDT.
- Se corrige un problema por el que, a veces, la lista de variables de ScriptTask desaparece de la interfaz de usuario de la tarea.
- Se corrige un problema por el que se genera un error al agregar una copia del paquete existente si la ubicación del paquete es SQL Server.
- Se corrige un problema por el que el foco se bloquea al acceder al cuadro combinado en algunos cuadros de diálogo del editor.
- Se corrige un problema por el que el fondo no cambia junto al tema de VS.
- Se corrige un problema por el que la etiqueta de anotación y carga es invisible en el tema oscuro.
- Se corrige un problema por el que la propiedad de estado no se define correctamente para los elementos eliminados del cuadro de herramientas de SSIS.
- Se corrige un problema por el que nunca se puede ejecutar WebServiceTask.
- Se corrige un problema por el que la implementación del paquete genera un error si la cadena de conexión está establecida como variable con una expresión que depende de los parámetros del proyecto.

**Instalador:**

- Se agrega el vínculo "Programa para la mejora de la experiencia del usuario de SQL Server Data Tools" en el aviso de privacidad.
- Se corrige un problema por el que la ventana del instalador de Visual Studio aparece como elemento emergente al seleccionar la opción "Instalar la nueva instancia de SQL Server Data Tools para Visual Studio 2017".

### <a name="known-issues"></a>Problemas conocidos:
- La tarea Ejecutar paquete de SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1552nbsp-ssdt-for-vs-2017"></a>15.5.2,&nbsp; SSDT para VS 2017

_Número de compilación:_ &nbsp; 14.0.16156.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

**SSIS**
- Se corrige un problema por el que la migración de proyectos de SSIS 2008 genera un error si SSAS y SSIS están instalados en la misma instancia de VS 2017.
- Se ha corregido un problema que impedía que los proyectos de Rdlc se compilaran cuando el diseñador de informes Rdlc y SSIS están instalados en la misma instancia de VS 2017.
- Se corrige un problema que impide que se actualice el color de la anotación.
- Se corrige un problema por el que algunas cadenas en el editor del administrador de conexiones de Hadoop se truncan en otros idiomas.
- Se corrige un problema por el que algunas cadenas se truncan en el editor del administrador de conexiones de OData.
- Se corrige un problema por el que algunas cadenas se truncan en la ventana del Asistente para importar proyectos de Integration Services.
- Se corrige un problema en el título de la ventana de información del cuadro de herramientas de SSIS.
- Se corrige un problema por el que algunas cadenas se truncan en la ventana del Asistente para implementación de Integration Services. 

**Instalador**
- Se ha corregido un problema por el que a veces se produce el error "No se puede encontrar el archivo especificado (0x80070002)" al descargar la carga.  

### <a name="known-issues"></a>Problemas conocidos
- La tarea Ejecutar paquete de SSIS no admite la depuración si *ExecuteOutofProcess* está establecido en *True*. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1551nbsp-ssdt-for-vs-2017"></a>15.5.1,&nbsp; SSDT para VS 2017

_Número de compilación:_ &nbsp; 14.0.16148.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

Visual Studio 2017 (15.5.1) es la misma versión que la 15.5.0 excepto por las siguientes correcciones de errores del instalador:

1. Se ha corregido un problema que provocaba que el instalador dejase de responder después de la instalación de SQL Server Integration Services.
2. Se ha corregido un problema que provocaba que el programa de instalación produjera un error con el mensaje siguiente: "La operación de metarchivo solicitada no es compatible (0x800707D3)".

Además de estas dos correcciones de errores, todavía se aplican los siguientes detalles de la versión 15.5.0 a la versión 15.5.1.

## <a name="1550nbsp-ssdt-for-vs-2017"></a>15.5.0,&nbsp; SSDT para VS 2017

_Número de compilación:_ &nbsp; 14.0.16146.0  
_SSDT para Visual Studio 2017._

### <a name="whats-new"></a>Novedades

SSDT para Visual Studio 2017 (15.5.0) deja de estar en versión preliminar y pasa a disponibilidad general (GA).

**Instalador**
1. La interfaz de usuario del programa de instalación está localizada.
1. Se reemplaza el icono con uno de mayor calidad.

**Integration Services (IS)**
1. Se ha agregado el paso de validación de paquetes en el Asistente para implementación al implementar Azure SSIS IR en ADF, con el que se pueden detectar potenciales errores de compatibilidad en paquetes de SSIS cuando se ejecutan en Azure SSIS IR. Para obtener más información, vea [Validación de paquetes de SSIS implementados en Azure](../integration-services/lift-shift/ssis-azure-validate-packages.md).
1. Se ha localizado la extensión SSIS.

### <a name="bug-fixes"></a>Corrección de errores

**Integration Services (IS)**
1. Se ha corregido un problema que provocaba que el diseño del administrador de conexiones de OLEDB y ADO.NET estuviese dañado.
2. Se ha corregido un problema que provocaba un error de ensamblado no encontrado al intentar editar una tarea de procesamiento de dimensiones.

### <a name="known-issues"></a>Problemas conocidos

La tarea Ejecutar paquete de **Integration Services (IS)** SSIS no admite la depuración cuando ExecuteOutofProcess está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="173nbsp-ssdt-for-vs-2015"></a>17.3,&nbsp; SSDT para VS 2015

_Número de compilación:_ &nbsp; 14.0.61712.050  
_SSDT para Visual Studio 2015._

### <a name="whats-new"></a>Novedades

**Proyectos de Analysis Services (AS)**
- Se han agregado tres opciones nuevas a los proyectos tabulares (en Opciones > Tabular de Analysis Services > Importación de datos):
  - Habilitar orígenes de datos heredados: permite que el usuario cree orígenes de datos de "modo de compatibilidad 1200" más antiguos en nuevos modos de compatibilidad.
  - Detección de tipos automática: cuando se habilita el Editor de consultas para orígenes de datos modernos, se intentarán detectar los tipos de datos para consultas no estructuradas cuando se cargan. Si la detección se realiza correctamente, puede que se agregue un paso a la consulta.
  - Ejecución de análisis en segundo plano: cuando se habilita el Editor de consultas para orígenes de datos modernos, se ejecutarán consultas en el origen de datos, ya que las consultas se cargan para analizar el esquema de salida de la consulta.

**Integration Services (IS)**
- Se ha agregado el paso de validación de paquetes en el Asistente para implementación al implementar Azure SSIS IR en ADF, con el que se pueden detectar potenciales errores de compatibilidad en paquetes de SSIS cuando se ejecutan en Azure SSIS IR. Para obtener más información, vea [Validación de paquetes de SSIS implementados en Azure](../integration-services/lift-shift/ssis-azure-validate-packages.md).

### <a name="bug-fixes"></a>Corrección de errores

**Proyectos de Analysis Services (AS):**
- Se ha corregido un problema que podía provocar una excepción no controlada durante la comprobación de cambios del modelo en TFS.
- Se ha corregido un problema que podía provocar una excepción al agregar una tabla con expresión M compleja a un modelo 1400.
- Se ha corregido un problema que podía provocar un bloqueo en Visual Studio al buscar metadatos en la vista de diagrama de modelo.
- Se ha corregido un problema con modelos 1400 que podía provocar que las columnas calculadas se quitasen de la definición de tabla al guardar los cambios en consultas M de partición.
- Se ha corregido un problema al usar Cambiar nombre de consulta en los modelos 1400 en la UI de Obtener datos o Editor de tablas que podía provocar un bloqueo al validar la compatibilidad con el modelo de datos actual.
- Se ha corregido un problema que provocaba la falta de una referencia de ensamblado de Newtonsoft al implementar el modelo 1400 en Azure Analysis Services.
- Se ha corregido un problema que provocaba un error al importar datos a través de PQ en un modelo 1400 en ciertos casos.
- Se ha corregido un problema de escala en los cuadros de diálogo de la interfaz de usuario de PowerQuery que podía aparecer con un escalado de Windows establecido.
- Se ha corregido un problema al cambiar el nombre de los roles.
- Se han corregido problemas con las configuraciones de proyecto que pueden haber causado que los cambios no se guarden o sincronicen correctamente en algunos casos.
- Se ha corregido un problema en el editor de PowerQuery que provocaba que se agregasen pasos de "Cambiar tipo" automáticamente.
- Se ha corregido un problema que provocaba un error al abrir el archivo BIM después de cambiar al modo de área de trabajo integrada o al salir de esta.
- Ahora, la propiedad MaxConnections es visible en los orígenes de datos de los modelos tabulares.
- Se ha aumentado el tamaño inicial de la ventana del editor de PowerQuery.
- Las palabras clave de consultas M, como "Source", ahora se mostrarán localizadas en el editor de PowerQuery.
- Las credenciales se almacenan en caché al trabajar con modelos 1400 y orígenes de datos estructurados para evitar tener que indicar las mismas credenciales para cada tabla editada.

**Proyectos de RS:**
- Se ha corregido un problema que provocaba que se evitase la implementación de un único informe en un proyecto de varios informes.
- Se ha corregido un problema con orígenes de datos compartidos que puede haber causado un problema en la implementación.
- Se ha corregido un problema que provocaba que el administrador de Deshacer se bloquease al cambiar entre la vista de código, de diseño y la ventana del editor de consultas.
- Se ha corregido un problema que podía causar que el panel de parámetros desapareciese después de que se produjese un error en tiempo de ejecución.
- Se ha corregido un problema con los proyectos de informe que podía causar que se perdiesen las asignaciones de controles de orígenes.

**Integration Services:**
- Se ha corregido un problema que podía producirse al cambiar una conexión en una tarea de proceso de Analysis Services.
- Se ha corregido un problema que hacía que algunos componentes o tareas no se localizaran correctamente.
- Se ha corregido un problema que provocaba que algunos componentes de CDC se rompiesen después de aplicar una corrección de SQL para CDC que agrega la columna \__$command\_id.

## <a name="1540-previewnbsp-ssdt-for-vs-2017"></a>15.4.0 (versión preliminar),&nbsp; SSDT para VS 2017

_Número de compilación:_ &nbsp; 14.0.16134.0  
_SSDT para Visual Studio 2017._
  
### <a name="whats-new"></a>Novedades

Esta versión proporciona un instalador web independiente para los proyectos de SQL Server Database, Analysis Services, Reporting Services e Integration Services en Visual Studio 2017 15.4 o versiones posteriores.

### <a name="installer"></a>Instalador

- Permite al usuario establecer un alias al instalar una nueva instancia de SSDT para VS2017.
- Oculta las casillas de selección de características del instalador si no hay seleccionada ninguna instancia de VS.
- Ajusta algunos mensajes del instalador según los comentarios del cliente.
- Corrige un problema que consiste en que el instalador no admite la actualización.

### <a name="ssis"></a>SSIS

- Se ha corregido un problema por el que el Asistente para importar o exportar no puede mostrar el origen de datos cuando está instalado el Feature Pack de Azure.
- Corrige un problema que consiste en que al editar una tarea de proceso de Analysis Services de SSIS se inicia una excepción al cambiar la conexión.
- Corrige un problema que consiste en que los componentes de CDC se bloquean después de aplicar una corrección de SQL que agrega la columna __ $command_id.
- Corrige un problema que consiste en que no se puede editar ni ejecutar un paquete de terceros cuando el destino es una instancia antigua de SQL Server.
- Corrige un problema que consiste en que el cuadro de diálogo de configuración Origen de archivo plano no se muestra correctamente al hacer doble clic en DTSWizard.exe y seleccionar Origen de archivo plano.
- Corrige un problema que consiste en que no se puede ejecutar un paquete que contiene una tarea o un componente de Azure Feature Pack cuando el destino es SQL Server 2017.

**Problemas conocidos**

- El instalador no está localizado.
- La tarea Ejecutar paquete de SSIS no admite la depuración cuando *ExecuteOutofProcess* está establecido en True. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.

## <a name="1730nbsp-ssdt-for-vs-2015"></a>17.30,&nbsp; SSDT para VS 2015

_Número de compilación:_ &nbsp; 14.0.61709.290  
_SSDT para Visual Studio 2015._

### <a name="whats-new"></a>Novedades

**Analysis Services (AS)**

- Cosmos DB y HDI Spark se han habilitado en los modelos 1400.
- Propiedades de orígenes de datos tabulares.
- "Consulta en blanco" es ahora una opción admitida para crear una nueva consulta en el Editor de consultas para los modelos del nivel de compatibilidad 1400.
- El Editor de consultas de los modelos del modo 1400 ahora permite guardar las consultas sin que se procesen de forma automática las tablas nuevas.

**Reporting Services (RS)**

- Los proyectos ahora solicitan al abrir el formato actualizado para admitir el uso de MSBuild para compilar e implementar.

### <a name="known-issues"></a>Problemas conocidos

**Analysis Services (AS)**

- Los modelos del nivel de compatibilidad 1400 en modo de consulta directa que tienen perspectivas experimentan un error al consultar o detectar metadatos.

**Reporting Services (RS)**

- El nuevo formato Proyecto de informe no conserva el enlace del control de código fuente y genera un error similar al mensaje:

   *El archivo de proyecto C:\path no está enlazado al control de código fuente, pero la solución contiene información de enlace del mismo.*

   Para solucionar este problema, haga clic en **Usar el enlace de la solución** cada vez que se abra la solución.

- Después de actualizar el proyecto al nuevo formato de MSBuild, se puede producir un error al guardar con un mensaje similar al siguiente:

   *"El parámetro "unevaluatedValue" no puede ser NULL".*

   Para solucionar este problema, actualice las *configuraciones del proyecto* y rellene la propiedad *Plataforma*.

### <a name="bug-fixes"></a>Correcciones de errores

**Analysis Services (AS)**

- Rendimiento considerablemente mejorado al cargar la vista de diagrama del modelo tabular.
- Se ha corregido una serie de problemas para mejorar la integración de PowerQuery y la experiencia de los modelos del nivel de compatibilidad 1400.
   - Se ha corregido un problema que evitaba la edición de permisos de orígenes de archivo.
   - Se ha corregido un problema que evitaba cambiar el origen de los orígenes de archivo.
   - Se ha corregido un problema que consistía en la visualización de la interfaz de usuario incorrecta para los orígenes de archivo.
- Se ha corregido un problema que hacía que la propiedad "JoinOnDate" se quitara cuando quedaba inactiva una relación "Unir en Fecha".
- Nueva opción de consulta en el Generador de consultas que permite crear una nueva consulta en blanco.
- Se ha corregido un problema que hacía que las ediciones de una consulta del origen de datos existente no actualizaran la definición del modelo de la tabla en el nivel de compatibilidad 1400.
- Se han corregido problemas con las expresiones de contexto personalizadas que podrían haber producido excepciones.
- Al importar la nueva tabla con nombre duplicado en los modelos tabulares 1400, al usuario ahora se le notifica que se ha producido un conflicto de nombre y que se ha ajustado el nombre para que sea único.
- Se ha quitado el modo de suplantación Usuario actual de los modelos del modo de importación, ya que no es un escenario admitido.
- La integración de PowerQuery ahora admite opciones para orígenes de datos adicionales (OData.Feed, Odbc.DataSource, Access.Database, SapBusinessWarehouse.Cubes).
- Las cadenas de opciones de PowerQuery para orígenes de datos ahora muestran correctamente el texto localizado en función de la configuración regional del cliente.
- La vista de diagrama ahora muestra las columnas recién creadas desde el Editor de consultas de M en los modelos del nivel de compatibilidad 1400.
- El editor de Power Query ahora ofrece la opción de no importar datos.
- Se ha corregido un problema con la instalación de un cartucho de datos usado para importar tablas de Oracle en modelos multidimensionales en VS2017.
- Se ha corregido un problema que, en algunos casos, podría dar lugar a un bloqueo cuando el cursor del mouse saliera de la barra de fórmulas tabular.
- Se ha corregido un problema en el cuadro de diálogo Editar propiedades de tabla que hacía que al cambiar el nombre de tabla se cambiara incorrectamente el nombre de la tabla de origen y se produjera un error inesperado.
- Se ha corregido un bloqueo que se podía producir en VS2017 al intentar invocar Probar seguridad del cubo en la pestaña Datos de celda del diseñador de roles en proyectos multidimensionales.
- SSDT: las propiedades de los orígenes de datos tabulares no son editables.
- Se ha corregido un problema que podría haber causado que las compilaciones de MSBuild y DevEnv no funcionaran correctamente en algunos casos con archivos de solución.
- Rendimiento considerablemente mejorado al confirmar cambios de modelo (modificaciones DAX para medidas, columnas calculadas) cuando el modelo tabular contiene metadatos mayores
- Se ha corregido una serie de problemas con la importación de datos mediante PowerQuery en modelos del nivel de compatibilidad 1400
   - La importación tarda mucho después de hacer clic en Importar y la interfaz de usuario no muestra ningún estado
   - Una lista grande de tablas en las vistas de Navegador al intentar seleccionar las tablas que se van a importar muy lentamente
   - Mal rendimiento del editor de consultas al trabajar con una lista de 35 consultas en la vista del editor de consultas (problema en el escritorio de PBI también)
   - La importación de varias tablas deshabilita la barra de herramientas y puede no finalizar en determinadas situaciones 
   - El diseñador de modelos aparece deshabilitado y no muestra ningún dato después de la importación de la tabla con PQ
   - La anulación de la selección de "Crear nueva tabla" en la interfaz de usuario de PQ aún se traduce en la creación de una nueva tabla
   - El origen de datos de carpeta no solicita credenciales 
   - La referencia de objeto no establece la excepción que se puede producir al intentar obtener credenciales actualizadas en el origen de datos estructurado
   - La apertura del Administrador de particiones con M-expression es muy lenta
   - Al seleccionar Propiedades en la tabla en el editor de PQ no se mostraban las propiedades
- Más solidez en la integración de la interfaz de usuario de Power Query para detectar las excepciones de nivel superior y mostrar en la ventana de salida
- Se ha corregido un problema que consiste en que ChangeSource en el origen de datos de estructura no conserva los cambios de expresión de contexto
- Se ha corregido un problema que hacía que los errores de expresión de M pudieran provocar errores en la actualización del modelo sin que apareciera mensaje de error
- Se ha corregido un problema de cierre de SSDT con el error "La compilación se debe detener para que la solución se pueda cerrar"
- Se ha corregido un problema que hacía que VS pareciera dejar de responder al establecer el modo de suplantación incorrecto en el modelo del nivel de compatibilidad 1400 
- La propiedad de filas de detalles ahora solo se serializa en JSON si no está vacía (cambio con respecto al comportamiento predeterminado)
- El controlador OLEDB de Oracle ahora está disponible en la lista del modo de consulta directa tabular
- La adición de expresiones de M en modelos tabulares de compatibilidad 1400 ahora aparece o se actualiza en el explorador de modelos tabulares (TME)
- Se ha corregido un problema que provocaba que el proveedor MSOLAP no apareciera en VS2017 al intentar importar mediante el origen de datos "Otro" en los modelos del nivel de compatibilidad previo a 1400
- Se ha corregido un problema que ocasionaba problemas al agregar una traducción a través de TME 
- Se ha corregido un problema en la interfaz Seguridad de nivel de objeto que hacía que la pestaña apareciera o se ocultara incorrectamente en determinados casos
- Se ha corregido un problema que hacía que pudiera producirse un error al intentar abrir un modelo multidimensional cargado previamente mediante el cuadro de diálogo Conectar a base de datos
- Se ha corregido un problema que provocaba un error al agregar ensamblados personalizados a un modelo multidimensional

**Reporting Services (RS)**

- Se ha corregido un problema con la compilación y la generación de RDLC en VS 2017

## <a name="1530-previewnbsp-ssdt-for-vs-2017"></a>15.3.0 (versión preliminar),&nbsp; SSDT para VS 2017

_Número de compilación:_ &nbsp; 14.0.16121.0  
_SSDT para Visual Studio 2017._
  
### <a name="whats-new"></a>Novedades

Esta versión preliminar es la primera versión de SSDT para Visual Studio 2017. Presenta una experiencia de instalación web independiente para los proyectos de SQL Server Database, Analysis Services, Reporting Services e Integration Services en Visual Studio 2017 15.3 y versiones posteriores.

**Problemas conocidos**

- El instalador no está localizado.
- SSIS no está localizado.
- La tarea Ejecutar paquete de SSIS no admite la depuración cuando *ExecuteOutofProcess* está establecido en *True*. Este problema solo se aplica a la depuración. Las funciones de guardado, implementación y ejecución mediante DTExec.exe o el catálogo de SSIS no se verán afectadas.
- Los paquetes de SSIS que contengan extensiones de terceros no se pueden modificar para destinarlos a otras versiones del servidor.


## <a name="172nbsp-ssdt-for-vs-2015"></a>17.2,&nbsp; SSDT para VS 2015

_Número de compilación:_ &nbsp; 14.0.61707.300  
_SSDT para Visual Studio 2015._

### <a name="whats-new"></a>Novedades

**Proyectos de AS:**
- Ahora puede configurarse la seguridad de nivel de objeto en el cuadro de diálogo *Roles* de seguridad avanzada en modelos tabulares de nivel de compatibilidad 1400.
- Nueva selección de miembros del rol de AAD para usuarios sin direcciones de correo electrónico en modelos de Azure AS en proyectos de AS de SSDT para VS2017.
- Nueva propiedad de proyecto "Preguntar siempre" en proyectos tabulares de AS de SSDT para personalizar el comportamiento del almacenamiento en caché de credenciales de ADAL.

### <a name="bug-fixes"></a>Correcciones de errores

**General**
- Se han actualizado las referencias de personalización de marca de SQL Server 2017.

**Proyectos de AS**
- Se han realizado correcciones de rendimiento significativas para mejorar la experiencia al confirmar los cambios de medida DAX y otras modificaciones de modelo.
- Se ha corregido una serie de problemas con la integración de Power Query en proyectos de Analysis Services que usan modelos tabulares de nivel de compatibilidad 1400.
- Se ha corregido un problema solo en los proyectos multidimensionales en VS2017 en que era posible que el Diseñador de agregaciones de diseño no se pudiera cargar.
- Se ha corregido un problema al arrastrar un elemento en el diagrama de DSV multidimensional de Analysis Services que hacía que se pudiera bloquear VS 2017.
- Se ha corregido un problema en los proyectos de AS que hacía que el cuadro de diálogo Implementar no estuviera siempre en primer plano en Visual Studio.
- Se ha quitado la importación de Analysis Services desde Data Marketplace como origen de datos ya que el servicio se ha retirado.
- Se ha corregido un problema que hacía que se deshabilitara el Diseñador de tablas después de importar la nueva tabla del origen de datos existente a través del Explorador de modelos tabulares.
- Se ha corregido un problema que hacía que los elementos del menú Modelo Importar del origen de datos o Agregar origen de datos permanecieran ocultos en el contexto equivocado.
- Se ha mejorado la experiencia al crear una medida desde el Explorador de modelos tabulares para evitar cambiar el foco a la columna usada para crear una medida.
- Al cambiar de área de trabajo integrada en proyectos tabulares de AS al servidor de área de trabajo explícito, los archivos de base de datos antiguos se limpian.
- Se ha corregido un problema en proyectos de modelos tabulares 1400 que hacía que el estado de la interfaz de usuario de la casilla Seguridad de nivel de fila se mostrara inicialmente como desactivada independientemente del estado del objeto subyacente real.
- Se ha corregido un bloqueo que se podía producir al importar un archivo de texto o un archivo de Excel en el modelo tabular en modo de compatibilidad con 1400 al usar Power Query y se producía una excepción no controlada.
- Se ha corregido un problema que se podía producir con el control de desplazamiento en el control de edición de la fórmula DAX en el Diseñador de modelos tabulares de AS.
- Se ha corregido un problema que hacía que no se pudiera modificar un origen de datos del mashup de Power Query cuando contenía una autenticación de usuario/contraseña.
- Se ha corregido un problema que podría impedir que un origen de datos se conectara al establecer propiedades adicionales en la cadena de conexión.
- Se ha corregido un problema que podría bloquear VS al cargar varios proyectos de modelos tabulares de AS y cerrar el segundo diseñador de modelos sin interactuar con nada en el diseñador en primer lugar.
- Se ha corregido un problema que hacía que las ediciones realizadas en el formato de KPI no se almacenaran en algunos casos.
- Se ha corregido un problema con la interfaz de usuario de Power Query que mostraba el estado incorrecto de activación de menú sobre si se mostraba la barra de fórmulas.
- Se ha corregido un problema en los proyectos tabulares de nivel de compatibilidad 1400 de AS con orígenes de datos de Power Query que podría bloquear VS al seleccionar el menú Cambiar origen de datos desde el Explorador de modelos tabulares.
- Se ha corregido un problema intermitente que hacía que, al cargar un modelo tabular 1400, se pudiera mostrar el error *"No se puede cargar el archivo o ensamblado 'Microsoft.ProBI.MashupLibrary'"* .

**Proyectos de RS**
- Las preferencias del usuario de la regla de RS y del estado de selección de la configuración del cuadro Parámetro se recuerdan correctamente entre sesiones.

**Proyectos de IS**
- Se ha corregido un problema que hacía que el contenedor ForEachLoop de ADO/ADO.NET no se mostrara correctamente
- Se ha corregido un problema que hacía que algunos componentes, tareas o asistentes no se localizaran
- Se ha cambiado la *TargetServerVersion* más reciente de "SQL Server vNext" a "SQL Server 2017"

## <a name="1710nbsp-ssdt-for-vs-2015"></a>17.10,&nbsp; SSDT para VS 2015

_Número de compilación:_ &nbsp; 14.0.61705.170  
_SSDT para Visual Studio 2015._

### <a name="whats-new"></a>Novedades
**Proyectos de AS:**
- Los usuarios pueden establecer sugerencias de codificación en columnas de la interfaz de usuario en los modelos 1400.
- IntelliSense no relacionado con el modelo ya está disponible en modo sin conexión.
- El explorador de modelos tabulares ahora contiene un nodo para representar expresiones de M con nombre disponibles en el modelo (modelos tabulares con nivel de compatibilidad 1400).
- El selector de personas de Azure Active Directory, similar a la IAM de Microsoft Azure Portal, ya está disponible al configurar los miembros del rol en los modelos tabulares.

**Proyectos de base de datos:**
- Actualización a DacFx 17.1

### <a name="bug-fixes"></a>Correcciones de errores
- Se ha corregido un problema que hacía que el nombre del grupo Diseñadores de Business Intelligence no se mostrara correctamente en las opciones de Visual Studio en VS2017.
- Se ha corregido un problema que hacía que se pudiera producir un bloqueo al generar un mapa de código para una solución con un proyecto de informe o de AS.
- Se ha corregido una serie de problemas con la integración de PowerQuery para modelos tabulares de nivel de compatibilidad 1400 de Analysis Services.
- Se ha corregido un problema en la nueva ventana de herramientas del editor DAX que hacía que el operador de asignación no pudiera estar en una línea independiente al definir una medida.
- Se ha corregido un problema que impedía que la vista de medida tabular se actualizase al cambiar el nombre de las medidas en perspectiva.
- Se ha actualizado el motor de área de trabajo integrado de Analysis Services y el modelo de objetos tabulares que corrige una regresión que ha provocado que se produzca un error en los proyectos tabulares 1200 que contienen traducciones al implementar el servidor de SQL Server 2016 Analysis Services.
- Se ha corregido un problema de rendimiento que hacía que la creación o eliminación de nuevos orígenes de datos tabulares 1400 se realizara muy lentamente.
- Se ha corregido un problema que hacía que el diagrama de DSV en modelos multidimensionales pudiera dejar de representarse al cambiar rápidamente la vista entre diferentes DSV.

## <a name="dacfx-171"></a>DacFx 17.1

- Se ha corregido un problema al cifrar una columna con tablas optimizadas para memoria con otras columnas de identidad.
- Compatibilidad de SQLDOM con la opción CATALOG_COLLATION para CREATE DATABASE

## <a name="dacfx-1701"></a>DacFx 17.0.1

- Se ha corregido el problema con las bases de datos con una clave asimétrica mediante un HSM con un [elemento Connect](https://connect.microsoft.com/SQLServer/feedback/details/3132749/sqlpackage-exe-fails-when-extracting-a-database-which-contains-an-asymmetric-key-using-an-ekm-provider) del proveedor EKM.

## <a name="170nbsp-ssdt-for-vs-2015"></a>17.0,&nbsp; SSDT para VS 2015

_Número de compilación:_ &nbsp; 14.0.61704.140  
_SSDT para Visual Studio 2015._  
_Admite hasta SQL Server 2017._

### <a name="whats-new"></a>Novedades
**Proyectos de base de datos:**
- La modificación de un índice agrupado en una vista ya no bloqueará la implementación.
- Las cadenas de comparación de esquemas relacionadas con el cifrado de columnas usan el nombre correcto en lugar del nombre de instancia.   
- Se agregó una nueva opción de línea de comandos a SqlPackage: ModelFilePath.  Esto proporciona una opción para que los usuarios avanzados especifiquen un archivo model.xml externo para las operaciones de importación, publicación y scripting.   
- La API DacFx se ha ampliado para admitir la autenticación universal de Azure AD y la autenticación multifactor (MFA).

**Proyectos de IS:**
- El origen OData de SSIS y el administrador de conexiones OData ahora admiten la conexión a fuentes de OData de Microsoft Dynamics AX Online y Microsoft Dynamics CRM Online.
- Los proyectos de SSIS ahora admiten la versión del servidor de destino de "SQL Server 2017". 
- Compatibilidad con Tarea Control CDC, Divisor CDC y Origen de CDC cuando la plataforma de destino es SQL Server 2017. 

**Proyectos de AS:**
- Integración de Analysis Services PowerQuery (modelos tabulares de nivel de compatibilidad 1400):
    - DirectQuery está disponible para SQL, Oracle y Teradata si el usuario ha instalado controladores de terceros.
    - Agregar columnas según el ejemplo en PowerQuery.
    - Opciones de acceso a datos en modelos 1400 (propiedades de nivel de modelo usadas por el motor de M).
        - Habilitar Combinación rápida (el valor predeterminado es false; si se establece en true, el motor de mashup omite los niveles de privacidad del origen de datos al combinar datos).
        - Habilitar Redireccionamientos heredados (el valor predeterminado es false; si se establece en true, el motor de mashup sigue los redireccionamientos HTTP que son potencialmente inseguros.    Por ejemplo, un redireccionamiento de HTTPS a un URI de HTTP).  
        - Devolver valores de error como nulos (el valor predeterminado es false; si se establece en true, los errores de nivel de celda se devuelven como nulos. Si se establece en false, se produce una excepción cuando una celda contiene un error).  
    - Orígenes de datos adicionales (orígenes de datos de archivo) mediante PowerQuery.
        - Excel 
        - Texto o CSV 
        - Xml 
        - Json 
        - Carpeta 
        - Base de datos de Access 
        - Azure Blob Storage 
    - Interfaz de usuario de PowerQuery localizada
- Ventana de herramientas del Editor DAX
    - Se ha mejorado la experiencia de edición de DAX para las expresiones de filas de detalles, columnas calculadas y medidas, disponibles en el menú Ver, Otras ventanas de SSDT.
    - Mejoras en IntelliSense y el analizador DAX


**Proyectos de RS:**
- El control RVC insertable ahora está disponible y es compatible con SSRS 2016.

### <a name="bug-fixes"></a>Corrección de errores
**Proyectos de AS:**
- Se ha corregido la prioridad de la plantilla de proyectos de BI para que no aparezcan en la parte superior de las categorías de proyectos nuevos en VS
- Se corrigió un bloqueo de VS que se puede producir en circunstancias excepcionales cuando se abre la solución de SSIS, SSAS o SSRS.
- Tabular: diversas mejoras y correcciones de rendimiento para el análisis DAX y la barra de fórmulas.
- Tabular: el Explorador de modelos tabulares ya no será visible si no hay abierto ningún proyecto tabular de SSAS.
- Multidimensional: se ha corregido un problema que hacía que el cuadro de diálogo de procesamiento no se pudiese usar en equipos con valores altos de PPP.
- Tabular: se ha corregido un problema que producía un error en SSDT al abrir cualquier proyecto de BI cuando SSMS ya estaba abierto. [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3100900/ssdt-faults-when-opening-any-bi-project-when-ssms-is-already-open)
- Tabular: se ha corregido un problema que hacía que las jerarquías no se guardasen correctamente en el archivo .bim en un modelo 1103. [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3105222/vs-2015-ssdt)
- Tabular: se ha corregido un problema que permitía el modo de área de trabajo integrada en equipos de 32 bits aunque no fuese compatible.
- Tabular: se ha corregido un problema que podía causar bloqueos al hacer clic en cualquier elemento en el modo de selección parcial (por ejemplo, escribir una expresión DAX pero hacer clic en una medida).
- Tabular: se ha corregido un problema que hacía que el Asistente para la implementación volviese a establecer la propiedad .Name del modelo en "Model". [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3107018/ssas-deployment-wizard-resets-modelname-to-model)
- Tabular: se ha corregido un problema que hacía que al seleccionar una jerarquía en TME se mostrasen las propiedades incluso si no estaba seleccionada la vista de diagrama.
- Tabular: se ha corregido un problema que hacía que al pegar desde determinadas aplicaciones en la barra de fórmulas DAX se pegasen imágenes u otro contenido en lugar de texto.
- Tabular: se ha corregido un problema que hacía que no se pudiesen abrir algunos modelos antiguos en la versión 1103 debido a la presencia de medidas con una definición específica.
- Tabular: se ha corregido un problema que hacía que las sesiones de XEvent no se pudiesen eliminar.
- Se ha corregido un problema que producía un error al intentar compilar archivos "smproj" de AS con devenv.com
- Se corrigió un problema que finalizaba con demasiada frecuencia los cambios de texto al usar Coreano IME en los títulos de las pestañas de hojas del modelo tabular de AS
- Se corrigió un problema en que IntelliSense para la función DAX Related() no funcionaba correctamente para mostrar las columnas de otras tablas
- Se mejoró la importación del proyecto tabular de AS desde el cuadro de diálogo de la base de datos al ordenar la lista de las bases de datos de AS
- Se ha corregido un problema en la creación de tablas calculadas en el modelo tabular de AS que provocaba que las tablas no aparecieran como objetos sugeridos en la expresión
- Se corrigió un problema al intentar abrir los modelos de AS 1400 de versión preliminar mediante el servidor de área de trabajo integrada después de ver el código
- Se corrigió un problema que impedía que algunos orígenes de datos (sin compatibilidad con el catálogo inicial) funcionaran correctamente en circunstancias determinadas 
- El Asistente para la implementación debe aplicar cambios a las particiones de tablas calculadas incluso cuando está habilitada la opción de mantener particiones
- Se ha corregido un problema que provocaba que el cuadro de diálogo de propiedades avanzadas para la conexión de AS no mostrara la lista completa hasta que se volvía a seleccionar
- Se ha corregido una serie de problemas con cadenas de interfaz de usuario recortadas que aparecían en algunas versiones localizadas.
- Se ha corregido una serie de problemas con la integración de PowerQuery en modelos tabulares de AS de nivel de compatibilidad 1400.
- Se ha corregido un problema con las plantillas de estilo del Asistente para informes que no aparecían correctamente.
- Se ha corregido un problema con el Asistente para informes que podía dar lugar a una configuración de origen de datos incorrecta al cambiar de SQL a AS.
- Se ha corregido un problema que hacía que se produjera un error de compilación del proyecto de Analysis Services (tabular) desde la línea de comandos (devenv.com\exe).
- Se ha corregido un problema con el analizador de medidas DAX para que muestre el color del texto resaltado y correcto al empezar con letras antes de :=.
- Se ha corregido un problema que hacía que se desencadenara una excepción ObjectRefException si las rutas de acceso tardaban demasiado en intentar mostrar todos los archivos del proyecto tabular en el modo de área de trabajo integrada.
- Se ha corregido un problema con el Diseñador de origen de datos del proveedor de datos del cliente Compact 4.0 donde aparecía como no disponible.
- Se ha corregido un problema que producía un error al intentar examinar el modelo de minería de datos de AS en VS2017.
- Se ha corregido un problema en el modelo multidimensional de AS en VS2017 en el que el diagrama de DSV deja de representarse después de cambiar de vista y, después, se produce una excepción.
- Se ha corregido un problema que hacía que no se pudieran obtener vistas previas de informes con una conexión de AS en VS2017.
 

**Proyectos de RS:**
- se corrigió un problema en el diseño de informes en SSDT en que la vista de árbol de los parámetros, los orígenes de datos y los conjuntos de datos se podría contraer cuando se hacen la mayoría de los datos 
- Se ha corregido un problema que hacía que la opción Guardar guardase la versión de RDL, no la versión más reciente.
- Se ha corregido un problema que hacía que RS de SSDT hiciese una copia de seguridad de los archivos cuando la copia de seguridad estaba desactivada y causase otros problemas.
- Se ha corregido un problema en el Generador de informes que hacía que se mostrase un error al hacer clic en "Dividir celdas". [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3101818/ssdt-2015-ssrs-designer-error-by-matrix-cell-split)
- Se ha corregido un problema que podía hacer que el almacenamiento en caché generase datos incorrectos en un informe. [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3102158/ssdtbi-14-0-60812-report-preview-data-is-frequently-wrong-due-to-bad-caching)

**Proyectos de IS:**
- Se ha corregido un problema que hacía que la configuración de run64bitruntime no se conservase.
- Se ha corregido un problema que hacía que DataViewer no guardase las columnas mostradas.
- Se ha corregido un problema que hacía que los elementos del paquete ocultasen las anotaciones. [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3106624/package-parts-hide-annotations)
- Se ha corregido un problema que hacía que los elementos del paquete descartasen las anotaciones y los diseños del flujo de datos. [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/3109241/package-parts-discard-data-flow-layouts-and-annotations)
- Se ha corregido un problema que hacía que SSDT se bloquease al importar un proyecto de SQL Server.
- Se ha corregido un problema que hacía que la tarea del sistema de archivos de Hadoop TimeoutInMinutes volviera al valor predeterminado 10 después de abrir un paquete de SSIS guardado y en tiempo de ejecución.

**Proyectos de base de datos:**
- Configuración de incorporación e implementación de SSDT DACPAC para el [elemento de Connect](https://connect.microsoft.com/SQLServer/feedback/details/1221587/ssdt-dacpac-deploy-add-setting-back-in-for-ignorecolumnorder) IgnoreColumnOrder
- SSDT no se puede compilar si se usa STRING_SPLIT [Artículo de Connect](https://connect.microsoft.com/SQLServer/feedback/details/2906200/ssdt-failing-to-compile-if-string-split-is-used)
- Se corrige problema en que la propiedad DeploymentContributors tiene acceso al modelo público pero no se ha inicializado el esquema de respaldo [Problema de GitHub](https://github.com/Microsoft/DACExtensions/issues/8)
- Corrección temporal de DacFx para la ubicación de FILEGROUP
- Corrección del error "Referencia sin resolver" para sinónimos externos. 
- Always Encrypted: el cifrado en línea no deshabilita el seguimiento de cambios en la cancelación ni tampoco funciona correctamente si el seguimiento de cambios no se ha limpiado antes de comenzar el cifrado

## <a name="165nbsp-ssdt-for-vs-2015"></a>16.5,&nbsp; SSDT para VS 2015

_Publicado:_ &nbsp; 20 de octubre de 2016  
_Número de compilación:_ &nbsp; 14.0.61021.0  
_SSDT para Visual Studio 2015._  
_Admite hasta SQL Server 2016._

**Novedades**

### <a name="connection-improvements"></a>Mejoras de conexión

* El nuevo cuadro de búsqueda de la pestaña **Examinar** le ayuda a filtrar los servidores locales, los servidores de red y las bases de datos SQL de Azure. Esto resulta útil si le aparecen un gran número de servidores o bases de datos en estas listas.
* La pestaña **Historial** tiene opciones de menú contextual para anclar y desanclar favoritos, y una nueva opción para quitar las conexiones de historial.

### <a name="sqlpackage-and-dacfx-api-improvements"></a>Mejoras de las API de SqlPackage y DacFx

Con las API de SqlPackage.exe y DacFx ahora puede generar un informe de implementación y un script de implementación, y publicar en una base de datos; todo en una sola acción. Esto supone un ahorro de tiempo para cualquier persona a que le gusta tener un informe de lo que se ha publicado durante una implementación. Otra ventaja es que se crean scripts independientes para la base de datos maestra y la base de datos de destino de la implementación en escenarios de Azure. Hasta ahora se creaba un único script que no resultaba útil para implementaciones repetidas.

Para las acciones Publish y Script de SqlPackage, se han agregado dos argumentos nuevos.

* DeployScriptPath (nombre corto: dsp). Se trata de una ruta opcional en la que escribir el script de implementación. Para la implementación de Azure, si hubiera comandos TSQL para crear o modificar la base de datos, se escribirá un script maestro en la misma ruta, pero con "Filename_Master.sql" como el nombre del archivo de salida.
* DeployReportPath (nombre corto: drp). Se trata de una ruta opcional en la que escribir el informe de implementación.

Para la acción Script, se deben usar los argumentos de la ruta de salida existentes o los nuevos argumentos específicos del script o informe, pero no ambos.

Ejemplo de uso:

**Acción Publish**

```Sqlpackage.exe /a:Publish /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

**Acción Script**

```Sqlpackage.exe /a:Script /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

En DacFx, se han agregado dos API nuevas: DacServices.Publish() y DacServices.Script(). Estas también permiten realizar acciones de publicación, scripts e informes en una única operación. Ejemplo de uso:

```
DacServices service = new DacServices(connectionString);
using(DacPackage package = DacPackage.Load(@"C:\My\db.dacpac")) {
var options = new PublishOptions() {
    GenerateDeploymentScript = true, // Should a deployment script be created?
    GenerateDeploymentReport = true, // Should an xml deploy report be created?
    DatabaseScriptPath = @"C:\My\OutputScript.sql", // optional path to save script to
    MasterDbScriptPath = @"C:\My\OutputScript_Master.sql", // optional path to save master script to
    DeployOptions = new DacDeployOptions()
};

// Call publish and receive deployment script & report in the results
PublishResult result = service.Publish(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);

// Call script and receive deployment script & report in results
result = service.Script(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);
```

**Analysis Services y Reporting Services**

Se ha mejorado el rendimiento del analizador de DAX de diseñador de modelos tabulares de SSAS cuando se trabaja con expresiones de DAX de gran tamaño.
Para obtener más información, lea la [entrada de blog de Analysis Services](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular).

### <a name="fixed--improved-this-month"></a>Correcciones y mejoras de este mes

**Herramientas para bases de datos**

* [Error de conexión 3055711](https://connect.microsoft.com/SQLServer/feedback/details/3055711/columns-can't-be-selected-from-cross-apply-openjson-with-explicit-schema): no se pueden seleccionar columnas desde CROSS APPLY OPENJSON con un esquema explícito.
* Corregido: problema con los índices de tablas de historial generados automáticamente, por el que DacFx eliminaba el índice al volver a implementar.
* Corregido: problema con el analizador por lotes de DacFx, que no analizaba los caracteres de corchete con escape "]", lo que provocaba un error en la publicación.
* Mejorado: SqlPackage ahora incluye descripciones para cada acción en los resultados de la ayuda.
* Corregido: la opción "Recordar contraseña" del cuadro de diálogo de conexión no se conservaba cuando se editaban las opciones avanzadas y al modificar una cadena de conexión guardada en Publicar, Comparación de esquemas y otros archivos.
* Corregido: para las conexiones mostradas en la pestaña Historial con IntegratedAuthentication=true, el campo Autenticación de las propiedades de conexión se quedaba en blanco. Ahora se muestra "Autenticación de Windows", como estaba previsto.
* Corregido: no se conservaban los cambios realizados en la configuración de Intellisense de SQL Server Tools en Herramientas -> Opciones -> Editor de texto.
* Mejorado: el botón Anclar/Desanclar del cuadro de diálogo de conexión de la pestaña Historial es ahora más compacto, lo que reduce la probabilidad de que aparezca una barra de desplazamiento.
* Corregido: se han corregido varios problemas de accesibilidad en el cuadro de diálogo de conexión.

**Analysis Services y Reporting Services**

* Se ha corregido un problema en el diseñador de modelos tabulares de SSDT AS por el que al hacer clic en el control de la barra de desplazamiento en una cuadrícula de datos se producía un bloqueo en determinadas situaciones.
* Se ha corregido un problema por el que no estaba disponible la opción para suplantar la conexión como el usuario actual en el modelo tabular de SSDT AS.
* Se ha corregido un problema en el diseñador de modelos tabulares de SSDT AS por el que expandir la barra de fórmulas demasiado lejos podía hacer que no se pudiera volver a abrir el proyecto.
* Se ha corregido un bloqueo en el diseñador de modelos tabulares de SSDT AS que se producía al presionar una tecla si estaba seleccionada la pestaña de tabla.
* Se ha corregido un problema en los proyectos de SSDT AS por el que Analizar en Excel no se conectaba a versiones de servidores de AS de nivel inferior.

**Servicio de integración**

* Se ha corregido el error de Connect [1608896](https://connect.microsoft.com/SQLServer/feedback/details/1608896/move-multiple-integration-service-package-tasks): mover varias tareas de un paquete del servicio de integración.

## <a name="164-ssdt-for-vs-2015"></a>16.4, SSDT para VS 2015

_Publicado:_ &nbsp; 20 de septiembre de 2016  
_Número de compilación:_ &nbsp; 14.0.60918  
_Para SQL Server 2016._

**Novedades**

Comparación de esquemas es ahora compatible con SqlPackage.exe y la API de Data-Tier Application Framework (DacFx). Para obtener más información, consulte [Schema Compare in SqlPackage and the Data-Tier Application Framework](/archive/blogs/ssdt/schema-compare-in-sqlpackage-and-the-data-tier-application-framework-dacfx) (Comparación de esquemas en SqlPackage y Data-Tier Application Framework).

**Analysis Services: modo de área de trabajo integrada de SSDT Tabular (SSAS)**

SSDT Tabular ahora incluye una instancia de SSAS interna que SSDT Tabular inicia automáticamente en segundo plano si se habilita el modo de área de trabajo integrada para que pueda agregar y ver tablas, columnas y datos en el Diseñador de modelos sin tener que proporcionar una instancia del servidor de área de trabajo externa. El modo de área de trabajo integrada no cambia el funcionamiento de SSDT Tabular con un servidor y una base de datos de área de trabajo. Lo que cambia es la ubicación en la que SSDT Tabular hospeda la base de datos del área de trabajo. Para habilitar el modo de área de trabajo integrada, seleccione la opción Área de trabajo integrada en el cuadro de diálogo del Diseñador de modelos tabulares que aparece al crear un nuevo proyecto tabular. Para los proyectos tabulares existentes que usan actualmente un servidor de área de trabajo explícito, puede cambiar al modo de área de trabajo integrada estableciendo el parámetro Modo de Área de trabajo integrada en verdadero en la ventana Propiedades, que se muestra cuando selecciona el archivo Model.bim en el Explorador de soluciones. Para obtener más información, consulte la [entrada de blog de Analysis Services](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular).

**Actualizaciones y correcciones de**
**herramientas para bases de datos:**

- [Problema de Connect 3087775](https://connect.microsoft.com/SQLServer/feedback/details/3087775): tablas temporales interrumpidas en la actualización de julio de VS Data Tools 14.0.60629.0, "El valor no puede ser NULL. Nombre de parámetro: reportedElement".
- [Problema de Connect 1026648](https://connect.microsoft.com/SQLServer/feedback/details/1026648): IsPersistedNullable se muestra como diferente en la comparación de SSDT.
- [Problema de Connect 2054735](https://connect.microsoft.com/SQLServer/feedback/details/2054735): la identidad se restablece al importar un BACPAC.
- [Problema de Connect 2900167](https://connect.microsoft.com/SQLServer/feedback/details/2900167): la ejecución de pruebas unitarias de SSDT deja archivos temporales.
- [Problema de Connect 1807712](https://connect.microsoft.com/SQLServer/feedback/details/1807712): interrupción de la compatibilidad con versiones anteriores: AppLocal e instalación de NuGet

**Analysis Services y Reporting Services**

* Se ha corregido un problema en SSDT por el que aparecían elementos emergentes de sugerencias de errores al editar DAX para columnas calculadas de DirectQuery.
* Se ha corregido un problema en la cuadrícula tabular de SSDT AS por el que el icono de KPI no se mostraba en la cuadrícula de medidas cuando el factor de escala de Windows se establecía en valores altos de PPP de 200 % o más.
* Se ha corregido un problema en SSDT AS por el que, al pegar un gran número de datos de tabla, el proyecto tabular no respondía.
* Se ha corregido un problema en el editor de modelos tabulares de SSDT AS para marcar que el modelo necesita guardar cambios al modificar el nombre descriptivo de la conexión.
* Se ha corregido un problema en los proyectos tabulares de SSDT AS por el que no se podía cambiar el tamaño del ancho de las columnas en el cuadro de diálogo Administrar relaciones.
* Se ha corregido un problema en los modelos de nivel 1200 tabulares de SSDT AS por el que al copiar datos de Excel con una configuración regional, por ejemplo, alemán, no se trataba la coma como el separador decimal correctamente.
* Se ha corregido un problema en los proyectos de SSDT AS con algunos conjuntos de iconos de KPI que podían producir un error "No se pueden recuperar los datos para este objeto visual".
* Se ha corregido un problema con el cuadro de diálogo de las propiedades del proyecto de SSDT AS para que se delimite correctamente cuando se cambie su tamaño en el ajuste con valores altos de PPP.
* Se ha corregido un problema en los proyectos SSDT AS que podría haber provocado un error al actualizar determinados modelos con las tablas pegadas.
* Se ha corregido un problema en SSDT AS por el que la acción de pegar filas de hojas completas desde Excel resultaba lenta y creaba muchas columnas no deseadas.
* Se ha corregido un problema en SSDT AS por el que las operaciones de análisis y resaltado de grandes expresiones de DataTable resultaban muy lentas o parecían dejar de responder.
* Se ha corregido un problema en SSDT AS para agregar valores de KPI y medidas a la perspectiva actual seleccionada en el editor.
* Se ha corregido un problema en SSDT por el que la importación de datos en el proyecto de AS desde SQL Azure no admitía tipos de esquemas distintos a "dbo".

## <a name="163nbsp-ssdt-for-vs-2015"></a>16.3,&nbsp; SSDT para VS 2015

_Publicado:_ &nbsp; 15 de agosto de 2016  
_Número de compilación:_ &nbsp; 14.0.60812.0  
_Para SQL Server 2016._

**Novedades**

- **Numeración y control de versiones de lanzamiento:** ahora las versiones se etiquetan numéricamente en lugar de por mes. Esto es coherente con la nueva directiva SSMS y simplifica los casos en los que existen numerosas versiones o revisiones en un mes. Esta versión es 16.3, lo que significa que es la tercera actualización tras el lanzamiento de RTM. Las revisiones se identificarían como 16.3.1 y así sucesivamente. Nuestra próxima actualización será la 16.4 (prevista para el próximo mes).
- **Analysis Services: Explorador de modelos tabulares:** el Explorador de modelos tabulares permite navegar fácilmente a través de los distintos objetos de metadatos de un modelo, como orígenes de datos, tablas, medidas y relaciones. Se implementa como una ventana de herramientas independiente que puede mostrar abriendo el menú Ver en Visual Studio, apuntando a Otras ventanas y haciendo clic luego en Explorador de modelos tabulares. El Explorador de modelos tabulares aparece de forma predeterminada en el área del Explorador de soluciones, en una pestaña independiente. El Explorador de modelos tabulares organiza los objetos de metadatos en una estructura de árbol muy similar al esquema de un modelo tabular 1200 y con muchas más características nuevas.
- **Herramientas de base de datos: Always Encrypted**:  en esta versión se proporcionan nuevos cuadros de diálogo de [administración de claves de Always Encrypted](../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) para agregar fácilmente claves maestras de columna o claves de cifrado de columna al proyecto de base de datos, o bien una base de datos activa en el Explorador de objetos de SQL Server. Esta versión admite certificados en el almacén de certificados de Windows. En las próximas versiones, se admitirán Azure Key Vault y proveedores CNG.
    - Al crear la clave maestra de columna o la clave de cifrado de columna, es posible que experimente que los cambios no se reflejan en el Explorador de objetos de SQL Server inmediatamente después de hacer clic en Actualizar base de datos. Para solucionar este problema, actualice el nodo de base de datos en el Explorador de objetos de SQL Server.
    - Si intenta cifrar una columna en una tabla con datos del Explorador de objetos de SQL Server, puede experimentar un error. Actualmente, esta característica se admite únicamente en proyectos de base de datos de SSDT y SSMS. La compatibilidad con el Explorador de objetos de SQL Server se habilitará en una versión posterior.

**Actualizaciones y correcciones**
* **Herramientas para bases de datos:**
    - **SSDT:**
        - Error de Connect 1898001: [se ha corregido un problema de descripción de columna con una limitación de 128 caracteres](https://connect.microsoft.com/SQLServer/feedback/details/1898001/column-description-limited-to-128-characters).
        - Se ha corregido un problema por el que, al publicar una base de datos de VS, no se aplicaba la propiedad DatabaseServiceObjective en el xml de perfil de publicación.
        - Error de Connect 2900167: [se ha corregido un problema con las pruebas unitarias que dejaban incorrectamente archivos temporales](https://connect.microsoft.com/SQLServer/feedback/details/2900167/running-ssdt-unit-tests-leaves-temp-files-behind).
        - Se ha corregido un problema por el que se truncaba el cuadro combinado Período de retención en Configuración de la base de datos.
        - Se ha corregido un problema de ausencia de verificación de contraseñas antiguas vacías en las propiedades del proyecto CLR de SQL al cambiar la contraseña.
    - **DACFx:**
        - Se ha corregido un problema por el que el nivel de compatibilidad de DACFx no se actualizaba para el error SqlAzureV12.
        - Se ha corregido un problema por el que la propiedad IsAutoGeneratedHistoryTable se excluía incorrectamente de la comparación de modelos.

- **Analysis Services y Reporting Services**
    - **SSDT:**
        - Se ha corregido un problema que hace que el modelo Tabular no se pueda guardar cuando se pierde la conexión al servidor.
        - Se ha corregido un problema por el que SSDT no responde debido a un posible problema de un bucle infinito en AS.
        - Se ha corregido un problema de expresión de DAX que provocaba comportamientos incoherentes según cómo se confirmara la expresión.
        - Se ha corregido un problema de bloqueo de VS al crear KPI.
        - Se ha corregido un problema que generaba informes no válidos para SQL Server 2008 R2, 2012 y 2014.
        - Se ha corregido un problema de orden de jerarquía que provocaba un error de bucle infinito para proyectos de .dwpro.
        - Se ha corregido un problema de RDL de RS por el que, al cambiar RDL a una versión anterior, se requería una recompilación completa, lo que causaba confusión al usuario.
        - Se ha corregido un problema de KPI por el que Ocultar en herramientas cliente no tenía efecto.

## <a name="july-2016nbsp-ssdt-for-vs-2015"></a>Julio de 2016,&nbsp; SSDT para VS 2015

_Publicado:_ &nbsp; 30 de junio de 2016  
_Número de compilación:_ &nbsp; 14.0.60629.0  
_Para SQL Server 2016._

**Novedades**  
- **Compatibilidad con Always Encrypted:** para las bases de datos que contienen columnas de Always Encrypted, en esta versión se agrega compatibilidad completa con Always Encrypted mediante nuestras API de núcleo y la herramienta de línea de comandos (SqlPackage.exe). Puede crear y publicar proyectos de base de datos con compatibilidad total con todas las características de Always Encrypted.  
- **Compatibilidad mejorada con tablas temporales:** se ha simplificado la experiencia mediante la desvinculación de las tablas temporales antes de las modificaciones y volviéndolas a vincular una vez que estas se hayan completado. Esto significa que las tablas temporales tienen paridad con otros tipos de tabla (estándar, en memoria) en cuanto a las operaciones que se admiten. 
- **Cambios de instalación y SqlPackage.exe:** cambios para aislar SSDT del motor de SQL Server y las actualizaciones de SSMS. Para obtener más información, consulte [Changes to SSDT and SqlPackage.exe installation and updates](/archive/blogs/ssdt/changes-to-ssdt-and-sqlpackage-exe-installation-and-updates) (Cambios en la instalación y las actualizaciones de SqlPackage.exe y SSDT).


**Actualizaciones y correcciones**
* **Herramientas para bases de datos:**
    * De ahora en adelante SSDT nunca deshabilitará el cifrado de datos transparente (TDE) en una base de datos. Antes, cuando la opción de cifrado predeterminada de la configuración de la base de datos de un proyecto estaba deshabilitada, se desactivaba el cifrado. Con esta corrección, se puede habilitar el cifrado, pero nunca deshabilitar durante la publicación. 
    * Se ha aumentado el número de reintentos y la resistencia para las conexiones de Azure SQL Database durante la conexión inicial.
    * Si el grupo de archivos predeterminado no es Principal, las acciones de importar y publicar en Azure V12 producían un error. Ahora, este valor se omite cuando se publica.
    * Se ha corregido un problema por el que, al exportar una base de datos con un objeto con identificador entre comillas, la validación de la exportación podía producir un error en algunas instancias.
    * Se ha corregido un problema por el que la opción TEXTIMAGE_ON se agregaba incorrectamente para creaciones de tabla de Hekaton cuando no está permitido.
    * Se ha corregido un problema por el que la opción Exportar tardaba mucho tiempo en exportar un gran número de datos debido a una escritura en el archivo model.xml después de que se completara la fase de datos, lo que provocaba la reescritura de los contenidos del archivo .bacpac.
    * Se ha corregido un problema por el que los usuarios no aparecían en la carpeta Seguridad de las conexiones APS y DW de Azure SQL.

* **Analysis Services y Reporting Services:**
    * Se ha corregido un problema de SxS con un proveedor de MSOLAP OLEDB por el que se estaba instalando el proveedor de 32 bits, lo que afectaba a la conexión de Excel 2016 de 64 bits a SQL Server 2014 (no se reproducía con instalaciones de ClickOnce de Office365, solo con instalaciones de Excel de MSI).
    * Se ha corregido un problema para que un caso extremo sea más sólido al actualizar el modelo de AS con tablas pegadas desde el nivel de compatibilidad 1103 hasta el 1200, que podía producir el error "Relationship uses an invalid column ID" (La relación usa un id. de columna no válido).
    * Se ha corregido un problema de SxS por el que SSDT-BI 2013 en la misma máquina no podía importar datos en un modelo de AS después de desinstalar SSDT 2015 (los cartuchos compartían la configuración del registro).
    * Se ha mejorado la potencia para abordar problemas o bloqueos cuando se pierde la conexión al motor de AS (es decir, SSDT se quedó abierto durante la noche y el servidor de AS se recicló, u otros casos en los que se pierde la conexión temporalmente). 
    * Se han corregido problemas con diálogos que se abrían en pantallas distintas a la de VS en los escenarios con varios monitores. 
    * Se ha corregido y habilitado la compatibilidad para pegar desde tablas HTML (datos de cuadrícula) en tablas pegadas del modelo de AS. 
    * Se ha corregido un problema por el que la actualización no actualizaba una tabla pegada vacía a 1200 (usada solo como tabla de contenedor para medidas). 
    * Se ha corregido un problema al actualizar el modelo tabular de AS con tablas pegadas a 1200 para solucionar un problema del motor de AS con CalcTables (que se usan para tablas pegadas en 1200), para realizar un proceso completo en las nuevas tablas de cálculo tras la actualización. 
    * Se ha corregido un problema por el que podría bloquearse la cancelación de la creación de una nueva tabla calculada del modelo 1200 con una expresión DAX incompleta. 
    * Se ha corregido un problema al importar el modelo 1200 del servidor de AS al proyecto de SSDT AS cuando el nombre de la base de datos y el de la tabla eran el mismo. 
    * Se ha corregido un problema con la edición de medidas de KPI en el modelo tabular 1103. 
    * Se ha corregido una excepción de referencia de objeto no establecida que se producía al pegar una medida de KPI en la cuadrícula para un modelo 1200 de AS. 
    * Se ha corregido un problema por el que no se podía eliminar una columna de una tabla calculada de la vista de diagrama en los modelos 1200. 
    * Se ha corregido una excepción de referencia de objeto no establecida que se producía al ver las propiedades del archivo de proyecto model.bim en la vista de código. 
    * Se ha corregido un problema por el que, al pegar datos en una cuadrícula de modelo de AS para crear una tabla pegada, se producían valores incorrectos en configuraciones regionales internacionales que usan la coma como separador decimal. 
    * Se ha corregido un problema que se producía al abrir un proyecto de 2008 RS en SSDT y elegir no actualizarlo. 
    * Se ha corregido un problema en la interfaz de usuario de la tabla calculada de los modelos del nivel de compatibilidad 1200 al usar el formato predeterminado del tipo de columna para permitir cambiar el tipo de formato desde la interfaz de usuario.

## <a name="june-2016nbsp-ssdt-for-vs-2015"></a>Junio de 2016,&nbsp; SSDT para VS 2015

_Publicado:_ &nbsp; 1 de junio de 2016  
_Número de compilación:_ &nbsp; 14.0.60525.0  
_Para SQL Server 2016._

Ya se ha publicado Disponibilidad general (GA) de SSDT. La actualización de SSDT GA para junio de 2016 agrega compatibilidad con las últimas actualizaciones de SQL Server 2016 RTM y varias correcciones de errores. Para obtener más información, consulte [SQL Server Data Tools GA update for June 2016](/archive/blogs/ssdt/sql-server-data-tools-ga-update-for-june-2016) (Actualización de SQL Server Data Tools GA para junio de 2016).

## <a name="additional-resources"></a>Recursos adicionales

- [Descargar SQL Server Data Tools &#40;SSDT&#41;](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Previous releases of SQL Server Data Tools &#40;SSDT and SSDT-BI&#41;](../ssdt/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md) (Versiones anteriores de SQL Server Data Tools [SSDT y SSDT-BI])
- [Novedades de SQL Server 2016 (motor de base de datos)](../sql-server/what-s-new-in-sql-server-2016.md)
- [Novedades de Analysis Services](/analysis-services/what-s-new-in-analysis-services)
- [Novedades de Integration Services en SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)
