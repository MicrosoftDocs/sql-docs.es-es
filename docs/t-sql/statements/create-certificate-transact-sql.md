---
description: CREATE CERTIFICATE (Transact-SQL)
title: CREATE CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTIFICATE
- CREATE_CERTIFICATE_TSQL
- sql13.swb.createcertificate.f1
- CERTIFICATE_TSQL
- CREATE CERTIFICATE
- sql13.swb.certificateproperties.f1
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- certificates [SQL Server], adding
- certificates [SQL Server]
- adding certificates
- cryptography [SQL Server], certificates
- CREATE CERTIFICATE statement
ms.assetid: a4274b2b-4cb0-446a-a956-1c8e6587515d
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: b670cd34ef9f0fed5fefbc788ac4774458f2c5a2
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421943"
---
# <a name="create-certificate-transact-sql"></a>CREATE CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Agrega un certificado a una base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

 Esta función no es compatible con la exportación de la base de datos con el Marco de trabajo de la aplicación de capa de datos (DACFx). Debe quitar todos los certificados antes de exportar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE CERTIFICATE certificate_name [ AUTHORIZATION user_name ]   
    { FROM <existing_keys> | <generate_new_keys> }  
    [ ACTIVE FOR BEGIN_DIALOG = { ON | OFF } ]  
  
<existing_keys> ::=   
    ASSEMBLY assembly_name  
    | {   
        [ EXECUTABLE ] FILE = 'path_to_file'  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]   
      }  
    | {   
        BINARY = asn_encoded_certificate  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]  
      }  
<generate_new_keys> ::=   
    [ ENCRYPTION BY PASSWORD = 'password' ]   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<private_key_options> ::=  
      {   
        FILE = 'path_to_private_key'  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
    |  
      {   
        BINARY = private_key_bits  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
  
<date_options> ::=  
    START_DATE = 'datetime' | EXPIRY_DATE = 'datetime'  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE CERTIFICATE certificate_name   
    { <generate_new_keys> | FROM <existing_keys> }  
    [ ; ]  
  
<generate_new_keys> ::=   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<existing_keys> ::=   
    {   
      FILE ='path_to_file'  
      WITH PRIVATE KEY   
         (   
           FILE = 'path_to_private_key'  
           , DECRYPTION BY PASSWORD ='password'   
         )  
    }  
  
<date_options> ::=  
    START_DATE ='datetime' | EXPIRY_DATE ='datetime'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *certificate_name*  
 Es el nombre del certificado de la base de datos.  
  
 AUTHORIZATION *user_name*  
 Es el nombre del usuario propietario del certificado.  
  
 ASSEMBLY *assembly_name*  
 Especifica un ensamblado firmado que se ha cargado en la base de datos.  
  
 [ EXECUTABLE ] FILE = "*ruta_al_archivo*"  
 Especifica la ruta completa, incluido el nombre de archivo, de acceso a un archivo codificado con DER que contiene el certificado. Si se usa la opción EXECUTABLE, el archivo es una DLL firmada por el certificado. *path_to_file* puede ser una ruta de acceso local o una ruta UNC a una ubicación de red. Se accede al archivo en el contexto de seguridad de la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta cuenta debe disponer de los necesarios permisos de sistema de archivos.  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no admite la creación de un certificado desde un archivo o mediante archivos de clave privada.
  
 BINARY = *asn_encoded_certificate*  
 Bytes de certificado codificados por ASN especificados como una constante binaria.  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 WITH PRIVATE KEY  
 Especifica que la clave privada del certificado se ha cargado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta cláusula no es válida si el certificado se crea desde un ensamblado. Para cargar la clave privada de un certificado creado desde un ensamblado, use [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md).  
  
 FILE ='*path_to_private_key*'  
 Especifica la ruta de acceso completa a la clave privada, incluido el nombre de archivo. *path_to_private_key* puede ser una ruta de acceso local o una ruta UNC a una ubicación de red. Se accede al archivo en el contexto de seguridad de la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta cuenta debe disponer de los necesarios permisos de sistema de archivos.  
  
> [!IMPORTANT]  
> Esta opción no está disponible en una base de dato independiente o en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 BINARY = *private_key_bits*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Bits de clave privada especificados como una constante binaria. Estos bits pueden estar en forma cifrada. Si están cifrados, el usuario debe proporcionar una contraseña de descifrado. No se realizan comprobaciones de directiva de contraseña en esta contraseña. Los bits de clave privada deben tener el formato de archivo PVK.  
  
 DECRYPTION BY PASSWORD = "*código de acceso_de_clave*"  
 Especifica la contraseña necesaria para descifrar una clave privada recuperada de un archivo. La cláusula es opcional si la clave privada está protegida por una contraseña NULL. No se recomienda guardar una clave privada de un archivo sin protección de contraseña. Si no se especifica una contraseña y es obligatorio hacerlo, se produce un error en la instrucción.  
  
 ENCRYPTION BY PASSWORD = '*password*'  
 Especifica la contraseña usada para cifrar la clave privada. Utilice esta opción solo si desea cifrar el certificado con una contraseña. Si se omite esta cláusula, la clave privada se cifra usando la clave maestra de la base de datos. *password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Password Policy](../../relational-databases/security/password-policy.md).  
  
 SUBJECT = "*nombre_del_asunto_del_certificado*"  
 El término *subject* se refiere a un campo de asunto en los metadatos del certificado, según lo establecido en el estándar X.509. El contenido del asunto no debe tener más de 64 caracteres; este límite se aplica para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Linux. Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Windows, este campo puede tener un máximo de 128 caracteres. Los asuntos que superen los 128 caracteres se truncan al almacenarlos en el catálogo, pero el objeto binario grande (BLOB) que contiene el certificado conserva el nombre de asunto completo.  
  
 START_DATE ="*fecha_y_hora*"  
 Es la fecha en la que el certificado comienza a ser válido. Si no se especifica, START_DATE coincide con la fecha actual. START_DATE se especifica en hora UTC y en cualquier formato que se pueda convertir a una fecha y hora.  
  
 EXPIRY_DATE ="*fecha_y_hora*"  
 Es la fecha en la que expira el certificado. Si no se especifica, EXPIRY_DATE es una fecha un año posterior a la indicada en START_DATE. EXPIRY_DATE se especifica en hora UTC y en cualquier formato que se pueda convertir a una fecha y hora. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker comprueba la fecha de expiración, Backup con cifrado que usa certificados también comprueba la fecha de expiración y no permitirá que una nueva copia de seguridad se cree con un certificado caducado, pero permitirá restauraciones con un certificado caducado. Sin embargo, no se exige la expiración cuando el certificado se usa para el cifrado de base de datos o para Always Encrypted.  
  
 ACTIVE FOR BEGIN_DIALOG = { **ON** | OFF }  
 Hace que el certificado esté disponible para el iniciador de una conversación de diálogo de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. El valor predeterminado es ON.  
  
## <a name="remarks"></a>Comentarios  
 Un certificado es un elemento protegible de nivel de base de datos que sigue el estándar X.509 y admite los campos V1 de X.509. `CREATE CERTIFICATE` puede cargar un certificado desde un archivo, una constante binaria o un ensamblado. Esta instrucción también puede generar un par de claves y crear un certificado con firma personal.  
  
 La clave privada debe ser \<= 2500 bytes en formato cifrado. Las claves privadas generadas por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen una longitud de 1024 bits hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y tienen 2048 bits a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Las claves privadas importadas de un origen externo presentan una longitud mínima de 384 bits y una máxima de 4.096 bits. La longitud de una clave privada importada debe ser un entero múltiplo de 64 bits. Los certificados que se usan para TDE tienen un límite de tamaño de clave privada de 3456 bits.  
  
 Se almacena el número de serie completo del certificado, pero solo se muestran los 16 primeros bytes en la vista de catálogo sys.certificates.  
  
 Se almacena todo el campo Emisor de certificado, pero solo se muestran los primeros 884 bytes en la vista de catálogo sys.certificates.  
  
 La clave privada debe corresponderse con la clave pública especificada por *certificate_name*.  
  
 Cuando se crea un certificado desde un contenedor, es opcional cargar la clave privada. Pero cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un certificado con firma personal, siempre se creará la clave privada. De manera predeterminada, la clave privada se cifra con la clave maestra de base de datos. Si no existe una clave maestra de base de datos y no se especifica una contraseña, se produce un error en la instrucción.  
  
 La opción `ENCRYPTION BY PASSWORD` no es necesaria si la clave privada está cifrada con la clave maestra de la base de datos. Use esta opción solo si se cifra la clave privada con una contraseña. Si no se especifica una contraseña, la clave privada del certificado se cifrará con la clave maestra de base de datos. Si no se puede abrir la clave maestra de la base de datos, se produce un error al abrir esta cláusula.  
  
 No es necesario especificar una contraseña de descifrado si se cifra la clave privada con la clave maestra de base de datos.  
  
> [!NOTE]  
> Las funciones integradas para el cifrado y firma no comprueban las fechas de expiración de los certificados. Los usuarios de estas funciones deben decidir cuándo comprobar la expiración de los certificados.  
  
 Se puede crear una descripción binaria de un certificado mediante las funciones [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md) y [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md). Para ver un ejemplo donde se usa **CERTPRIVATEKEY** y **CERTENCODED** para copiar un certificado en otra base de datos, vea el ejemplo B del artículo [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md).  

Los algoritmos MD2, MD4, MD5, SHA y SHA1 están en desuso en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], un certificado autofirmado se creaba mediante SHA1. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], un certificado autofirmado se crea mediante SHA2_256.

## <a name="permissions"></a>Permisos  
 Debe tener el permiso `CREATE CERTIFICATE` para la base de datos. Solo los inicios de sesión de Windows, los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los roles de aplicación pueden poseer certificados. Los grupos y roles no pueden poseer los certificados.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-self-signed-certificate"></a>A. Crear un certificado autofirmado  
 En el siguiente ejemplo se crea un certificado denominado `Shipping04`. La clave privada de este certificado está protegida con una contraseña.  
  
```sql  
CREATE CERTIFICATE Shipping04   
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
   WITH SUBJECT = 'Sammamish Shipping Records',   
   EXPIRY_DATE = '20201031';  
GO  
```  
  
### <a name="b-creating-a-certificate-from-a-file"></a>B. Crear un certificado desde un archivo  
 En el ejemplo siguiente se crea un certificado en la base de datos y se carga el par de claves desde los archivos.  
  
```sql  
CREATE CERTIFICATE Shipping11   
    FROM FILE = 'c:\Shipping\Certs\Shipping11.cer'   
    WITH PRIVATE KEY (FILE = 'c:\Shipping\Certs\Shipping11.pvk',   
    DECRYPTION BY PASSWORD = 'sldkflk34et6gs%53#v00');  
GO   
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no admite la creación de un certificado desde un archivo.
   
### <a name="c-creating-a-certificate-from-a-signed-executable-file"></a>C. Crear un certificado desde un archivo ejecutable firmado  
  
```sql  
CREATE CERTIFICATE Shipping19   
    FROM EXECUTABLE FILE = 'c:\Shipping\Certs\Shipping19.dll';  
GO  
```  
  
 Como alternativa, puede crear un ensamblado desde el archivo `dll` y crear un certificado desde el ensamblado.  
  
```sql  
CREATE ASSEMBLY Shipping19   
    FROM ' c:\Shipping\Certs\Shipping19.dll'   
    WITH PERMISSION_SET = SAFE;  
GO  
CREATE CERTIFICATE Shipping19 FROM ASSEMBLY Shipping19;  
GO  
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no admite la creación de un certificado desde un archivo.

> [!IMPORTANT]
> A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la opción de configuración del servidor ["CLR strict security"](../../database-engine/configure-windows/clr-strict-security.md) evita la carga de ensamblados sin configurar primero su seguridad. Cargue el certificado, cree un inicio de sesión a partir del mismo, conceda `UNSAFE ASSEMBLY` a ese inicio de sesión y, a continuación, cargue el ensamblado.

### <a name="d-creating-a-self-signed-certificate"></a>D. Crear un certificado autofirmado  
 En este ejemplo se crea un certificado denominado `Shipping04` sin especificar una contraseña de cifrado. Este ejemplo se puede usar con [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
```sql  
CREATE CERTIFICATE Shipping04   
   WITH SUBJECT = 'Sammamish Shipping Records';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)   
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


