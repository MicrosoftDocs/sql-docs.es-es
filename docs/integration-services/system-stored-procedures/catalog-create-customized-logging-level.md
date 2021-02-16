---
description: catalog.create_customized_logging_level
title: catalog.create_customized_logging_level | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 20b3ba0a-126f-49bf-b70f-61b2a0fcb750
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5fbedb41369300353d7b0f8ce64da355592b6eaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346844"
---
# <a name="catalogcreate_customized_logging_level"></a>catalog.create_customized_logging_level 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Crea un nuevo nivel de registro personalizado. Para obtener más información sobre los niveles de registro personalizados, consulte [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.create_customized_logging_level [ @level_name = ] level_name  
    , [ @level_description = ] level_description  
    , [ @profile_value = ] profile_value  
    , [ @events_value = ] events_value  
    , [ @level_id = ] level_id OUT   
```  
  
## <a name="arguments"></a>Argumentos  
 [ @level_name = ] *level_name*  
 Nombre del nuevo nivel de registro personalizado.  
  
 *level_name* es **nvarchar(128)**.  
  
 [ @level_description = ] *level_description*  
 Descripción del nuevo nivel de registro personalizado.  
  
 El parámetro *level_description* es de tipo **nvarchar(max)**.  
  
 [ @profile_value =] *profile_value*  
 Estadísticas que quiere que registre el nuevo nivel de registro personalizado.  
  
 Los valores válidos de las estadísticas incluyen los siguientes. Estos valores se corresponden con los valores de la pestaña **Estadísticas** del cuadro de diálogo **Administración del nivel de registro personalizado**.  
  
-   Ejecución = 0  
  
-   Volumen = 1  
  
-   Rendimiento = 2    
  
 El parámetro *property_value* es de tipo **bigint**.  
  
 [ @events_value = ] *events_value*  
 Eventos que quiere que registre el nuevo nivel de registro personalizado.  
  
 Los valores válidos de los eventos incluyen los siguientes. Estos valores se corresponden con los valores de la pestaña **Eventos** del cuadro de diálogo **Administración del nivel de registro personalizado**.  
  
|Eventos sin contexto del evento|Eventos con contexto del evento|  
|----------------------------------|-------------------------------|  
|OnVariableValueChanged = 0<br /><br /> OnExecutionStatusChanged = 1<br /><br /> OnPreExecute = 2<br /><br /> OnPostExecute = 3<br /><br /> OnPreValidate = 4<br /><br /> OnPostValidate = 5<br /><br /> OnWarning = 6<br /><br /> OnInformation = 7<br /><br /> OnError = 8<br /><br /> OnTaskFailed = 9<br /><br /> OnProgress = 10<br /><br /> OnQueryCancel = 11<br /><br /> OnBreakpointHit = 12<br /><br /> OnCustomEvent = 13<br /><br /> Diagnostic = 14<br /><br /> DiagnosticEx = 15<br /><br /> NonDiagnostic = 16|OnVariableValueChanged_IncludeContext = 32<br /><br /> OnExecutionStatusChanged_IncludeContext = 33<br /><br /> OnPreExecute_IncludeContext = 34<br /><br /> OnPostExecute_IncludeContext = 35<br /><br /> OnPreValidate_IncludeContext = 36<br /><br /> OnPostValidate_IncludeContext = 37<br /><br /> OnWarning_IncludeContext = 38<br /><br /> OnInformation_IncludeContext = 39<br /><br /> OnError_IncludeContext = 40<br /><br /> OnTaskFailed_IncludeContext = 41<br /><br /> OnProgress_IncludeContext = 42<br /><br /> OnQueryCancel_IncludeContext= 43<br /><br /> OnBreakpointHit_IncludeContext = 44<br /><br /> OnCustomEvent_IncludeContext = 45<br /><br /> Diagnostic_IncludeContext = 46<br /><br /> DiagnosticEx_IncludeContext = 47<br /><br /> NonDiagnostic_IncludeContext = 48|  
  
 El parámetro *events_value* es de tipo **bigint**.  
  
 [ @level_id =] *level_id* OUT  
 Identificador del nuevo nivel de registro personalizado.  
  
 El parámetro *level_id* es de tipo **bigint**.  
  
## <a name="remarks"></a>Observaciones  
 Para combinar varios valores en Transact-SQL para el argumento *profile_value* o *events_value*, siga este ejemplo. Para capturar los eventos OnError (8) y DiagnosticEx (15), la fórmula para calcular *events_value* es `2^8 + 2^15 = 33024`.  
  
## <a name="return-codes"></a>Códigos de retorno  
 0 (correcto)  
  
 Cuando se produce un error en el procedimiento almacenado, se genera un error.  
  
## <a name="result-set"></a>Tipo de cursor  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Pertenencia al rol de base de datos **ssis_admin**  
  
-   Pertenencia al rol de servidor **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la lista siguiente se describen las condiciones que hacen que el procedimiento almacenado genere un error.  
  
-   El usuario no tiene los permisos requeridos.  
  
  
