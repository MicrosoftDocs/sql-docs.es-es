---
description: Ejemplo de la propiedad de elemento (VC ++)
title: Ejemplo de la propiedad Item (VC + +) | Microsoft Docs
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
- Item property [ADO], VC++ example
ms.assetid: 05ae3f5a-a0c1-459d-aa7d-ed7f3b2ecd60
author: rothja
ms.author: jroth
ms.openlocfilehash: fdbecc77e8a86f2135c6b12fd9fbf6a4727c31cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041865"
---
# <a name="item-property-example-vc"></a>Ejemplo de la propiedad de elemento (VC ++)
En este ejemplo se muestra cómo la propiedad del [elemento](./item-property-ado.md) obtiene acceso a los miembros de una colección. En el ejemplo se abre la tabla ***authors** _ de la base de datos _ *_pubs_** con un comando con parámetros.  
  
 Se tiene acceso al parámetro del comando emitido en la base de datos desde la colección de [parámetros](./parameters-collection-ado.md) del objeto de [comando](./command-object-ado.md) por índice y nombre. A continuación, se obtiene acceso a los campos del [conjunto de registros](./recordset-object-ado.md) devuelto desde la colección de [campos](./fields-collection-ado.md) de ese objeto por índice y nombre.  
  
```  
// BeginItemCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void ItemX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if (FAILED(::CoInitialize(NULL)))  
      return -1;  
  
   ItemX();  
   ::CoUninitialize();  
}  
  
void ItemX() {  
   HRESULT hr = S_OK;  
  
   // Define ADO object pointers.  Initialize pointers on define.  These are in the ADODB::  namespace.  
   _ConnectionPtr pConnection = NULL;  
   _RecordsetPtr pRst = NULL;  
   _CommandPtr pCmd = NULL;  
   _ParameterPtr pPrm = NULL;  
   FieldPtr pFld = NULL;  
  
   // Other Variables.  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
   _variant_t Column[9];  
   _variant_t vIndex;  
  
   try {  
      // Open connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      TESTHR(pRst.CreateInstance(__uuidof(Recordset)));  
      TESTHR(pCmd.CreateInstance(__uuidof(Command)));  
  
      // Set the array with the authors table field (column) names  
      Column[0] = "au_id";  
      Column[1] = "au_lname";  
      Column[2] = "au_fname";  
      Column[3] = "phone";  
      Column[4] = "address";  
      Column[5] = "city";  
      Column[6] = "state";  
      Column[7] = "zip";  
      Column[8] = "contract";  
  
      _bstr_t strText("SELECT * FROM authors WHERE state = ?");  
      pCmd->CommandText = strText;  
  
      pPrm = pCmd->CreateParameter("ItemXparm", adChar, adParamInput, 2, "CA");  
      pCmd->Parameters->Append(pPrm);  
  
      pConnection->Open(strCnn, "", "", adConnectUnspecified);  
      pCmd->ActiveConnection = pConnection;  
  
      // Connection and CommandType are omitted because a Command object is provided.  
      _variant_t Conn;  
      Conn.vt = VT_ERROR;  
      Conn.scode = DISP_E_PARAMNOTFOUND;  
  
      pRst->Open((_variant_t((IDispatch *) pCmd)), Conn, adOpenStatic, adLockReadOnly, -1);  
  
      printf("The Parameters collection accessed by index...\n");  
  
      vIndex = (short) 0;  
      pPrm = pCmd->Parameters->GetItem(&vIndex);  
      printf("Parameter name = '%s', value = '%s'\n",  
         (LPCSTR)pPrm->Name, (LPSTR)(_bstr_t)pPrm->Value);  
  
      printf("\nThe Parameters collection accessed by name...\n");  
      pPrm = pCmd->Parameters->Item["ItemXparm"];  
      printf("Parameter name = '%s', value = '%s'\n",  
         (LPCSTR)pPrm->Name, (LPSTR)(_bstr_t)pPrm->Value);  
  
      printf("\nThe Fields collection accessed by index...\n");  
      pRst->MoveFirst();  
      long limit = 0;  
      limit = ((pRst->Fields->Count) - 1);  
      int intLineCnt = 8;   
      for (short iIndex = 0 ; iIndex <= limit ; iIndex++) {  
         vIndex = iIndex;  
  
         pFld = pRst->Fields->GetItem(&vIndex);  
         printf("Field %d : Name =  '%s', ", iIndex, (LPCSTR)pFld->GetName());  
         _variant_t FldVal = pFld->GetValue();    
  
         // Because Value is the default property of a Property object, the use   
         // of the actual keyword here is optional.  
         switch(FldVal.vt) {  
         case (VT_BOOL):  
            if (FldVal.boolVal)  
               printf("Value = 'True'");  
            else  
               printf("Value = 'False'");  
            printf("\n");  
            break;  
         case (VT_BSTR):  
            printf("Value = '%s'",  
               (LPCSTR)(_bstr_t)FldVal.bstrVal);  
            printf("\n");  
            break;  
         case (VT_I4):  
            printf("Value = '%s'",(LPCSTR)FldVal.iVal);  
            printf("\n");  
            break;  
         case (VT_EMPTY):  
            printf("Value = '%s'",(LPCSTR)FldVal.lVal);  
            printf("\n");  
            break;  
         default:  
            break;  
         }  
      }  
  
      printf("\nThe Fields collection accessed by name...\n");  
      pRst->MoveFirst();  
  
      for (int iIndex = 0; iIndex <= 8 ; iIndex++) {  
         intLineCnt++;  
  
         pFld = pRst->Fields->GetItem(Column[iIndex]);  
         printf("Field name = '%s', ", (LPCSTR)pFld->GetName());  
         _variant_t FldVal = pFld->GetValue();  
  
         // Because Value is the default property of a Property object,  
         // the use of the actual keyword here is optional.  
         switch(FldVal.vt) {  
         case (VT_BOOL):  
            if (FldVal.boolVal)  
               printf("Value = 'True'");  
            else  
               printf("Value = 'False'");  
            printf("\n");  
            break;  
         case (VT_BSTR):  
            printf("Value = '%s'",  
               (LPCSTR)(_bstr_t)FldVal.bstrVal);  
            printf("\n");  
            break;  
         case (VT_I4):  
            printf("Value = '%s'", (LPCSTR)FldVal.iVal);  
            printf("\n");  
            break;  
         case (VT_EMPTY):  
            printf("Value = '%s'", (LPCSTR)FldVal.lVal);  
            printf("\n");  
            break;  
         default:  
            break;  
         }  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      // Pass a connection pointer accessed from the Recordset.  
      PrintProviderError(pConnection);  
      PrintComError(e);  
   }  
  
   // Clean up objects before exit.  
   if (pRst)  
      if (pRst->State == adStateOpen)  
         pRst->Close();  
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if( (pConnection->Errors->Count) > 0) {  
      long nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for (long i = 0 ; i < nCount ; i++) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("\t Error number: %x\t%s", pErr->Number, pErr->Description);  
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
  
## <a name="see-also"></a>Consulte también  
 [Command (objeto) (ADO)](./command-object-ado.md)   
 [Fields (colección) (ADO)](./fields-collection-ado.md)   
 [Item (propiedad) (ADO)](./item-property-ado.md)   
 [Parameters (colección) (ADO)](./parameters-collection-ado.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)