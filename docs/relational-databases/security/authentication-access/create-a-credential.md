---
title: Crear una credencial | Microsoft Docs
description: Obtenga información sobre cómo crear una credencial en SQL Server mediante SQL Server Management Studio o Transact-SQL. Averigüe cómo trabajar dentro de las limitaciones y restricciones.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- credentials [SQL Server], creating
- authentication [SQL Server], credentials
- logins [SQL Server], credentials
ms.assetid: c1e77e91-2a69-40d9-b8b3-97cffc710586
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 98fddc3b614cd53ad88d761365243bb81a3eca10
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250322"
---
# <a name="create-a-credential"></a>Create a Credential
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear una credencial en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Las credenciales proporcionan un método para permitir que los usuarios de la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dispongan de una identidad fuera de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se utilizan principalmente para ejecutar código en ensamblados con el conjunto de permisos EXTERNAL_ACCESS. También se pueden utilizar cuando un usuario de la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] necesita obtener acceso a un recurso de dominio, como una ubicación de archivo para almacenar una copia de seguridad.  
  
 Una credencial se puede asignar a varios inicios de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a la vez. Un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] solo se puede asignar a una credencial a la vez. Después de crear la credencial, use **Propiedades de inicio de sesión (página General)** para asignar un inicio de sesión a una credencial.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear una credencial, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si no hay ninguna credencial de inicio de sesión asignada para el proveedor, se utiliza la credencial asignada a la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Un inicio de sesión puede tener asignadas varias credenciales, siempre y cuando se utilicen con proveedores distintos. Solo debe haber una credencial asignada por cada proveedor y por cada inicio de sesión. La misma credencial puede estar asignada a otros inicios de sesión.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER ANY CREDENTIAL para crear o modificar una credencial y el permiso ALTER ANY LOGIN para asignar un inicio de sesión a una credencial.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-credential"></a>Para crear una credencial  
  
1.  En el Explorador de objetos, expanda la carpeta **Seguridad** .  
  
2.  Haga clic con el botón derecho en la carpeta **Credenciales** y seleccione **Nueva credencial...** .  
  
3.  En el cuadro de diálogo **Nueva credencial** , en el cuadro **Nombre de credencial** , escriba un nombre para la credencial.  
  
4.  En el cuadro **Identidad** , escriba el nombre de la cuenta empleada en las conexiones salientes (cuando salga del contexto de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). Normalmente, será una cuenta de usuario de Windows, pero la identidad puede ser una cuenta de otro tipo.  
  
     También puede hacer clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar usuarios o grupos**.  
  
5.  En los cuadros **Contraseña** y **Confirmar contraseña** , escriba la contraseña de la cuenta especificada en el cuadro **Identidad** . Si se ha especificado una cuenta de usuario de Windows en **Identidad** , ésta será la contraseña de Windows. Se puede dejar **Contraseña** en blanco si no se requiere ninguna.  
  
6.  Seleccione **Usar proveedor de cifrado** para establecer la credencial que debe ser comprobada por un proveedor de Administración extensible de claves (EKM). Para obtener más información, vea [Administración extensible de claves &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-credential"></a>Para crear una credencial  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Creates the credential called "AlterEgo.".   
    -- The credential contains the Windows user "Mary5" and a password.  
    CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',   
        SECRET = '<EnterStrongPasswordHere>';  
    GO  
    ```  
  
 Para obtener más información, vea [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
  
