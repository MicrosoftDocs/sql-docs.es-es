---
title: Selección de un algoritmo de cifrado | Microsoft Docs
description: Use esta guía para elegir un algoritmo de cifrado que ayude a proteger una instancia de SQL Server, que admite varios algoritmos comunes.
ms.custom: ''
ms.date: 08/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: acb57d952832f5ae4d8fbfea0ee037170a854839
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236629"
---
# <a name="choose-an-encryption-algorithm"></a>Elegir un algoritmo de cifrado
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  El cifrado es una de las medidas defensivas con que cuenta cualquier administrador que desee proteger una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Los algoritmos de cifrado definen las transformaciones de datos que los usuarios no autorizados no pueden revertir fácilmente. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permite a los administradores y los desarrolladores de software elegir entre varios algoritmos, incluidos DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 de 128 bits, DESX, AES de 128 bits, AES de 192 bits y AES de 256 bits.  
  
> [!NOTE]  
>  A partir de [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], todos los algoritmos, a excepción de AES_128, AES_192 y AES_256, están en desuso. Para usar algoritmos anteriores (no se recomienda), debe establecer la base de datos en el nivel de compatibilidad de base de datos 120 o inferior.  
  
 Ningún algoritmo único resulta idóneo para todas las situaciones. Además, ofrecer información detallada sobre las ventajas de cada uno queda fuera del ámbito de los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . No obstante, se aplican los siguientes principios generales:  
  
-   El cifrado seguro suele consumir más recursos de la CPU que un cifrado menos seguro.  
  
-   Las claves largas suelen producir un cifrado más seguro que las claves cortas.  
  
-   El cifrado asimétrico es más lento que el simétrico.  
  
-   Las contraseñas largas y complejas son más seguras que las contraseñas cortas.  

-   Por lo general, el cifrado simétrico se recomienda en los casos en los que la clave solo se almacena de forma local, mientras que el asimétrico es pertinente cuando las claves deben compartirse a través de la conexión.
  
-   Si cifra una gran cantidad de datos, debe cifrar los datos con una clave simétrica y cifrar la clave simétrica con una clave asimétrica.  
  
-   Los datos cifrados no se pueden comprimir, pero los datos comprimidos se pueden cifrar. Si usa compresión, debe comprimir los datos antes de cifrarlos.  
  
> [!IMPORTANT]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
>   
>  El uso repetido de la misma RC4 o RC4_128 KEY_GUID en bloques diferentes de datos producirá la misma clave RC4 porque [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no proporciona un valor de salt automáticamente. El uso repetido de la misma clave RC4 es un error conocido que producirá un cifrado muy poco seguro. Por consiguiente, hemos dejado de utilizar las palabras clave RC4_128 y RC4. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 Para obtener más información acerca de los algoritmos y la tecnología de cifrado, vea la sección referente a [conceptos claves de seguridad](/previous-versions/aa720225(v=vs.71)) de la publicación .NET Framework Developer's Guide en MSDN.  
  
 **Clarificación con respecto a los algoritmos DES:**  
  
-   DESX se denominó incorrectamente. Las claves simétricas creadas con ALGORITHM = DESX realmente utilizan el cifrado TRIPLE DES con una clave de 192 bits. No se proporciona el algoritmo DESX. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES_3KEY utilizan TRIPLE DES con una clave de 192 bits.  
  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES utilizan TRIPLE DES con una clave de 128 bits.  
  
## <a name="related-tasks"></a>Related Tasks  
  
| Tarea | Tipo |
| ---- | ---- |
|Cifrar mediante una clave simétrica.|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Cifrar mediante una clave asimétrica.|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|Cifrar mediante un certificado.|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|Cifrar los archivos de base de datos mediante el cifrado de datos transparente.|[Cifrado de datos transparente &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|Cómo cifrar una columna de una tabla.|[Cifrar una columna de datos](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Cifrado de SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Jerarquía de cifrado](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
