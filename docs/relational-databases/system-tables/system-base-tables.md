---
description: Tablas base del sistema
title: Tablas base del sistema | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system base tables [SQL Server]
- hobt [SQL Server]
- base tables
ms.assetid: 31f2df90-651f-4699-8067-19f59b60904f
author: cawrites
ms.author: chadam
ms.openlocfilehash: e01c89ef3903e78b8ba312914c97eed61bd8757e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208896"
---
# <a name="system-base-tables"></a>Tablas base del sistema
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Las tablas base del sistema son las tablas subyacentes que almacenan los metadatos para una base de datos específica. La base de datos **maestra** es especial en este sentido porque contiene algunas tablas adicionales que no se encuentran en ninguna de las otras bases de datos. Estas tablas contienen metadatos persistentes con un ámbito para todo el servidor.  
  
> [!IMPORTANT]  
>  Las tablas base del sistema se utilizan solo en [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y no son para el uso general de los clientes. Están sujetos a cambios y su compatibilidad no está garantizada.  
  
## <a name="system-base-table-metadata"></a>Metadatos de tablas base del sistema  
 Un receptor que tenga el permiso CONTROL, ALTER o VIEW DEFINITION en una base de datos puede ver los metadatos de la tabla base del sistema en la vista de catálogo **Sys. Objects** . El receptor también puede resolver los nombres y los identificadores de objeto de las tablas base del sistema mediante funciones integradas como [OBJECT_NAME](../../t-sql/functions/object-name-transact-sql.md) y [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md).  
  
 Para enlazar con una tabla base del sistema, un usuario tiene que conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando la conexión de administrador dedicada (DAC). Si intentar ejecutar una consulta SELECT de una tabla base del sistema sin conectarse a través de la conexión DAC, se producirá un error.  
  
> [!IMPORTANT]  
>  El acceso a las tablas base del sistema mediante DAC solo está diseñado para el personal de [!INCLUDE[msCoName](../../includes/msconame-md.md)] y no es un escenario de cliente compatible.  
  
## <a name="system-base-tables"></a>Tablas base del sistema  
 En la tabla siguiente se enumeran y describen todas las tablas base del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Tabla base|Descripción|  
|----------------|-----------------|  
|**sys.sysschobjs**|Existe en todas las bases de datos. Cada fila representa un objeto en la base de datos.|  
|**sys.sysbinobjs**|Existe en todas las bases de datos. Contiene una fila para cada entidad de Service Broker en la base de datos. Las entidades de Service Broker contienen los siguientes elementos:<br /><br /> Tipo de mensaje<br /><br /> Contrato de servicio<br /><br /> Servicio<br /><br /> Los nombres y tipos utilizan intercalación binaria fija.|  
|**sys.sysclsobjs**|Existe en todas las bases de datos. Contiene una fila para cada entidad clasificada que comparte las mismas propiedades comunes, entre las que se incluyen las siguientes:<br /><br /> Ensamblado<br /><br /> Dispositivo de copia de seguridad<br /><br /> Catálogo de texto completo<br /><br /> Función de partición<br /><br /> Esquema de partición<br /><br /> Grupo de archivos<br /><br /> Clave de ofuscación|  
|**sys.sysnsobjs**|Existe en todas las bases de datos. Contiene una fila para cada entidad centrada en el espacio de nombres. Esta tabla se usa para almacenar entidades de la colección de XML.|  
|**sys.syscolpars**|Existe en todas las bases de datos. Contiene una fila para cada columna en una tabla, vista o función con valores de tabla. También contiene las filas para cada parámetro de un procedimiento o función.|  
|**sys.systypedsubobjs**|Existe en todas las bases de datos. Contiene una fila para cada subentidad escrita. Solo se incluyen en esta categoría los parámetros de la función de partición.|  
|**sys.sysidxstats**|Existe en todas las bases de datos. Contiene una fila para cada índice o estadísticas para tablas y vistas indizadas<br /><br /> Nota: cada índice (excepto el montón) está asociado a una estadística que tiene el mismo nombre que el índice.|  
|**sys.sysiscols**|Existe en todas las bases de datos. Contiene una fila para cada índice persistente y para columna de estadísticas.|  
|**sys.sysscalartypes**|Existe en todas las bases de datos. Contiene una fila por cada tipo de sistema o cada tipo definido por el usuario.|  
|**sys.sysdbreg**|Solo existe en la base de datos **maestra** . Contiene una fila por cada base de datos registrada.|  
|**sys.sysxsrvs**|Solo existe en la base de datos **maestra** . Contiene una fila para cada servidor local, vinculado o remoto.|  
|**sys.sysrmtlgns**|Esta tabla base del sistema solo existe en la base de datos **maestra** . Contiene una fila para cada asignación de inicio de sesión remota. Se utiliza para asignar inicios de sesión entrantes originados en el servidor correspondiente para un inicio de sesión local real.|  
|**sys.syslnklgns**|Solo existe en la base de datos **maestra** . Contiene una fila para cada asignación de inicio de sesión vinculada. Las llamadas a procedimiento remoto y las consultas distribuidas que proceden de un servidor local fuera de un servidor vinculado correspondiente utilizan las asignaciones de inicio de sesión vinculadas.|  
|**sys.sysxlgns**|Solo existe en la base de datos **maestra** . Contiene una fila para cada entidad de seguridad de servidor.|  
|**sys.sysdbfiles**|Existe en todas las bases de datos. Si el **DBID** de la columna es cero, la fila representa un archivo que pertenece a esta base de datos. En la base de datos **maestra** , la columna **DBID** puede ser distinto de cero. Cuando eso ocurra, la fila representará un archivo maestro.|  
|**sys.sysusermsg**|Solo existe en la base de datos **maestra** . Cada fila representa un mensaje de error definido por el usuario.|  
|**sys.sysprivs**|Existe en todas las bases de datos. Contiene una fila para cada permiso de base de datos o de servidor.<br /><br /> Nota: los permisos de nivel de servidor se almacenan en la base de datos **maestra** .|  
|**sys.sysowners**|Existe en todas las bases de datos. Cada fila representa una entidad de base de datos.|  
|**sys.sysobjkeycrypts**|Existe en todas las bases de datos. Contiene una fila para cada clave simétrica, cifrado o propiedad criptográfica asociada a un objeto.|  
|**sys.syscerts**|Existe en todas las bases de datos. Contiene una fila para cada certificado en una base de datos.|  
|**sys.sysasymkeys**|Existe en todas las bases de datos. Cada fila representa una clave asimétrica.|  
|**sys.ftinds**|Existe en todas las bases de datos. Contiene una fila para cada índice de texto completo de la base de datos.|  
|**sys.sysxprops**|Existe en todas las bases de datos. Contiene una fila para cada propiedad extendida.|  
|**sys.sysallocunits**|Existe en todas las bases de datos. Contiene una fila para cada unidad de asignación de almacenamiento.|  
|**sys.sysrowsets**|Existe en todas las bases de datos. Contiene una fila para cada conjunto de filas de particiones para un índice o montón.|  
|**sys.sysrowsetrefs**|Existe en todas las bases de datos. Contiene una fila para cada índice de la referencia del conjunto de filas.|  
|**sys.syslogshippers**|Solo existe en la base de datos **maestra** . Contiene una fila para cada testigo de creación de reflejo de la base de datos.|  
|**sys.sysremsvcbinds**|Existe en todas las bases de datos. Contiene una fila para cada enlace de servicio remoto.|  
|**sys.sysconvgroup**|Existe en todas las bases de datos. Contiene una fila para cada instancia de servicio de Service Broker.|  
|**sys.sysxmitqueue**|Existe en todas las bases de datos. Contiene una fila por cada cola de transmisión de Service Broker.|  
|**sys.sysdesend**|Existe en todas las bases de datos. Contiene una fila para cada extremo de envío de una conversación de Service Broker.|  
|**sys.sysdercv**|Existe en todas las bases de datos. Contiene una fila para cada extremo de recepción de una conversación de Service Broker.|  
|**sys.sysendpts**|Solo existe en la base de datos **maestra** . Contiene una fila para cada extremo creado en el servidor.|  
|**sys.syswebmethods**|Solo existe en la base de datos **maestra** . Contiene una fila para cada método SOAP definido en un extremo HTTP habilitado por SOAP que se crea en el servidor.|  
|**sys.sysqnames**|Existe en todas las bases de datos. Contiene una fila para cada espacio de nombres o nombre completo de un token del identificador de 4 bytes.|  
|**sys.sysxmlcomponent**|Existe en todas las bases de datos. Cada fila representa un componente de esquema XML.|  
|**sys.sysxmlfacet**|Existe en todas las bases de datos. Contiene una fila para cada aspecto de XML (restricción) de definición de tipo de XML.|  
|**sys.sysxmlplacement**|Existe en todas las bases de datos. Contiene una fila para cada ubicación XML de componentes XML.|  
|**sys.syssingleobjrefs**|Existe en todas las bases de datos. Contiene una fila para cada referencia general N a 1.|  
|**sys.sysmultiobjrefs**|Existe en todas las bases de datos. Contiene una fila para cada referencia general N a N.|  
|**sys.sysobjvalues**|Existe en todas las bases de datos. Contiene una fila para cada propiedad de valor general de una entidad.|  
|**sys.sysguidrefs**|Existe en todas las bases de datos. Contiene una fila para cada referencia del identificador clasificado por GUID.|  
  
## <a name="updating-system-base-tables"></a>Actualizar tablas base del sistema    
Puede ver los datos de las tablas del sistema mediante las vistas de catálogo del sistema. Para actualizar los metadatos de una tabla base del sistema, utilice la interfaz TSQL adecuada (por ejemplo, instrucciones DDL). No se pueden actualizar manualmente las tablas del sistema. SQL Server notifica los siguientes mensajes al realizar actualizaciones directas en las tablas del sistema.

### <a name="a-system-table-is-manually-updated"></a>Una tabla del sistema se actualiza manualmente
Mensaje 17659: Advertencia: Se ha actualizado la tabla del sistema con id. <id> directamente en la base de datos con id. <id> y es posible que no se haya mantenido la coherencia de la caché. Debe reiniciar SQL Server.

### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Inicio de una base de datos con una tabla del sistema que se ha actualizado manualmente
MSG 3859: ADVERTENCIA: el catálogo del sistema se actualizó directamente en la base de datos con el ID. 17, más recientemente en date_time.

### <a name="executing-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Ejecutar el comando DBCC_CHECKDB después de actualizar manualmente una tabla del sistema
MSG 3859: ADVERTENCIA: el catálogo del sistema se actualizó directamente en la base de datos con el ID. 17, más recientemente en date_time.

Si realiza actualizaciones manuales en una tabla del sistema y encuentra un problema, es posible que se le pida que realice una restauración a partir de una copia de seguridad o que copie los datos de la base de datos afectada en una nueva base de datos. Más información sobre [los mensajes de error de acción del usuario](../errors-events/mssqlserver-8992-database-engine-error.md#user-action).