---
description: CREATE BROKER PRIORITY (Transact-SQL)
title: CREATE BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE BROKER PRIORITY
- PRIORITY_TSQL
- CREATE_BROKER_PRIORITY_TSQL
- PRIORITY
- CREATE BROKER
- BROKER_TSQL
- BROKER
- CREATE_BROKER_TSQL
- BROKER PRIORITY
- BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE BROKER PRIORITY statement
ms.assetid: e0bbebfa-b7c3-4825-8169-7281f7e6de98
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 64f5c5970e45304af41bea1c5c329c55f5b6ad70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204975"
---
# <a name="create-broker-priority-transact-sql"></a>CREATE BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Define un nivel de prioridad y el conjunto de criterios para determinar a qué conversaciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se debe asignar el nivel de prioridad. El nivel de prioridad se asignará a cualquier extremo de conversación que use la misma combinación de contratos y servicios especificados en la prioridad de conversación. El valor de las prioridades va de 1 (bajo) a 10 (alto). El valor predeterminado es 5.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
[ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = {PriorityValue | DEFAULT } ]  
       )  
]  
[;]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *ConversationPriorityName*  
 Especifica el nombre para esta prioridad de conversación. El nombre debe ser único en la base de datos actual y debe cumplir las reglas de los  [identificadores](../../relational-databases/databases/database-identifiers.md) de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 SET  
 Especifica los criterios para determinar si la prioridad de conversación se aplica a una conversación. Si se especifica, SET debe contener al menos un criterio: CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME o PRIORITY_LEVEL. Si no se especifica SET, se establecen los valores predeterminados para los tres criterios.  
  
 CONTRACT_NAM = {*ContractName* | **ANY**}  
 Especifica el nombre de un contrato que se va a usar como criterio para determinar si la prioridad de conversación se aplica a una conversación. *ContractName* es un identificador de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y debe especificar el nombre de un contrato en la base de datos actual.  
  
 *ContractName*  
 Especifica que la prioridad de conversación solo se puede aplicar a las conversaciones cuya instrucción BEGIN DIALOG que inició la conversación ha especificado ON CONTRACT *ContractName*.  
  
 ANY  
 Especifica que la prioridad de conversación se puede aplicar a cualquier conversación, independientemente de qué contrato use.  
  
 El valor predeterminado es ANY.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Especifica el nombre de un servicio que se va a usar como criterio para determinar si la prioridad de conversación se aplica a un extremo de una conversación.  
  
 *LocalServiceName* es un identificador de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Debe especificar el nombre de un servicio de la base de datos actual.  
  
 *LocalServiceName*  
 Especifica que la prioridad de conversación se puede aplicar a lo siguiente:  
  
-   Cualquier extremo iniciador de conversación cuyo nombre de servicio de iniciador coincida con *LocalServiceName*.  
  
-   Cualquier extremo de destino de conversación cuyo nombre de servicio de destino coincida con *LocalServiceName*.  
  
 ANY  
 -   Especifica que la prioridad de conversación se puede aplicar a cualquier extremo de conversación, independientemente del nombre del servicio local utilizado por el extremo.  
  
 El valor predeterminado es ANY.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Especifica el nombre de un servicio que se va a usar como criterio para determinar si la prioridad de conversación se aplica a un extremo de una conversación.  
  
 *RemoteServiceName* es un literal de tipo **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa una comparación byte a byte para buscar una coincidencia con la cadena *RemoteServiceName*. En la comparación se distinguen mayúsculas y minúsculas, y no se considera la intercalación actual. El servicio de destino puede estar en la instancia actual de [!INCLUDE[ssDE](../../includes/ssde-md.md)] o en una instancia remota de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 "*RemoteServiceName*"  
 Especifica que la prioridad de conversación se puede aplicar a lo siguiente:  
  
-   Cualquier extremo de iniciador de conversación cuyo nombre de servicio de destino asociado coincida con *RemoteServiceName*.  
  
-   Cualquier extremo de destino de conversación cuyo nombre de servicio de iniciador asociado coincida con *RemoteServiceName*.  
  
 ANY  
 Especifica que la prioridad de conversación puede aplicarse a cualquier extremo de conversación, independientemente del nombre del servicio remoto que esté asociado al extremo.  
  
 El valor predeterminado es ANY.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Especifica la prioridad que se asigna a cualquier extremo de conversación que utilice los contratos y servicios que se han especificado en la prioridad de conversación. *PriorityValue* debe ser un literal entero comprendido entre 1 (prioridad más baja) y 10 (prioridad más alta). El valor predeterminado es 5.  
  
## <a name="remarks"></a>Comentarios  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] asigna los niveles de prioridad a los extremos de conversación. Los niveles de prioridad controlan la prioridad de las operaciones asociadas al extremo. Cada conversación tiene dos extremos:  
  
-   El extremo iniciador de conversación asocia un lado de la conversación con el servicio iniciador y con la cola del iniciador. El extremo iniciador de la conversación se crea cuando se ejecuta la instrucción BEGIN DIALOG. Las operaciones que realiza el extremo iniciador de conversación son:  
  
    -   Envía desde el servicio iniciador.  
  
    -   Recibe desde la cola del iniciador.  
  
    -   Obtención del siguiente grupo de conversación desde la cola del iniciador.  
  
-   El extremo de destino de conversación asocia el otro lado de la conversación con el servicio y la cola de destino. El extremo de destino de conversación se crea cuando la conversación se utiliza para enviar un mensaje a la cola de destino. Las operaciones que realiza el extremo de destino de conversación son:  
  
    -   Recibe desde la cola de destino.  
  
    -   Envía desde el servicio de destino.  
  
    -   Obtiene el siguiente grupo de conversación desde la cola de destino.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] asigna niveles de prioridad de conversación cuando se crean los extremos de conversación. El extremo de conversación conserva el nivel de prioridad hasta que finaliza la conversación. Las prioridades nuevas o los cambios en las existentes no se aplican a las conversaciones existentes.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] asigna a un extremo de conversación el nivel de prioridad de la prioridad de conversación cuyos criterios de contrato y de servicios coinciden mejor con las propiedades del extremo. En la tabla siguiente se muestra la prioridad de la coincidencia:  
  
|Contrato de la operación|Servicio local de la operación|Servicio remoto de la operación|  
|------------------------|-----------------------------|------------------------------|  
|*ContractName*|*LocalServiceName*|*RemoteServiceName*|  
|*ContractName*|*LocalServiceName*|ANY|  
|*ContractName*|ANY|*RemoteServiceName*|  
|*ContractName*|ANY|ANY|  
|ANY|*LocalServiceName*|*RemoteServiceName*|  
|ANY|*LocalServiceName*|ANY|  
|ANY|ANY|*RemoteServiceName*|  
|ANY|ANY|ANY|  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] primero busca una prioridad cuyo contrato, servicio local y servicio remoto coincide con los que utiliza la operación. Si no lo encuentra, [!INCLUDE[ssSB](../../includes/sssb-md.md)] busca una prioridad para la que se especificó ANY como servicio remoto y cuyo contrato y servicio local coinciden con los que utiliza la operación. Esto continúa para todas las variaciones incluidas en la tabla de prioridad. Si no se encuentra ninguna coincidencia, se asigna a la operación la prioridad predeterminada, es decir, 5.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] asigna independientemente un nivel de prioridad a cada extremo de conversación. Para hacer que [!INCLUDE[ssSB](../../includes/sssb-md.md)] asigne niveles de prioridad tanto al extremo iniciador de conversación como al de destino, debe asegurarse de que ambos extremos estén cubiertos por prioridades de conversación. Si los extremos iniciador y de destino de la conversación están en bases de datos independientes, debe crear prioridades de conversación en cada base de datos. Normalmente se especifica el mismo nivel de prioridad para los dos extremos de una conversación, pero puede especificar niveles de prioridad diferentes.  
  
 Los niveles de prioridad siempre se aplican a las operaciones que reciben mensajes o identificadores de grupo de conversación procedentes de una cola. También se aplican cuando se transmiten mensajes desde una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] a otra.  
  
 Los niveles de prioridad no se utilizan cuando se transmiten mensajes:  
  
-   Desde una base de datos cuya opción de base de datos HONOR_BROKER_PRIORITY está establecida en OFF. Para obtener más información, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
-   Entre servicios de la misma instancia del motor de base de datos.  
  
-   Todas las operaciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)] de una base de datos tienen asignadas prioridades predeterminadas de 5 si no se ha creado ninguna prioridad de conversación en la base de datos.  
  
## <a name="permissions"></a>Permisos  
 El permiso para crear una prioridad de conversación recae de forma predeterminada en los miembros de los roles fijos de base de datos db_ddladmin o db_owner, y en el rol fijo de servidor sysadmin. Requiere el permiso ALTER en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-assigning-a-priority-level-to-both-directions-of-a-conversation"></a>A. Asignar un nivel de prioridad a las dos direcciones de una conversación.  
 Estas dos prioridades de conversación permiten asegurarse de que se asigna el nivel de prioridad 3 a todas las operaciones que utilizan `SimpleContract` entre `TargetService` e `InitiatorAService`.  
  
```sql  
CREATE BROKER PRIORITY InitiatorAToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = InitiatorServiceA,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 3);  
CREATE BROKER PRIORITY TargetToInitiatorAPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceA',  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-setting-the-priority-level-for-all-conversations-that-use-a-contract"></a>B. Establecer el nivel de prioridad para todas las conversaciones que utilizan un contrato  
 Asigna un nivel de prioridad `7` a todas las operaciones que utilizan un contrato denominado `SimpleContract`. Esto supone que no hay ninguna otra prioridad que especifica `SimpleContract` y un servicio local o un servicio remoto.  
  
```sql 
CREATE BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 7);  
```  
  
### <a name="c-setting-a-base-priority-level-for-a-database"></a>C. Establecer un nivel de prioridad base para una base de datos.  
 Define prioridades de conversación para dos servicios concretos y, a continuación, define una prioridad de conversación que coincidirá con todos los demás extremos de conversación. Esto no reemplaza la prioridad predeterminada, que siempre es 5, pero minimiza el número de elementos a los que se asigna el valor predeterminado.  
  
```sql 
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ClaimPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 9);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ApprovalPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/BasePriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="d-creating-three-priority-levels-for-a-target-service-by-using-services"></a>D. Crear tres niveles de prioridad para un servicio de destino utilizando servicios  
 Es compatible con un sistema que proporciona tres niveles de rendimiento: Gold (alto), Silver (medio) y Bronze (bajo). Hay un contrato, pero cada nivel tiene un servicio iniciador independiente. Todos los servicios iniciadores se comunican con un servicio de destino central.  
  
```sql  
CREATE BROKER PRIORITY GoldInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = GoldInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY GoldTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'GoldInitiatorService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = SilverInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY SilverTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'SilverInitiatorService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzeInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = BronzeInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 2);  
CREATE BROKER PRIORITY BronzeTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'BronzeInitiatorService',  
         PRIORITY_LEVEL = 2);  
```  
  
### <a name="e-creating-three-priority-levels-for-multiple-services-using-contracts"></a>E. Crear tres niveles de prioridad para varios servicios utilizando contratos  
 Es compatible con un sistema que proporciona tres niveles de rendimiento: Gold (alto), Silver (medio) y Bronze (bajo). Cada nivel tiene un contrato independiente. Estas prioridades se aplican a cualquier servicio al que hagan referencia las conversaciones que utilicen los contratos.  
  
```sql  
CREATE BROKER PRIORITY GoldPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = GoldContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SilverContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzePriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = BronzeContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 2);  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
