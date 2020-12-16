---
description: 'Tutorial: Firmar procedimientos almacenados con un certificado'
title: 'Tutorial: Firmar procedimientos almacenados con un certificado'
ms.custom: seo-dt-2019
ms.date: 08/23/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- signing stored procedures tutorial [SQL Server]
ms.assetid: a4b0f23b-bdc8-425f-b0b9-e0621894f47e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 07be01325e1940c0915fc4e6b198c06631e61a8d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474596"
---
# <a name="tutorial-signing-stored-procedures-with-a-certificate"></a>Tutorial: Firmar procedimientos almacenados con un certificado
[!INCLUDE [SQL Server Azure SQL Database SQL Managed Instance](../includes/applies-to-version/sql-asdb-asdbmi.md)]
En este tutorial se describe cómo se firman los procedimientos almacenados con un certificado generado por [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Para ejecutar el código de este tutorial, la seguridad de modo mixto debe estar configurada y la base de datos AdventureWorks2017 debe estar instalada.   
  
La firma de procedimientos almacenados mediante un certificado es útil si desea exigir permisos en el procedimiento almacenado sin conceder explícitamente esos derechos al usuario. Aunque esto se puede conseguir de distintas maneras, por ejemplo, mediante la instrucción EXECUTE AS, los certificados le permiten usar un seguimiento para buscar al autor de la llamada original del procedimiento almacenado. De este modo, se consigue un alto nivel de auditoría, especialmente durante las operaciones de seguridad o de lenguaje de definición de datos (DDL).  
  
Puede crear un certificado en la base de datos maestra para permitir permisos de nivel de servidor o puede crear un certificado en cualquier base de datos de usuario para permitir permisos de nivel de base de datos. En este escenario, un usuario sin derechos en las tablas base tendrá que acceder a un procedimiento almacenado de la base de datos AdventureWorks2017 y deberá realizar una auditoría del acceso a los objetos. En lugar de usar los métodos de cadenas de propiedad, deberá crear una cuenta de servidor y de usuario de base de datos sin derechos en los objetos base, y una cuenta de usuario de base de datos con derechos en una tabla y en un procedimiento almacenado. Tanto el procedimiento almacenado como la segunda cuenta de usuario de base de datos estarán protegidos con un certificado. La segunda cuenta de la base de datos tendrá acceso a todos los objetos y podrá conceder acceso al procedimiento almacenado en la primera cuenta de usuario de la base de datos.  
  
En este escenario, en primer lugar creará un certificado de base de datos, un procedimiento almacenado y un usuario y, a continuación, comprobará el proceso mediante los pasos siguientes:  
  
Cada bloque de código incluido en este ejemplo se describe en línea. Para copiar el ejemplo completo, vea [Ejemplo completo](#CompleteExample) al final de este tutorial.  

## <a name="prerequisites"></a>Prerequisites
Para llevar a cabo este tutorial necesita tener SQL Server Management Studio, acceso a un servidor que ejecute SQL Server y una base de datos de AdventureWorks.

- Instale [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue las [bases de datos de ejemplo de AdventureWorks2017](../samples/adventureworks-install-configure.md).

Para obtener instrucciones sobre cómo restaurar una base de datos en SQL Server Management Studio, vea [Restauración de una base de datos](./backup-restore/restore-a-database-backup-using-ssms.md).   
  
## <a name="1-configure-the-environment"></a>1. Configurar el entorno  
Para establecer el contexto inicial del ejemplo, abra una consulta nueva en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] y ejecute el código que se indica a continuación para abrir la base de datos Adventureworks2017. Este código cambia el contexto de la base de datos a `AdventureWorks2012` y crea un inicio de sesión de servidor y una cuenta de usuario de base de datos nuevos (`TestCreditRatingUser`) mediante una contraseña.  
  
```sql  
USE AdventureWorks2017;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
```  
  
Para obtener más información sobre la instrucción CREATE USER, consulte [CREATE USER &#40;Transact-SQL&#41;](../t-sql/statements/create-user-transact-sql.md). Para obtener más información sobre la instrucción CREATE LOGIN, consulte [CREATE LOGIN &#40;Transact-SQL&#41;](../t-sql/statements/create-login-transact-sql.md).  
  
## <a name="2-create-a-certificate"></a>2. Crear un certificado  
Puede crear certificados en los servidores mediante la base de datos maestra como contexto y mediante una base de datos de usuario, o ambas. Existen varias opciones para proteger el certificado. Para obtener más información sobre los certificados, consulte [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../t-sql/statements/create-certificate-transact-sql.md).  
  
Ejecute este código para crear un certificado de base de datos y protéjalo mediante una contraseña.  
  
```sql  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/05/2020';  -- Error 3701 will occur if this date is not in the future
GO  
```  
  
## <a name="3-create-and-sign-a-stored-procedure-using-the-certificate"></a>3. Crear y firmar un procedimiento almacenado mediante el certificado  
Utilice el siguiente código para crear un procedimiento almacenado que seleccione los datos de la tabla `Vendor` en el esquema de la base de datos `Purchasing` y restrinja el acceso únicamente a las compañías con una solvencia de 1. Tenga en cuenta que la primera sección del procedimiento almacenado muestra el contexto de la cuenta de usuario que ejecuta el procedimiento almacenado, que solo se usa para demostrar los conceptos. No es necesario cumplir los requisitos.  
  
```sql  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Show who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1  
END  
GO  
```  
  
Ejecute este código para firmar el procedimiento almacenado con el certificado de base de datos mediante una contraseña.  
  
```sql  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
```  
  
Para obtener más información sobre los procedimientos almacenados, consulte [Procedimientos almacenados &#40;motor de base de datos&#41;](../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
Para obtener más información sobre cómo firmar los procedimientos almacenados, consulte [ADD SIGNATURE &#40;Transact-SQL&#41;](../t-sql/statements/add-signature-transact-sql.md).  
  
## <a name="4-create-a-certificate-account-using-the-certificate"></a>4. Crear una cuenta de certificado mediante el certificado  
Ejecute este código para crear un usuario de base de datos (`TestCreditRatingcertificateAccount`) a partir del certificado. Esta cuenta no tiene inicio de sesión en el servidor y en última instancia controlará el acceso a las tablas subyacentes.  
  
```sql  
USE AdventureWorks2017;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="5-grant-the-certificate-account-database-rights"></a>5. Conceder al certificado derechos de base de datos de cuentas  
Ejecute este código para conceder derechos `TestCreditRatingcertificateAccount` a la tabla base y al procedimiento almacenado.  
  
```sql  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
```  
  
Para obtener más información sobre cómo conceder permisos a los objetos, consulte [GRANT &#40;Transact-SQL&#41;](../t-sql/statements/grant-transact-sql.md).  
  
## <a name="6-display-the-access-context"></a>6. Mostrar el contexto de acceso  
Para mostrar los derechos asociados al acceso al procedimiento almacenado, ejecute el siguiente código para conceder al usuario de `TestCreditRatingUser` los derechos para ejecutar el procedimiento almacenado.  
  
```sql  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
```  
  
A continuación, ejecute el siguiente código para ejecutar el procedimiento almacenado como el inicio de sesión dbo que ha usado en el servidor. Compruebe la salida de la información de contexto del usuario. Mostrará la cuenta dbo como el contexto con sus derechos propios y no a través de una pertenencia a un grupo.  
  
```sql  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Ejecute el siguiente código para que la instrucción `EXECUTE AS` se convierta en la cuenta `TestCreditRatingUser` y ejecute el procedimiento almacenado. En esta ocasión, el contexto de usuario se establece en el contexto USER MAPPED TO CERTIFICATE. Tenga en cuenta que esta opción no se admite en una base de datos independiente, en Azure SQL Database o en Azure Synapse Analytics.
  
```sql  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Se muestra la auditoría disponible porque ha firmado el procedimiento almacenado.  
  
> [!NOTE]  
> Use EXECUTE AS para cambiar contextos dentro de una base de datos.  
  
## <a name="7-reset-the-environment"></a>7. Restablecer el entorno  
El código siguiente usa la instrucción `REVERT` para devolver el contexto de la cuenta actual a dbo y, a continuación, restablece el entorno.  
  
```sql  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
Para obtener más información sobre la instrucción REVERT, consulte [REVERT &#40;Transact-SQL&#41;](../t-sql/statements/revert-transact-sql.md).  
  
## <a name="complete-example"></a><a name="CompleteExample"></a>Ejemplo completo  
En esta sección se muestra el código de ejemplo completo.  
  
```sql  
/* Step 1 - Open the AdventureWorks2017 database */  
USE AdventureWorks2017;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
  
/* Step 2 - Create a certificate in the AdventureWorks2012 database */  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/05/2020';   -- Error 3701 will occur if this date is not in the future
GO  
  
/* Step 3 - Create a stored procedure and  
sign it using the certificate */  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Shows who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token;     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1;  
END  
GO  
  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
  
/* Step 4 - Create a database user for the certificate.   
This user has the ownership chain associated with it. */  
USE AdventureWorks2012;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
  
/* Step 5 - Grant the user database rights */  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE  
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
  
/* Step 6 - Test, using the EXECUTE AS statement */  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
  
-- Run the procedure as the dbo user, notice the output for the type  
EXEC TestCreditRatingSP;  
GO  
  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXEC TestCreditRatingSP;  
GO  
  
/* Step 7 - Clean up the example */  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
