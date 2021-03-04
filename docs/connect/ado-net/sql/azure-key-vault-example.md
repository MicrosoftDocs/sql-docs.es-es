---
description: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted
title: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-daenge
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 3ac1790e6e41d4024ef7d092378f6d548ed41c4f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835675"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

En este ejemplo se muestra el uso del proveedor de Azure Key Vault proveedor al obtener acceso a las columnas cifradas.

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - Para usar la característica Always Encrypted sin enclaves seguros para una aplicación .NET Standard, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior. La versión de .NET Standard admitida es 2.0 o superior. 
>
> - Para usar la característica Always Encrypted en Linux y macOS, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior.

## <a name="see-also"></a>Consulte también

- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros](azure-key-vault-enclave-example.md)
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso de Always Encrypted con el proveedor de datos Microsoft .NET para SQL Server](sqlclient-support-always-encrypted.md)
