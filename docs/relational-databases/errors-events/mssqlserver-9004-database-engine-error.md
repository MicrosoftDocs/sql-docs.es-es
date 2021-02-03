---
description: MSSQLSERVER_9004
title: MSSQLSERVER_9004 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9004 (Database Engine error)
ms.assetid: b528bb49-340c-4a81-9c8d-cefce6562f16
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b4706982db7c2c121af0658142344c0558321b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161970"
---
# <a name="mssqlserver_9004"></a>MSSQLSERVER_9004
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|9004|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LOG_CORRUPT|  
|Texto del mensaje|Se produjo un error mientras se procesaba el registro para la base de datos '%.*ls'.  If possible, restore from backup. Si no dispone de una copia de seguridad, puede ser necesario generar de nuevo el registro.|  
  
## <a name="explanation"></a>Explicación  
Se encontró un error al procesar el registro durante la operación de reversión, recuperación o replicación. Esto puede indicar un error detectado por el sistema operativo o un error de coherencia interno detectado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] realiza comprobaciones lógicas de la coherencia del contenido del registro de transacciones mientras lo lee y lo procesa. No se comprueban todos los aspectos del encabezado de registro, de los bloques de registro y de las entradas de registro. El número de estado proporciona más información sobre el tipo de error:

 - **Estado 1** El encabezado del archivo de registro del archivo de registro virtual (VLF) estaba dañado.  Si se encuentra un encabezado del archivo de registro dañado como parte del inicio de la base de datos al iniciar el servicio, es posible que aparezca el error 9004 en el REGISTRO DE ERRORES. El encabezado del archivo de registro es la primera parte de cada VLF dentro de un registro de transacciones. El encabezado del archivo de registro no es el mismo que el encabezado de un solo archivo o que los primeros 8 KB del archivo de registro. Si el encabezado de archivo del archivo de registro está dañado, puede recibir el mensaje 5172, de forma similar a lo que ocurre cuando la página del encabezado de archivos de bases de datos está dañada.
 - **Estados 2 y 3** Un bloque de registro no era válido al realizar la recuperación durante una operación de RESTAURACIÓN.
 - **Estados 4 a 12** Son todas las comprobaciones de los bloques de registro al procesar los registros. Entre ellas se incluyen la paridad, el sector y otras comprobaciones lógicas de la coherencia del registro de transacciones.

En la mayoría de los casos, este error solo aparece en el REGISTRO DE ERRORES o en el registro de eventos de la aplicación de Windows con el identificador de evento 9004, porque la operación que procesa el registro no se basa en un comando de usuario directo (como una recuperación que se ejecuta cuando se inicia el motor de SQL Server). En estas situaciones, el error 9004 suele aparecer junto con el error 3414. Sin embargo, algunas consultas como ALTER DATABASE podrían requerir el procesamiento del registro y, por tanto, aparecerán estos errores. Dado que el error se presenta como Gravedad=21, la sesión de usuario se desconecta.

## <a name="cause"></a>Causa
El error 9004 es un error general que indica que el contenido del registro de transacciones está dañado. La razón por la que el registro se vuelve incoherente es similar a cualquier problema de daños en la base de datos detectado por el motor de SQL Server. Para encontrar la causa de los daños en el registro, debe seguir técnicas similares que se usan para los daños en la base de datos, incluido un análisis de posibles problemas de hardware, del sistema de archivos y de E/S. Tenga en cuenta que DBCC CHECKDB no comprueba el registro de transacciones como parte de sus operaciones y no puede detectar errores de daños en el registro. El motor de SQL Server genera el error 9004.

## <a name="user-action"></a>Acción del usuario  
Este error se corregirá con una de las siguientes acciones:  
  
-   **Restauración a partir de una copia de seguridad**:  restaure a partir de una copia de seguridad correcta conocida para recuperarse de este problema. Si la parte del registro de la copia de seguridad de una base de datos o de un registro incluye contenido dañado, puede que se produzca el error 9004 en la RESTAURACIÓN. En esta situación, el registro de transacciones de la copia de seguridad está dañado.
  
-   **Recompilación del registro**:  si no puede realizar la restauración a partir de una copia de seguridad, es posible que pueda poner en línea la base de datos mediante la recompilación del registro de transacciones. Debe comprender de manera exhaustiva las ramificaciones de la recompilación del registro de transacciones. Esto incluye una *posible pérdida de coherencia transaccional de la base de datos*. Para más información sobre cómo recompilar el registro de transacciones, vea [Resolver errores en modo de emergencia de base de datos](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode).
  
-   **Análisis de registros en busca de problemas del sistema**: además, consulte el registro de eventos del sistema y los registros de errores para identificar posibles causas del problema en el sistema.  
  
