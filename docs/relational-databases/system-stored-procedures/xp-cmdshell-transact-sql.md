---
description: xp_cmdshell (Transact-SQL)
title: xp_cmdshell (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_cmdshell
- xp_cmdshell_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: 18935cf4-b320-4954-b6c1-e007fcefe358
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73bea06c7919c40b1080458f3e6982d2c10ebe47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99125006"
---
# <a name="xp_cmdshell-transact-sql"></a>xp_cmdshell (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Genera un shell de comandos de Windows y lo pasa a una cadena para ejecutarlo. Los resultados se devuelven como filas de texto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
xp_cmdshell { 'command_string' } [ , no_output ]  
```  
  
## <a name="arguments"></a>Argumentos  
 **'** *command_string* **'**  
 Es la cadena que contiene un comando que se pasa al sistema operativo. *command_string* es **VARCHAR (8000)** o **nvarchar (4000)** y no tiene ningún valor predeterminado. *command_string* no puede contener más de un conjunto de comillas dobles. Se requiere un solo par de comillas si hay espacios en las rutas de acceso de archivo o en los nombres de programa a los que se hace referencia en *command_string*. Si tiene problemas con espacios incrustados, considere el uso de nombres de archivo de tipo FAT 8.3 como solución.  
  
 **no_output**  
 Es un parámetro opcional que especifica que no se devuelven resultados al cliente.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 La ejecución de la instrucción `xp_cmdshell` devuelve una lista de directorios del directorio actual.  
  
```  
EXEC xp_cmdshell 'dir *.exe';  
GO  
```  
  
 Las filas se devuelven en una columna **nvarchar (255)** . Si se utiliza la opción **no_output** , solo se devolverá lo siguiente:  
  
```  
The command(s) completed successfully.  
```  
  
## <a name="remarks"></a>Observaciones  
 El proceso de Windows generado por **xp_cmdshell** tiene los mismos derechos de seguridad que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio.  
 
> [!IMPORTANT]
>  **xp_cmdshell** es una característica muy eficaz y está deshabilitada de forma predeterminada. **xp_cmdshell** se pueden habilitar y deshabilitar mediante la administración basada en directivas o ejecutando **sp_configure**. Para obtener más información, vea [configuración de área expuesta](../../relational-databases/security/surface-area-configuration.md) y [Xp_cmdshell opción de configuración del servidor](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 **xp_cmdshell** funciona de forma sincrónica. No se devolverá el control al llamador hasta que el comando del shell de comandos esté completo. 
 
> [!IMPORTANT]
>  Si **xp_cmdshell** se ejecuta dentro de un lote y devuelve un error, se producirá un error en el lote. Es un cambio de comportamiento. En versiones anteriores del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lote continuaría ejecutándose.  
 
## <a name="xp_cmdshell-proxy-account"></a>Cuenta de proxy xp_cmdshell  
 Cuando lo llama un usuario que no es miembro del rol fijo de servidor **sysadmin** , **xp_cmdshell** se conecta a Windows mediante el nombre de cuenta y la contraseña almacenados en la credencial denominada **# #xp_cmdshell_proxy_account # #**. Si esta credencial de proxy no existe, se producirá un error en **xp_cmdshell** .  
  
 La credencial de la cuenta de proxy se puede crear ejecutando **sp_xp_cmdshell_proxy_account**. Como argumentos, este procedimiento almacenado utiliza un nombre de usuario y una contraseña de Windows. Por ejemplo, el siguiente comando crea una credencial de proxy para el usuario de dominio de Windows `SHIPPING\KobeR` que tiene la contraseña de Windows `sdfh%dkc93vcMt0`.  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'SHIPPING\KobeR','sdfh%dkc93vcMt0';  
```  
  
 Para obtener más información, vea [sp_xp_cmdshell_proxy_account &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Dado que a veces los usuarios malintencionados intentan elevar sus privilegios mediante **xp_cmdshell**, **xp_cmdshell** está deshabilitado de forma predeterminada. Use **sp_configure** o la **Administración basada en directivas** para habilitarla. Para más información, vea [xp_cmdshell (opción de configuración del servidor)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 Cuando se habilita por primera vez, **xp_cmdshell** requiere el permiso Control Server para ejecutarse y el proceso de Windows creado por **xp_cmdshell** tiene el mismo contexto de seguridad que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]A menudo, la cuenta de servicio tiene más permisos de los necesarios para el trabajo realizado por el proceso creado por **xp_cmdshell**. Para mejorar la seguridad, el acceso a **xp_cmdshell** debe estar restringido a los usuarios con privilegios elevados.  
  
 Para permitir que los usuarios que no son administradores puedan usar **xp_cmdshell** y permitir [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que cree procesos secundarios con el token de seguridad de una cuenta con menos privilegios, siga estos pasos:  
  
1.  Cree y personalice una cuenta de usuario local de Windows o una cuenta de dominio con los permisos mínimos que los procesos requieran.  
  
2.  Utilice el procedimiento del sistema **sp_xp_cmdshell_proxy_account** para configurar **xp_cmdshell** para que use esa cuenta con privilegios mínimos.  
  
    > [!NOTE]  
    >  También puede configurar esta cuenta de proxy mediante haciendo clic [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] con el botón secundario en **propiedades** en el nombre del servidor en explorador de objetos y en la pestaña **seguridad** de la sección **cuenta de proxy del servidor** .  
  
3.  En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , mediante la base de datos maestra, ejecute la `GRANT exec ON xp_cmdshell TO N'<some_user>';` instrucción para proporcionar a los usuarios que no son **administradores** de bases de datos específicos la capacidad de ejecutar **xp_cmdshell**. El usuario especificado debe existir en la base de datos maestra.  
  
 Ahora, los usuarios que no son administradores pueden iniciar procesos del sistema operativo con **xp_cmdshell** y esos procesos se ejecutan con los permisos de la cuenta de proxy que ha configurado. Los usuarios con el permiso CONTROL SERVER (miembros del rol fijo de servidor **sysadmin** ) seguirán recibiendo los permisos de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio para los procesos secundarios iniciados por **xp_cmdshell**.  
  
 Para determinar la cuenta de Windows utilizada por **xp_cmdshell** al iniciar procesos del sistema operativo, ejecute la siguiente instrucción:  
  
```  
xp_cmdshell 'whoami.exe'  
  
```  
  
 Para determinar el contexto de seguridad de otro inicio de sesión, ejecute lo siguiente:  
  
```  
EXECUTE AS LOGIN = '<other_login>' ;  
GO  
xp_cmdshell 'whoami.exe' ;  
REVERT ;  
  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-a-list-of-executable-files"></a>A. Devolver una lista de archivos ejecutables  
 En el siguiente ejemplo se muestra el procedimiento almacenado extendido `xp_cmdshell` ejecutando un comando de directorio.  
  
```  
EXEC master..xp_cmdshell 'dir *.exe'  
```  
  
### <a name="b-returning-no-output"></a>B. No devolver resultados  
 En el siguiente ejemplo se utiliza `xp_cmdshell` para ejecutar una cadena de comandos sin devolver los resultados al cliente.  
  
```  
USE master;  
  
EXEC xp_cmdshell 'copy c:\SQLbcks\AdvWorks.bck  
    \\server2\backups\SQLbcks', NO_OUTPUT;  
GO  
```  
  
### <a name="c-using-return-status"></a>C. Utilizar el estado de retorno  
 En el ejemplo siguiente, el `xp_cmdshell` procedimiento almacenado extendido también sugiere el estado de devolución. El valor del código de retorno se almacena en la variable `@result`.  
  
```  
DECLARE @result int;  
EXEC @result = xp_cmdshell 'dir *.exe';  
IF (@result = 0)  
   PRINT 'Success'  
ELSE  
   PRINT 'Failure';  
```  
  
### <a name="d-writing-variable-contents-to-a-file"></a>D. Escribir el contenido de variables en un archivo  
 En el siguiente ejemplo se escribe el contenido de la variable `@var` en un archivo denominado `var_out.txt` en el directorio actual del servidor.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'Hello world';  
SET @cmd = 'echo ' + @var + ' > var_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
### <a name="e-capturing-the-result-of-a-command-to-a-file"></a>E. Capturar el resultado de un comando en un archivo  
 En el siguiente ejemplo se escribe el contenido del directorio actual en un archivo denominado `dir_out.txt` en el directorio actual del servidor.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'dir/p';  
SET @cmd = @var + ' > dir_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados extendidos generales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_cmdshell (opción de configuración del servidor)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)   
 [Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)   
 [sp_xp_cmdshell_proxy_account &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)  
  
  
