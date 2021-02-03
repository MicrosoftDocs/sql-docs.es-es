---
description: '&#x40;&#x40;OPTIONS (Transact-SQL)'
title: '@@OPTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@OPTIONS'
- '@@OPTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, current SET options
- '@@OPTIONS function'
- current SET options
ms.assetid: 3d5c7f6e-157b-4231-bbb4-4645a11078b3
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 43da748085a31a9d9f25ef20bdb690f2b94e4cb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183939"
---
# <a name="x40x40options-transact-sql"></a>&#x40;&#x40;OPTIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de las opciones SET actuales.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
@@OPTIONS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **integer**  
  
## <a name="remarks"></a>Comentarios  
 Las opciones pueden proceder del empleo del comando **SET** o del valor **sp_configure user options**. Los valores para la sesión que se hayan configurado con el comando **SET** invalidan las opciones de **sp_configure**. Muchas herramientas, como [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] configuran opciones automáticamente. Cada usuario tiene una función @@OPTIONS que representa la configuración.  
  
 Con la instrucción SET puede cambiar el idioma y las opciones de procesamiento de consultas para una sesión de usuario específica. **\@\@OPTIONS** solo puede detectar las opciones establecidas en ON o en OFF.  
  
 La función **\@\@OPTIONS** devuelve un mapa de bits de las opciones, convertido a un entero en base 10 (decimal). La configuración de bits se almacena en las ubicaciones descritas en una tabla en el tema [Establecer la opción de configuración del servidor Opciones de usuario](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md).  
  
 Para descodificar el valor **\@\@OPTIONS**, convierta el entero que ha devuelto **\@\@OPTIONS** a binario y, después, busque los valores en la tabla del tema [Establecimiento de la opción de configuración del servidor Opciones de usuario](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md). Por ejemplo, si `SELECT @@OPTIONS;` devuelve el valor `5496`, use la calculadora del programador de Windows (**calc.exe**) para convertir el valor decimal `5496` a binario. El resultado es `1010101111000`. Los caracteres en el extremo derecho (binario 1, 2 y 4) son 0, lo cual indica que los tres primeros elementos de la tabla están establecidos en OFF. Al consultar la tabla, ve que esos elementos son **DISABLE_DEF_CNST_CHK**, **IMPLICIT_TRANSACTIONS** y **CURSOR_CLOSE_ON_COMMIT**. El siguiente elemento (**ANSI_WARNINGS** en la posición `1000`) está establecido en ON. Siga examinando hacia la izquierda en el mapa de bits y trabaje en dirección descendente en la lista de opciones. Cuando las opciones en el extremo izquierdo sean 0, se indica que están truncadas por el tipo de conversión. El mapa de bits `1010101111000` es en realidad `001010101111000` para poder representar la totalidad de las 15 opciones.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-demonstration-of-how-changes-affect-behavior"></a>A. Demostración de cómo los cambios repercuten en el comportamiento  
 En el siguiente ejemplo se muestran las diferencias en el comportamiento de concatenación con dos configuraciones distintas de la opción **CONCAT_NULL_YIELDS_NULL**.  
  
```sql  
SELECT @@OPTIONS AS OriginalOptionsValue;  
SET CONCAT_NULL_YIELDS_NULL OFF;  
SELECT 'abc' + NULL AS ResultWhen_OFF, @@OPTIONS AS OptionsValueWhen_OFF;  
  
SET CONCAT_NULL_YIELDS_NULL ON;  
SELECT 'abc' + NULL AS ResultWhen_ON, @@OPTIONS AS OptionsValueWhen_ON;  
```  
  
### <a name="b-testing-a-client-nocount-setting"></a>B. Probar una configuración NOCOUNT de cliente  
 En el siguiente ejemplo se especifica `NOCOUNT``ON` y, después, se prueba el valor de `@@OPTIONS`. La opción `NOCOUNT``ON` impide que se envíe al cliente que hace la solicitud el mensaje sobre el número de filas afectadas por cada instrucción de una sesión. El valor de `@@OPTIONS` se establece en `512` (0x0200). Esto representa la opción NOCOUNT. En este ejemplo se prueba si la opción NOCOUNT está habilitada en el cliente. Por ejemplo, puede ayudar a hacer un seguimiento de las diferencias de rendimiento de un cliente.  
  
```sql  
SET NOCOUNT ON  
IF @@OPTIONS & 512 > 0   
RAISERROR ('Current user has SET NOCOUNT turned on.', 1, 1)  
```  
  
## <a name="see-also"></a>Vea también  
 [Funciones de configuración &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Establecer la opción de configuración del servidor Opciones de usuario](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)  
  
  
