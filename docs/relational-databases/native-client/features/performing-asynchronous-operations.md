---
title: Realización de operaciones asincrónicas | Microsoft Docs
description: Permita que las aplicaciones realicen operaciones asincrónicas de base de datos con el proveedor de bases de datos de SQL Server Native Client antiguo.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- initialization [SQL Server Native Client]
- database connections [SQL Server Native Client]
- data access [SQL Server Native Client], asynchronous operations
- connections [SQL Server Native Client]
- asynchronous operations [SQL Server Native Client]
- rowsets [SQL Server], initializing
- SQLNCLI, asynchronous operations
- SQL Server Native Client, asynchronous operations
ms.assetid: 8fbd84b4-69cb-4708-9f0f-bbdf69029bcc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9ccf1741e211c2d465359162a5deb73636a40af7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97418648"
---
# <a name="performing-asynchronous-operations-in-sql-server-native-client"></a>Realización de operaciones asincrónicas en SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permite a las aplicaciones realizar operaciones de base de datos asincrónicas. El procesamiento asincrónico permite a los métodos devolver resultados inmediatamente sin bloquear el subproceso que hace la llamada. Esto hace posible gran parte de la potencia y la flexibilidad del subprocesamiento múltiple, sin que el programador tenga que crear explícitamente subprocesos o administrar la sincronización. Las aplicaciones solicitan el procesamiento asincrónico al inicializar una conexión a la base de datos, o al inicializar el resultado de la ejecución de un comando.  
  
## <a name="opening-and-closing-a-database-connection"></a>Abrir y cerrar una conexión a la base de datos  
 Cuando se usa el [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client, las aplicaciones diseñadas para inicializar de forma asincrónica un objeto de origen de datos pueden establecer el bit de DBPROPVAL_ASYNCH_INITIALIZE en la propiedad DBPROP_INIT_ASYNCH antes de llamar a **IDBInitialize:: Initialize**. Cuando se establece esta propiedad, el resultado de la llamada del proveedor a **Initialize** es S_OK si la operación se ha completado inmediatamente, o bien DB_S_ASYNCHRONOUS si la inicialización continúa de forma asincrónica. Las aplicaciones pueden consultar la interfaz **IDBAsynchStatus** o [ISSAsynchStatus](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)en el objeto de origen de datos y, a continuación, llamar a **IDBAsynchStatus:: getStatus** o [ISSAsynchStatus:: WaitForAsynchCompletion](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) para obtener el estado de la inicialización.  
  
 Además, se ha agregado la propiedad SSPROP_ISSAsynchStatus al conjunto de propiedades DBPROPSET_SQLSERVERROWSET. Los proveedores que admiten la interfaz **ISSAsynchStatus** deben implementar esta propiedad con un valor de VARIANT_TRUE.  
  
 Se puede llamar a **IDBAsynchStatus::Abort** o [ISSAsynchStatus::Abort](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-abort-ole-db.md) para cancelar la llamada asincrónica a **Initialize**. El consumidor debe solicitar explícitamente la inicialización de origen de datos asincrónica. De lo contrario, **IDBInitialize::Initialize** no devuelve resultados hasta que se inicializa completamente el objeto de origen de datos.  
  
> [!NOTE]  
>  Los objetos de origen de datos usados para la agrupación de conexiones no pueden llamar a la interfaz **ISSAsynchStatus** en el [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client. La interfaz **ISSAsynchStatus** no se expone para objetos de origen de datos agrupados.  
>   
>  Si una aplicación obliga explícitamente a usar el motor de cursor, **IOpenRowset::OpenRowset** e **IMultipleResults::GetResult** no admitirán el procesamiento asincrónico.  
>   
>  Además, la dll de código auxiliar/proxy de comunicación remota (en MDAC 2,8) no puede llamar a la interfaz **ISSAsynchStatus** en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. La interfaz **ISSAsynchStatus** no se expone a través de la comunicación remota.  
>   
>  Los componentes de servicio no admiten **ISSAsynchStatus**.  
  
## <a name="execution-and-rowset-initialization"></a>Ejecución e inicialización de conjuntos de filas  
 Las aplicaciones diseñadas para abrir de forma asincrónica el resultado de la ejecución de un comando pueden establecer el bit DBPROPVAL_ASYNCH_INITIALIZE en la propiedad DBPROP_ROWSET_ASYNCH. Si se establece este bit antes de llamar a **IDBInitialize::Initialize**, **ICommand::Execute**, **IOpenRowset::OpenRowset** o **IMultipleResults::GetResult**, el argumento *riid* debe establecerse en IID_IDBAsynchStatus, IID_ISSAsynchStatus o IID_IUnknown.  
  
 El método devuelve inmediatamente S_OK si la inicialización del conjunto de filas se completa de forma inmediata, o DB_S_ASYNCHRONOUS si el conjunto de filas sigue inicializándose de forma asincrónica, con *ppRowset* establecido en la interfaz solicitada en el conjunto de filas. Para el [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client, esta interfaz solo puede ser **IDBAsynchStatus** o **ISSAsynchStatus**. Hasta que el conjunto de filas está totalmente inicializado, esta interfaz se comporta como si estuviera en estado suspendido y las llamadas a **QueryInterface** para interfaces distintas de **IID_IDBAsynchStatus** o **IID_ISSAsynchStatus** pueden devolver E_NOINTERFACE. A menos que el consumidor solicite explícitamente el procesamiento asincrónico, el conjunto de filas se inicializa de forma sincrónica. Todos las interfaces solicitadas están disponibles cuando **IDBAsynchStaus::GetStatus** o **ISSAsynchStatus::WaitForAsynchCompletion** devuelven la indicación de que se ha completado la operación asincrónica. Esto no significa necesariamente que el conjunto de filas esté totalmente rellenado, pero está completo y es totalmente funcional.  
  
 Si el comando ejecutado no devuelve un conjunto de filas, sigue devolviendo inmediatamente un objeto que admite **IDBAsynchStatus**.  
  
 Si necesita recibir varios resultados de la ejecución de comandos asincrónica, debe hacer lo siguiente:  
  
-   Establezca el bit DBPROPVAL_ASYNCH_INITIALIZE de la propiedad DBPROP_ROWSET_ASYNCH antes de ejecutar el comando.  
  
-   Llame a **ICommand::Execute** y solicite **IMultipleResults**.  
  
 Las interfaces **ISSAsynchStatus** e **IDBAsynchStatus** se pueden obtener al consultar la interfaz de varios resultados mediante **QueryInterface**.  
  
 Una vez completada la ejecución del comando, **IMultipleResults** se puede usar de forma normal, con una excepción del caso sincrónico: se puede devolver DB_S_ASYNCHRONOUS, en cuyo caso se puede usar **IDBAsynchStatus** o **ISSAsynchStatus** para determinar cuándo se ha completado la operación.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente, la aplicación llama a un método de no bloqueo, realiza algún otro procesamiento y, después, vuelve a procesar los resultados. **ISSAsynchStatus::WaitForAsynchCompletion** espera en el objeto de evento interno hasta que se completa la operación de ejecución asincrónica o transcurre el tiempo especificado por *dwMilisecTimeOut*.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 **ISSAsynchStatus::WaitForAsynchCompletion** espera en el objeto de evento interno hasta que se completa la operación de ejecución asincrónica o transcurre el valor de *dwMilisecTimeOut*.  
  
 En el ejemplo siguiente se muestra el procesamiento asincrónico con varios conjuntos de resultados:  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 Para evitar bloqueos, el cliente puede comprobar el estado de una operación asincrónica en ejecución, como en el ejemplo siguiente:  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 El ejemplo siguiente muestra cómo puede cancelar la operación asincrónica que se está ejecutando en este momento:  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Características de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [Propiedades y comportamientos de conjuntos de filas](../../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
