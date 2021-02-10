---
description: Ejemplo de la propiedad de tipo (campo) (VC ++)
title: Ejemplo de la propiedad de tipo (campo) (VC + +) | Microsoft Docs
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
- Type property [field] [ADO], VC++ example
ms.assetid: 440dbdb1-16fc-4cfe-9451-59a153852537
author: rothja
ms.author: jroth
ms.openlocfilehash: 41ef9c6809302e39d63e5cca53bc3c0f29963683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056420"
---
# <a name="type-property-example-field-vc"></a>Ejemplo de la propiedad de tipo (campo) (VC ++)
En este ejemplo se muestra la propiedad [Type](./type-property-ado.md) al mostrar el nombre de la constante que corresponde al valor de la propiedad **Type** de todos los objetos [Field](./field-object.md) de la tabla **_Employees_** . La función FieldType es necesaria para que se ejecute este procedimiento.  
  
## <a name="example"></a>Ejemplo  
  
```  
// BeginTypeFieldCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
  
void TypeX();  
_bstr_t FieldType(int intType);   
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   TypeX();  
   ::CoUninitialize();  
}  
  
void TypeX() {  
   // Define string variables.  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='(local)'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   // Define ADO object pointers, initialize pointers.  These are in the ADODB:: namespace.  
   _RecordsetPtr pRstEmployees = NULL;  
   FieldsPtr pFldLoop = NULL;  
  
   try {    
      // Open recordset with data from Employee table.  
      TESTHR(pRstEmployees.CreateInstance(__uuidof(Recordset)));  
      pRstEmployees->Open ("employee", strCnn, adOpenForwardOnly, adLockReadOnly, adCmdTable);  
  
      printf("Fields in Employee Table:\n\n");  
  
      // Enumerate the Fields collection of the Employees table.  
      pFldLoop = pRstEmployees->GetFields();  
      for (short int intFields = 0 ; intFields < (int)pFldLoop->GetCount() ; intFields++) {  
         _variant_t Index;  
         Index.vt = VT_I2;  
         Index.iVal = intFields;  
         printf("  Name: %s\n" , (LPCSTR) pFldLoop->GetItem(Index)->GetName());  
         printf("  Type: %s\n\n", (LPCTSTR)FieldType(pFldLoop->GetItem(Index)->Type));  
      }  
   }  
   catch (_com_error &e) {  
      // Display errors, if any.  Pass connection pointer accessed from the Recordset.  
      _variant_t vtConnect = pRstEmployees->GetActiveConnection();  
  
      // GetActiveConnection returns connect string if connection  
      // is not open, else returns Connection object.  
      switch(vtConnect.vt) {  
      case VT_BSTR:  
         PrintComError(e);  
         break;  
      case VT_DISPATCH:  
         PrintProviderError(vtConnect);  
         break;  
      default:  
         printf("Errors occured.");  
         break;  
      }  
   }  
  
   // Clean up objects before exit.  
   if (pRstEmployees)  
      if (pRstEmployees->State == adStateOpen)  
         pRstEmployees->Close();  
}  
  
_bstr_t FieldType(int intType) {  
   _bstr_t strType;   
   switch(intType) {  
   case adChar:  
      strType = "adChar";  
      break;  
   case adVarChar:  
      strType = "adVarChar";  
      break;  
   case adSmallInt:  
      strType = "adSmallInt";  
      break;  
   case adUnsignedTinyInt:  
      strType = "adUnsignedTinyInt";  
      break;  
   case adDBTimeStamp:  
      strType = "adDBTimeStamp";  
      break;  
   default:  
      break;  
   }  
   return strType;  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0 ) {  
      long nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("Error number: %x\t%s\n", pErr->Number, (LPCSTR) pErr->Description);  
      }  
   }  
}  
  
void PrintComError(_com_error &e) {  
   _bstr_t bstrSource(e.Source());  
   _bstr_t bstrDescription(e.Description());  
  
   // Print Com errors.    
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
 **Campos de la tabla Employee:**  
 **Nombre: emp_id**  
 **Tipo: adChar**  
 **Nombre: fname**  
 **Tipo: advarchar**  
 **Nombre: MINIT**  
 **Tipo: adChar**  
 **Nombre: LNAME**  
 **Tipo: advarchar**  
 **Nombre: job_id**  
 **Tipo: adSmallInt**  
 **Nombre: job_lvl**  
 **Tipo: adUnsignedTinyInt**  
 **Nombre: pub_id**  
 **Tipo: adChar**  
 **Nombre: hire_date**  
 **Tipo: adDBTimeStamp**   
## <a name="see-also"></a>Consulte también  
 [Field (objeto)](./field-object.md)   
 [Tipo (propiedad, ADO)](./type-property-ado.md)