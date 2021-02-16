---
title: clr enabled (opción de configuración del servidor) | Microsoft Docs
description: Aprenda a usar la opción clr enabled para especificar si SQL Server puede ejecutar ensamblados de usuario. Vea cuándo no se admite la ejecución de Common Language Runtime.
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- assemblies [CLR integration], verifying can run
- clr enabled option
ms.assetid: 0722d382-8fd3-4fac-b4a8-cd2b7a7e0293
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 600b70e751fe4fcd3d80c7e86bd494ecc8cf1758
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347033"
---
# <a name="clr-enabled-server-configuration-option"></a>clr enabled (opción de configuración del servidor)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilice la opción clr enabled para especificar si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]puede ejecutar ensamblados de usuario. La opción clr enabled proporciona los valores que se indican a continuación: 
  
|Value|Descripción|  
|-----------|-----------------|  
|0|Ejecución de ensamblado no permitida en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|1|Ejecución de ensamblado permitida en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
Solo en WOW64. Reinicie los servidores de WOW64 para efectuar los cambios de la configuración. No se requiere el reinicio para otros tipos de servidor.  

Cuando se ejecuta RECONFIGURE y se cambia el valor de ejecución de la opción clr enabled de 1 a 0, se descargan inmediatamente todos los dominios de aplicación que incluyen ensamblados de usuario.  
  
>  **No se admite la ejecución de Common Language Runtime (CLR) con agrupación ligera**. Deshabilite una de las dos opciones: "clr enabled" o "lightweight pooling". Entre las características que dependen de CLR y que no funcionan correctamente en modo de fibra se encuentran el tipo de datos **hierarchyid**, la función `FORMAT`, la replicación y la administración basada en directivas.  
> 
> [!WARNING]
>  CLR usa la seguridad de acceso del código (CAS) de .NET Framework, que ya no se admite como un límite de seguridad. Un ensamblado CLR creado con la opción `PERMISSION_SET = SAFE` puede tener acceso a los recursos externos del sistema, llamar a código no administrado y adquirir privilegios sysadmin. A partir de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], se incluye una opción de `sp_configure` denominada `clr strict security` para mejorar la seguridad de los ensamblados CLR. La opción `clr strict security` está habilitada de forma predeterminada y trata los ensamblados `SAFE` y `EXTERNAL_ACCESS` como si estuvieran marcados con `UNSAFE`. La opción `clr strict security` se puede deshabilitar para permitir la compatibilidad con versiones anteriores, pero no se recomienda hacerlo. Microsoft recomienda que todos los ensamblados estén firmados con un certificado o clave asimétrica con el correspondiente inicio de sesión que tenga concedido el permiso `UNSAFE ASSEMBLY` en la base de datos maestra. Los administradores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también pueden agregar ensamblados a una lista de los ensamblados en los que el motor de base de datos debe confiar. Para más información, vea [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente muestra primero la configuración actual de la opción clr enabled y después habilita la opción estableciendo su valor en 1. Para deshabilitar la opción, establezca el valor en 0.  
  
```sql  
EXEC sp_configure 'clr enabled';  
EXEC sp_configure 'clr enabled' , '1';  
RECONFIGURE;    
```  
  
## <a name="see-also"></a>Consulte también  
 [lightweight pooling (opción de configuración del servidor)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [lightweight pooling (opción de configuración del servidor)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)  
  
  
