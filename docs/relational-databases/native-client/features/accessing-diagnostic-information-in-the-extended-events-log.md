---
description: Obtener acceso a la información de diagnóstico de SQL Server Native Client en el registro de eventos extendidos
title: Información de diagnóstico en el registro de eventos extendidos
ms.date: 03/14/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: native-client
ms.topic: reference
ms.assetid: aaa180c2-5e1a-4534-a125-507c647186ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dbc9398f964089b33d24b499fe25d9ec559189b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463406"
---
# <a name="accessing-sql-server-native-client-diagnostic-information-in-the-extended-events-log"></a>Obtener acceso a la información de diagnóstico de SQL Server Native Client en el registro de eventos extendidos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  A partir de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] , [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] el seguimiento de acceso a datos y de Native Client ([seguimiento de acceso a datos](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100))) se ha actualizado para facilitar la obtención de información de diagnóstico sobre los errores de conexión desde el búfer de anillo de conectividad y la información de rendimiento de la aplicación desde el registro de eventos extendidos.  
  
 Para obtener información sobre la lectura del registro de eventos extendidos, vea [Ver datos de sesión de evento](../../extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).  
  
> [!NOTE]  
>  Esta función está dirigida únicamente a la solución de problemas y al diagnóstico, además es posible que no sea adecuada para fines de auditoría o seguridad.  
  
## <a name="remarks"></a>Observaciones  
 Para las operaciones de conexión, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client enviará un identificador de conexión de cliente. Si se produce un error en la conexión, puede tener acceso al búfer de anillo de conectividad ([Solución de problemas de conectividad en SQL Server 2008 con el búfer de anillo de conectividad](https://techcommunity.microsoft.com/t5/sql-server/connectivity-troubleshooting-in-sql-server-2008-with-the/ba-p/383393)) y buscar el campo **ClientConnectionID** para obtener información de diagnóstico sobre el error de conexión. Los identificadores de conexión del cliente se registran en el búfer de anillo únicamente si se produce un error. (Si se produce un error en una conexión antes de enviar el paquete de inicio de sesión previo, no se generará un identificador de conexión del cliente). El identificador de conexión del cliente es un GUID de 16 bytes. También puede buscar el identificador de conexión del cliente en el destino de salida de eventos extendidos si se agrega la acción **client_connection_id** a los eventos de una sesión de eventos extendidos. Puede habilitar el seguimiento de acceso a datos, volver a ejecutar el comando de conexión y observar el campo **ClientConnectionID** en el seguimiento de acceso a datos de una operación que no se ha realizado correctamente si necesita asistencia adicional de diagnóstico.  
  
 Si usa ODBC en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client y una conexión se realiza correctamente, puede obtener el identificador de conexión de cliente mediante el atributo **SQL_COPT_SS_CLIENT_CONNECTION_ID** con [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client también envía un identificador de actividad específico para cada subproceso. El identificador de actividad se captura en las sesiones de eventos extendidos si las sesiones se inician con la opción TRACK_CAUSAILITY habilitada. En el caso de problemas de rendimiento con una conexión activa, puede obtener el identificador de actividad del seguimiento de acceso a datos del cliente (campo **ActivityID**) y, después, buscar el identificador de actividad en la salida de eventos extendidos. El identificador de actividad en los eventos extendidos es un GUID de 16 bytes (no es el mismo que el GUID para el identificador de conexión del cliente) anexado a un número de secuencia de cuatro bytes. El número de secuencia representa el orden de una solicitud en un subproceso e indica el orden relativo del lote, así como las instrucciones RPC para el subproceso. **ActivityID** se envía opcionalmente para las instrucciones por lotes de SQL y las solicitudes RPC cuando el seguimiento de acceso a datos se habilita y el bit décimo octavo de la palabra de configuración del seguimiento de acceso a datos se establece en ON.  
  
 A continuación, se aporta un ejemplo que utiliza [!INCLUDE[tsql](../../../includes/tsql-md.md)] para iniciar una sesión de eventos extendidos que se va a almacenar en un búfer de anillo y que registrará el identificador de actividad que se envía desde un cliente en operaciones de RPC y por lotes.  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
  
```  
  
## <a name="control-file"></a>Archivo de control  
 En [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], el contenido del archivo de control de SQL Server Native Client (ctrl.guid.snac11) es:  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x00000000  0   MSDADIAG.ETW  
{2DA81B52-908E-7DB6-EF81-76856BB47C4F}  0xFFFFFFFF  0   SQLNCLI11.1  
```  
  
## <a name="mof-file"></a>Archivo MOF  
 En [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], el contenido del archivo MOF de SQL Server Native Client es:  
  
```  
#pragma classflags("forceupdate")  
#pragma namespace ("\\\\.\\Root\\WMI")  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSDADIAG.ETW  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F3-3CC6-0B9C-6651-9649CCE5C752}"),  
 DisplayName("msdadiag"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace : Bid2Etw_MSDADIAG_ETW  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextA : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextW : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  SQLNCLI11.1  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B52-908E-7DB6-EF81-76856BB47C4F}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B53-908E-7DB6-EF81-76856BB47C4F}"),  
 DisplayName("SQLNCLI11.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace : Bid2Etw_SQLNCLI11_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextA : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextW : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
```  
  
## <a name="see-also"></a>Consulte también  
 [Controlar errores y mensajes](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
