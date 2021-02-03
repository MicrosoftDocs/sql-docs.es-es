---
description: MSSQLSERVER_844
title: MSSQLSERVER_844 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 844 (Database Engine error)
ms.assetid: 2060c886-1226-4066-bc0c-de90a1cfb82b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0ae116b0ef06d82111e8ca356ae3f9c9a272f729
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186798"
---
# <a name="mssqlserver_844"></a>MSSQLSERVER_844
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|844|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|BUFLATCH_TIMEOUT_CONTINUE|  
|Texto del mensaje|Tiempo de espera agotado mientras se esperaba el bloqueo temporal del búfer -- tipo %d, bp %p, página %d:%d, stat %#x, id. de base de datos: %d, id. de unidad de asignación: %I64d%ls, tarea 0x%p : %d, tiempo de espera %d, marcas 0x%I64x, tarea propietaria 0x%p.  Esperando.|  
  
## <a name="explanation"></a>Explicación
Un proceso de SQL está esperando para adquirir un bloqueo temporal. Este problema puede estar causado por una operación de E/S que está tardando mucho en llevarse a cabo. Normalmente, este tipo de error es el resultado de que otras tareas estén bloqueando los procesos del sistema. En algunos casos, este error puede deberse a un problema de hardware.  Cuando se produce este mensaje de error, puede ser que el equipo y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dejen de responder.

## <a name="cause"></a>Causa
Este mensaje de error depende del entorno general del sistema. Cualquiera de las siguientes circunstancias puede conducir a la sobrecarga del sistema:

- Hardware que no cumple los requisitos de entrada y salida (E/S) ni de memoria.
- Configuraciones mal establecidas y mal probadas.
- Diseño ineficaz.

 Puede ser que reciba el error 844 cuando el sistema esté sometido a una carga pesada y no pueda satisfacer las demandas de la carga de trabajo. Algunas de las causas más comunes de un entorno con estrés son:

- Problemas de hardware
- Volúmenes comprimidos
- Ajustes de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no predeterminados
- Diseño del índice o consultas ineficaces
- Operaciones frecuentes de crecimiento automático o reducción automática de la base de datos

## <a name="user-action"></a>Acción del usuario  
Para evitar que ocurra este error, intente lo siguiente:  
  
- Determine si tiene algún cuellos de botella de hardware. Consulte [Identificar los cuellos de botella](../performance/identify-bottlenecks.md) para empezar. Si es necesario, actualice el hardware para que pueda satisfacer las necesidades de la configuración, las consultas y la carga del entorno.

- Compruebe que todo el hardware funcione correctamente. Compruebe si hay algún error registrado y ejecute los diagnósticos suministrados por el proveedor de hardware. Busque errores de E/S asociados en el registro de errores o el registro de eventos. Normalmente, los errores de E/S indican un funcionamiento incorrecto del disco.  
- Asegúrese de que los volúmenes del disco no estén comprimidos. No se admite el almacenamiento de datos y archivos de registro en unidades comprimidas. Vea [Archivos y grupos de archivos de base de datos](../databases/database-files-and-filegroups.md). Para obtener más información sobre la compatibilidad con unidades comprimidas, consulte el artículo siguiente: [Bases de datos de SQL Server no admitidas en volúmenes comprimidos](https://support.microsoft.com/EN-US/help/231347).

- Compruebe si los mensajes de error desaparecen al desactivar todas las siguientes opciones de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:
   - [Opción de aumento de prioridad](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)
   - [Opción de agrupación ligera (modo de fibra)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)
   - [Opción "set working set size"](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md)

    Para obtener más información, consulte [Cómo determinar la configuración apropiada de SQL Server](https://support.microsoft.com/EN-US/help/319942).

- Optimice las consultas para reducir los recursos utilizados en el sistema. La optimización del rendimiento ayudará a reducir la demanda en un sistema y a mejorar el tiempo de respuesta de las consultas individuales.
- Establezca la propiedad AutoShrink en OFF para reducir la sobrecarga de los cambios de tamaño de la base de datos.
- Asegúrese de establecer la propiedad AutoGrow en incrementos lo suficientemente grandes como para ser infrecuentes. Programe un trabajo para comprobar el espacio disponible en las bases de datos y, a continuación, aumente el tamaño de la base de datos durante las horas de menor actividad.
- Compruebe el registro de errores para ver si hay tareas que no rinden y otros errores críticos. Resuelva los errores primero, ya que podrían apuntar a la causa principal del problema subyacente.
- Si se producen frecuentemente errores críticos, como aserciones, solucione estos problemas.
- Si los mensajes de error 844 son poco frecuentes, puede omitirlos.