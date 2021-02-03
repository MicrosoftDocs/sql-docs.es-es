---
title: Realizar una copia de seguridad de la clave maestra de servicio | Microsoft Docs
description: Obtenga información sobre cómo hacer una copia de seguridad de una clave maestra de servicio en SQL Server mediante Transact-SQL. La clave maestra de servicio es la raíz de la jerarquía de cifrado.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- service master key [SQL Server], exporting
ms.assetid: f60b917c-6408-48be-b911-f93b05796904
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7c95feda5457c3623a893e77771dbdc10ea28257
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250695"
---
# <a name="back-up-the-service-master-key"></a>Hacer una copia de seguridad de la clave maestra de servicio
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este artículo se describe cómo realizar una copia de seguridad de la clave maestra de servicio en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La clave maestra de servicio es la raíz de la jerarquía de cifrado. Debe realizar una copia de seguridad de la clave maestra de servicio y almacenarla en una ubicación segura y fuera de las instalaciones. Crear esta copia de seguridad debe ser una de las primeras acciones de administración que se realicen en el servidor.  

## <a name="before-you-begin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  

- Es necesario abrir la clave maestra y, por tanto, descifrarla antes de realizar una copia de seguridad de la misma. Si está cifrada con la clave maestra de servicio, la clave maestra no tiene que abrirse explícitamente; sin embargo, si la clave maestra está cifrada únicamente con una contraseña, debe abrirse explícitamente.  
  
- Se recomienda realizar una copia de seguridad de la clave maestra inmediatamente después de crearla y guardarla en un lugar seguro y alejado de las instalaciones.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos
Necesita el permiso CONTROL en la base de datos.  
  
## <a name="using-transact-sql"></a>Usar Transact-SQL  
  
### <a name="to-back-up-the-service-master-key"></a>Para hacer una copia de seguridad de la clave maestra de servicio
  
1. En [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], conéctese a la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que contenga la clave maestra de servicio de la que desea hacer una copia de seguridad.  
  
2. Elija una contraseña que se utilizará para cifrar la clave maestra de servicio en el medio de la copia de seguridad. Esta contraseña se somete a comprobaciones de complejidad. Para obtener más información, vea [Password Policy](../../../relational-databases/security/password-policy.md).  
  
3. Hágase con un medio de copia de seguridad extraíble para guardar una copia de seguridad de la clave.  
  
4. Identifique un directorio NTFS en el que va a crear la copia de seguridad de la clave. En este directorio se creará el archivo indicado en el paso siguiente. El directorio debe estar protegido mediante listas de control de acceso (ACL) muy restrictivas.  
  
5. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
6. En la barra de Estándar, haga clic en **Nueva consulta**.  
  
7. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql
    -- Creates a backup of the service master key.
    USE master;
    GO
    BACKUP SERVICE MASTER KEY TO FILE = 'c:\temp_backups\keys\service_master_ key'
        ENCRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';
    GO
    ```  
  
    > [!NOTE]  
    > La ruta de acceso de archivo a la clave y la contraseña de la clave (si existe) serán distintas de las que se indica más arriba. Asegúrese de que ambas son específicas para la instalación del servidor y de la clave.
  
8. Copie el archivo en el medio de copia de seguridad y compruebe que la copia es correcta.  
  
9. Guarde la copia de seguridad en una ubicación segura fuera de las instalaciones.  

 Para obtener más información, vea [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-master-key-transact-sql.md) y [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-master-key-transact-sql.md).  
