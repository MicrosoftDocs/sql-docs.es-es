---
title: Almacén remoto de blobs (RBS) (SQL Server) | Microsoft Docs
description: El almacén remoto de blobs (RBS) de SQL Server almacena objetos binarios grandes en un almacenamiento de mercancía en lugar de en el servidor de base de datos principal. Obtenga información sobre este componente complementario.
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 89a79b07977d8fec2e2dd6f2a8b9acd9cea0ba5e
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100343162"
---
# <a name="remote-blob-store-rbs-sql-server"></a>Remote Blob Store (RBS) (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  El almacén remoto de blobs RBS de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un componente complementario opcional que permite a los administradores de bases de datos almacenar directamente objetos binarios grandes en soluciones de almacenamiento de artículos en lugar de en el servidor de base de datos principal.  
  
 RBS se incluye en el disco de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pero no lo instala el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Busque RBS.msi en el disco de instalación para localizar el archivo de instalación.

 Si no tiene el soporte de instalación [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede descargar RBS en una de las ubicaciones siguientes:

| Versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | Ubicación de descarga de RBS |
|:---|:---|
| [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]Feature Pack SP2](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] | [[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] Página de descarga de RBS](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>¿Por qué elegir RBS?  
  
### <a name="optimized-database-storage-and-performance"></a>Rendimiento y almacenamiento de base de datos optimizados  
 Si se almacenan los blobs en la base de datos, puede usarse una gran cantidad de espacio en los archivos y caros recursos del servidor. RBS transfiere los blobs a la solución de almacenamiento especializada que prefiera y almacena las referencias a estos en la base de datos. Esto libera almacenamiento en el servidor para los datos estructurados y también recursos del servidor para las operaciones de base de datos.  
  
### <a name="efficient-blob-management"></a>Administración eficaz de blobs  
 Varias características de RBS permiten la administración de blobs almacenados:  
  
-   Los blobs se administran con transacciones ACID (atomicidad, coherencia, aislamiento, durabilidad).  
  
-   Los blobs se organizan en colecciones.  
  
-   Se incluyen la recolección de elementos no utilizados, la comprobación de la coherencia y otras funciones de mantenimiento.  
  
### <a name="standardized-api"></a>API normalizada  
 RBS define un conjunto de API que proporcionan un modelo de programación normalizado para que las aplicaciones obtengan acceso y modifiquen almacenes de blobs. Cada almacén de blobs puede especificar su propia biblioteca de proveedores, que se conecta a la biblioteca cliente de RBS y especifica cómo se almacenan los blobs y cómo se obtiene acceso a ellos.  
  
 Varios proveedores de soluciones de almacenamiento han desarrollado proveedores RBS que se ajustan a estas API estándar y son compatibles con el almacenamiento de blobs en varias plataformas de almacenamiento.  
  
## <a name="rbs-requirements"></a>Requisitos de RBS  
 - RBS requiere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise para el servidor de base de datos principal en el que se almacenan los metadatos de los blobs.  Sin embargo, si usa el proveedor FILESTREAM suministrado, puede almacenar los propios blobs en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard. Para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], RBS requiere al menos la versión 11 del controlador de ODBC para [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] y la versión 13 del controlador ODBC para [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. Los controladores están disponibles en [Descarga del controlador ODBC para SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).    
  
 RBS incluye un proveedor FILESTREAM que permite usar RBS para almacenar los blobs en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si desea usar RBS para almacenar los blobs en una solución de almacenamiento diferente, tiene que usar un proveedor RBS de terceros desarrollado para dicha solución de almacenamiento o desarrollar un proveedor RBS personalizado con la API RBS. En [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190)hay disponible como recurso de aprendizaje un ejemplo de proveedor que almacena los blobs en el sistema de archivos NTFS.  
  
## <a name="rbs-security"></a>Seguridad de RBS  
 El blog del equipo del almacenamiento remoto de blobs de SQL es una estupenda fuente de información sobre esta característica. El modelo de seguridad de RBS se describe en la entrada [RBS Security Model](/archive/blogs/sqlrbs/rbs-security-model)(Modelo de seguridad de RBS).  
  
### <a name="custom-providers"></a>Proveedores personalizados  
 Cuando use un proveedor personalizado para almacenar blobs fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], asegúrese de que protege los blobs almacenados con opciones de cifrado y permisos apropiados para el medio de almacenamiento que use el proveedor personalizado.  
  
### <a name="credential-store-symmetric-key"></a>Clave simétrica de almacén de credenciales  
 Si un proveedor requiere que se instale y use un secreto almacenado en el almacén de credenciales, RBS usa una clave simétrica para cifrar los secretos de proveedor que un cliente puede usar para obtener autorización en el almacén de blobs del proveedor.  
  
-   RBS 2016 usa una clave simétrica **AES_128** . [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no permite crear nuevas claves **TRIPLE_DE** S, salvo por motivos de compatibilidad con versiones anteriores. Para obtener más información, vea [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
-   RBS 2014 y las versiones anteriores usan un almacén de credenciales que contiene secretos cifrados con el algoritmo de clave simétrica **TRIPLE_DES** , actualmente obsoleto. Si a día de hoy usa **TRIPLE_DES**, [!INCLUDE[msCoName](../../includes/msconame-md.md)] le recomienda mejorar la seguridad con los pasos de este tema para rotar la clave a un método de cifrado más seguro.  
  
 Puede determinar las propiedades de la clave simétrica de almacén de credenciales si ejecuta la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en la base de datos RBS:   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` Si el resultado de esa instrucción muestra que **TRIPLE_DES** todavía se usa, debería rotar esta clave.  
  
### <a name="rotating-the-symmetric-key"></a>Rotación de la clave simétrica  
 Si usa RBS, conviene rotar la clave simétrica del almacén de credenciales cada cierto tiempo. Se trata de un procedimiento de seguridad recomendado muy habitual para cumplir las directivas de seguridad de una organización.  Una manera de rotar la clave simétrica del almacén de credenciales de RBS consiste en usar [este script](#Key_rotation) en la base de datos de RBS.  Este script también sirve para migrar a propiedades de intensidad de cifrado más seguras, como la longitud de clave o algoritmo. Haga una copia de seguridad de la base de datos antes de realizar la rotación.  Cuando el script finalice, es necesario realizar algunos pasos de comprobación.  
Si las directivas de seguridad requieren propiedades de clave (por ejemplo, longitud de clave o algoritmo) diferentes de las provistas, el script se puede usar como plantilla. Modifique las propiedades de clave en dos sitios: 1) en la creación de la clave temporal y 2) en la creación de la clave permanente.  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> Recursos de RBS  
  
 **Ejemplos de RBS**  
 Los ejemplos de RBS disponibles en [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190) demuestran cómo desarrollar una aplicación de RBS y cómo desarrollar e instalar un proveedor de RBS personalizado.  
  
 **Blog de RBS**  
 En el [blog de RBS](/archive/blogs/sqlrbs/) se proporciona información adicional para ayudarle a entender, implementar y mantener RBS.  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> Script de rotación de clave  
 En este ejemplo se crea un procedimiento almacenado denominado `sp_rotate_rbs_symmetric_credential_key` para reemplazar la clave simétrica de almacén de credenciales de RBS usada actualmente  
por una de su elección.  Es posible que quiera hacerlo si hay una directiva de seguridad que exige   
la rotación de claves periódica o si hay requisitos de algoritmo concretos.  
 En este procedimiento almacenado se sustituirá la clave actual por una clave simétrica que usa **AES_256** .  Como resultado de  
la sustitución de la clave simétrica, es necesario volver a cifrar los secretos con la nueva clave.  Este procedimiento almacenado   
también volverá a cifrar los secretos.  Antes de rotar la clave, haga una copia de seguridad de la base de datos.  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 Ya puede usar el procedimiento almacenado `sp_rotate_rbs_symmetric_credential_key` para rotar la clave simétrica del almacén de credenciales de RBS. Los secretos seguirán siendo los mismos antes y después de la rotación de clave.  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>Consulte también  
[Almacén remoto de blobs y grupos de disponibilidad AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
