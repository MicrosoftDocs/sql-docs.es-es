---
title: Escribir eventos de auditoría de SQL Server en el registro de seguridad | Microsoft Docs
description: Obtenga información sobre cómo escribir eventos de auditoría de SQL Server en el registro de seguridad de Windows. Obtenga información sobre las limitaciones y restricciones de uso de ese registro.
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], Security Log
- server audit [SQL Server]
- audits [SQL Server], writing to Security Log
- security logs [SQL Server]
ms.assetid: 6fabeea3-7a42-4769-a0f3-7e04daada314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abdd25c6166b40436584a1260d4b1b09d40900c4
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186092"
---
# <a name="write-sql-server-audit-events-to-the-security-log"></a>Escribir eventos de auditoría de SQL Server en el registro de seguridad  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

En un entorno de alta seguridad, el registro de seguridad de Windows es la ubicación adecuada para escribir los eventos que registran el acceso a los objetos. Se admiten otras ubicaciones de auditoría pero están más expuestas a alteraciones.  
  
 Hay tres requisitos clave para escribir las auditorías del servidor [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el registro de seguridad de Windows:  
  
-   El valor Auditar el acceso a objetos se debe configurar para capturar los eventos. La herramienta de directiva de auditoría (`auditpol.exe`) expone diversos valores de subdirectivas en la categoría **Auditar el acceso a objetos** . Para permitir que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] audite el acceso a los objetos, configure el valor **Aplicación generada** .  
-   La cuenta en la que el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se esté ejecutando debe tener el permiso **Generar auditorías de seguridad** para escribir en el registro de seguridad de Windows. De forma predeterminada, las cuentas LOCAL SERVICE y NETWORK SERVICE tienen este permiso. No se requiere este paso si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se está ejecutando en alguna de esas cuentas.  
-   Proporcione permisos completos para la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el subárbol del registro `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security`.  

  > [!IMPORTANT]  
  > [!INCLUDE[ssnoteregistry-md](../../../includes/ssnoteregistry-md.md)]   
  
La directiva de auditoría de Windows puede afectar a la auditoría de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si se configura para escribir en el registro de seguridad de Windows, con la posibilidad de perder eventos si la directiva de auditoría se configura incorrectamente. Por lo general, el registro de seguridad de Windows se establece para sobrescribir los eventos más antiguos. De esta forma se conservan los eventos más recientes. Sin embargo, si el registro de seguridad de Windows no se establece para sobrescribir los eventos más antiguos, entonces, si el registro de seguridad se llena, el sistema emitirá el evento 1104 de Windows (el registro está lleno). En ese punto:  
-   No se registrará ningún evento de seguridad más  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no podrá detectar que el sistema no puede grabar eventos en el registro de seguridad, lo que provoca la posible pérdida de eventos de auditoría  
-   Después de que el administrador corrija el registro de seguridad, el comportamiento de registro volverá a la normalidad.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Los administradores del equipo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deberían saber que una directiva de dominio puede sobrescribir la configuración regional del registro de seguridad. En este caso, la directiva de dominio puede sobrescribir el valor de subcategoría (**auditpol /get /subcategory:"aplicación generada"** ). Esto puede afectar a la capacidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de registrar los eventos sin que haya ninguna manera de detectar que los eventos que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] está intentando auditar no van a ser grabados.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe ser administrador de Windows para configurar estos valores.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-auditpol"></a><a name="auditpolAccess"></a> Para configurar el valor Auditar el acceso a objetos en Windows utilizando auditpol  
  
1.  Abra un símbolo del sistema con permisos administrativos.  
  
    1.  En el menú **Inicio** , seleccione **Todos los programas**, **Accesorios**, haga clic con el botón derecho en **Símbolo del sistema** y, después, haga clic en **Ejecutar como administrador**.  
  
    2.  Si se abre el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Continuar**.  
  
2.  Ejecute la instrucción siguiente para habilitar la auditoría de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    ```  
    auditpol /set /subcategory:"application generated" /success:enable /failure:enable  
    ```  
  
3.  Cierre la ventana del símbolo del sistema.  
  
##  <a name="to-grant-the-generate-security-audits-permission-to-an-account-using-secpol"></a><a name="secpolAccess"></a> Para conceder el permiso Generar auditorías de seguridad a una cuenta utilizando secpol  
  
1.  En cualquier sistema operativo Windows, en el menú **Inicio** , haga clic en **Ejecutar**.  
  
2.  Escriba **secpol.msc** y, a continuación, haga clic en **Aceptar**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Continuar**.  
  
3.  En la herramienta Directiva de seguridad local, expanda **Configuración de seguridad**, expanda **Directivas locales** y, a continuación, haga clic en **Asignación de derechos de usuario**.  
  
4.  En el panel de resultados, haga doble clic en **Generar auditorías de seguridad**.  
  
5.  En la pestaña **Configuración de seguridad local** , haga clic en **Agregar usuario o grupo**.  
  
6.  En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre de la cuenta de usuario, como **domain1\user1** y, después, haga clic en **Aceptar**, o haga clic en **Opciones avanzadas** y busque la cuenta.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
8.  Cierre la herramienta Directiva de seguridad.  
  
9. Reinicie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para habilitar este valor.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-secpol"></a><a name="secpolPermission"></a> Para configurar el valor Auditar el acceso a objetos en Windows utilizando secpol  
  
1.  Si el sistema operativo es anterior a [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] o Windows Server 2008, en el menú **Inicio** , haga clic en **Ejecutar**.  
  
2.  Escriba **secpol.msc** y, a continuación, haga clic en **Aceptar**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Continuar**.  
  
3.  En la herramienta Directiva de seguridad local, expanda **Configuración de seguridad**, expanda **Directivas locales** y, a continuación, haga clic en **Directiva de auditoría**.  
  
4.  En el panel de resultados, haga doble clic en **Auditar el acceso a objetos**.  
  
5.  En la pestaña **Configuración de seguridad local** , en el área **Auditar estos intentos** , seleccione **Correcto** y **Error**.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Cierre la herramienta Directiva de seguridad.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Audit &#40;motor de base de datos&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
