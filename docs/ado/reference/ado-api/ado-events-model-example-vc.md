---
description: Ejemplo de modelo de eventos de ADO (VC ++)
title: Ejemplo de modelo de eventos de ADO (VC + +) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- C++
helpviewer_keywords:
- Visual C++ code examples [ADO], event model
ms.assetid: 29530153-b963-4a7c-8665-2335f1d604a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 15999a2d20aa8374fcd29b007bc95035bb5f6185
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161627"
---
# <a name="ado-events-model-example-vc"></a>Ejemplo de modelo de eventos de ADO (VC ++)
En la sección Visual C++ de la [creación de instancias de eventos de ADO por lenguaje](../../guide/data/ado-event-instantiation-by-language.md) se ofrece una descripción general de cómo crear una instancia del modelo de eventos de ADO. El siguiente es un ejemplo específico de la creación de instancias del modelo de eventos dentro del entorno creado por la directiva **#import** .  
  
 La descripción general utiliza **adoint. h** como referencia para las firmas de método. Sin embargo, algunos detalles de la descripción general cambian ligeramente como resultado del uso de la directiva **#import** :  
  
-   La directiva **#import** resuelve los tipos de datos de la firma del método, los tipos y los modificadores de **typedef** en sus formatos fundamentales.  
  
-   Todos los métodos virtuales puros que se deben sobrescribir tienen el prefijo "**raw_**".  
  
 Parte del código simplemente refleja el estilo de codificación.  
  
-   El puntero a **IUnknown** utilizado por el método **Advise** se obtiene explícitamente con una llamada a **QueryInterface**.  
  
-   No es necesario codificar explícitamente un destructor en las definiciones de clase.  
  
-   Puede que desee codificar implementaciones más sólidas de QueryInterface, AddRef y Release.  
  
-   La directiva **__uuidof ()** se usa en gran medida para obtener los identificadores de interfaz.  
  
 Por último, el ejemplo contiene código de trabajo.  
  
-   El ejemplo se escribe como una aplicación de consola.  
  
-   Debe insertar su propio código bajo el comentario " `// Do some work` ".  
  
-   De forma predeterminada, todos los controladores de eventos no hacen nada y cancelan más notificaciones. Debe insertar el código adecuado para la aplicación y permitir las notificaciones si es necesario.  
  
```  
// ADO_Events_Model_Example.cpp  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// The Connection events  
class CConnEvent : public ConnectionEventsVt {  
private:  
   ULONG m_cRef;  
  
public:  
   CConnEvent() { m_cRef = 0; };  
   ~CConnEvent() {};  
  
   STDMETHODIMP QueryInterface(REFIID riid, void ** ppv);  
   STDMETHODIMP_(ULONG) AddRef();  
   STDMETHODIMP_(ULONG) Release();  
  
   STDMETHODIMP raw_InfoMessage( struct Error *pError,   
                                 EventStatusEnum *adStatus,   
                                 struct _Connection *pConnection);  
  
   STDMETHODIMP raw_BeginTransComplete( LONG TransactionLevel,  
                                        struct Error *pError,  
                                        EventStatusEnum *adStatus,  
                                        struct _Connection *pConnection);  
  
   STDMETHODIMP raw_CommitTransComplete( struct Error *pError,   
                                         EventStatusEnum *adStatus,  
                                         struct _Connection *pConnection);  
  
   STDMETHODIMP raw_RollbackTransComplete( struct Error *pError,   
                                           EventStatusEnum *adStatus,  
                                           struct _Connection *pConnection);  
  
   STDMETHODIMP raw_WillExecute( BSTR *Source,  
                                 CursorTypeEnum *CursorType,  
                                 LockTypeEnum *LockType,  
                                 long *Options,  
                                 EventStatusEnum *adStatus,  
                                 struct _Command *pCommand,  
                                 struct _Recordset *pRecordset,  
                                 struct _Connection *pConnection);  
  
   STDMETHODIMP raw_ExecuteComplete( LONG RecordsAffected,  
                                     struct Error *pError,  
                                     EventStatusEnum *adStatus,  
                                     struct _Command *pCommand,  
                                     struct _Recordset *pRecordset,  
                                     struct _Connection *pConnection);  
  
   STDMETHODIMP raw_WillConnect( BSTR *ConnectionString,  
                                 BSTR *UserID,  
                                 BSTR *Password,  
                                 long *Options,  
                                 EventStatusEnum *adStatus,  
                                 struct _Connection *pConnection);  
  
   STDMETHODIMP raw_ConnectComplete( struct Error *pError,  
                                     EventStatusEnum *adStatus,  
                                     struct _Connection *pConnection);  
  
   STDMETHODIMP raw_Disconnect( EventStatusEnum *adStatus, struct _Connection *pConnection);  
};  
  
// The Recordset events  
class CRstEvent : public RecordsetEventsVt {  
private:  
   ULONG m_cRef;     
  
public:  
   CRstEvent() { m_cRef = 0; };  
   ~CRstEvent() {};  
  
   STDMETHODIMP QueryInterface(REFIID riid, void ** ppv);  
   STDMETHODIMP_(ULONG) AddRef();  
   STDMETHODIMP_(ULONG) Release();  
  
   STDMETHODIMP raw_WillChangeField( LONG cFields,        
                                     VARIANT Fields,  
                                     EventStatusEnum *adStatus,  
                                     struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_FieldChangeComplete( LONG cFields,  
                                         VARIANT Fields,  
                                         struct Error *pError,  
                                         EventStatusEnum *adStatus,  
                                         struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_WillChangeRecord( EventReasonEnum adReason,  
                                      LONG cRecords,  
                                      EventStatusEnum *adStatus,  
                                      struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_RecordChangeComplete( EventReasonEnum adReason,  
                                          LONG cRecords,  
                                          struct Error *pError,  
                                          EventStatusEnum *adStatus,  
                                          struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_WillChangeRecordset( EventReasonEnum adReason,  
                                         EventStatusEnum *adStatus,  
                                         struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_RecordsetChangeComplete( EventReasonEnum adReason,  
                                             struct Error *pError,  
                                             EventStatusEnum *adStatus,  
                                             struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_WillMove( EventReasonEnum adReason,  
                              EventStatusEnum *adStatus,  
                              struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_MoveComplete( EventReasonEnum adReason,  
                                  struct Error *pError,  
                                  EventStatusEnum *adStatus,  
                                  struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_EndOfRecordset( VARIANT_BOOL *fMoreData,  
                                    EventStatusEnum *adStatus,  
                                    struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_FetchProgress( long Progress,  
                                   long MaxProgress,  
                                   EventStatusEnum *adStatus,  
                                   struct _Recordset *pRecordset);  
  
   STDMETHODIMP raw_FetchComplete( struct Error *pError,   
                                   EventStatusEnum *adStatus,  
                                   struct _Recordset *pRecordset);  
};  
  
// Implement each connection method  
STDMETHODIMP CConnEvent::raw_InfoMessage( struct Error *pError,   
                                         EventStatusEnum *adStatus,  
                                         struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_BeginTransComplete( LONG TransactionLevel,  
                                                 struct Error *pError,  
                                                 EventStatusEnum *adStatus,  
                                                 struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_CommitTransComplete( struct Error *pError,  
                                                  EventStatusEnum *adStatus,  
                                                  struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_RollbackTransComplete( struct Error *pError,  
                                                    EventStatusEnum *adStatus,  
                                                    struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_WillExecute( BSTR *Source,  
                                          CursorTypeEnum *CursorType,  
                                          LockTypeEnum *LockType,  
                                          long *Options,  
                                          EventStatusEnum *adStatus,  
                                          struct _Command *pCommand,  
                                          struct _Recordset *pRecordset,  
                                          struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_ExecuteComplete( LONG RecordsAffected,  
                                              struct Error *pError,  
                                              EventStatusEnum *adStatus,  
                                              struct _Command *pCommand,  
                                              struct _Recordset *pRecordset,  
                                              struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_WillConnect( BSTR *ConnectionString,  
                                          BSTR *UserID,  
                                          BSTR *Password,  
                                          long *Options,  
                                          EventStatusEnum *adStatus,  
                                          struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_ConnectComplete( struct Error *pError,  
                                              EventStatusEnum *adStatus,  
                                              struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CConnEvent::raw_Disconnect( EventStatusEnum *adStatus, struct _Connection *pConnection) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
// Implement each recordset method  
STDMETHODIMP CRstEvent::raw_WillChangeField( LONG cFields,  
                                             VARIANT Fields,  
                                             EventStatusEnum *adStatus,  
                                             struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_FieldChangeComplete( LONG cFields,  
                                                 VARIANT Fields,  
                                                 struct Error *pError,  
                                                 EventStatusEnum *adStatus,  
                                                 struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_WillChangeRecord( EventReasonEnum adReason,  
                                              LONG cRecords,  
                                              EventStatusEnum *adStatus,  
                                              struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_RecordChangeComplete( EventReasonEnum adReason,  
                                                  LONG cRecords,  
                                                  struct Error *pError,  
                                                  EventStatusEnum *adStatus,  
                                                  struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_WillChangeRecordset( EventReasonEnum adReason,  
                                                 EventStatusEnum *adStatus,  
                                                 struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_RecordsetChangeComplete( EventReasonEnum adReason,  
                                                     struct Error *pError,  
                                                     EventStatusEnum *adStatus,  
                                                     struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_WillMove( EventReasonEnum adReason,   
                                      EventStatusEnum *adStatus,  
                                      struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_MoveComplete( EventReasonEnum adReason,  
                                          struct Error *pError,  
                                          EventStatusEnum *adStatus,  
                                          struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_EndOfRecordset( VARIANT_BOOL *fMoreData,  
                                            EventStatusEnum *adStatus,  
                                            struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_FetchProgress( long Progress,  
                                           long MaxProgress,  
                                           EventStatusEnum *adStatus,  
                                           struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::raw_FetchComplete( struct Error *pError,  
                                           EventStatusEnum *adStatus,  
                                           struct _Recordset *pRecordset) {  
   *adStatus = adStatusUnwantedEvent;  
   return S_OK;  
};  
  
STDMETHODIMP CRstEvent::QueryInterface(REFIID riid, void ** ppv) {  
   *ppv = NULL;  
   if (riid == __uuidof(IUnknown) || riid == __uuidof(RecordsetEventsVt))   
      *ppv = this;  
  
   if (*ppv == NULL)  
      return ResultFromScode(E_NOINTERFACE);  
  
   AddRef();  
   return NOERROR;  
}  
  
STDMETHODIMP_(ULONG) CRstEvent::AddRef() {   
   return ++m_cRef;   
};  
  
STDMETHODIMP_(ULONG) CRstEvent::Release() {   
   if (0 != --m_cRef)   
      return m_cRef;  
   delete this;  
   return 0;  
}  
  
STDMETHODIMP CConnEvent::QueryInterface(REFIID riid, void ** ppv) {  
   *ppv = NULL;  
   if (riid == __uuidof(IUnknown) || riid == __uuidof(ConnectionEventsVt))   
      *ppv = this;  
  
   if (*ppv == NULL)  
      return ResultFromScode(E_NOINTERFACE);  
  
   AddRef();  
   return NOERROR;  
}  
  
STDMETHODIMP_(ULONG) CConnEvent::AddRef() {   
   return ++m_cRef;   
};  
  
STDMETHODIMP_(ULONG) CConnEvent::Release() {   
   if (0 != --m_cRef)   
      return m_cRef;  
   delete this;  
   return 0;  
}  
  
int main() {  
   HRESULT hr;  
   DWORD dwConnEvt;  
   DWORD dwRstEvt;  
   IConnectionPointContainer *pCPC = NULL;  
   IConnectionPoint *pCP = NULL;  
   IUnknown *pUnk = NULL;  
   CRstEvent *pRstEvent = NULL;  
   CConnEvent *pConnEvent= NULL;  
   int rc = 0;  
   _RecordsetPtr pRst;   
   _ConnectionPtr pConn;  
  
   ::CoInitialize(NULL);  
  
   hr = pConn.CreateInstance(__uuidof(Connection));  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pRst.CreateInstance(__uuidof(Recordset));  
  
   if (FAILED(hr))   
      return rc;  
  
   // Start using the Connection events  
   hr = pConn->QueryInterface(__uuidof(IConnectionPointContainer), (void **)&pCPC);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCPC->FindConnectionPoint(__uuidof(ConnectionEvents), &pCP);  
   pCPC->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   pConnEvent = new CConnEvent();  
   hr = pConnEvent->QueryInterface(__uuidof(IUnknown), (void **) &pUnk);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCP->Advise(pUnk, &dwConnEvt);  
   pCP->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   // Start using the Recordset events  
   hr = pRst->QueryInterface(__uuidof(IConnectionPointContainer), (void **)&pCPC);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCPC->FindConnectionPoint(__uuidof(RecordsetEvents), &pCP);  
   pCPC->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   pRstEvent = new CRstEvent();  
   hr = pRstEvent->QueryInterface(__uuidof(IUnknown), (void **) &pUnk);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCP->Advise(pUnk, &dwRstEvt);  
   pCP->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   // Do some work  
   pConn->Open("dsn=DataPubs;", "", "", adConnectUnspecified);  
   pRst->Open("SELECT * FROM authors", (IDispatch *) pConn, adOpenStatic, adLockReadOnly, adCmdText);  
  
   pRst->MoveFirst();  
   while (pRst->EndOfFile == FALSE) {  
      wprintf(L"Name = '%s'\n", (wchar_t*) ((_bstr_t) pRst->Fields->GetItem("au_lname")->Value));  
      pRst->MoveNext();  
   }  
  
   pRst->Close();  
   pConn->Close();  
  
   // Stop using the Connection events  
   hr = pConn->QueryInterface(__uuidof(IConnectionPointContainer), (void **) &pCPC);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCPC->FindConnectionPoint(__uuidof(ConnectionEvents), &pCP);  
   pCPC->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCP->Unadvise( dwConnEvt );  
   pCP->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   // Stop using the Recordset events  
   hr = pRst->QueryInterface(__uuidof(IConnectionPointContainer), (void **) &pCPC);  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCPC->FindConnectionPoint(__uuidof(RecordsetEvents), &pCP);  
   pCPC->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   hr = pCP->Unadvise( dwRstEvt );  
   pCP->Release();  
  
   if (FAILED(hr))   
      return rc;  
  
   CoUninitialize();  
}  
```