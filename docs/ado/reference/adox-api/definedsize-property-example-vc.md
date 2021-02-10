---
description: Ejemplo de propiedad DefinedSize (VC++)
title: Ejemplo de la propiedad DefinedSize (VC + +) | Microsoft Docs
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
- DefinedSize property [ADOX], VC++ example
ms.assetid: cc752ae4-58c4-4a7b-bfb2-0454e90fe2e7
author: rothja
ms.author: jroth
ms.openlocfilehash: ff16d18f3584fbc97dc9b605afe7dc13d56636d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050155"
---
# <a name="definedsize-property-example-vc"></a>Ejemplo de propiedad DefinedSize (VC++)
En este ejemplo se muestra la propiedad [DefinedSize](./definedsize-property-adox.md) de una [columna](./column-object-adox.md). El código redefinirá el tamaño de la columna FirstName de la tabla **Employees** de la base de datos *Northwind* . A continuación, se muestra el cambio en los valores del [campo](../ado-api/field-object.md) FirstName de un [conjunto de registros](../ado-api/recordset-object-ado.md) basado en la tabla **Employees** . Tenga en cuenta que, de forma predeterminada, el campo FirstName se rellena con espacios después de volver a definir la propiedad **DefinedSize** .  
  
```  
// BeginDefinedSizeCpp.cpp  
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
  
   // Define ADOX object pointers, initialize pointers. These are in the ADODB namespace.  
   _CatalogPtr m_pCatNorthwind = NULL;  
   _ColumnPtr m_pColFirstName = NULL;  
   _ColumnPtr m_pColNewFirstName = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_RecordsetPtr m_pRstEmployees = NULL;  
  
   // Define string variables.  
   _bstr_t strCnn("Provider='Microsoft.JET.OLEDB.4.0';data source='c:\\Northwind.mdb';");  
   _bstr_t aryFirstName[10];  
  
   try {  
      // Open a Recordset for the Employees table.  
      TESTHR(hr = m_pRstEmployees.CreateInstance(__uuidof(ADODB::Recordset)));  
      TESTHR(hr = m_pCatNorthwind.CreateInstance(__uuidof (Catalog)));  
      TESTHR(hr = m_pColNewFirstName.CreateInstance(__uuidof(Column)));  
  
      m_pRstEmployees->Open("Employees", strCnn,ADODB::adOpenKeyset, ADODB::adLockReadOnly, ADODB::adCmdTable);  
  
      long lngSize = m_pRstEmployees->RecordCount;  
      aryFirstName[lngSize];  
  
      // Open a catalog for the Northwind database, using same connection as rstEmployees.  
      m_pCatNorthwind->PutActiveConnection(m_pRstEmployees->GetActiveConnection());  
  
      // Loop through the recordset displaying the contents, of the FirstName field, the field's defined size,  
      // and its actual size.  Also store FirstName values in aryFirstName array.  
      m_pRstEmployees->MoveFirst();  
      printf("\nOriginal Defined Size and Actual Size");  
      int iCount = 0;  
      while (!(m_pRstEmployees->EndOfFile)) {  
         printf("\nEmployee Name:");  
         printf("%s ", (LPSTR)(_bstr_t)m_pRstEmployees->Fields->GetItem("FirstName")->Value);  
         printf("%s\n", (LPSTR)(_bstr_t)m_pRstEmployees->Fields->GetItem("LastName")->Value);  
         printf("  FirstName Defined size: %d\n", m_pRstEmployees->Fields->GetItem("FirstName")->DefinedSize);  
         printf("  FirstName Actual size: %d\n", m_pRstEmployees->Fields->GetItem("FirstName")->ActualSize);  
         aryFirstName[iCount] = (_bstr_t) m_pRstEmployees->Fields->GetItem("FirstName")->Value;  
         m_pRstEmployees->MoveNext();  
      }  
      m_pRstEmployees->Close();  
  
      // Redefine the DefinedSize of FirstName in the catalog.  
      m_pColFirstName = m_pCatNorthwind->Tables->GetItem("Employees")->Columns->GetItem("FirstName");  
      m_pColNewFirstName->Name = m_pColFirstName->Name;  
      m_pColNewFirstName->Type = m_pColFirstName->Type;  
      m_pColNewFirstName->DefinedSize = (m_pColFirstName->DefinedSize) + 1;  
  
      // Append new FirstName column to catalog.  
      m_pCatNorthwind->Tables->GetItem("Employees")->Columns->Delete(m_pColFirstName->Name);  
      m_pCatNorthwind->Tables->GetItem("Employees")->Columns->Append(_variant_t((IDispatch*)m_pColNewFirstName, true),   
         adVarWChar,  
         m_pColNewFirstName->DefinedSize);  
  
      // Open Employee table in Recordset for updating.  
      m_pRstEmployees->Open("Employees",  
         m_pCatNorthwind->GetActiveConnection(),   
         ADODB::adOpenKeyset,  
         ADODB::adLockOptimistic,  
         ADODB::adCmdTable);  
  
      // Loop through the recordset displaying the contents of the FirstName field,the field's defined size,  
      // and its actual size. Also restore FirstName values from aryFirstName.  
      m_pRstEmployees->MoveFirst();  
      printf("\n\nNew Defined Size and Actual Size");  
      iCount = 0;  
      while (!(m_pRstEmployees->EndOfFile)) {  
         m_pRstEmployees->Fields->GetItem("FirstName")->Value = aryFirstName[iCount];  
         printf("\nEmployee Name: ");  
         printf("%s ", (LPSTR) (_bstr_t)m_pRstEmployees->Fields->GetItem("FirstName")->Value);  
         printf("%s\n", (LPSTR)(_bstr_t)m_pRstEmployees->Fields->GetItem("LastName")->Value);  
         printf("  FirstName Defined size: %d\n", m_pRstEmployees->Fields->GetItem("FirstName")->DefinedSize);  
         printf("  FirstName Actual size: %d\n", m_pRstEmployees->Fields->GetItem("FirstName")->ActualSize);  
         m_pRstEmployees->MoveNext();  
      }  
      m_pRstEmployees->Close();  
  
      // Restore original FirstName column to catalog  
      m_pCatNorthwind->Tables->GetItem("Employees")->Columns->Delete(m_pColNewFirstName->Name);  
  
      m_pCatNorthwind->Tables->GetItem("Employees")->Columns->Append(_variant_t((IDispatch*)m_pColFirstName,   
         true),   
         adVarWChar,  
         m_pColFirstName->DefinedSize);  
  
      // Restore original FirstName values to Employees table.  
      m_pRstEmployees->Open("Employees",   
         m_pCatNorthwind->GetActiveConnection(),   
         ADODB::adOpenKeyset,   
         ADODB::adLockOptimistic,  
         ADODB::adCmdTable);  
  
      m_pRstEmployees->MoveFirst();  
      iCount = 0;  
      while ( !(m_pRstEmployees->EndOfFile) ) {  
         m_pRstEmployees->Fields->GetItem("FirstName")->Value = aryFirstName[iCount];  
         m_pRstEmployees->MoveNext();  
         iCount++;  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in DefinedSizeX...." << endl;  
   }  
  
   if (m_pRstEmployees)  
      if (m_pRstEmployees->State == 1)  
         m_pRstEmployees->Close();  
  
   m_pCatNorthwind = NULL;  
  
   ::CoUninitialize();  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto column (ADOX)](./column-object-adox.md)   
 [DefinedSize (propiedad, ADOX)](./definedsize-property-adox.md)