---
description: ADD SIGNATURE (Transact-SQL)
title: ADD SIGNATURE (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD SIGNATURE
- ADD_SIGNATURE_TSQL
helpviewer_keywords:
- ADD SIGNATURE statement
- adding digital signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 64d8b682-6ec1-4e5b-8aee-3ba11e72d21f
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.custom: ''
ms.date: 06/10/2020
ms.openlocfilehash: 86513345502531da670b870b5ecf70de9270f18f
ms.sourcegitcommit: ece104654ac14e10d32e59f45916fa944665f4df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102474909"
---
# <a name="add-signature-transact-sql"></a>ADD SIGNATURE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Agrega una firma digital a un procedimiento almacenado, una función, un ensamblado o un desencadenador de DML. También agrega una contrafirma a un procedimiento almacenado, una función, un ensamblado o un desencadenador de DML.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintaxis

```syntaxsql
ADD [ COUNTER ] SIGNATURE TO module_class::module_name
    BY <crypto_list> [ ,...n ]
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | CERTIFICATE cert_name [ WITH PASSWORD = 'password' ]
    | CERTIFICATE cert_name WITH SIGNATURE = signed_blob
    | ASYMMETRIC KEY Asym_Key_Name  
    | ASYMMETRIC KEY Asym_Key_Name [ WITH PASSWORD = 'password'.]
    | ASYMMETRIC KEY Asym_Key_Name WITH SIGNATURE = signed_blob
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*module_class*  
Es la clase del módulo al que se agrega la firma. El valor predeterminado para módulos de ámbito de esquema es OBJECT.  
  
 *module_name*  
 Es el nombre de un procedimiento almacenado, una función, un ensamblado o un desencadenador que se firmará o contrafirmará.  
  
 CERTIFICATE *cert_name*  
 Es el nombre de un certificado con el que está firmado o contrafirmado el procedimiento almacenado, la función, el ensamblado o el desencadenador.  
  
 WITH PASSWORD ='*password*'  
 Es la contraseña necesaria para descifrar la clave privada del certificado o la clave simétrica. Esta cláusula solo se requiere si la clave privada no la protege la clave maestra de la base de datos.  
  
 SIGNATURE =*signed_blob*  
 Especifica el objeto binario grande (BLOB) firmado del módulo. Esta cláusula es útil si desea enviar un módulo sin enviar la clave privada. Si utiliza esta cláusula, solo se necesitan el módulo, la firma y la clave pública para agregar el objeto binario grande firmado a una base de datos. *signed_blob* es el propio blob en formato hexadecimal.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Es el nombre de una clave asimétrica con el que está firmado o contrafirmado el procedimiento almacenado, la función, el ensamblado o el desencadenador.  
  
## <a name="remarks"></a>Observaciones

El módulo que se va a firmar o contrafirmar, y el certificado o la clave simétrica utilizada para firmarlo ya deben existir. En el cálculo de la firma se incluyen todos los caracteres del módulo. Esto incluye saltos de línea y retornos de carro iniciales.  
  
 Un módulo se puede firmar y contrafirmar por diversos certificados y claves simétricas.  
  
 La firma de un módulo se quita cuando se cambia el módulo.  
  
 Si un módulo contiene una cláusula EXECUTE AS, el Id. de seguridad (SID) de la entidad de seguridad también se incluye como parte del proceso de firma.  
  
> [!CAUTION]
> La firma de módulos solo se debe utilizar para conceder permisos, nunca para denegarlos ni revocarlos.  
  
 Los desencadenadores del lenguaje de definición de datos (DDL) y las funciones con valores de tabla insertadas no se pueden firmar.  
  
 Para obtener más información acerca de las firmas, vea la vista de catálogo sys.crypt_properties.  
  
> [!WARNING]
>  Al crear un procedimiento para la firma, todas las instrucciones del lote original deben coincidir con el lote de regeneración. Si alguna parte del lote es diferente, incluso en los espacios o los comentarios, la firma resultante será distinta.  
  
## <a name="countersignatures"></a>Contrafirmas  
 Al ejecutar un módulo firmado, las firmas se agregarán temporalmente al token de SQL, pero se pierden si el módulo ejecuta otro módulo o si termina la ejecución. Una contrafirma es una forma especial de firma. Por si sola, una contrafirma no concede ningún permiso; sin embargo, permite a las firmas realizadas por el mismo certificado o clave asimétrica conservarse mientras dure la llamada realizada al objeto contrafirmado.  
  
 Por ejemplo, suponga que la usuaria Alice llama al procedimiento ProcForAlice, que llama al procedimiento ProcSelectT1, que realiza una selección en la tabla T1. Alice dispone del permiso EXECUTE en ProcForAlice y ProcSelectT1, pero no tiene el permiso SELECT en T1 y no hay ninguna cadena de propiedad implicada en esta cadena completa. Alice no puede acceder a la tabla T1, ni directamente ni a través del uso de ProcForAlice y ProcSelectT1. Como queremos que Alice use siempre ProcForAlice para el acceso, no es aconsejable concederle permiso para ejecutar ProcSelectT1. ¿Cómo podemos lograr esto?  
  
-   Si firmamos ProcSelectT1, ProcSelectT1 puede acceder a T1, de modo que Alice puede invocar a ProcSelectT1 directamente sin tener que llamar a ProcForAlice.  
  
-   Podríamos denegar el permiso EXECUTE en ProcSelectT1 a Alice, pero entonces Alice no podría llamar a ProcSelectT1 a través de ProcForAlice.
  
-   Si solo se firma ProcForAlice, no funcionaría, porque la firma se perdería en la llamada a ProcSelectT1.  
  
Pero al contrafirmar ProcSelectT1 con el mismo certificado que se usó para firmar ProcForAlice, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantendrá la firma a través de la cadena de llamadas y permitirá el acceso a T1. Si Alice intenta llamar directamente a ProcSelectT1, no podrá acceder a T1, porque la contrafirma no concede ningún derecho. El ejemplo C siguiente muestra el código de [!INCLUDE[tsql](../../includes/tsql-md.md)] para este ejemplo.  
  
## <a name="permissions"></a>Permisos  

Requiere el permiso ALTER para el objeto y el permiso CONTROL para el certificado o la clave asimétrica. Si una clave privada asociada está protegida por una contraseña, el usuario también debe tener la contraseña.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-signing-a-stored-procedure-by-using-a-certificate"></a>A. Firmar un procedimiento almacenado mediante el uso de un certificado

 En el siguiente ejemplo se firma el procedimiento almacenado `HumanResources.uspUpdateEmployeeLogin` con el certificado `HumanResourcesDP`.  
  
```sql  
USE AdventureWorks2012;  
ADD SIGNATURE TO HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
### <a name="b-signing-a-stored-procedure-by-using-a-signed-blob"></a>B. Firmar un procedimiento almacenado mediante el uso de un BLOB firmado

En el siguiente ejemplo se crea una base de datos nueva y un certificado que se utilizará en el ejemplo. El ejemplo crea y firma un procedimiento almacenado simple y recupera la firma BLOB de `sys.crypt_properties`. La firma se quita y se agrega otra vez. En el ejemplo se firma el procedimiento utilizando la sintaxis WITH SIGNATURE.  
  
```sql  
CREATE DATABASE TestSignature ;  
GO  
USE TestSignature ;  
GO  
-- Create a CERTIFICATE to sign the procedure.  
CREATE CERTIFICATE cert_signature_demo   
    ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
    WITH SUBJECT = 'ADD SIGNATURE demo';  
GO  

-- Create a simple procedure.  
CREATE PROC [sp_signature_demo]  
AS  
    PRINT 'This is the content of the procedure.' ;  
GO  
-- Sign the procedure.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]   
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  

-- Get the signature binary BLOB for the sp_signature_demo procedure.  
SELECT cp.crypt_property  
    FROM sys.crypt_properties AS cp  
    JOIN sys.certificates AS cer  
        ON cp.thumbprint = cer.thumbprint  
    WHERE cer.name = 'cert_signature_demo' ;  
GO  
```  
  
 La firma `crypt_property` que devuelve esta instrucción será diferente cada vez que cree un procedimiento. Tome nota del resultado para su uso posteriormente en este ejemplo. Para este ejemplo, el resultado que se obtiene es: `0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373`.  
  
```sql  
-- Drop the signature so that it can be signed again.  
DROP SIGNATURE FROM [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo];  
GO  

-- Add the signature. Use the signature BLOB obtained earlier.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]  
    WITH SIGNATURE = 0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373 ;  
GO  
```  
  
### <a name="c-accessing-a-procedure-using-a-countersignature"></a>C. Acceder a un procedimiento utilizando una contrafirma

El siguiente ejemplo muestra el modo en que la contrafirma puede ayudar a controlar el acceso a un objeto.  
  
```sql  
-- Create tesT1 database  
CREATE DATABASE testDB;  
GO  
USE testDB;  
GO  
-- Create table T1  
CREATE TABLE T1 (c VARCHAR(11));  
INSERT INTO T1 VALUES ('This is T1.');  
  
-- Create a TestUser user to own table T1  
CREATE USER TestUser WITHOUT LOGIN;  
ALTER AUTHORIZATION ON T1 TO TestUser;  
  
-- Create a certificate for signing  
CREATE CERTIFICATE csSelectT  
  ENCRYPTION BY PASSWORD = 'SimplePwd01'  
  WITH SUBJECT = 'Certificate used to grant SELECT on T1';  
CREATE USER ucsSelectT1 FROM CERTIFICATE csSelectT;  
GRANT SELECT ON T1 TO ucsSelectT1;  
  
-- Create a principal with low privileges  
CREATE LOGIN Alice WITH PASSWORD = 'SimplePwd01';  
CREATE USER Alice;  
  
-- Verify Alice cannoT1 access T1;  
EXECUTE AS LOGIN = 'Alice';  
    SELECT * FROM T1;  
REVERT;  
  
-- Create a procedure that directly accesses T1  
CREATE PROCEDURE procSelectT1 AS  
BEGIN  
    PRINT 'Now selecting from T1...';  
    SELECT * FROM T1;  
END;  
GO  
GRANT EXECUTE ON ProcSelectT1 to public;  
  
-- Create special procedure for accessing T1  
CREATE PROCEDURE  ProcForAlice AS  
BEGIN  
   IF USER_ID() <> USER_ID('Alice')  
    BEGIN  
        PRINT 'Only Alice can use this.';  
        RETURN  
    END  
   EXEC ProcSelectT1;  
END;  
GO;  
GRANT EXECUTE ON ProcForAlice TO PUBLIC;  
  
-- Verify procedure works for a sysadmin user  
EXEC ProcForAlice;  
  
-- Alice still can't use the procedure yet  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
REVERT;  
  
-- Sign procedure to grant it SELECT permission  
ADD SIGNATURE TO ProcForAlice BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Counter sign ProcSelectT1, to make this work  
ADD COUNTER SIGNATURE TO ProcSelectT1 BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Now the proc works.   
-- Note that calling ProcSelectT1 directly still doesn't work  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
    EXEC ProcSelectT1;  
REVERT;  
  
-- Cleanup  
USE master;  
GO  
DROP DATABASE testDB;  
DROP LOGIN Alice;  
```  
  
## <a name="see-also"></a>Consulte también

- [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
- [DROP SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-signature-transact-sql.md)
