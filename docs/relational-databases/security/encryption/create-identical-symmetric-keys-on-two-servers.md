---
title: Creación de claves simétricas idénticas en dos servidores | Microsoft Docs
description: Obtenga información sobre cómo crear claves simétricas idénticas en dos servidores de SQL Server mediante Transact-SQL. Esto admite el cifrado en bases de datos o servidores independientes.
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 780944c299548aaa9a5664fef4688c11cdf69646
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250345"
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>Crear claves simétricas idénticas en dos servidores
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo crear claves simétricas idénticas en dos servidores diferentes de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Para descifrar el texto cifrado, se necesita la clave que se usó para cifrarlo. Cuando el cifrado y el descifrado tienen lugar en una sola base de datos, la clave se almacena en la base de datos y está disponible, según los permisos, tanto para el cifrado como para el descifrado. Pero cuando ambos procesos ocurren en bases de datos diferentes o en servidores diferentes, la clave almacenada en una base de datos no está disponible para usarla en la segunda base de datos.
  
## <a name="before-you-begin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
  
- Cuando se crea una clave simétrica, se debe cifrar mediante uno de los siguientes métodos: certificado, contraseña, clave simétrica, clave asimétrica o PROVIDER. La clave puede tener más de un cifrado de cada tipo. En otras palabras, una misma clave simétrica puede cifrarse con varios certificados, contraseñas, claves simétricas y claves asimétricas a la vez.  
  
- Si se utiliza una contraseña para cifrar una clave simétrica, en lugar de la clave pública de la clave maestra de base de datos, se utiliza el algoritmo de cifrado TRIPLE DES. Por ello, las claves creadas con un algoritmo de cifrado seguro, como AES, se protegen mediante un algoritmo menos seguro.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Necesita el permiso ALTER ANY SYMMETRIC KEY para la base de datos. Si se especifica la cláusula AUTHORIZATION, es necesario el permiso IMPERSONATE para el usuario de base de datos o el permiso ALTER para el rol de aplicación. Si el cifrado se realiza mediante una clave asimétrica o un certificado, es necesario el permiso VIEW DEFINITION para el certificado o en la clave asimétrica. Solo los inicios de sesión de Windows, los inicios de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y los roles de aplicación pueden poseer claves simétricas. Los grupos y roles no pueden poseer claves simétricas.  
  
## <a name="using-transact-sql"></a>Usar Transact-SQL  
  
### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>Para crear claves simétricas idénticas en dos servidores diferentes  
  
1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3. Cree una clave ejecutando las siguientes instrucciones, CREATE MASTER KEY, CREATE CERTIFICATE y CREATE SYMMETRIC KEY.  
  
    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4. Conéctese a una instancia de servidor independiente, abra otra ventana de consulta y ejecute las instrucciones SQL anteriores para crear la misma clave en el segundo servidor.  
  
5. Pruebe las claves ejecutando primero la instrucción OPEN SYMMETRIC KEY y la instrucción SELECT después en el primer servidor.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6. En el segundo servidor, pegue el resultado de la instrucción SELECT previa en el siguiente código como el valor de `@blob` y ejecute el siguiente código para comprobar que la clave duplicada pueda descifrar el texto cifrado.  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7. Cierre la clave simétrica en ambos servidores.  
  
    ```sql
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  

### <a name="encryption-changes-in-sql-server-2017-cu2"></a>Cambios de cifrado en SQL Server 2017 CU2

SQL Server 2016 utiliza el algoritmo hash SHA1 para su trabajo de cifrado. A partir de SQL Server 2017, se usa SHA2 en su lugar. Esto significa que tal vez se necesiten pasos adicionales para que la instalación de SQL Server 2017 descifre elementos que se cifraron en SQL Server 2016. Estos son los pasos adicionales:

- Asegúrese de que SQL Server 2017 esté actualizado al menos a la actualización acumulativa 2 (CU2).
  - Vea [Actualización acumulativa 2 (CU2) para SQL Server 2017](https://support.microsoft.com/help/4052574) para obtener detalles importantes.
- Después de instalar la CU2, active la marca de seguimiento 4631 en SQL Server 2017: `DBCC TRACEON(4631, -1);`
  - La marca de seguimiento 4631 es nueva en SQL Server 2017. La marca de seguimiento 4631 debe ser `ON` globalmente para poder crear la clave maestra, un certificado o una clave simétrica en SQL Server 2017. Esto permite que estos elementos creados interoperen con SQL Server 2016 y versiones anteriores.

Para obtener más instrucciones, vea:

- [FIX: SQL Server 2017 no puede descifrar datos cifrados con versiones anteriores de SQL Server mediante la misma clave simétrica](https://support.microsoft.com/help/4053407/sql-server-2017-cannot-decrypt-data-encrypted-by-earlier-versions)
- [Las claves simétricas idénticas no funcionan entre SQL Server 2017 y otra versión de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/33116269-identical-symmetric-keys-do-not-work-between-sql-s) <!-- Issue 2225. Thank you Stephen W and Sam Rueby. -->

## <a name="for-more-information"></a>Para obtener más información

-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
