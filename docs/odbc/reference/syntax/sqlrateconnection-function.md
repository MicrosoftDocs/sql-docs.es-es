---
description: Función SQLRateConnection
title: Función SQLRateConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLRateConnection function [ODBC]
ms.assetid: e8da2ffb-d6ef-4ca7-824f-57afd29585d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40aec3459e43d2684bd80f0ced55171314551d92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192547"
---
# <a name="sqlrateconnection-function"></a>Función SQLRateConnection
**Conformidad**  
 Versión introducida: ODBC 3,81 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLRateConnection** determina si un controlador puede reutilizar una conexión existente en el grupo de conexiones.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp
  
SQLRETURN  SQLRateConnection(  
                SQLHDBC_INFO_TOKEN   hRequest,  
                SQLHDBC              hCandidateConnection,  
                BOOL                 fRequiredTransactionEnlistment,  
                TRANSID              transId,  
                DWORD *              pRating );  
```  
  
## <a name="arguments"></a>Argumentos  
 *hRequest*  
 Entradas Identificador de token que representa la nueva solicitud de conexión de la aplicación.  
  
 *hCandidateConnection*  
 Entradas La conexión existente en el grupo de conexiones. La conexión debe estar en un estado abierto.  
  
 *fRequiredTransactionEnlistment*  
 Entradas Si es TRUE, la reutilización de la *hCandidateConnection* de la conexión existente para la nueva solicitud de conexión (*hRequest*) requiere una inscripción adicional.  
  
 *transId*  
 Entradas Si *fRequiredTransactionEnlistment* es true, *transId* representa la transacción DTC a la que se dará de alta la solicitud. Si *fRequiredTransactionEnlistment* es false, *transId* se omitirá.  
  
 *pRating*  
 Genere clasificación de reutilización de *hCandidateConnection* para *hRequest*. Esta clasificación estará entre 0 y 100 (inclusive).  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 El administrador de controladores no procesará la información de diagnóstico devuelta por esta función.  
  
## <a name="remarks"></a>Observaciones  
 **SQLRateConnection** genera una puntuación entre 0 y 100 (inclusive) que indica el grado de coincidencia de una conexión existente con la solicitud.  
  
|Puntuación|Significado (cuando se devuelve SQL_SUCCESS)|  
|-----------|-----------------------------------------------|  
|0|*hCandidateConnection* no se debe reutilizar para *hRequest*.|  
|Cualquier valor entre 1 y 98 (inclusivo)|Cuanto mayor sea la puntuación, más cerca de *hCandidateConnection* coincidirá con *hRequest*.|  
|99|Solo hay discrepancias en los atributos insignificantes.  El administrador de controladores debe detener el bucle de clasificación.|  
|100|Coincidencia perfecta.  El administrador de controladores debe detener el bucle de clasificación.|  
|Cualquier otro valor mayor que 100|*hCandidateConnection* está marcado como Dead y no se volverá a usar incluso en una solicitud de conexión futura.|  
  
 El administrador de controladores marcará una conexión como muerta si el código de retorno es distinto de SQL_SUCCESS (incluido SQL_SUCCESS_WITH_INFO) o si la clasificación es mayor que 100. La conexión inactiva no se reutilizará (ni siquiera en solicitudes de conexión futuras) y, finalmente, se agotará el tiempo de espera después de que pase CPTimeout. El administrador de controladores seguirá encontrando otra conexión del grupo para valorar.  
  
 Si el administrador de controladores reutiliza una conexión cuya puntuación es estrictamente menor que 100 (incluido 99), el administrador de controladores llamará a SQLSetConnectAttr (SQL_ATTR_DBC_INFO_TOKEN) para restablecer la conexión al estado solicitado por la aplicación. El controlador no debe restablecer la conexión en esta llamada de función.  
  
 Si *fRequiredTransactionEnlistment* es true, la reutilización de *hCandidateConnection* necesita una inscripción adicional (*transId* ! = null) o una baja (*transId* = = null). Indica el costo de reutilizar una conexión y si el controlador debe dar de alta o baja la conexión si va a volver a usar la conexión. Si *fRequireTransactionEnlistment* es false, el controlador debe omitir el valor de *transId*.  
  
 El administrador de controladores garantiza que el identificador HENV primario de *hRequest* y *hCandidateConnection* es el mismo. El administrador de controladores garantiza que el identificador de grupo asociado con *hRequest* y *hCandidateConnection* es el mismo.  
  
 Las aplicaciones no deben llamar directamente a esta función. Un controlador ODBC que admita la agrupación de conexiones compatible con controladores debe implementar esta función.  
  
 Incluya sqlspi. h para el desarrollo del controlador ODBC.  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Desarrollar el conocimiento de la agrupación de conexiones en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
