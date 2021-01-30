---
description: Interfaz ADORecordConstruction
title: Interfaz ADORecordConstruction | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction
helpviewer_keywords:
- ADORecordConstruction interface [ADO]
ms.assetid: 52a5429e-5829-455e-be3b-31f05cbecf2d
author: rothja
ms.author: jroth
ms.openlocfilehash: 45cf9d5bc72178e5ab56264e9f5ec3475f00eef1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171601"
---
# <a name="adorecordconstruction-interface"></a>Interfaz ADORecordConstruction
La interfaz **ADORecordConstruction** se utiliza para construir un objeto **Record** de ADO a partir de un objeto de **fila** OLE DB en una aplicación de C/C++.  
  
 Esta interfaz admite las siguientes propiedades:  
  
## <a name="properties"></a>Propiedades  
  
|Propiedad|Descripción|  
|-|-|  
|[ParentRow](./parentrow-property-ado.md)|De solo escritura.<br />Establece el contenedor de un objeto de **fila** OLE DB en este objeto **Record** de ADO.|  
|[Row](./row-property-ado.md)|Lectura y escritura.<br />Obtiene o establece un objeto de **fila** OLE DB de/en este objeto **Record** de ADO.|  
  
## <a name="methods"></a>Métodos  
 Ninguno.  
  
## <a name="events"></a>Events  
 Ninguno.  
  
## <a name="remarks"></a>Observaciones  
 Dado un OLE DB objeto **Row** ( `pRow` ), la construcción de un objeto **Record** de ADO ( `adoR` ) se corresponde con las tres operaciones básicas siguientes:  
  
1.  Cree un objeto de **registro** de ADO:  
  
    ```  
    _RecordPtr adoR;  
    adoRs.CreateInstance(__uuidof(_Record));  
    ```  
  
2.  Consulte la interfaz **IADORecordConstruction** en el objeto de **registro** :  
  
    ```  
    adoRecordConstructionPtr adoRConstruct=NULL;  
    adoR->QueryInterface(__uuidof(ADORecordConstruction),  
                        (void**)&adoRConstruct);  
    ```  
  
3.  Llame al método de propiedad **IADORecordConstruction::p ut_Row** para establecer el objeto de **fila** OLE DB en el objeto de **registro** de ADO:  
  
    ```  
    IUnknown *pUnk=NULL;  
    pRow->QueryInterface(IID_IUnknown, (void**)&pUnk);  
    adoRConstruct->put_Row(pUnk);  
    ```  
  
 El objeto **adoR** resultante representa ahora el objeto de **registro** de ADO construido a partir del objeto de **fila** OLE DB.  
  
 También se puede crear un objeto **Record** de ADO a partir del contenedor de un objeto de **fila** OLE DB.  
  
## <a name="requirements"></a>Requisitos  
 **Versión:** ADO 2,0 y versiones posteriores  
  
 **Biblioteca:** msado15.dll  
  
 **UUID:** 00000567-0000-0010-8000-00AA006D2EA4