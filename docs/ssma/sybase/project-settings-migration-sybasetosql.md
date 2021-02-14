---
description: Configuración del proyecto (migración) (SybaseToSQL)
title: Configuración del proyecto (migración) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 82f8857f-7ab1-4738-ab6e-b1e95ea94924
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 96b5550449e11cd314e736a36e3845ba92f542c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069945"
---
# <a name="project-settings-migration-sybasetosql"></a>Configuración del proyecto (migración) (SybaseToSQL)
La página migración del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan el modo en que SSMA migra los datos de Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
El panel Migración está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** .  
  
-   Para especificar la configuración de todos los proyectos de SSMA, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione el tipo de proyecto de migración para el que se deben ver los valores de configuración/Changed en el menú desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **migración**.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , seleccione **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, haga clic en **migración**.  
  
## <a name="date-correction-options"></a>Opciones de corrección de fecha  
  
|Término|Definición|  
|--------|--------------|  
|**Reemplazar fechas no admitidas**|Especifica si SSMA debe corregir las fechas anteriores a la fecha de fecha [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **y hora** más antigua (01 de enero 1753).<br /><br />Para mantener los valores de fecha actuales, seleccione **no hacer nada**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no aceptará fechas anteriores al 01 de enero de 1753 en una columna de fecha y hora. Si utiliza fechas anteriores, debe convertir los valores DATETIME en valores de caracteres.<br /><br />Para convertir las fechas anteriores al 01 de enero de 1753 a NULL, seleccione **reemplazar con NULL**.<br /><br />Para reemplazar las fechas anteriores al 01 de enero de 1753 con una fecha admitida, seleccione **reemplazar con la fecha más cercana admitida**.<br /><br />**Modo predeterminado**: no hacer nada<br /><br />**Modo optimista**: no hacer nada<br /><br />**Modo completo**: reemplazar con la fecha más cercana admitida|  
  
## <a name="migration-engine"></a>Motor de migración  
  
|Término|Definición|  
|--------|--------------|  
|**Motor de migración**|Especifica el motor de base de datos usado durante la migración de datos. La migración de datos del lado cliente hace referencia al cliente de SSMA que recupera los datos del origen y realiza la inserción masiva de esos datos en SQL Server. La migración de datos del lado servidor hace referencia a SSMA Data Migration Engine (programa de copia masiva) que se ejecuta en el cuadro de SQL Server como un trabajo del Agente SQL que recupera datos del origen e inserta directamente en SQL Server evitando así un salto de cliente adicional (mejor rendimiento).<br /><br />**Modo predeterminado**: motor de migración de datos del lado cliente<br /><br />**Modo optimista**: motor de migración de datos del lado cliente<br /><br />**Modo completo**: motor de migración de datos del lado cliente|  
  
> [!IMPORTANT]  
> Cuando la opción **motor de migración** está establecida en el motor de migración de **datos del lado servidor**, se muestra una nueva opción **de configuración del proyecto usar el motor de migración de datos del lado servidor de 32** bits. Especifica si se utiliza la utilidad de copia masiva (BCP) de 32 bits o de 64 bits para migrar los datos.  
  
## <a name="miscellaneous-options"></a>Otras opciones  
  
|Término|Definición|  
|--------|--------------|  
|**Tamaño de lote**|Especifica el tamaño del lote que se utiliza durante la migración de datos.<br /><br />**Modo predeterminado**: 10000<br /><br />**Modo optimista**: 10000<br /><br />**Modo completo**: 10000|  
|**Restricciones CHECK**|Especifica si SSMA debe comprobar las restricciones cuando inserta datos en SQL Server tablas.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
|**Tiempo de espera de migración de datos**|Especifica el tiempo de espera usado durante la migración de datos<br /><br />**Modo predeterminado**: 15<br /><br />**Modo optimista**: 15<br /><br />**Modo completo**: 15|  
|**Opciones de migración de datos extendidos**|Muestra opciones de migración de datos adicionales para cada tabla en pestañas de detalles independientes.<br /><br />**Modo predeterminado**: ocultar<br /><br />**Modo optimista**: ocultar<br /><br />**Modo completo**: ocultar|  
|**Activar desencadenadores**|Especifica si SSMA debe activar los desencadenadores de inserción cuando agrega datos a SQL Server tablas.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
|**Mantener valores de identidad**|Especifica si SSMA conserva los valores de identidad de Sybase cuando agrega datos a SQL Server. Un valor false hace que el destino asigne los valores de identidad.<br /><br />**Modo predeterminado**: true<br /><br />**Modo optimista**: true<br /><br />**Modo completo**: true|  
|**Mantener valores NULL**|Especifica si SSMA conserva los valores NULL en los datos de origen cuando agrega datos a SQL Server, independientemente de los valores predeterminados que se especifiquen en SQL Server.<br /><br />**Modo predeterminado**: true<br /><br />**Modo optimista**: true<br /><br />**Modo completo**: true|  
|**Al producirse un error**|Detiene la migración de datos cuando se produce un error. Tiene tres opciones:<br /><br />**Detener migración:** Detiene la operación de migración de datos<br /><br />**Continuar con la siguiente tabla:** Detiene la migración de datos a la tabla actual y continúa con el siguiente.<br /><br />**Continuar con el siguiente lote:** Detiene la migración de datos al lote actual y continúa con el siguiente.<br /><br />**Modo predeterminado**: continuar con el siguiente lote<br /><br />**Modo optimista**: continuar con el siguiente lote<br /><br />**Modo completo**: continuar con el siguiente lote|  
|**Redondear parte fraccionaria de números**|Especifica si se deben recortar las partes fraccionarias de datos decimales y numéricos durante la migración a tipos enteros o mostrar un mensaje de error si la parte fraccionaria no es trivial.<br /><br />**Modo predeterminado**: no<br /><br />**Modo optimista**: no<br /><br />**Modo completo**: no|  
|**Sybase Unicode endian**|Especifica el tipo endian para las cadenas Unicode de Sybase. Se pueden establecer las siguientes opciones para esta configuración determinada:<br /><br />Little-endian<br /><br />Big-Endian<br /><br />**Modo predeterminado**: Little-endian<br /><br />**Modo optimista**: Little-endian<br /><br />**Modo completo**: Little-endian|  
|**Bloqueo de tabla**|Especifica si SSMA bloquea las tablas cuando agrega datos a las tablas durante la migración de datos. Obtiene un bloqueo de actualización masiva mientras dure la operación de copia masiva. Si el valor es false, se establece un bloqueo en el nivel de fila.<br /><br />**Modo predeterminado**: true<br /><br />**Modo optimista**: true<br /><br />**Modo completo**: true|  
|**Usar cursores**|Los datos se recuperan de la base de datos de origen mediante cursores si se establece esta opción.<br /><br />**Modo predeterminado**: false<br /><br />**Modo optimista**: false<br /><br />**Modo completo**: false|  
  
## <a name="parallel-data-migration"></a>Migración de datos en paralelo  
  
|Término|Definición|  
|--------|--------------|  
|**Modo de migración de datos paralelos**|Especifica el modo utilizado para bifurcar los subprocesos para habilitar la migración de datos en paralelo. En el modo auto, SSMA elige el número de subprocesos (10 de forma predeterminada) bifurcado para migrar los datos. En el modo personalizado, el usuario puede especificar el número de subprocesos bifurcados para migrar datos (el mínimo es 1 y el máximo es 100). Actualmente, solo el motor de migración de datos del lado cliente admite la migración de datos en paralelo.<br /><br />**Modo predeterminado**: automático<br /><br />**Modo optimista**: automático<br /><br />**Modo completo**: automático|  
  
> [!IMPORTANT]  
> Cuando la opción **modo de migración de datos paralelos** está establecida en **personalizado**, se muestra una nueva opción de configuración de proyecto **recuento de subprocesos** . Especifica el número de subprocesos usados para la migración de datos.  
  
