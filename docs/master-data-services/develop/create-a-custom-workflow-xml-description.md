---
description: 'Crear un flujo de trabajo personalizado: Descripción del código XML'
title: Descripción del código XML de flujo de trabajo personalizado
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e267e5f4-38bb-466d-82e8-871eabeec07e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 416670f703ba40a54650a2dfc28b2f508c202439
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350142"
---
# <a name="create-a-custom-workflow---xml-description"></a>Crear un flujo de trabajo personalizado: Descripción del código XML

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , se llama al método [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130)) mediante SQL Server servicio de integración de flujos de trabajo de MDS cuando se inicia un flujo de trabajo. Este método recibe los metadatos y los datos del elemento que desencadenó la regla de negocio del flujo de trabajo como un bloque de XML. Para ver un código de ejemplo que implemente un controlador de flujo de trabajo, vea [Ejemplo de flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md).  
  
 El ejemplo siguiente es un ejemplo del código XML que se envía al controlador del flujo de trabajo:  
  
```scr  
<ExternalAction>  
  <Type>TEST</Type>  
  <SendData>1</SendData>  
  <Server_URL>This is my test!</Server_URL>  
  <Action_ID>Test Workflow</Action_ID>  
  <Model_ID>5</Model_ID>  
  <Model_Name>Customer</Model_Name>  
  <Entity_ID>34</Entity_ID>  
  <Entity_Name>Customer</Entity_Name>  
  <Version_ID>8</Version_ID>  
  <MemberType_ID>1</MemberType_ID>  
  <Member_ID>12</Member_ID>  
  <MemberData>  
    <ID>12</ID>  
    <Version_ID>8</Version_ID>  
    <ValidationStatus_ID>3</ValidationStatus_ID>  
    <ChangeTrackingMask>0</ChangeTrackingMask>  
    <EnterDTM>2011-02-25T20:16:36.650</EnterDTM>  
    <EnterUserID>2</EnterUserID>  
    <EnterUserName>MyUserName</EnterUserName>  
    <EnterUserMuid>EEF91D48-B673-4D83-B95F-5A363C11DE91</EnterUserMuid>  
    <EnterVersionId>8</EnterVersionId>  
    <EnterVersionName>VERSION_1</EnterVersionName>  
    <EnterVersionMuid>52B788C2-2750-4651-9DB0-2CB05A88AA5A</EnterVersionMuid>  
    <LastChgDTM>2011-02-25T20:16:36.650</LastChgDTM>  
    <LastChgUserID>2</LastChgUserID>  
    <LastChgUserName>MyUserName</LastChgUserName>  
    <LastChgUserMuid>EEF91D48-B673-4D83-B95F-5A363C11DE91</LastChgUserMuid>  
    <LastChgVersionId>8</LastChgVersionId>  
    <LastChgVersionName>VERSION_1</LastChgVersionName>  
    <LastChgVersionMuid>52B788C2-2750-4651-9DB0-2CB05A88AA5A</LastChgVersionMuid>  
    <Name>Test Customer</Name>  
    <Code>TC</Code>  
  </MemberData>  
</ExternalAction>  
```  
  
 En la tabla siguiente se describen algunas de las etiquetas incluidas en este XML:  
  
|Etiqueta|Descripción|  
|---------|-----------------|  
|\<Type>|El texto que especificó en el cuadro de texto **Tipo de flujo de trabajo** de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] para identificar el ensamblado del flujo de trabajo personalizado que se debe cargar.|  
|\<SendData>|Valor booleano controlado por la casilla **Incluir datos de miembro en el mensaje** de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Un valor de 1 significa que \<MemberData> se envía la sección; de lo contrario, \<MemberData> no se envía la sección.|  
|<Server_URL>|El texto que especificó en el cuadro de texto **Sitio de flujo de trabajo** de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
|<Action_ID>|El texto que especificó en el cuadro de texto **Nombre de flujo de trabajo** de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
|\<MemberData>|Contiene los datos del miembro que desencadenó la acción del flujo de trabajo. Solo se incluye si el valor de \<SendData> es 1.|  
|\<Enter*xxx*>|Este conjunto de etiquetas contiene metadatos sobre la creación del miembro, por ejemplo, cuándo se creó y quién lo creó.|  
|\<LastChg*xxx*>|Este conjunto de etiquetas contiene metadatos sobre el último cambio realizado en el miembro, como cuándo se realizó el cambio y quién lo hizo.|  
|\<Name>|Se cambió el primer atributo del miembro. Este miembro de ejemplo contiene únicamente los atributos Name y Code.|  
|\<Code>|Se cambió el siguiente atributo del miembro. Si este miembro de ejemplo contuviera más atributos, se incluirían detrás de este.|  
  
## <a name="see-also"></a>Consulte también  
 [Crear un flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)   
 [Ejemplo de flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md)  
  
  
