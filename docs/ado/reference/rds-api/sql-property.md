---
description: Propiedad SQL
title: Propiedad SQL | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SQL property [RDS]
ms.assetid: e0dabf23-a159-4fe5-a962-3df544a21f5c
author: rothja
ms.author: jroth
ms.openlocfilehash: c24b191e11f95428b80a9ecfb3cced4f605e3e76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166102"
---
# <a name="sql-property"></a>Propiedad SQL
Indica la cadena de consulta utilizada para recuperar el [conjunto de registros](../ado-api/recordset-object-ado.md).  
  
 Puede establecer la propiedad **SQL** en tiempo de diseño en [RDS.](./datacontrol-object-rds.md) Etiquetas de objeto del objeto DataControl o en tiempo de ejecución en el código de scripting.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Design time: <PARAM NAME="SQL" VALUE="QueryString">  
Run time: DataControl.SQL = "QueryString"  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Cadenas*  
 Valor de **cadena** que contiene una solicitud de datos SQL válida.  
  
 *DataControl*  
 Variable de objeto que representa un objeto **RDS. Objeto DataControl** .  
  
## <a name="remarks"></a>Observaciones  
 En general, se trata de una instrucción SQL (con el dialecto del servidor de base de datos), como `"Select * from NewTitles"` . Para asegurarse de que los registros coinciden y se actualizan con precisión, una consulta actualizable debe contener un campo que no sea un campo binario largo o un campo calculado.  
  
 La propiedad **SQL** es opcional si un objeto empresarial personalizado del lado servidor recupera los datos para el cliente.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad SQL (VBScript)](./sql-property-example-vbscript.md)   
 [Propiedad Connect (RDS)](./connect-property-rds.md)   
 [Query (método) (RDS)](./query-method-rds.md)   
 [Método Refresh (RDS)](./refresh-method-rds.md)   
 [Método SubmitChanges (RDS)](./submitchanges-method-rds.md)