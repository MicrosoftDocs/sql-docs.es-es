---
description: CREATE DIAGNOSTICS SESSION (Transact-SQL)
title: CREATE DIAGNOSTICS SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 662d019e-f217-49df-9e2f-b5662fa0342d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 14a913922b62500d527928ca37e5b10f50acb0da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208687"
---
# <a name="create-diagnostics-session-transact-sql"></a>CREATE DIAGNOSTICS SESSION (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Las sesiones de diagnóstico le permiten guardar información de diagnóstico detallada y definida por el usuario sobre el rendimiento del sistema o de la consulta.  
  
 Las sesiones de diagnóstico se suelen usar para depurar el rendimiento de una consulta específica o para supervisar el comportamiento de un componente específico del dispositivo durante el funcionamiento del dispositivo.  
  
> [!NOTE]  
>  Debe estar familiarizado con XML para usar las sesiones de diagnóstico.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Creating a new diagnostics session:  
CREATE DIAGNOSTICS SESSION diagnostics_name AS N'{<session_xml>}';  
  
<session_xml>::  
<Session>  
   [ <MaxItemCount>max_item_count_num</MaxItemCount> ]  
   <Filter>  
      { \<Event Name="event_name"/>  
         [ <Where>\<filter_property_name Name="value" ComparisonType="comp_type"/></Where> ] [ ,...n ]  
      } [ ,...n ]  
   </Filter> ]   
   <Capture>  
      \<Property Name="property_name"/> [ ,...n ]  
   </Capture>  
<Session>  
  
-- Retrieving results for a diagnostics session:  
SELECT * FROM master.sysdiag.diagnostics_name ;  
  
-- Removing results for a diagnostics session:  
DROP DIAGNOSTICS SESSION diagnostics_name ;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *diagnostics_name*  
 Nombre de la sesión de diagnóstico. Los nombres de sesión de diagnóstico pueden incluir únicamente los caracteres a-z, A-Z y 0-9. Además, los nombres de sesión de diagnóstico deben empezar por un carácter. *diagnostics_name* tiene un límite de 127 caracteres.  
  
 *max_item_count_num*  
 Número de eventos que van a conservarse en una vista. Por ejemplo, si se especifica 100, se conservarán en la sesión de diagnóstico los 100 eventos más recientes que cumplan los criterios del filtro. Si se detectan menos de 100 eventos coincidentes, la sesión de diagnóstico contendrá menos de 100 eventos. *max_item_count_num* debe ser como mínimo 100 y menor o igual que 100 000.  
  
 *event_name*  
 Define los eventos reales que se recopilan en la sesión de diagnóstico.  *event_name* es uno de los eventos enumerados en [sys.pdw_diag_events](../../relational-databases/system-catalog-views/sys-pdw-diag-events-transact-sql.md), donde `sys.pdw_diag_events.is_enabled='True'`.  
  
 *filter_property_name*  
 Nombre de la propiedad en la que se van a restringir los resultados. Por ejemplo, si quiere limitar en función del identificador de sesión, *filter_property_name* debe ser *SessionId*. Vea *property_name* más abajo para obtener una lista de posibles valores de *filter_property_name*.  
  
 *value*  
 Valor que se va a evaluar con respecto a *filter_property_name*. El tipo de valor debe coincidir con el tipo de propiedad. Por ejemplo, si el tipo de propiedad es decimal, el tipo de *value* debe ser decimal.  
  
 *comp_type*  
 Tipo de comparación. Los posibles valores son Equals, EqualsOrGreaterThan, EqualsOrLessThan, GreaterThan, LessThan, NotEquals, Contains y RegEx.  
  
 *property_name*  
 Propiedad relacionada con el evento.  Los nombres de propiedad pueden formar parte de la etiqueta de captura o usarse como parte de los criterios de filtrado.  
  
|Nombre de propiedad|Descripción|  
|-------------------|-----------------|  
|UserName|Nombre de usuario (inicio de sesión).|  
|SessionId|Identificador de sesión.|  
|QueryId|Identificador de consulta.|  
|CommandType|Tipo de comando.|  
|CommandText|Texto dentro de un comando procesado.|  
|OperationType|Tipo de operación del evento.|  
|Duration|Duración del evento.|  
|SPID|Identificador de proceso del servicio.|  
  
## <a name="remarks"></a>Comentarios  
 Se permite para cada usuario un máximo de 10 sesiones de diagnóstico simultáneas. Vea [sys.pdw_diag_sessions](../../relational-databases/system-catalog-views/sys-pdw-diag-sessions-transact-sql.md) para obtener una lista de las sesiones actuales y anule las que ya no necesite mediante `DROP DIAGNOSTICS SESSION`.  
  
 Las sesiones de diagnóstico seguirán recopilando metadatos hasta que se anulen.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **ALTER SERVER STATE**.  
  
## <a name="locking"></a>Bloqueo  
 Aplica un bloqueo compartido en la tabla de sesiones de diagnóstico.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-diagnostics-session"></a>A. Creación de una sesión de diagnóstico  
 En este ejemplo se crea una sesión de diagnóstico para registrar las métricas del rendimiento del motor de base de datos. En el ejemplo se crea una sesión de diagnóstico que escucha los eventos de ejecución y finalización de la consulta del motor y un evento DMS de bloqueo. Lo que se devuelve es el texto del comando, el nombre del equipo, el identificador de la solicitud (identificador de consulta) y la sesión en la que se ha creado el evento.  
  
```sql  
CREATE DIAGNOSTICS SESSION MYDIAGSESSION AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:EngineQueryRunningEvent" />  
      <Event Name="DmsCoreInstrumentation:DmsBlockingQueueEnqueueBeginEvent" />  
      <Where>  
         <SessionId Value="381" ComparisonType="NotEquals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Query.CommandText" />  
      <Property Name="MachineName" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Alias" />  
      <Property Name="Duration" />  
      <Property Name="Session.SessionId" />  
   </Capture>  
</Session>';  
```  
  
 Después de crear la sesión de diagnóstico, ejecute una consulta.  
  
```sql  
SELECT COUNT(EmployeeKey) FROM AdventureWorksPDW2012..FactSalesQuota;  
```  
  
 Después, vea los resultados de la sesión de diagnóstico. Para ello, selecciónela en el esquema sysdiag.  
  
```sql  
SELECT * FROM master.sysdiag.MYDIAGSESSION;  
```  
  
 Observe que el esquema sysdiag contiene una vista denominada con el nombre de la sesión de diagnóstico.  
  
 Para ver únicamente la actividad de la conexión, agregue la propiedad `Session.SPID` y agregue `WHERE [Session.SPID] = @@spid;` a la consulta.  
  
 Cuando haya terminado con la sesión de diagnóstico, anúlela con el comando **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION MYDIAGSESSION;  
```  
  
### <a name="b-alternative-diagnostic-session"></a>B. Sesión de diagnóstico alternativo  
 Un segundo ejemplo con propiedades ligeramente diferentes.  
  
```sql  
-- Determine the session_id of your current session  
SELECT TOP 1 session_id();  
-- Replace \<*session_number*> in the code below with the numbers in your session_id  
CREATE DIAGNOSTICS SESSION PdwOptimizationDiagnostics AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:MemoGenerationBeginEvent" />  
      <Event Name="EngineInstrumentation:MemoGenerationEndEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationBeginEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationEndEvent" />  
      <Event Name="DSQLInstrumentation:BuildRelOpContextTreeBeginEvent" />  
      <Event Name="DSQLInstrumentation:PostPlanGenModifiersEndEvent" />  
      <Where>  
         <SessionId Value="\<*session_number*>" ComparisonType="Equals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Session.SessionId" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Query.CommandText" />  
      <Property Name="Name" />  
      <Property Name="DateTimePublished" />  
      <Property Name="DateTimePublished.Ticks" />  
  </Capture>  
</Session>';  
```  
  
 Ejecute una consulta, por ejemplo:  
  
```sql  
USE ssawPDW;  
GO  
SELECT * FROM dbo.FactFinance;  
```  
  
 La consulta siguiente devuelve el intervalo de autorización:  
  
```sql  
SELECT *   
FROM master.sysdiag.PdwOptimizationDiagnostics   
ORDER BY DateTimePublished;  
```  
  
 Cuando haya terminado con la sesión de diagnóstico, anúlela con el comando **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION PdwOptimizationDiagnostics;  
```  
  
  
