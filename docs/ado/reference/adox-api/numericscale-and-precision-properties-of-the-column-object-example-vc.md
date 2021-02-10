---
description: Ejemplo de propiedades NumericScale y Precision del objeto Column (VC++)
title: Ejemplo de las propiedades NumericScale y Precision de la columna (VC + +) | Microsoft Docs
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
- Precision property [ADOX], VC++ example
- NumericScale property [ADOX], VC++ example
ms.assetid: 69653366-ebd7-4ff6-a654-761772223b0c
author: rothja
ms.author: jroth
ms.openlocfilehash: b8923ad7ea8adb0de769af264f39eb031c4c6d46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049925"
---
# <a name="numericscale-and-precision-properties-of-the-column-object-example-vc"></a>Ejemplo de propiedades NumericScale y Precision del objeto Column (VC++)
En este ejemplo se muestran las propiedades [NumericScale](./numericscale-property-adox.md) y [Precision](./precision-property-adox.md) del objeto [Column](./column-object-adox.md) . Este código muestra su valor para la tabla **Order Details** de la base de datos *Northwind* .  
  
```  
// BeginNumericScalePrecCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   HRESULT hr = S_OK;  
  
   // Define and initialize ADOX object pointers. These are in ADODB namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
   _ColumnPtr m_pColumn = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Declare string variables  
   _bstr_t strCnn("Provider='Microsoft.JET.OLEDB.4.0';Data Source='c:\\Northwind.mdb';");  
  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof (Catalog)));  
  
      // Connect the catalog.  
      m_pCnn->Open (strCnn, "", "", NULL);  
  
      m_pCatalog->PutActiveConnection(variant_t((IDispatch *)m_pCnn));  
  
      // Retrieve the Order Details table  
      m_pTable = m_pCatalog->Tables->GetItem("Order Details");  
  
      // Display numeric scale and precision of small integer fields.  
      _variant_t vIndex;  
      for ( long lIndex = 0 ; lIndex < m_pTable->Columns->Count ; lIndex++ ) {  
         vIndex = lIndex ;  
         m_pColumn = m_pTable->Columns->GetItem(vIndex);  
         if (m_pColumn->Type == adSmallInt) {  
            cout << "Column: " << m_pColumn->GetName() << endl;  
            cout << "Numeric scale: " << (_bstr_t) m_pColumn->GetNumericScale() << endl;  
            cout << "Precision: " << m_pColumn->GetPrecision() << endl;  
         }  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in NumericScalePrecX...." << endl;  
   }  
   ::CoUninitialize();  
}  
```