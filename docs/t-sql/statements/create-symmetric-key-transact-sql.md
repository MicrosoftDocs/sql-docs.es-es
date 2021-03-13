---
description: CREATE SYMMETRIC KEY (Transact-SQL)
title: CREATE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE SYMMETRIC KEY
- SYMMETRIC KEP
- CREATE_SYMMETRIC_KEY_TSQL
- SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SYMMETRIC KEY statement
- temporary symmetric keys [SQL Server]
- symmetric keys [SQL Server], creating
- symmetric keys [SQL Server]
ms.assetid: b5d23572-b79d-4cf1-9eef-d648fa3b1358
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 1bdfcae4b6d7ef8461cb5ddec4bb1133a92d6d93
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421928"
---
# <a name="create-symmetric-key-transact-sql"></a>CREATE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Genera una clave simétrica y especifica sus propiedades en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Esta función no es compatible con la exportación de la base de datos con el Marco de trabajo de la aplicación de capa de datos (DACFx). Debe quitar todas las claves simétricas antes de exportar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)] 
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SYMMETRIC KEY key_name   
    [ AUTHORIZATION owner_name ]  
    [ FROM PROVIDER provider_name ]  
    WITH 
      [
          <key_options> [ , ... n ]  
        | ENCRYPTION BY <encrypting_mechanism> [ , ... n ] 
      ]
  
<key_options> ::=  
      KEY_SOURCE = 'pass_phrase'  
    | ALGORITHM = <algorithm>  
    | IDENTITY_VALUE = 'identity_phrase'  
    | PROVIDER_KEY_NAME = 'key_name_in_provider'   
    | CREATION_DISPOSITION = {CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
    DES | TRIPLE_DES | TRIPLE_DES_3KEY | RC2 | RC4 | RC4_128  
    | DESX | AES_128 | AES_192 | AES_256   
  
<encrypting_mechanism> ::=  
      CERTIFICATE certificate_name   
    | PASSWORD = 'password'   
    | SYMMETRIC KEY symmetric_key_name   
    | ASYMMETRIC KEY asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Key_name*  
 Especifica el nombre único por el que se conoce la clave simétrica en la base de datos. Las claves temporales se designan cuando _key_name_ comienza por el signo de número (#). Por ejemplo, **#temporaryKey900007**. No se puede crear una clave simétrica que presente un nombre que comienza por más de un signo de número. No puede crear ninguna clave simétrica temporal mediante un proveedor EKM.  
  
 AUTHORIZATION *owner_name*  
 Especifica el nombre del usuario de la base de datos o el rol de aplicación que será propietario de la clave.  
  
 FROM PROVIDER *provider_name*  
 Especifica un nombre y proveedor de Administración extensible de claves (EKM). La clave no se exporta desde el dispositivo de EKM. El proveedor debe definirse antes mediante la instrucción CREATE PROVIDER. Para más información sobre la creación de proveedores de claves externas, vea [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 KEY_SOURCE **='** _pass\_phrase_ **'**  
 Especifica una frase de contraseña a partir de la cual se derivará la clave.  
  
 IDENTITY_VALUE **='** _identity\_phrase_ **'**  
 Especifica una frase de identidad a partir de la cual se generará una GUID para etiquetar los datos cifrados con una clave temporal.  
  
 PROVIDER_KEY_NAME **='** _key\_name\_in\_provider_ **'**  
 Especifica el nombre al que se hace referencia en el proveedor de Administración extensible de claves.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 CREATION_DISPOSITION **=** CREATE_NEW  
 Crea una nueva clave en el dispositivo de Administración extensible de claves.  Si ya existe una clave en el dispositivo, se producirá un error en la instrucción.  
  
 CREATION_DISPOSITION **=** OPEN_EXISTING  
 Asigna una clave simétrica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una clave de Administración extensible de claves. Si no se proporciona CREATION_DISPOSITION = OPEN_EXISTING, de forma predeterminada es CREATE_NEW.  
  
 *certificate_name*  
 Especifica el nombre del certificado que se utilizará para cifrar la clave simétrica. El certificado debe existir en la base de datos.  
  
 **'** *password* **'**  
 Especifica una contraseña de la que se deriva una clave TRIPLE_DES con la que se va a proteger la clave simétrica. *password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use siempre contraseñas seguras.  
  
 *symmetric_key_name*  
 Especifica una clave simétrica que se usará para cifrar la clave que se va a crear. La clave especificada debe existir en la base de datos y debe estar abierta.  
  
 *asym_key_name*  
 Especifica una clave asimétrica que se usará para cifrar la clave que se va a crear. La clave asimétrica debe existir en la base de datos.  
  
 \<algorithm>  
Especifica el algoritmo de cifrado.   
> [!WARNING]  
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], todos los algoritmos, a excepción de AES_128, AES_192 y AES_256, están en desuso. Para usar algoritmos anteriores (no se recomienda), debe establecer la base de datos en el nivel de compatibilidad de base de datos 120 o inferior.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se crea una clave simétrica, se debe cifrar mediante uno de los siguientes métodos: certificado, contraseña, clave simétrica, clave asimétrica o PROVIDER. La clave puede tener más de un cifrado de cada tipo. En otras palabras, una misma clave simétrica puede cifrarse con varios certificados, contraseñas, claves simétricas y claves asimétricas a la vez.  
  
> [!CAUTION]  
>  Si se usa una contraseña para cifrar una clave simétrica, en lugar de usar un certificado (u otra clave), se usa el algoritmo de cifrado TRIPLE DES para cifrar la contraseña. Por ello, las claves creadas con un algoritmo de cifrado seguro, como AES, se protegen mediante un algoritmo menos seguro.  
  
 Puede utilizar la contraseña opcional para cifrar la clave simétrica antes de distribuir la clave a varios usuarios.  
  
 Las claves temporales son propiedad del usuario que las creó. Las claves temporales son solo válidas para la sesión actual.  
  
 IDENTITY_VALUE genera una GUID con la que se etiquetan los datos cifrados con la nueva clave simétrica. Este etiquetado puede utilizarse para asignar claves a datos cifrados. El GUID generado por una frase específica siempre es el mismo. Si se utiliza una frase para generar un GUID, esta no se puede volver a utilizar mientras haya al menos una sesión que la esté usando activamente. IDENTITY_VALUE es una cláusula opcional; sin embargo, le recomendamos que la utilice para almacenar los datos cifrados con una clave temporal.  
  
 No existe un algoritmo de cifrado predeterminado.  
  
> [!IMPORTANT]  
>  No se recomienda usar los cifrados de flujos RC4 y RC4_128 para proteger información confidencial. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no realiza ninguna codificación adicional al cifrado efectuado con estas claves.  
  
 Puede ver información sobre claves simétricas en la vista de catálogo [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md).  
  
 Las claves simétricas creadas a partir del proveedor de cifrado no pueden cifrar las claves simétricas.  
  
 **Clarificación con respecto a los algoritmos DES:**  
  
-   DESX se denominó incorrectamente. Las claves simétricas creadas con ALGORITHM = DESX realmente utilizan el cifrado TRIPLE DES con una clave de 192 bits. No se proporciona el algoritmo DESX. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES_3KEY utilizan TRIPLE DES con una clave de 192 bits.  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES utilizan TRIPLE DES con una clave de 128 bits.  
  
 **Degradación del algoritmo RC4:**  
  
 El uso repetido de la misma RC4 o RC4_128 KEY_GUID en distintos bloques de datos producirá la misma clave RC4 porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no proporciona un valor de sal automáticamente. El uso repetido de la misma clave RC4 es un error conocido que producirá un cifrado muy poco seguro. Por tanto, las palabras clave RC4_128 y RC4 están desusadas. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!WARNING]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso ALTER ANY SYMMETRIC KEY para la base de datos. Si se especifica la cláusula AUTHORIZATION, es necesario el permiso IMPERSONATE para el usuario de base de datos o el permiso ALTER para el rol de aplicación. Si el cifrado se realiza mediante una clave asimétrica o un certificado, es necesario el permiso VIEW DEFINITION para el certificado o en la clave asimétrica. Solo los inicios de sesión de Windows, los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los roles de aplicación pueden poseer claves simétricas. Los grupos y roles no pueden poseer claves simétricas.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-symmetric-key"></a>A. Crear una clave simétrica  
 En el siguiente ejemplo se crea una clave simétrica denominada `JanainaKey09` mediante el algoritmo `AES 256` y se cifra la nueva clave con el certificado `Shipping04`.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey09   
WITH ALGORITHM = AES_256  
ENCRYPTION BY CERTIFICATE Shipping04;  
GO  
```  
  
### <a name="b-creating-a-temporary-symmetric-key"></a>B. Crear una clave simétrica temporal  
 En el siguiente ejemplo se crea una clave simétrica temporal denominada `#MarketingXXV` a partir de la frase de contraseña: `The square of the hypotenuse is equal to the sum of the squares of the sides`. La clave se suministra con un GUID generado a partir de la cadena `Pythagoras` y se cifra con el certificado `Marketing25`.  
  
```sql 
CREATE SYMMETRIC KEY #MarketingXXV   
WITH ALGORITHM = AES_128,  
KEY_SOURCE   
     = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
IDENTITY_VALUE = 'Pythagoras'  
ENCRYPTION BY CERTIFICATE Marketing25;  
GO  
```  
  
### <a name="c-creating-a-symmetric-key-using-an-extensible-key-management-ekm-device"></a>C. Crear una clave simétrica mediante un dispositivo de Administración extensible de claves (EKM)  
 El ejemplo siguiente crea una clave simétrica llamada `MySymKey` mediante un proveedor llamado `MyEKMProvider` y un nombre de clave de `KeyForSensitiveData`. Asigna la autorización a `User1` y se supone que el administrador del sistema ya ha registrado el proveedor llamado `MyEKMProvider` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
CREATE SYMMETRIC KEY MySymKey  
AUTHORIZATION User1  
FROM PROVIDER EKMProvider  
WITH  
PROVIDER_KEY_NAME='KeyForSensitiveData',  
CREATION_DISPOSITION=OPEN_EXISTING;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Administración extensible de claves con el Almacén de claves de Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
