---
title: Limitaciones y problemas conocidos de SSIS en Linux
description: En este artículo se describen las limitaciones y los problemas conocidos de SQL Server Integration Services (SSIS) en los equipos con Linux.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 06/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 0c95b3a843cbec0e703efea846b9c80f99b9dc41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346230"
---
# <a name="limitations-and-known-issues-for-ssis-on-linux"></a>Limitaciones y problemas conocidos de SSIS en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describen las limitaciones y los problemas conocidos de SQL Server Integration Services (SSIS) en Linux.

## <a name="general-limitations-and-known-issues"></a>Limitaciones y problemas conocidos generales

Las siguientes características no se admiten en esta versión de SSIS en Linux:
  - Base de datos de catálogos de SSIS
  - Ejecución de paquetes programada por el Agente SQL
  - Autenticación de Windows
  - Componentes de terceros
  - Captura de datos modificados (CDC)
  - Escalabilidad horizontal de SSIS
  - Azure Feature Pack para SSIS
  - Compatibilidad con Hadoop y HDFS
  - Microsoft Connector for SAP BW

Para consultar otras limitaciones y problemas conocidos de SSIS en Linux, vea las [Notas de la versión](sql-server-linux-release-notes.md#ssis).

## <a name="supported-and-unsupported-components"></a><a name="components"></a> Componentes admitidos y no admitidos

En Linux se admiten los siguientes componentes de Integration Services integrados. Algunos de ellos tienen limitaciones en la plataforma Linux. Los componentes integrados que no aparecen aquí no se admiten en Linux.

## <a name="supported-control-flow-tasks"></a>Tareas de flujo de control admitidas
- Inserción masiva, tarea
- tarea Flujo de datos
- Tarea de generación de perfiles de datos
- Tarea Ejecutar SQL
- Tarea Ejecutar instrucción T-SQL
- Texto Expresión
- Tarea FTP
- Tarea Servicio web
- Tarea XML

## <a name="control-flow-tasks-supported-with-limitations"></a>Tareas de flujo de control admitidas con limitaciones

| Tarea | Limitaciones |
|------------|---|
| Tarea Ejecutar proceso | Solo admite el modo en proceso. |
| Tarea Sistema de archivos | No se admiten las acciones *Mover directorio* ni *Establecer atributos de archivo*. |
| tarea Script | Solo admite las API de .NET Framework estándar. |
| Enviar correo, tarea | Solo admite el modo de usuario anónimo. |
| Tarea Transferir bases de datos | No se admiten las rutas de acceso UNC. |
| | |

## <a name="supported-and-unsupported-maintenance-plan-tasks"></a>Tareas del plan de mantenimiento admitidas y no admitidas

En un plan de mantenimiento de SQL Server, normalmente puede usar una variedad de tareas de SSIS.

Las siguientes tareas del plan de mantenimiento no se admiten en Linux:
- Notificar al operador
- Ejecutar trabajo del Agente SQL Server

Las siguientes tareas del plan de mantenimiento se admiten en Linux:
- Comprobar la integridad de la base de datos
- Reducir base de datos
- Reorganizar índice
- Recompilar índice
- Actualizar estadísticas
- Limpiar historial
- Copia de seguridad de base de datos
- Instrucción T-SQL

## <a name="supported-control-flow-containers"></a>Contenedores de flujo de control admitidos
- contenedor de secuencias
- Contenedor de bucles For
- Contenedor Foreach Loop

## <a name="supported-data-flow-sources-and-destinations"></a>Orígenes y destinos de flujos de datos admitidos
- Origen y destino de archivo sin formato
- Origen XML

## <a name="data-flow-sources-and-destinations-supported-with-limitations"></a>Orígenes y destinos de flujos de datos admitidos con limitaciones

| Componente | Limitaciones |
|------------|---|
| Origen y destino de ADO.NET | Solo se admite el proveedor de datos SQLClient. |
| Origen y destino de archivo plano | Solo se admiten rutas de acceso de archivo de tipo Windows a las que se aplica la regla de asignación de rutas de acceso predeterminada. Por ejemplo, `D:\home\ssis\travel.csv` se convierte en `/home/ssis/travel.csv`. |
| Origen OData | Solo admite la autenticación básica. |
| Origen y destino de ODBC | Admite controladores ODBC Unicode de 64 bits en Linux. Depende del administrador de controladores UnixODBC en Linux. |
| Origen y destino de OLE DB | Solo se admiten SQL Server Native Client 11.0 y el proveedor OLE DB de Microsoft para SQL Server. |
| | |

## <a name="supported-data-flow-transformations"></a>Transformaciones de flujos de datos admitidas
- Agregado
- Auditoría
- Balanced Data Distributor
- Mapa de caracteres
- División condicional
- Copiar columna
- Conversión de datos
- Columna derivada
- Exportar columna
- agrupación aproximada
- Búsqueda aproximada
- Importar columna
- Búsqueda
- Merge
- Merge Join
- Multidifusión
- Dinamización
- Recuento de filas
- Dimensión de variación lenta
- Sort
- Búsqueda de términos
- Unión de todo
- Anulación de dinamización

## <a name="data-flow-transformations-supported-with-limitations"></a>Transformaciones de flujos de datos admitidas con limitaciones

| Componente | Limitaciones |
|------------|---|
| transformación Comando de OLE DB | Las mismas limitaciones que el origen y destino de OLE DB. |
| componente de script | Solo admite las API de .NET Framework estándar. |
| | |

## <a name="supported-and-unsupported-log-providers"></a>Proveedores de registro admitidos y no admitidos
En Linux se admiten todos los proveedores de registro de SSIS integrados, excepto el proveedor de registro de eventos de Windows.

El proveedor de registro de SQL Server solo admite la autenticación de SQL; no admite la autenticación de Windows.

Los proveedores de registro de SSIS para archivos de texto, archivos XML y SQL Server Profiler escriben los resultados en el archivo que especifique. Las siguientes consideraciones son válidas para la ruta de acceso del archivo:
-   Si no proporciona una ruta de acceso, el proveedor de registro escribe en el directorio actual del host. Si el usuario actual no tiene permiso para escribir en el directorio actual del host, el proveedor de registro produce un error.
-   No se puede usar una variable de entorno en una ruta de acceso de archivo. Si especifica una variable de entorno, el texto literal que especifique aparece en la ruta de acceso del archivo. Por ejemplo, si especifica `%TMP%/log.txt`, el proveedor de registro anexa el texto literal `/%TMP%/log.txt` al directorio del host actual.

## <a name="related-content-about-ssis-on-linux"></a>Contenido relacionado sobre SSIS en Linux
-   [Extracción, transformación y carga de datos en Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md)
-   [Configurar SQL Server Integration Services en Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Programación de la ejecución de paquetes de SQL Server Integration Services en Linux con cron](sql-server-linux-schedule-ssis-packages.md)
