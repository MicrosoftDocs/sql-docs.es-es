---
description: sp_pdw_add_network_credentials (Azure Synapse Analytics)
title: sp_pdw_add_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: dcc54d2d6367182ab6363ef7be2e3c9de6d615eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210772"
---
# <a name="sp_pdw_add_network_credentials-azure-synapse-analytics"></a>sp_pdw_add_network_credentials (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Esto almacena las credenciales de red en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y las asocia a un servidor. Por ejemplo, utilice este procedimiento almacenado para proporcionar [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] los permisos de lectura y escritura adecuados para realizar operaciones de copia de seguridad y restauración de bases de datos en un servidor de destino, o para crear una copia de seguridad de un certificado usado para TDE.  
  
 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', 'password'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos  
 '*target_server_name*'  
 Especifica el nombre de host o la dirección IP del servidor de destino. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] tendrá acceso a este servidor mediante las credenciales de nombre de usuario y contraseña que se pasaron a este procedimiento almacenado.  
  
 Para conectarse a través de la red InfiniBand, use la dirección IP InfiniBand del servidor de destino.  
  
 *target_server_name* se define como nvarchar (337).  
  
 '*user_name*'  
 Especifica el user_name que tiene permisos de acceso al servidor de destino. Si ya existen credenciales para el servidor de destino, se actualizarán a las nuevas credenciales.  
  
 *user_name* se define como nvarchar (513).  
  
 '*password*"  
 Especifica la contraseña de *user_name*.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **ALTER Server State** .  
  
## <a name="error-handling"></a>Tratamiento de errores  
 Se produce un error si la adición de credenciales no se realiza correctamente en el nodo de control y en todos los nodos de proceso.  
  
## <a name="general-remarks"></a>Notas generales  
 Este procedimiento almacenado agrega credenciales de red a la cuenta NetworkService para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . La cuenta NetworkService ejecuta cada instancia de SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el nodo de control y los nodos de proceso. Por ejemplo, cuando se ejecuta una operación de copia de seguridad, el nodo de control y cada nodo de proceso utilizarán las credenciales de la cuenta NetworkService para obtener permiso de lectura y escritura en el servidor de destino.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>A. Agregar credenciales para realizar una copia de seguridad de base de datos  
 En el ejemplo siguiente se asocian las credenciales de nombre de usuario y contraseña para el usuario de dominio seattle\david con un servidor de destino que tiene una dirección IP de 10.172.63.255. El usuario seattle\david tiene permisos de lectura y escritura en el servidor de destino. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] almacenará estas credenciales y las usará para leer y escribir en el servidor de destino, según sea necesario para las operaciones de copia de seguridad y restauración.  
  
```sql  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 El comando backup requiere que el nombre del servidor se escriba como una dirección IP.  
  
> [!NOTE]  
>  Para realizar la copia de seguridad de base de datos a través de InfiniBand, asegúrese de usar la dirección IP InfiniBand del servidor de copia de seguridad.  
  
## <a name="see-also"></a>Consulte también  
 [sp_pdw_remove_network_credentials &#40;Azure Synapse Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

