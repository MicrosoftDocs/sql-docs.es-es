---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a483870b4cc4d7419d853c5002f06c29b9713120
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348560"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Cambia la contraseña utilizada para cifrar la clave privada de un certificado, elimina la clave privada o importa la clave privada si no hay ninguna presente. Cambia la disponibilidad de un certificado a [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *certificate_name*  
 Es el nombre exclusivo por el que se conoce el certificado en la base de datos.  
  
 REMOVE PRIVATE KEY  
 Indica que no debe seguir manteniéndose la clave privada en la base de datos.  
  
 WITH PRIVATE KEY Especifica que la clave privada del certificado se ha cargado en SQL Server.

 FILE ='*path_to_private_key*'  
 Especifica la ruta de acceso completa a la clave privada, incluido el nombre de archivo. Este parámetro puede ser una ruta de acceso local o una ruta de acceso UNC a una ubicación de red. Se obtiene acceso a este archivo dentro del contexto de seguridad de la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando utilice esta opción, asegúrese de que la cuenta de servicio tenga acceso al archivo especificado.
 
 Si solo se especifica un nombre de archivo, el archivo se guarda en la carpeta de datos de usuario predeterminada para la instancia. Esta carpeta puede (o no) ser la carpeta DATA de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para LocalDB de SQL Server Express, la carpeta de datos de usuario predeterminada para la instancia es la ruta de acceso especificada por la variable de entorno `%USERPROFILE%` para la cuenta que ha creado la instancia.  
  
 BINARY ='*private_key_bits*'  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Bits de clave privada especificados como una constante binaria. Estos bits pueden estar en forma cifrada. Si están cifrados, el usuario debe proporcionar una contraseña de descifrado. No se realizan comprobaciones de directiva de contraseña en esta contraseña. Los bits de clave privada deben tener el formato de archivo PVK.  
  
 DECRYPTION BY PASSWORD ='*current_password*'  
 Especifica la contraseña necesaria para descifrar la clave privada.  
  
 ENCRYPTION BY PASSWORD ='*new_password*'  
 Especifica la contraseña que se usa para cifrar la clave privada del certificado en la base de datos. *new_password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Password Policy](../../relational-databases/security/password-policy.md).  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Hace que el certificado esté disponible para el iniciador de una conversación de diálogo de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="remarks"></a>Comentarios  
 La clave privada debe corresponderse con la clave pública especificada por *certificate_name*.  
  
 Puede omitir la cláusula DECRYPTION BY PASSWORD si la contraseña del archivo está protegida mediante una contraseña NULL.  
  
 Cuando la clave privada de un certificado que ya existe en la base de datos se importa, esta clave privada se protegerá automáticamente mediante la clave maestra de la base de datos. Para proteger la clave privada con una contraseña, utilice la cláusula ENCRYPTION BY PASSWORD.  
  
 La opción REMOVE PRIVATE KEY eliminará de la base de datos la clave privada del certificado. Puede quitar la clave privada cuando el certificado se utilice para comprobar firmas o en escenarios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que no necesiten una clave privada. No elimine la clave privada de un certificado que proteja una clave simétrica. La clave privada debe restaurarse para firmar los módulos adicionales o cadenas que deben comprobarse con el certificado o para descifrar un valor que se haya cifrado con el certificado.   
  
 No es necesario especificar una contraseña de descifrado cuando la clave privada se ha cifrado mediante la clave maestra de la base de datos.  
 
 Para cambiar la contraseña utilizada para cifrar la clave privada, no especifique las cláusulas FILE o BINARY.
  
> [!IMPORTANT]  
>  Haga siempre una copia de archivo de una clave privada antes de quitarla de una base de datos. Para obtener más información, vea [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md) y [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md).  
  
 La opción WITH PRIVATE KEY no está disponible en una base de datos independiente.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en el certificado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>A. Eliminar la clave privada de un certificado  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. Cambiar la contraseña utilizada para cifrar una clave privada  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. Importar una clave privada para un certificado que ya existe en la base de datos  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D. Cambiar la protección de una clave privada, desde una contraseña a la clave maestra de la base de datos  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

