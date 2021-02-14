---
description: Operaciones de servicio web clasificadas (Master Data Services)
title: Operaciones de servicio web clasificadas
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e3f346b5-7e26-481d-9821-1846e2e91289
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 65656aa902b5500d1dcc57dde914b3a0b2265008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350131"
---
# <a name="categorized-web-service-operations-master-data-services"></a>Operaciones de servicio web clasificadas (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  El servicio web de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] contiene un conjunto completo de operaciones que le permiten escribir código para controlar todas las características que proporciona [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] a través de su interfaz de usuario. Las operaciones del servicio web se definen mediante la interfaz <xref:Microsoft.MasterDataServices.IService> y se implementan como métodos en la clase <xref:Microsoft.MasterDataServices.ServiceClient>. En este tema se agrupan las operaciones del servicio web en categorías conceptuales para ayudarle a entender cómo se usa la API del servicio web.  
  
## <a name="model-operations"></a>Operaciones de modelo  
 Estas operaciones se utilizan para crear, actualizar y eliminar modelos, así como para usar todo el contenido del modelo, como entidades, jerarquías y versiones. Para obtener más información, consulte [Modelos &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>
  
## <a name="entity-operations"></a>Operaciones de entidad  
 Estas operaciones se utilizan para crear, actualizar y eliminar los miembros de una sola entidad. Para más información, vea [Entidades &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md) y [Miembros &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberKeyLookup%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersUpdate%2A>
  
## <a name="member-operations"></a>Operaciones de miembro  
 Estas operaciones se utilizan para obtener, actualizar y eliminar miembros. El conjunto de miembros con los que se trabaja puede contener miembros de varias entidades. Para obtener más información, consulte [Miembros &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersGet%2A>
  
## <a name="attribute-and-hierarchy-operations"></a>Operaciones de atributo y jerarquía  
 Estas operaciones se utilizan para obtener información de atributos y jerarquías. Los atributos y las jerarquías se pueden modificar también mediante las operaciones de modelo, como <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>. Para más información, vea [Atributos &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) y [Jerarquías &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAttributesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.HierarchyMembersGet%2A>
  
## <a name="business-rule-operations"></a>Operaciones de regla de negocio  
 Estas operaciones se utilizan para crear, actualizar, eliminar y publicar reglas de negocio. Para más información, vea [Reglas de negocios &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPaletteGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPublish%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesUpdate%2A>
  
## <a name="annotation-operations"></a>Operaciones de anotación  
 Estas operaciones se utilizan para crear, actualizar y eliminar anotaciones. Para más información, vea [Anotaciones &#40;Master Data Services&#41;](../../master-data-services/annotations-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsGet%2A>
  
## <a name="transaction-operations"></a>Operaciones de transacción  
 Estas operaciones se utilizan para obtener y revertir transacciones. Para obtener más información, consulte [Transacciones &#40;Master Data Services&#41;](../../master-data-services/transactions-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsReverse%2A>
  
## <a name="version-and-validation-operations"></a>Operaciones de versión y validación  
 Estas operaciones se utilizan para copiar y validar versiones. Para más información, vea [Versiones &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md) y [Validación &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.VersionCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationProcess%2A>
  
## <a name="data-quality-operations"></a>Operaciones de calidad de los datos  
 Estas operaciones se utilizan para realizar tareas de calidad de los datos y examinar sus resultados.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityCleansingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityMatchingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityInstalledState%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityKnowledgeBasesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStart%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationResultsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStatus%2A>
  
## <a name="data-import-operations"></a>Operaciones de importación de datos  
 Estas operaciones se utilizan para importar datos en una base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Para obtener más información, vea [información general: importar datos de tablas &#40;Master Data Services&#41;](../../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingLoad%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingProcess%2A>
  
 Las operaciones siguientes se utilizan para importar datos mediante el proceso de almacenamiento temporal incluido en [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Estas operaciones solo se deben utilizar para permitir el uso de bases de datos existentes. Para nuevas implementaciones, utilice las operaciones anteriormente indicadas.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingNameCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingProcess%2A>
  
## <a name="data-export-operations"></a>Operaciones de exportación de datos  
 Estas operaciones se utilizan para exportar datos a través del uso de vistas de suscripción. Para obtener más información, consulte [Información general: exportar datos &#40;Master Data Services&#41;](../../master-data-services/overview-exporting-data-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewUpdate%2A>
  
## <a name="security-operations"></a>Operaciones de seguridad  
 Estas operaciones se utilizan para modificar la configuración de seguridad que controla el acceso a la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Para obtener más información, consulte [Seguridad &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesUpdate%2A>
  
## <a name="system-operations"></a>Operaciones del sistema  
 Estas operaciones se utilizan para obtener y actualizar la configuración del sistema y las preferencias de usuario.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceVersionGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemDomainListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemPropertiesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesUpdate%2A>
  
  
