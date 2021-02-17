---
title: Empty (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la función XQuery vacía () que devuelve un valor que indica si una secuencia especificada de elementos está vacía.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- empty function
- fn:empty function
ms.assetid: 46da89a8-0cd9-4913-8521-4087589a04ba
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f9a09225562cbabe7ced75d80f26e37ee53d3e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335841"
---
# <a name="functions-on-sequences---empty"></a>Funciones usadas en secuencias: empty
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve true si el valor de *$arg* es una secuencia vacía. De lo contrario, la función devuelve False.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:empty($arg as item()*) as xs:boolean  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de elementos. Si la secuencia está vacía, la función devuelve True. De lo contrario, la función devuelve False.  
  
## <a name="remarks"></a>Observaciones  
 La función **FN: exists ()** no se admite. Como alternativa, se puede usar la función **Not ()** .  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-empty-xquery-function-to-determine-if-an-attribute-is-present"></a>A. Utilizar la función empty() de XQuery para determinar si un atributo está presente  
 En el proceso de fabricación del modelo de producto 7, esta consulta devuelve todas las ubicaciones de centro de trabajo que no tienen un atributo **MachineHours** .  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     for $i in /AWMI:root/AWMI:Location[empty(@MachineHours)]  
     return  
       <Location  
            LocationID="{ ($i/@LocationID) }"  
            LaborHrs="{ ($i/@LaborHours) }" >  
            {   
              $i/@MachineHours  
            }    
       </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```  
ProductModelID      Result          
-------------- ------------------------------------------  
7              <Location LocationID="30" LaborHrs="1"/>  
               <Location LocationID="50" LaborHrs="3"/>  
               <Location LocationID="60" LaborHrs="4"/>  
```  
  
 La consulta siguiente, ligeramente modificada, devuelve "NotFound" si el atributo **MachineHour** no está presente:  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace p14="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     for $i in /p14:root/p14:Location  
     return  
       <Location  
            LocationID="{ ($i/@LocationID) }"  
            LaborHrs="{ ($i/@LaborHours) }" >  
            {   
                 if (empty($i/@MachineHours)) then  
                    attribute MachineHours { "NotFound" }  
                 else  
                    attribute MachineHours { data($i/@MachineHours) }  
            }    
       </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```  
ProductModelID Result                         
-------------- -----------------------------------  
7                
  <Location LocationID="10" LaborHrs="2.5" MachineHours="3"/>  
  <Location LocationID="20" LaborHrs="1.75" MachineHours="2"/>  
  <Location LocationID="30" LaborHrs="1" MachineHours="NotFound"/>  
  <Location LocationID="45" LaborHrs="0.5" MachineHours="0.65"/>  
  <Location LocationID="50" LaborHrs="3" MachineHours="NotFound"/>  
  <Location LocationID="60" LaborHrs="4" MachineHours="NotFound"/>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos XML](../xquery/xquery-functions-against-the-xml-data-type.md)   
 [Método exist&#40;&#41; &#40;tipo de datos xml&#41;](../t-sql/xml/exist-method-xml-data-type.md)  
  
  
