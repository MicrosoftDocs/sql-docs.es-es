---
title: Seguridad de la integración CLR | Microsoft Docs
description: SQL Server integración con la .NET Framework seguridad de CLR administra el acceso entre los objetos. Las comprobaciones de seguridad realizadas en objetos dependen de las llamadas implicadas.
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- security [CLR integration]
- authorization [CLR integration]
- common language runtime [SQL Server], security
- database objects [CLR integration], security
ms.assetid: 05d7a471-c5d5-4730-b903-e4edc8157bb4
author: rothja
ms.author: jroth
ms.openlocfilehash: b0f5c1ba58efb8fddcf5375d2a38fbf9f227f43f
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196875"
---
# <a name="clr-integration-security"></a>Seguridad de la integración CLR

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  El modelo de seguridad de la integración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) administra y protege el acceso entre los distintos tipos de objetos que se ejecutan dentro de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], tanto objetos CLR como objetos no CLR. Es posible llamar a estos objetos mediante una instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] u otro objeto CLR que se ejecute en el servidor. Las llamadas entre objetos reciben el nombre de vínculos. Los tipos de comprobaciones de seguridad que se realizan en estos objetos dependen de los tipos de vínculos implicados.  
  
 El modelo de seguridad de la integración CLR tiene estos objetivos:  
  
-   De forma predeterminada, el hecho de ejecutar código de usuario administrado en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no debería poner en peligro la integridad y la estabilidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La realización de operaciones que podrían poner en peligro la estabilidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debería protegerse mediante los permisos de alto nivel pertinentes.  
  
-   El código de usuario administrado no debería obtener un acceso no autorizado a los datos del usuario ni a ningún otro código de usuario de la base de datos. El código definido por el usuario debería ejecutarse bajo el contexto de seguridad de la sesión del usuario que lo invocó y con los privilegios adecuados para ese contexto de seguridad.  
  
-   Debería haber controles que restringiesen el acceso del código de usuario a cualquier recurso situado fuera del servidor, y utilizarlo única y exclusivamente para el acceso a datos local y la realización de cálculos.  
  
-   El código definido por el usuario no debería poder obtener un acceso no autorizado a los recursos del sistema por el hecho de ejecutarse en el proceso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Ahora, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] integra el modelo de seguridad basado en usuario de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con el modelo de seguridad basado en acceso a código de CLR. Algunas de las ventajas que este enfoque combinado ofrece para la seguridad se describen en esta sección.  
  
 En la siguiente tabla se muestran los temas de esta sección.  
  
 [Seguridad de acceso del código de integración CLR](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)  
 Describe el modelo de seguridad de acceso del código (CAS) para el código administrado.  
  
 [Atributos de protección del host y programación de la integración CLR](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)  
 Ofrece información sobre los valores del atributo de protección de host (HPA) que no se permiten en los ensamblados SAFE y EXTERNAL_ACCESS.  
  
 [Vínculos en el ámbito de seguridad de la integración CLR]()  
 Describe la forma en que los fragmentos de código del usuario pueden llamarse entre sí en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Suplantación y seguridad de la integración CLR](../data-access/impersonation-and-credentials-for-connections.md)  
 Explica la forma en que el código administrado obtiene acceso a los recursos externos utilizando la suplantación.  
  
 Indica los problemas que surgen cuando un método administrado invoca a un método en una clase incluida dentro de otro ensamblado.  
  
 [Dominios de aplicación y seguridad de la integración CLR](/previous-versions/sql/2014/database-engine/dev-guide/allowing-partially-trusted-callers?view=sql-server-2014)  
 Describe la forma en que los ensamblados se cargan en los dominios de aplicación.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar ensamblados de integración CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)  
  
