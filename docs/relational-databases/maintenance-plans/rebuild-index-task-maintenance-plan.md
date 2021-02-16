---
title: Tarea Volver a generar índice (Plan de mantenimiento) | Microsoft Docs
description: Obtenga información sobre cómo volver a crear los índices de las tablas de la base de datos de SQL Server con un nuevo factor de relleno mediante la tarea Recompilar índice.
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- reindex
- sql13.swb.maint.reindex.f1
helpviewer_keywords:
- Rebuild Index Task dialog box
ms.assetid: 33e2940b-139f-4563-b0cb-5683f08bd879
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6c95f60259fb25b457e4349b6e0798ce4fa0e6fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351187"
---
# <a name="rebuild-index-task-maintenance-plan"></a>Tarea Volver a generar índice (Plan de mantenimiento)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Use el cuadro de diálogo **Tarea Volver a generar índice** para volver a crear los índices de las tablas de la base de datos con un nuevo factor de relleno. El factor de relleno determina la cantidad de espacio vacío de cada una de las páginas del índice, para adaptarse a una futura expansión. Al agregar datos a la tabla, el espacio disponible se llena, ya que no se conserva el factor de relleno. Al reorganizar las páginas de datos y de índices, puede restablecer el espacio disponible.  
  
 La **tarea Volver a generar índice** usa la instrucción ALTER INDEX. Para obtener más información sobre las opciones descritas en esta página, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
## <a name="options"></a>Opciones  
 **Connection**  
 Seleccione la conexión al servidor que va a utilizar para la realización de esta tarea.  
  
 **Nuevo**  
 Cree una nueva conexión de servidor que utilizará al realizar esta tarea. El cuadro de diálogo **Nueva conexión** se describe a continuación.  
  
 **Bases de datos**  
 Especifique las bases de datos a las que afecta esta tarea.  
  
-   **Todas las bases de datos**  
  
     Genera un plan de mantenimiento que ejecuta tareas de mantenimiento en todas las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , a excepción de tempdb.  
  
-   **Todas las bases de datos del sistema**  
  
     Genera un plan de mantenimiento que ejecuta tareas de mantenimiento en todas las bases de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , a excepción de tempdb. No se ejecutarán tareas de mantenimiento en las bases de datos creadas por usuarios.  
  
-   **Todas las bases de datos de usuario**  
  
     Genera un plan de mantenimiento que ejecuta tareas de mantenimiento en todas las bases de datos creadas por usuarios. No se ejecutarán tareas de mantenimiento en las bases de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Las bases de datos**  
  
     Genera un plan de mantenimiento que ejecuta tareas de mantenimiento únicamente en las bases de datos seleccionadas. Si elige esta opción, deberá seleccionar al menos una base de datos de la lista.  
  
    > [!NOTE]  
    >  Los planes de mantenimiento solo se ejecutan en bases de datos con un nivel de compatibilidad de 80 o superior. Las bases de datos con un nivel de compatibilidad de 70 o inferior no se muestran.  
  
 **Object**  
 Limite la cuadrícula **Selección** para mostrar tablas, vistas o ambas cosas.  
  
 **Selección**  
 Especifique las tablas o índices que se ven afectados por esta tarea. No estará disponible cuando se seleccione **Tablas y vistas** en el cuadro Objeto.  
  
 **Espacio disponible predeterminado por página**  
 Quita los índices de las tablas de la base de datos y vuelve a crearlos con el factor de relleno que se especificó al crear los índices.  
  
 **Cambiar el espacio disponible por página a**  
 Quita los índices de las tablas de la base de datos y vuelve a crearlos con un nuevo factor de relleno calculado automáticamente, de forma que reserva la cantidad de espacio disponible especificada en las páginas de índice. Cuanto mayor sea el porcentaje, más espacio disponible se reservará en las páginas de índice y mayor tamaño tendrá el índice. Los valores válidos son de 0 a 100.  
  
 **Ordenar resultados de tempdb**  
 Use la opción `SORT_IN_TEMPDB` , que determina el lugar de almacenamiento temporal de los resultados de orden intermedio generados durante la creación del índice. En caso de que sea necesario realizar una operación de ordenación o de que esta pueda realizarse en la memoria, se omitirá la opción `SORT_IN_TEMPDB`.  
  
 **Rellenar índice**  
 Especifique el relleno del índice.  
  
 **Mantener el índice en línea**  
 Utilice la opción `ONLINE` para permitir a los usuarios obtener acceso a los datos de la tabla subyacente o del índice clúster y a todos los índices no clúster asociados durante las operaciones de índice.  
  
> [!NOTE]
>  Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **No volver a generar índices | Volver a generar índices sin conexión**  
 Especifique qué hacer para los tipos de índice que no pueden regenerarse mientras estén en línea.  
  
 **MAXDOP**  
 Utilice MAXDOP para limitar el número de procesadores utilizados en la ejecución de un plan paralelo.  
  
 **Low Priority Used (Prioridad baja usada)**  
 Seleccione esta opción para esperar bloqueos de prioridad baja.  
  
 **Abort after Wait (Anular tras la espera)**  
 Especifique qué hacer después de que el tiempo especificado por **Duración máxima** haya transcurrido.  
  
 **Duración máxima**  
 Especifique cuánto tiempo debe esperar bloqueos de prioridad baja.  
  
 **Ver T-SQL**  
 Muestra las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] realizadas en el servidor para esta tarea, en función de las opciones seleccionadas.  
  
> [!NOTE]  
>  Si el número de objetos afectados es elevado, es posible que deba esperar un rato hasta que se muestren.  


[!INCLUDE[index-stats-options-reorg-5589131-2999104](../../includes/paragraph-content/index-stats-options-reorganize-maintenance-plan-include.md)]

  
## <a name="new-connection-dialog-box"></a>Cuadro de diálogo Nueva conexión  
 **Nombre de la conexión**  
 Escriba un nombre para la nueva conexión.  
  
 **Seleccionar o especificar un nombre de servidor**  
 Seleccione un servidor al que conectarse cuando se realice esta tarea.  
  
 **Actualizar**  
 Actualiza la lista de servidores disponibles.  
  
 **Especificar información para iniciar sesión en el servidor**  
 Especifica el modo de autenticación en el servidor.  
  
 **Usar seguridad integrada de Windows NT**  
 Se conecta a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] con la autenticación de Windows.  
  
 **Utilizar un nombre de usuario y una contraseña específicos**  
 Se conecta a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] con la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esta opción no está disponible.  
  
 **Nombre de usuario**  
 Proporcione un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la autenticación. Esta opción no está disponible.  
  
 **Contraseña**  
 Proporcione una contraseña para que se utilice en la autenticación. Esta opción no está disponible.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [Opción SORT_IN_TEMPDB para índices](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)   
 [Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md)   
 [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md)   
 [Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)  
  
  
