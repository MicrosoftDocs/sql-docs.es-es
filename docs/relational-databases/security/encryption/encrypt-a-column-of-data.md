---
title: Cifrar una columna de datos | Microsoft Docs
description: Obtenga información sobre cómo cifrar una columna de datos mediante el cifrado simétrico en SQL Server con Transact-SQL, que a veces se conoce como cifrado de nivel de columna o de nivel de celda.
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: ae34b381a6f1d46329855662d0a796ea6ab3d265
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250201"
---
# <a name="encrypt-a-column-of-data"></a>Cifrar una columna de datos

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

En este artículo se describe cómo cifrar una columna de datos utilizando el cifrado simétrico en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)]. A veces, esto se conoce como cifrado de nivel de columna, o cifrado de nivel de celda. Esta característica se encuentra en versión preliminar para Azure Synapse Analytics.

Los ejemplos de este artículo se han validado con AdventureWorks2017. Para obtener bases de datos de ejemplo, vea [Bases de datos de ejemplo AdventureWorks](../../../samples/adventureworks-install-configure.md).

## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  

Los siguientes permisos son necesarios para realizar los pasos siguientes:  
  
- El permiso `CONTROL` en la base de datos.  
- El permiso `CREATE CERTIFICATE` en la base de datos. Solo los inicios de sesión de Windows, los inicios de sesión de SQL Server y los roles de aplicación pueden poseer certificados. Los grupos y roles no pueden poseer los certificados.  
- Permiso `ALTER` en la tabla.  
- Algún permiso en la clave y no debe haberse denegado el permiso `VIEW DEFINITION`.  
  
## <a name="create-database-master-key"></a>Creación de la clave maestra de base de datos  

Para usar los ejemplos siguientes, debe tener una clave maestra de base de datos. Si la base de datos aún no tiene una clave maestra de base de datos, cree una. Para crear una, conéctese a la base de datos y ejecute el script siguiente. Asegúrese de utilizar una contraseña compleja.

Copie y pegue el ejemplo siguiente en la ventana de consulta que está conectada a la base de datos de ejemplo AdventureWorks. Haga clic en **Ejecutar**.  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

Realice siempre una copia de seguridad de la clave maestra de base de datos. Para obtener más información sobre las claves maestras de base de datos, vea [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>Ejemplo: Cifrar con cifrado simétrico y autenticador
  
1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3. Copie y pegue el ejemplo siguiente en la ventana de consulta que está conectada a la base de datos de ejemplo AdventureWorks. Haga clic en **Ejecutar**.

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>Cifrado simétrico simple  

1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2. En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3. Copie y pegue el ejemplo siguiente en la ventana de consulta que está conectada a la base de datos de ejemplo AdventureWorks. Haga clic en **Ejecutar**.  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 Para obtener más información, vea lo siguiente:  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
