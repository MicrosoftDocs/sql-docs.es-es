---
title: Certificados y claves asimétricas de SQL Server | Microsoft Docs
description: Obtenga información sobre los certificados y las claves asimétricas en SQL Server, incluidos los certificados, las herramientas y las tareas relacionadas que se hayan generado externamente o mediante SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server], certificates and asymmetric keys
ms.assetid: 8519aa2f-f09c-4c1c-96b5-abc24811e60c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb81d6e334cfe5911635640f8c0e9d9cf2abfcad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340146"
---
# <a name="sql-server-certificates-and-asymmetric-keys"></a>Certificados y claves asimétricas de SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 La criptografía de clave pública es un método para mantener la confidencialidad de los mensajes mediante el cual el usuario crea una clave *pública* y una clave *privada*. La clave privada se mantiene en secreto, mientras que la clave pública se puede distribuir a otras personas. Aunque existe una relación matemática entre las claves, no resulta sencillo deducir la clave privada partiendo de la clave pública. La clave pública se puede usar para cifrar los datos que solamente la clave privada correspondiente podrá descifrar. Esto puede usarse para cifrar los mensajes al propietario de la clave privada. Del mismo modo, el propietario de una clave privada puede cifrar los datos que solo pueden descifrarse con la clave pública. Este uso constituye la base de los certificados digitales en los que se cifra la información contenida en el certificado por parte del propietario de una clave privada, garantizando así el autor del contenido. Dado que las claves de cifrado y descifrado son diferentes, se conocen como claves *asimétricas*.
  
 Tanto los certificados como las claves asimétricas son métodos para utilizar el cifrado asimétrico. Los certificados se suelen emplear como contenedores para las claves asimétricas porque pueden contener más información, como las fechas de expiración y los emisores. No hay ninguna diferencia entre los dos mecanismos en cuanto al algoritmo criptográfico y tampoco hay diferencia en cuanto al nivel de cifrado si no varía la longitud de la clave. Generalmente, se utiliza un certificado para cifrar otros tipos de claves de cifrado en una base de datos o para firmar módulos de código.  
  
 Los certificados y las claves asimétricas pueden descifrar los datos cifrados por los demás. El cifrado asimétrico se suele utilizar para cifrar una clave simétrica para su almacenamiento en una base de datos.  
  
 Una clave pública no tiene un formato determinado como ocurre con un certificado, y no puede exportarse a un archivo.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene características y herramientas que le permiten crear y administrar certificados y claves para usar con el servidor y la base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede usar para crear y administrar certificados y claves con otras aplicaciones o en el sistema operativo.  
  
## <a name="certificates"></a>Certificados  
 Un certificado es un objeto de seguridad firmado digitalmente que contiene una clave pública (y opcionalmente una privada) para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pueden utilizarse certificados generados externamente o generados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Los certificados cumplen con la norma de certificados IETF X.509v3.  
  
 Los certificados son útiles debido a que ofrecen la opción de exportar e importar claves a archivos de certificado X.509. La sintaxis para crear certificados ofrece opciones de creación para los certificados, como establecer una fecha de expiración.  
  
### <a name="using-a-certificate-in-sql-server"></a>Utilizar un certificado en SQL Server  
 Los certificados se pueden utilizar para proteger las conexiones, en la creación de reflejo de la base de datos, para firmar paquetes y otros objetos, o para cifrar datos o conexiones. En la tabla siguiente se muestran recursos adicionales para los certificados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|Explica el comando para crear certificados.|  
|[Identificar el origen de paquetes con firmas digitales](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md)|Muestra información sobre cómo utilizar los certificados para firmar paquetes de software.|  
|[Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|Contiene información sobre cómo utilizar los certificados con la creación de reflejo de la base de datos.|  
  
## <a name="asymmetric-keys"></a>Claves asimétricas  
 Las claves asimétricas se utilizan para proteger las claves simétricas. También se pueden utilizar para el cifrado de datos limitado y para firmar digitalmente objetos de base de datos. Una clave asimétrica se compone de una clave privada y su correspondiente clave pública. Para obtener más información sobre las claves asimétricas, vea [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md).  
  
 Las claves asimétricas se pueden importar de archivos de clave de nombre seguro, pero no se pueden exportar. Tampoco tienen opciones de expiración. Las claves asimétricas no pueden cifrar las conexiones.  
  
### <a name="using-an-asymmetric-key-in-sql-server"></a>Utilizar una clave asimétrica en SQL Server  
 Las claves asimétricas se pueden utilizar para proteger datos o para firmar texto simple. En la tabla siguiente se muestran recursos adicionales para las claves asimétricas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)|Explica el comando para crear claves asimétricas.|  
|[SIGNBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/signbyasymkey-transact-sql.md)|Muestra las opciones para firmar objetos.|  
  
## <a name="tools"></a>Herramientas  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] proporciona herramientas y utilidades para generar certificados y archivos de clave de nombre seguro. Estas herramientas proporcionan una mayor flexibilidad en el proceso de generación de claves que la sintaxis de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede utilizar estas herramientas para crear claves RSA con longitudes de clave más complejas y, a continuación, importarlas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En la tabla siguiente se muestra dónde se encuentran estas herramientas.  
  
| Herramienta | Propósito |
| ---- | ------- |
|[New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)|Crea certificados autofirmados.|  
|[makecert](/windows/desktop/SecCrypto/makecert)|Crea certificados. En desuso en favor de **New-SelfSignedCertificate**.|  
|[sn](/dotnet/framework/tools/sn-exe-strong-name-tool)|Crea nombres seguros para claves simétricas.|  
  
## <a name="related-tasks"></a>Related Tasks  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)   
 [Cifrado de datos transparente &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
  
