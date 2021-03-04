---
description: 'Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros'
title: 'Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros | Microsoft Docs'
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: v-daenge
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 735d0b552843a95ea388b91e2ed1da3fcfec2790
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837022"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

En este tutorial aprenderá a desarrollar una aplicación que emite consultas de base de datos que usan un enclave seguro del lado servidor para [Always Encrypted con enclaves seguros](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted con enclaves seguros solo se admite en Windows.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que ha completado uno de los tutoriales mostrados a continuación antes de seguir los pasos siguientes de este tutorial:

- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Además, se necesita Visual Studio (se recomienda la versión 2019); puede descargarlo desde [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). El entorno de desarrollo de la aplicación debe usar .NET Framework 4.6 o posterior o .NET Core 2.1 o posterior.

## <a name="step-1-set-up-your-visual-studio-project"></a>Paso 1: Configuración de su proyecto de Visual Studio

Para usar Always Encrypted con enclaves seguros en una aplicación .NET Framework, debe asegurarse de que la aplicación tiene como destino .NET Framework 4.6 o superior. Para usar Always Encrypted con enclaves seguros en una aplicación .NET Core, debe asegurarse de que la aplicación tiene como destino .NET Core 2.1 o superior.

Además, si almacena su clave maestra de columna en Azure Key Vault, también debe integrar su aplicación con el [NuGet Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Abra Visual Studio.

2. Cree un proyecto de aplicación de consola de C\# (.NET Framework/Core).

3. Asegúrese de que su proyecto tenga como destino al menos .NET Framework 4.6 o .NET Core 2.1. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones, seleccione **Propiedades** y establezca el marco de destino.

4. Instale el siguiente paquete NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Si usa Azure Key Vault para almacenar sus claves maestras de columna, instale los siguientes paquetes NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="step-2-implement-your-application-logic"></a>Paso 2: Implementar la lógica de la aplicación

La aplicación se conectará a la base de datos **ContosoHR** del [Tutorial: Introducción a Always Encrypted con enclaves seguros con SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) o [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) y ejecutará una consulta que contiene el predicado `LIKE` en la columna **SSN** y una comparación de rangos en la columna **Salary**.

1. Reemplace el contenido del archivo Program.cs (generado por Visual Studio) por el siguiente código. 

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                // Connection string for SQL Server
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                // Connection string for Azure SQL Database
                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = AAS; Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Actualice la cadena de conexión de base de datos.
    1. Establezca el nombre de servidor válido y la configuración de autenticación de base de datos.
    2. Establezca el valor de la palabra clave `Attestation Protocol` en:
       - `HGS` si utiliza [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS).
       - `AAS` si utiliza [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation.
    3. Establezca `Enclave Attestation URL` en una dirección URL de atestación para su entorno.

3. Compile y ejecute la aplicación.

## <a name="see-also"></a>Consulte también

- [Uso de Always Encrypted con el proveedor de datos Microsoft .NET para SQL Server](sqlclient-support-always-encrypted.md)
- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros](azure-key-vault-enclave-example.md)
