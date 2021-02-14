---
title: Consultas XQuery que requiere el orden | Microsoft Docs
description: Ver ejemplos de consultas XQuery basados en la secuencia en la que aparecen los nodos en un documento.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- XQuery, sequence
- ordered expressions [XQuery]
ms.assetid: 4f1266c5-93d7-402d-94ed-43f69494c04b
author: rothja
ms.author: jroth
ms.openlocfilehash: d877c75223c42a8dff3a9a4c4615975a136cd6b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352336"
---
# <a name="xqueries-involving-order"></a>Consultas XQuery basadas en el orden
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Las bases de datos relacionales no reconocen el concepto de secuencia. Por ejemplo, no se puede realizar una solicitud similar a "Obtener el primer cliente de la base de datos". Sin embargo, puede consultar un documento XML y recuperar el primer \<Customer> elemento. A partir de ahí, siempre se recuperará el mismo cliente.  
  
 En este tema se describen las consultas que se basan en el orden en que aparecen los nodos en el documento.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieve-manufacturing-steps-at-the-second-work-center-location-for-a-product"></a>A. Recuperar los pasos de fabricación de un producto en el segundo centro de trabajo  
 En el caso de un modelo de producto específico, la consulta siguiente recupera los pasos de fabricación del segundo centro de trabajo de una serie de centros de trabajo del proceso de fabricación.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    <ManuStep ProdModelID = "{sql:column("Production.ProductModel.ProductModelID")}"  
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
     <Location>  
       { (//AWMI:root/AWMI:Location)[2]/@* }  
       <Steps>  
         { for $s in (//AWMI:root/AWMI:Location)[2]//AWMI:step  
           return  
              <Step>  
               { string($s) }  
              </Step>  
         }  
        </Steps>  
      </Location>  
     </ManuStep>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Las expresiones entre llaves se sustituyen por el resultado de su evaluación. Para obtener más información, vea [XML Construction &#40;XQuery&#41;](../xquery/xml-construction-xquery.md).  
  
-   **@\*** recupera todos los atributos de la ubicación del segundo centro de trabajo.  
  
-   La iteración FLWOR (para... Return) recupera todos los <`step`> elementos secundarios de la ubicación del segundo centro de trabajo.  
  
-   La [función SQL: column () (XQuery)](../xquery/xquery-extension-functions-sql-column.md) incluye el valor relacional en el XML que se está construyendo.  
  
 El resultado es el siguiente:  
  
```  
<ManuStep ProdModelID="7" ProductModelName="HL Touring Frame">  
  <Location LocationID="20" SetupHours="0.15"   
              MachineHours="2"  LaborHours="1.75" LotSize="1">  
  <Steps>  
   <Step>Assemble all frame components following blueprint 1299.</Step>  
     ...  
  </Steps>  
 </Location>  
</ManuStep>    
```  
  
 La consulta anterior recupera solamente los nodos de texto. Si desea que se `step` devuelva el elemento <> completo en su lugar, quite la función **String ()** de la consulta:  
  
### <a name="b-find-all-the-material-and-tools-used-at-the-second-work-center-location-in-the-manufacturing-of-a-product"></a>B. Encontrar todas las herramientas y todo el material utilizados en el segundo centro de trabajo en la fabricación de un producto  
 Para un modelo de producto específico, la consulta siguiente recupera las herramientas y el material utilizados en el segundo centro de trabajo de una serie de centros de trabajo del proceso de fabricación.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <Location>  
      { (//AWMI:root/AWMI:Location)[1]/@* }  
       <Tools>  
         { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:tool  
           return  
              <Tool>  
                { string($s) }  
              </Tool>  
          }  
        </Tools>  
        <Materials>  
            { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:material  
              return  
                 <Material>  
                    { string($s) }  
                 </Material>  
             }  
         </Materials>  
  </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La consulta construye el elemento <loca `tion`> y recupera sus valores de atributo de la base de datos.  
  
-   Utiliza dos iteraciones FLWOR (for...return): una para recuperar las herramientas y otra para recuperar el material.  
  
 El resultado es el siguiente:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
          MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
    <Tool>router with a carbide tip 15</Tool>  
    <Tool>Forming Tool FT-15</Tool>  
  </Tools>  
  <Materials>  
    <Material>aluminum sheet MS-2341</Material>  
  </Materials>  
</Location>  
```  
  
### <a name="c-retrieve-the-first-two-product-feature-descriptions-from-the-product-catalog"></a>C. Recuperar las dos primeras descripciones de las características de un producto del catálogo de productos  
 Para un modelo de producto específico, la consulta recupera las dos primeras descripciones de características del `Features` elemento <> del catálogo de modelos de producto.  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <ProductModel ProductModelID= "{ data( (/p1:ProductDescription/@ProductModelID)[1] ) }"  
                   ProductModelName = "{ data( (/p1:ProductDescription/@ProductModelName)[1] ) }" >  
       {  
         for $F in /p1:ProductDescription/p1:Features  
         return   
           $F/*[position() <= 2]   
       }  
     </ProductModel>  
      ') as x  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
 El cuerpo de la consulta crea XML que incluye el <`ProductModel`> elemento que tiene los atributos ProductModelID y ProductModelName.  
  
-   La consulta utiliza un... Devuelva el bucle para recuperar las descripciones de las características del modelo de producto. La función **Position ()** se usa para recuperar las dos primeras características.  
  
 El resultado es el siguiente:  
  
```xml
<ProductModel ProductModelID="19" ProductModelName="Mountain 100">  
 <p1:Warranty   
  xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
  <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
 <p2:Maintenance   
  xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p2:NoOfYears>10</p2:NoOfYears>  
  <p2:Description>maintenance contact available through your dealer   
            or any AdventureWorks retail store.  
  </p2:Description>  
 </p2:Maintenance>  
</ProductModel>   
```  
  
### <a name="d-find-the-first-two-tools-used-at-the-first-work-center-location-in-the-manufacturing-process-of-the-product"></a>D. Encontrar las dos primeras herramientas utilizadas en el primer centro de trabajo en el proceso de fabricación de un producto  
 Para un modelo de producto, esta consulta recupera las dos primeras herramientas utilizadas en el primer centro de trabajo de una serie de centros de trabajo del proceso de fabricación. La consulta se especifica en las instrucciones de fabricación almacenadas en la columna **instructions** de la tabla **Production. ProductModel** .  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   for $Inst in (//AWMI:root/AWMI:Location)[1]  
   return   
     <Location>  
       { $Inst/@* }  
       <Tools>  
         { for $s in ($Inst//AWMI:step//AWMI:tool)[position() <= 2]  
           return  
             <Tool>  
               { string($s) }  
             </Tool>  
         }  
       </Tools>  
     </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
  </Tools>  
</Location>   
```  
  
### <a name="e-find-the-last-two-manufacturing-steps-at-the-first-work-center-location-in-the-manufacturing-of-a-specific-product"></a>E. Encontrar los dos últimos pasos de fabricación del primer centro de trabajo de fabricación de un producto específico  
 La consulta utiliza la función **Last ()** para recuperar los dos últimos pasos de fabricación.  
  
```sql
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  <LastTwoManuSteps>  
   <Last-1Step>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[(last()-1)]/text() }  
   </Last-1Step>  
   <LastStep>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[last()]/text() }  
   </LastStep>  
  </LastTwoManuSteps>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```xml
<LastTwoManuSteps>  
   <Last-1Step>When finished, inspect the forms for defects per   
               Inspection Specification .</Last-1Step>  
   <LastStep>Remove the frames from the tool and place them in the   
             Completed or Rejected bin as appropriate.</LastStep>  
</LastTwoManuSteps>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Construcción XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
