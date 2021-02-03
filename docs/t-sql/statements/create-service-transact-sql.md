---
description: CREATE SERVICE (Transact-SQL)
title: CREATE SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SERVICE_TSQL
- SERVICE
- CREATE SERVICE
- SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- services [Service Broker], creating
- CREATE SERVICE statement
- contracts [Service Broker], service creation
ms.assetid: fb804fa2-48eb-4878-a12f-4e0d5f4bc9e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3d7c8981e7b8d79f9819c6df4327037f0d2e930b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179154"
---
# <a name="create-service-transact-sql"></a>CREATE SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuevo servicio. Un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un nombre para una tarea o un conjunto de tareas específicos. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utiliza el nombre del servicio para enrutar mensajes, entregar mensajes a la cola correcta en una base de datos y aplicar el contrato para una conversación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SERVICE service_name  
   [ AUTHORIZATION owner_name ]  
   ON QUEUE [ schema_name. ]queue_name  
   [ ( contract_name | [DEFAULT][ ,...n ] ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *service_name*  
 Es el nombre del servicio que se va a crear. El servicio nuevo se crea en la base de datos actual, la cual pertenece a la entidad de seguridad especificada en la cláusula AUTHORIZATION. No se pueden especificar nombres de servidor, base de datos o esquema. *service_name* debe ser un **sysname** válido.  
  
> [!NOTE]  
> No cree un servicio que use la palabra clave ANY para *service_name*. Cuando especifica `ANY` para un nombre de servicio en `CREATE BROKER PRIORITY`, la prioridad se considera para todos los servicios. No se limita a un servicio cuyo nombre sea ANY.  
  
 AUTHORIZATION *owner_name*  
 Establece el propietario del servicio en el usuario o el rol de base de datos especificado. Cuando el usuario actual es **dbo** o **sa**, *owner_name* puede ser el nombre de cualquier usuario o rol válidos. En caso contrario, *owner_name* debe ser el nombre del usuario actual, el nombre de un usuario para el que el usuario actual tiene permisos IMPERSONATE o el nombre de un rol al que pertenece el usuario actual.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Especifica la cola que recibe mensajes para el servicio. La cola debe existir en la misma base de datos que el servicio. Si no se proporciona *schema_name*, el valor es el esquema predeterminado del usuario que ejecuta la instrucción.  
  
 *contract_name*  
 Especifica un contrato para el que este servicio puede ser un destino. Los programas de servicio inician conversaciones con este servicio utilizando los contratos especificados. Si no se especifica ningún contrato, el servicio solo puede iniciar conversaciones.  
  
 **[** DEFAULT **]**  
 Especifica que el servicio puede ser un destino para conversaciones que sigan el contrato DEFAULT. En el contexto de esta cláusula, DEFAULT no es una palabra clave y debe delimitarse como un identificador. El contrato DEFAULT permite que los dos extremos de la conversación envíen mensajes del tipo DEFAULT. El tipo de mensajes DEFAULT utiliza la validación NONE.  
  
## <a name="remarks"></a>Comentarios  
 Un servicio muestra la funcionalidad proporcionada por los contratos a los que está asociado, de modo que puedan usarlos otros servicios. Este servicio es destino de los contratos que se especifican en la instrucción `CREATE SERVICE`. Un servicio solo puede ser destino de conversaciones que utilizan los contratos especificados por el servicio. Un servicio que no especifica ningún contrato no muestra ninguna funcionalidad para otros servicios.  
  
 Las conversaciones iniciadas desde este servicio pueden utilizar cualquier contrato. Puede crear un servicio sin especificar contratos solo si el servicio inicia conversaciones.  
  
 Cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] acepta una nueva conversación de un servicio remoto, el nombre del servicio de destino determina la cola en la que el agente colocará los mensajes de la conversación.  
  
## <a name="permissions"></a>Permisos  
 Tienen permiso para crear un servicio los miembros de los roles fijos de base de datos `db_ddladmin` o `db_owner` o el rol fijo de servidor `sysadmin`. El usuario que ejecuta la instrucción `CREATE SERVICE` debe tener permiso `REFERENCES` en la cola y en todos los contratos especificados.  
  
 De forma predeterminada, el permiso `REFERENCES` para un servicio corresponde al propietario del servicio, a los miembros de los roles fijos de base de datos `db_ddladmin` o `db_owner` o a los miembros del rol fijo de servidor `sysadmin`. De forma predeterminada, los permisos `SEND` para un servicio corresponden al propietario del servicio, a los miembros del rol fijo de base de datos `db_owner` o a los miembros del rol fijo de servidor `sysadmin`.  
  
 Un servicio no puede ser un objeto temporal. Los servicios que empiezan por **#** están permitidos, pero son objetos permanentes.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-service-with-one-contract"></a>A. Crear un servicio con un contrato  
 En el ejemplo siguiente se crea el servicio `//Adventure-Works.com/Expenses` en la cola `ExpenseQueue` del esquema `dbo`. Los diálogos destinados a este servicio deben seguir el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE [dbo].[ExpenseQueue]  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="b-creating-a-service-with-multiple-contracts"></a>B. Crear un servicio con varios contratos  
 En el ejemplo siguiente se crea el servicio `//Adventure-Works.com/Expenses` en la cola `ExpenseQueue`. Los diálogos destinados a este servicio deben seguir el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission` o el contrato `//Adventure-Works.com/Expenses/ExpenseProcessing`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission],  
     [//Adventure-Works.com/Expenses/ExpenseProcessing]) ;  
```  
  
### <a name="c-creating-a-service-with-no-contracts"></a>C. Crear un servicio sin contratos  
 En el siguiente ejemplo se crea la cola del servicio `//Adventure-Works.com/Expenses on the ExpenseQueue`. Este servicio no tiene información de contrato. Por lo tanto, el servicio solo puede ser el iniciador de un diálogo.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
