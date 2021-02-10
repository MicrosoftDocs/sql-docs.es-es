---
description: Configuración del proyecto (migración) (MySQLToSQL)
title: Configuración del proyecto (migración) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2a3cba9e-cd54-4a8b-b858-8fc4cf2580d9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92c6e91ca49046561c24d7716dd5a3e152e686f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017375"
---
# <a name="project-settings-migration-mysqltosql"></a>Configuración del proyecto (migración) (MySQLToSQL)
La página migración del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan el modo en que SSMA migra los datos de MySQL a SQL Server.  
  
El panel Migración está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** .  
  
-   Para especificar la configuración de todos los proyectos de SSMA, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione el tipo de proyecto en la lista desplegable de **versiones de destino** de la migración de la que desea obtener acceso a la configuración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **migración**.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , seleccione **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **migración**.  
  
## <a name="options"></a>Opciones  
  
### <a name="bulk-copy"></a>Copia masiva  
  
|Término|Definición|  
|--------|--------------|  
|**Tamaño de lote**|Especifica el tamaño del lote que se utiliza durante la migración de datos.<br /><br />**Modo predeterminado**: 1000<br /><br />**Modo optimista**: 1000<br /><br />**Modo completo**: 1000|  
|**Restricciones CHECK**|Especifica si SSMA debe comprobar las restricciones cuando inserta datos en SQL Server tablas.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
|**Activar desencadenadores**|Especifica si SSMA debe activar los desencadenadores de inserción cuando agrega datos a SQL Server tablas.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
|**Mantener valores de identidad**|Especifica si SSMA conserva los valores de identidad de MySQL cuando agrega datos a SQL Server. Un valor false hace que el destino asigne los valores de identidad.<br /><br />**Modo predeterminado**: true<br /><br />**Modo optimista**: true<br /><br />**Modo completo**: true|  
|**Mantener valores NULL**|Especifica si SSMA conserva los valores NULL en los datos de origen cuando agrega datos a SQL Server, independientemente de los valores predeterminados que se especifiquen en SQL Server.<br /><br />**Modo predeterminado**: true<br /><br />**Modo optimista**: true<br /><br />**Modo completo**: true|  
|**Bloqueo de tabla**|Especifica si SSMA bloquea las tablas cuando agrega datos a las tablas durante la migración de datos. Obtiene un bloqueo de actualización masiva mientras dure la operación de copia masiva. Si el valor es false, se establece un bloqueo en el nivel de fila.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
  
### <a name="data-modification"></a>Modificación de datos  
  
|Término|Definición|  
|--------|--------------|  
|**Migración de fechas no válidas**|Especifica cómo migrar fechas no válidas con like ' 2007-04-23 ' o ' 2000-06-31 10:00:00 ' en formatos DATE y DATETIME.<br /><br />**Modo predeterminado**: establecer null<br /><br />**Modo optimista**: establecer null<br /><br />**Modo completo**: establecer null|  
|**Migración de valores de tiempo negativos**|Especifica cómo migrar valores negativos como '-30:11:00 ' en columnas de tiempo.<br /><br />**Modo predeterminado**: establecer null<br /><br />**Modo optimista**: establecer null<br /><br />**Modo completo**: establecer null|  
|**Migración de valores de tiempo durante 24 horas**|Especifica cómo migrar los valores de hora de más de ' 23:59:59 ' en las columnas de tiempo.<br /><br />**Modo predeterminado**: establecer null<br /><br />**Modo optimista**: establecer null<br /><br />**Modo completo**: establecer null|  
|**Truncar valores binarios para caber en la columna**|En caso afirmativo, SSMA trunca los valores binarios de MySQL que no caben en las columnas de la tabla SQL y genera un mensaje de error adecuado. Si no es así, la fila produce un error.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: no|  
|**Truncar valores de caracteres para caber en la columna**|SSMA trunca los valores de caracteres de MySQL que no se ajustan a las columnas de la tabla SQL y genera un mensaje de error adecuado.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: no|  
|**Migración de fechas cero**|Especifica cómo migrar fechas cero como ' 0000-00-00 ' o ' 0000-00-00 00:00:00 ' en columnas DATE y DATETIME.<br /><br />**Modo predeterminado**: establecer null<br /><br />**Modo optimista**: establecer null<br /><br />**Modo completo**: establecer null|  
|**Migración de cero en fechas**|Especifica cómo migrar fechas con cero partes como ' 2009-01-00 ' o ' 2000-00-00 11:00:00 ' en columnas DATE y DATETIME.<br /><br />**Modo predeterminado**: establecer null<br /><br />**Modo optimista**: establecer null<br /><br />**Modo completo**: establecer null|  
  
### <a name="migration-engine"></a>Motor de migración  
  
|Término|Definición|  
|--------|--------------|  
|**Motor de migración**|Especifica el motor de base de datos usado durante la migración de datos. La migración de datos del lado cliente hace referencia al cliente de SSMA que recupera los datos del origen y realiza la inserción masiva de esos datos en SQL Server. La migración de datos del lado servidor hace referencia a SSMA Data Migration Engine (programa de copia masiva) que se ejecuta en el cuadro de SQL Server como un trabajo del Agente SQL que recupera datos del origen e inserta directamente en SQL Server evitando así un salto de cliente adicional (mejor rendimiento).<br /><br />**Modo predeterminado**: motor de migración de datos del lado cliente<br /><br />**Modo optimista**: motor de migración de datos del lado cliente<br /><br />**Modo completo**: motor de migración de datos del lado cliente|  
  
> [!IMPORTANT]  
> Cuando la opción **motor de migración** está establecida en el motor de migración de **datos del lado servidor**, se muestra una nueva opción **de configuración del proyecto usar el motor de migración de datos del lado servidor de 32** bits. Especifica si se utiliza la utilidad de copia masiva (BCP) de 32 bits o de 64 bits para migrar los datos.  
  
### <a name="misc"></a>Varios  
  
|Término|Definición|  
|--------|--------------|  
|**Opciones de migración de datos extendidos**|Muestra opciones de migración de datos adicionales para cada tabla en pestañas de detalles independientes.<br /><br />**Modo predeterminado**: ocultar<br /><br />**Modo optimista**: ocultar<br /><br />**Modo completo**: ocultar|  
|**Al producirse un error**|Detiene la migración de datos cuando se produce un error. Tiene tres opciones:<br /><br />**Detener migración:** Detiene la operación de migración de datos<br /><br />**Continuar con la siguiente tabla:** Detiene la migración de datos a la tabla actual y continúa con el siguiente.<br /><br />**Continuar con el siguiente lote:** Detiene la migración de datos al lote actual y continúa con el siguiente.<br /><br />**Modo predeterminado**: continuar con el siguiente lote<br /><br />**Modo optimista**: continuar con el siguiente lote<br /><br />**Modo completo**: continuar con el siguiente lote|  
  
### <a name="parallel-data-migration"></a>Migración de datos en paralelo  
  
|Término|Definición|  
|--------|--------------|  
|**Modo de migración de datos paralelos**|Especifica el modo utilizado para bifurcar los subprocesos para habilitar la migración de datos en paralelo. En el modo auto, SSMA elige el número de subprocesos (10 de forma predeterminada) bifurcado para migrar los datos. En el modo personalizado, el usuario puede especificar el número de subprocesos bifurcados para migrar datos (el mínimo es 1 y el máximo es 100). Actualmente, solo el motor de migración de datos del lado cliente admite la migración de datos en paralelo.<br /><br />**Modo predeterminado**: automático<br /><br />**Modo optimista**: automático<br /><br />**Modo completo**: automático|  
  
> [!IMPORTANT]  
> Cuando la opción **modo de migración de datos paralelos** está establecida en **personalizado**, se muestra una nueva opción de configuración de proyecto **recuento de subprocesos** . Especifica el número de subprocesos usados para la migración de datos.  
  
### <a name="spatial-data"></a>Datos espaciales  
  
|Término|Definición|  
|--------|--------------|  
|**Control de errores**|Especifica cómo controlar los errores en la migración de valores de tipos de datos espaciales. Si se especifica ' Replace with NULL ', todos los valores espaciales que producen errores se reemplazarán por NULL. En caso contrario, no se realiza ningún reemplazo.<br /><br />**Modo predeterminado**: generar un error<br /><br />**Modo optimista**: generar un error<br /><br />**Modo completo**: generar un error|  
|**Validación de valores**|Especifica cómo tratar los valores espaciales no válidos. Si se especifica ' try make Valid ', se realiza un intento de modificar los valores no válidos para que sean válidos.<br /><br />**Modo predeterminado**: make Valid<br /><br />**Modo optimista**: no cambiar<br /><br />**Modo completo**: make Valid|  
  
