---
title: IBCPSession2::BCPSetBulkMode (controlador OLE DB) | Microsoft Docs
description: Obtenga información sobre cómo usar el método IBCPSession2::BCPSetBulkMode para crear una copia masiva de una consulta o una tabla en OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BCPSetBulkMode function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a29e4de3d4c0cdd3572b1ce9f8f257bad65e57e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192071"
---
# <a name="ibcpsession2bcpsetbulkmode"></a>IBCPSession2::BCPSetBulkMode
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  IBCPSession2::BCPSetBulkMode proporciona una alternativa a [IBCPSession::BCPColFmt &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) para especificar el formato de columna. A diferencia de IBCPSession::BCPColFmt, que establece atributos de formato de columna, IBCPSession2::BCPSetBulkMode establece todos los atributos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
HRESULT BCPSetBulkMode (  
      int property,  
   void * pField,  
   int cbField,  
   void * pRow,  
   int cbRow  
);  
```  
  
## <a name="arguments"></a>Argumentos  
 *property*  
 Constante de tipo BYTE. Vea la tabla en la sección Comentarios para obtener una lista de las constantes.  
  
 *pField*  
 Puntero al valor de terminador de campo.  
  
 cbField  
 Longitud en bytes del valor de terminador de campo.  
  
 pRow  
 Puntero al valor de terminador de fila.  
  
 cbRow  
 Longitud en bytes del valor de terminador de fila.  
  
## <a name="returns"></a>Devuelve  
 IBCPSession2::BCPSetBulkMode puede devolver uno de los siguientes elementos:  
  
|||  
|-|-|  
|**S_OK**|El método se ha llevado a cabo de forma correcta.|  
|**E_FAIL**|Se ha producido un error específico del proveedor; para obtener más información, use la interfaz ISQLServerErrorInfo.|  
|**E_UNEXPECTED**|No se esperaba la llamada al método. Por ejemplo, no se llamó al método **IBCPSession2::BCPInit** antes de llamar a IBCPSession2::BCPSetBulkMode.|  
|**E_INVALIDARG**|El argumento no era válido.|  
|**E_OUTOFMEMORY**|Error de memoria insuficiente.|  
  
## <a name="remarks"></a>Observaciones  
 Se puede usar IBCPSession2::BCPSetBulkMode para crear una copia masiva fuera de una consulta o una tabla. Cuando IBCPSession2::BCPSetBulkMode se usa para la copia masiva de una instrucción de consulta, es necesario realizar antes una llamada a `IBCPSession::BCPControl(BCP_OPTIONS_HINTS, ...)` para especificar la instrucción de consulta.  
  
 Debe evitarse combinar la sintaxis de llamada RPC con la sintaxis de consulta por lotes (`{rpc func};SELECT * from Tbl`, por ejemplo) en el texto del mismo comando,  porque haría que ICommandPrepare::Prepare devolviese un error y le impediría recuperar metadatos. Utilice la sintaxis de ODBC CALL (`{call func}; SELECT * from Tbl`, por ejemplo) si necesita combinar la ejecución del procedimiento almacenado y la consulta por lotes en el texto del mismo comando.  
  
 En la lista siguiente se enumeran las constantes del parámetro *property* .  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|BCP_OUT_CHARACTER_MODE|Especifica el modo de salida de caracteres.<br /><br /> Corresponde a la opción -c en BCP.EXE y a IBCPSession::BCPColFmt con la propiedad *eUserDataType* establecida en **BCP_TYPE_SQLCHARACTER**.|  
|BCP_OUT_WIDE_CHARACTER_MODE|Especifica el modo de salida de Unicode.<br /><br /> Corresponde a la opción -w en BCP.EXE e IBCPSession::BCPColFmt con la propiedad *eUserDataType* establecida en **BCP_TYPE_SQLNCHAR**.|  
|BCP_OUT_NATIVE_TEXT_MODE|Especifica los tipos nativos para los tipos no de caracteres y Unicode para los tipos de caracteres.<br /><br /> Corresponde a la opción -N en BCP.EXE e IBCPSession::BCPColFmt con la propiedad *eUserDataType* establecida en **BCP_TYPE_SQLNCHAR** si el tipo de columna es una cadena o **BCP_TYPE_DEFAULT** si no lo es.|  
|BCP_OUT_NATIVE_MODE|Especifica los tipos de base de datos nativos.<br /><br /> Corresponde a la opción -n en BCP.EXE e IBCPSession::BCPColFmt con la propiedad *eUserDataType* establecida en **BCP_TYPE_DEFAULT**.|  
  
 Puede llamar a IBCPSession::BCPControl e IBCPSession2::BCPSetBulkMode para las opciones IBCPSession::BCPControl que no entran en conflicto con IBCPSession2::BCPSetBulkMode. Por ejemplo, puede llamar a IBCPSession::BCPControl con **BCP_OPTION_FIRST** e IBCPSession2::BCPSetBulkMode.  
  
 No puede llamar a IBCPSession::BCPControl con **BCP_OPTION_TEXTFILE** e IBCPSession2::BCPSetBulkMode.  
  
 Si intenta llamar a IBCPSession2::BCPSetBulkMode con una secuencia de llamadas a funciones que incluye IBCPSession::BCPColFmt, IBCPSession::BCPControl e IBCPSession::BCPReadFmt, una de las llamadas a funciones devolverá un error de secuencia. Si elige corregir el error, llame a IBCPSession::BCPInit para restablecer los valores de configuración y volver a empezar.  
  
 A continuación se presentan algunos ejemplos de llamadas a función que producen un error de secuencia de función:  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_IN);  
BCPSetBulkMode();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPReadFmt();  
```  
  
```cpp  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPSetBulkMode();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPColFmt();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPReadFmt();  
BCPColFmt();  
```  
  
```cpp  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetBulkMode();  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPReadFmt();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPColumns();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetColFmt();  
```  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se crean cuatro archivos con valores distintos de IBCPSession2::BCPSetBulkMode.  
  
```cpp  
  
// compile with: oleaut32.lib ole32.lib  
  
#include <stdio.h>  
#include "msoledbsql.h"  
  
IDBInitialize*  g_pIDBInitialize = NULL;  
IBCPSession2 * g_pIBcpSession = NULL;  
class COLEDBPropSet : public DBPROPSET {  
public:  
   COLEDBPropSet() {  
      rgProperties = NULL;  
      cProperties = 0;  
   };  
   COLEDBPropSet(const GUID& guid) {  
      rgProperties = NULL;  
      cProperties = 0;  
      guidPropertySet = guid;  
   };  
   ~COLEDBPropSet() {  
      for ( ULONG i = 0 ; i < cProperties ; i++ )  
         VariantClear(&rgProperties[i].vValue);  
      CoTaskMemFree(rgProperties);  
   }  
   void SetGUID(const GUID& guid) {  
      guidPropertySet = guid;  
   };  
   bool AddProperty(DWORD dwPropertyID, bool bValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BOOL;  
      rgProperties[cProperties].vValue.boolVal = (bValue) ? VARIANT_TRUE : VARIANT_FALSE;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID, long nValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID  = dwPropertyID;  
      rgProperties[cProperties].vValue.vt     = VT_I4;  
      rgProperties[cProperties].vValue.lVal   = nValue;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID,LPCWSTR szValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BSTR;  
      rgProperties[cProperties].vValue.bstrVal = SysAllocString(szValue);  
      cProperties++;  
      return true;  
   };  
   bool Add() {  
      DBPROP* p = (DBPROP*)CoTaskMemRealloc(rgProperties, (cProperties + 1) * sizeof(DBPROP));  
      if (p != NULL) {  
         rgProperties = p;  
         rgProperties[cProperties].dwOptions = DBPROPOPTIONS_REQUIRED;  
         rgProperties[cProperties].colid = DB_NULLID;  
         rgProperties[cProperties].vValue.vt = VT_EMPTY;  
         return true;  
      }  
      else  
         return false;  
   };  
};  
  
void OLEDBCleanUp() {  
   if (g_pIDBInitialize) {  
      g_pIDBInitialize->Release();  
      g_pIDBInitialize = NULL;  
   }  
   if (g_pIBcpSession) {  
      g_pIBcpSession->Release();  
      g_pIBcpSession = NULL;  
   }  
}  
  
BOOL MakeOLEDBConnect(LPWSTR  pServer) {  
   BOOL ret = true;  
   IDBProperties * pIDBProperties = NULL;  
   IDBCreateSession * pIDBCreateSession = NULL;  
   COLEDBPropSet PropSet(DBPROPSET_DBINIT);  
   COLEDBPropSet BcpProperty(DBPROPSET_SQLSERVERDATASOURCE);  
   try {  
      HRESULT hr = CoInitializeEx(NULL,COINIT_MULTITHREADED);   
      hr = CoCreateInstance(MSOLEDBSQL_CLSID, NULL, CLSCTX_INPROC_SERVER, IID_IDBInitialize, (LPVOID *)&g_pIDBInitialize);  
      if (FAILED(hr)) {  
         printf("CoCreateInstance failed\n");  
         return false;  
      }  
      PropSet.AddProperty(DBPROP_INIT_DATASOURCE, (LPWSTR)pServer);  
      PropSet.AddProperty(DBPROP_AUTH_INTEGRATED, L"SSPI");  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBProperties, (void**) &pIDBProperties);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->QueryInterface(IID_IDBProperties...) failed\n");  
         throw false;  
      }  
      hr = pIDBProperties->SetProperties(1, &PropSet);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties(...) failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->Initialize();  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->Initialize() failed\n");  
         throw false;  
      }  
      BcpProperty.AddProperty(SSPROP_ENABLEFASTLOAD, true);  
      BcpProperty.AddProperty(SSPROP_ENABLEBULKCOPY, true);  
      hr = pIDBProperties->SetProperties(1, &BcpProperty);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties() for bcp failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void**) &pIDBCreateSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->QueryInterface(IID_IDBCreateSession..) failed\n");  
         throw false;  
      }  
  
      hr = pIDBCreateSession->CreateSession(NULL, IID_IBCPSession2, (IUnknown**) &g_pIBcpSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBCreateSession->CreateSession() failed\n");  
         throw false;  
      }  
   }  
   catch(...) {  
      ret = false;  
   }  
   if (pIDBProperties)  
      pIDBProperties->Release();  
   if (pIDBCreateSession)  
      pIDBCreateSession->Release();  
   return ret;  
}  
  
BOOL BCPSetBulkMode(LPWSTR pszServer, LPTSTR pszQureryOut, char BCPType, LPWSTR pszDataFile) {  
   HRESULThr;  
   if (!MakeOLEDBConnect(pszServer))  
      return false;  
   hr = g_pIBcpSession->BCPInit(NULL, pszDataFile, NULL, BCP_DIRECTION_OUT );   // bcp init for queryout  
   if (FAILED(hr)) {  
      printf("BCP init failed\n");  
      OLEDBCleanUp();  
      return false;  
   }  
   // setbulkmode  
   char ColTerm[] = "\t";  
   char RowTerm[] = "\r\n";  
   wchar_t wColTerm[] = L"\t";  
   wchar_t wRowTerm[] = L"\r\n";  
   BYTE * pColTerm = NULL;  
   int cbColTerm = NULL;  
   BYTE * pRowTerm = 0;  
   int cbRowTerm = 0;  
   int bulkmode = -1;  
  
   if(BCPType == 'c') {   // bcp -c  
      pColTerm = (BYTE*)ColTerm;  
      pRowTerm = (BYTE*)RowTerm;  
      cbColTerm = 1;  
      cbRowTerm = 2;  
      bulkmode = BCP_OUT_CHARACTER_MODE;  
   }  
   else  
      if(BCPType == 'w') {   // bcp -w  
         pColTerm = (BYTE*)wColTerm;  
         pRowTerm = (BYTE*)wRowTerm;  
         cbColTerm = 2;  
         cbRowTerm = 4;  
         bulkmode = BCP_OUT_WIDE_CHARACTER_MODE;  
      }  
      else  
         if (BCPType == 'n')   // bcp -n  
            bulkmode = BCP_OUT_NATIVE_MODE;  
         else  
            if (BCPType == 'N')   // bcp -n  
               bulkmode = BCP_OUT_NATIVE_TEXT_MODE;  
            else {  
               printf("unknown bcp mode\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            hr = g_pIBcpSession->BCPSetBulkMode(bulkmode, pColTerm, cbColTerm, pRowTerm, cbRowTerm);  
            if (FAILED(hr)) {  
               printf("BCPSetBulkMode failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
  
            // set queryout TSQL statement  
            hr = g_pIBcpSession->BCPControl(BCP_OPTION_HINTS, pszQureryOut);  
            if (FAILED(hr)) {  
               printf("BCPControl failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            // bcp copy  
            DBROWCOUNT nRowsInserted = 0;  
            hr = g_pIBcpSession->BCPExec(&nRowsInserted);  
            if (FAILED(hr)) {  
               printf("BCPExec failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            printf("bcp done\n");  
            OLEDBCleanUp();  
            return true;  
}  
  
int main() {  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'c', L"bcpc.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'w', L"bcpw.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'n', L"bcpn.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'N', L"bcp_N.dat");  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [IBCPSession2 &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession2-ole-db.md)  
  
  
