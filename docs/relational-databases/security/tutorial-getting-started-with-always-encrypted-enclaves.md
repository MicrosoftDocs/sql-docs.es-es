---
title: 'Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server'
description: En este tutorial aprenderá a crear un entorno básico para Always Encrypted con enclaves seguros en SQL Server, mediante enclaves de Seguridad basada en virtualización (VBS) y el Servicio de protección de host (HGS) para la atestación. También aprenderá a cifrar datos en contexto y a emitir consultas confidenciales enriquecidas sobre columnas cifradas mediante SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 2334a132c8d2b56f0eaf0909b9f83544cb6e0af5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236506"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-sql-server"></a>Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

En este tutorial se explica cómo empezar a trabajar con [Always Encrypted con enclaves seguros](encryption/always-encrypted-enclaves.md) en [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]. En él encontrará:

> [!div class="checklist"]
> - Cómo crear un entorno básico para probar y evaluar Always Encrypted con enclaves seguros.
> - Cómo cifrar datos en contexto y emitir consultas confidenciales enriquecidas en columnas cifradas mediante SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Requisitos previos

Para empezar a trabajar con Always Encrypted con enclaves seguros, necesita al menos dos equipos (pueden ser máquinas virtuales):

- El equipo [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] para hospedar [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] y SSMS.
- El equipo HGS para ejecutar el servicio de protección de host, que es necesario para la atestación del enclave.

### <a name="sql-server-computer-requirements"></a>Requisitos del equipo con SQL Server

- [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] o una versión posterior
- Windows 10 Enterprise, versión 1809 o posteriores; Windows Server 2019 Datacenter Edition. Otras ediciones de Windows 10 y Windows Server no admiten la atestación con HGS.
- Compatibilidad de CPU con tecnologías de virtualización:
  - Intel VT-x con tablas de página extendida.
  - AMD-V con indexación de virtualización rápida.
  - Si va a ejecutar [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] en una máquina virtual, el hipervisor y la CPU física deben ofrecer funciones de virtualización anidadas. 
    - En Hyper-V 2016 o versiones posteriores, [habilite las extensiones de virtualización anidada en el procesador de máquina virtual](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - En Azure, seleccione un tamaño de máquina virtual que admita la virtualización anidada. Esto incluye todas las máquinas virtuales de la serie v3, por ejemplo, Dv3 y Ev3. Consulte [Creación de una máquina virtual de Azure compatible con el anidamiento](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - En VMware vSphere 6.7 o posterior, habilite la compatibilidad con la seguridad basada en virtualización en la máquina virtual, como se describe en la [documentación de VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html).
    - Es posible que otros hipervisores y nubes públicas admitan funciones de virtualización anidada que también permitan Always Encrypted con enclaves de VBS. Consulte la documentación de la solución de virtualización para obtener instrucciones relativas a la compatibilidad y configuración.
- [SQL Server Management Studio (SSMS) 18.3 o versiones posteriores](../../ssms/download-sql-server-management-studio-ssms.md)

También puede instalar SSMS en otro equipo.

> [!WARNING]
> En entornos de producción, no debe usar nunca SSMS ni otras herramientas para administrar las claves Always Encrypted, ni ejecutar consultas en datos cifrados en el equipo con SQL Server, ya que esto podría reducir o impedir la finalidad de utilizar Always Encrypted. Consulte [Consideraciones de seguridad para la administración de claves](encryption/overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management) para más información.

### <a name="hgs-computer-requirements"></a>Requisitos del equipo de HGS

- Windows Server 2019, edición Standard o Datacenter
- 2 CPU
- 8 GB de RAM
- 100 GB de almacenamiento

> [!NOTE]
> El equipo HGS no puede formar parte de un dominio antes de empezar.

## <a name="step-1-configure-the-hgs-computer"></a>Paso 1: Configurar el equipo HGS

En este paso, configurará el equipo HGS para ejecutar la atestación de la clave de host que admite el servicio de protección de host.

1. Inicie sesión en el equipo HGS como administrador (administrador local), abra una consola de Windows PowerShell con privilegios y agregue el rol del servicio de protección de host ejecutando el comando siguiente:

   ```powershell
   Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
   ```

2. Una vez reiniciado el equipo HGS, inicie sesión de nuevo con su cuenta de administrador, abra una consola de Windows PowerShell con privilegios y ejecute los comandos siguientes para instalar el servicio de protección de host y configurar su dominio. La contraseña que especifique aquí solo se aplicará a la contraseña del modo de reparación de servicios del directorio de Active Directory, pero no cambiará su contraseñas de inicio de sesión en la cuenta de administrador. Puede proporcionar el nombre de dominio que quiera para HgsDomainName.

   ```powershell
   $adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force
   Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
   ```

3. Después de que el equipo se reinicie de nuevo, inicie sesión con su cuenta de administrador (que ahora también es un administrador de dominio), abra una consola de Windows PowerShell con privilegios y configure la atestación de la clave de host para la instancia HGS. 

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey  
   ```

4. Ejecute el comando siguiente para obtener la dirección IP del equipo HGS. Guarde esta dirección IP para los pasos posteriores.

   ```powershell
   Get-NetIPAddress  
   ```

> [!NOTE]
> Como alternativa, si quiere hacer referencia a su equipo HGS con un nombre DNS, puede configurar un reenviador desde los servidores DNS corporativos para el nuevo controlador de dominio HGS.  

## <a name="step-2-configure-the-sql-server-computer-as-a-guarded-host"></a>Paso 2: Configurar el equipo con SQL Server como un host protegido
En este paso, deberá configurar el equipo con SQL Server como un host protegido registrado en HGS con una atestación de clave de host.

> [!WARNING]
> Solo se recomienda utilizar la atestación de clave de host para usarla en entornos de prueba. Debe usar la atestación de TPM para los entornos de producción.

1. Inicie sesión en el equipo de SQL Server como administrador, abra una consola de Windows PowerShell con privilegios elevados y recupere el nombre de su equipo mediante el acceso a la variable computername.

   ```powershell
   $env:computername 
   ```

2. Instale la característica de host protegido, que también instala Hyper-V (si aún no está instalada).

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
   ```

3. Reinicie el equipo con SQL Server cuando se le pida para completar la instalación de Hyper-V.

4. Si el equipo con SQL Server es una máquina virtual, una máquina física que no admite el arranque seguro de UEFI o una máquina física que no está equipada con una unidad IOMMU, tendrá que quitar el requisito de VBS de las características de seguridad de plataforma.
    1. Para quitar el requisito de arranque seguro e IOMMU, ejecute el comando siguiente en el equipo con SQL Server en una consola de PowerShell con privilegios elevados:

        ```powershell
       Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
       ```

    1. Vuelva a reiniciar el equipo con SQL Server para que VBS se ponga en línea con los requisitos reducidos.

        ```powershell
       Restart-Computer
       ```

5. Vuelva a iniciar sesión en el equipo con SQL Server como un administrador, abra una consola de Windows PowerShell con privilegios, genere una clave de host única y exporte la clave pública resultante a un archivo.

   ```powershell
   Set-HgsClientHostKey 
   Get-HgsClientHostKey -Path $HOME\Desktop\hostkey.cer
   ```

6. Copie manualmente el archivo de clave de host, generado en el paso anterior, en la máquina HGS. En las instrucciones siguientes se presupone que el nombre de archivo es hostkey.cer y que lo va a copiar en el escritorio de la máquina HGS.

7. En el equipo HGS, abra una consola de Windows PowerShell con privilegios y registre la clave de host del equipo con SQL Server con HGS:

   ```powershell
   Add-HgsAttestationHostKey -Name <your SQL Server computer name> -Path $HOME\Desktop\hostkey.cer
   ```

8. En el equipo con SQL Server, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios para indicar al equipo con SQL Server dónde debe hacer la atestación. Asegúrese de que especifica la dirección IP o el nombre de DNS del equipo HGS en ambas ubicaciones de dirección. 

   ```powershell
   # use http, and not https
   Set-HgsClientConfiguration -AttestationServerUrl http://<IP address or DNS name>/Attestation -KeyProtectionServerUrl http://<IP address or DNS name>/KeyProtection/  
   ```

El resultado del comando anterior debe mostrar que el estado de la atestación es correcto.

Si se produce un error HostUnreachable, significa que el equipo con SQL Server no puede comunicarse con HGS. Asegúrese de que se puede hacer ping al equipo HGS.

Un error UnauthorizedHost indica que la clave pública no se ha registrado con el servidor HGS. Repita los pasos 5 y 6 para resolver el error.

Si todo lo demás falla, ejecute Remove-HgsClientHostKey y repita los pasos del 4 al 7.

## <a name="step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server"></a>Paso 3: Configurar Always Encrypted con enclaves seguros en SQL Server

En este paso, deberá habilitar la funcionalidad de Always Encrypted usando enclaves en su instancia de SQL Server.

1. Con SSMS, conéctese a la instancia de SQL Server como sysadmin **sin** Always Encrypted habilitado para la conexión de base de datos.
    1. Inicie SSMS.
    1. En el cuadro de diálogo **Conectar a servidor**, especifique el nombre del servidor, seleccione un método de autenticación e indique sus credenciales.
    1. Haga clic en **Opciones >>** y seleccione la pestaña **Always Encrypted**.
    1. Asegúrese de que la casilla **Habilitar Always Encrypted (cifrado de columna)** **no** esté activada.

          ![Conexión al servidor sin Always Encrypted](./encryption/media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    1. Seleccione **Conectar**.

2. Abra una nueva ventana de consulta y ejecute la siguiente instrucción para establecer el tipo de enclave seguro en la seguridad basada en la virtualización (VBS).

   ```sql
   EXEC sys.sp_configure 'column encryption enclave type', 1;
   RECONFIGURE;
   ```

3. Reinicie su instancia de SQL Server para que se aplique el cambio anterior. Puede reiniciar la instancia en SSMS haciendo clic con el botón derecho en ella en el Explorador de objetos y seleccionando Reiniciar. Cuando se reinicie la instancia, vuelva a conectarse a ella.

4. Confirme que el enclave seguro se ha cargado ejecutando la siguiente consulta:

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type';
   ```

    La consulta debe devolver el resultado siguiente:  

    | name                           | value | value_in_use |
    | ------------------------------ | ----- | -------------- |
    | tipo de enclave de cifrado de columnas | 1     | 1              |

## <a name="step-4-create-a-sample-database"></a>Paso 4: Crear una base de datos de ejemplo
En este paso, creará una base de datos con algunos datos de ejemplo, que cifrará más adelante.

1. Con la instancia de SSMS del paso anterior, ejecute la siguiente instrucción en una ventana de consulta para crear una base de datos denominada **ContosoHR**.

    ```sql
    CREATE DATABASE [ContosoHR];
    ```

1. Cree una tabla denominada **Empleados**.

    ```sql
    USE [ContosoHR];
    GO

    CREATE SCHEMA [HR];
    GO
    
    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    ```

1. Agregue a esta tabla algunos registros de **empleados**.

    ```sql
    USE [ContosoHR];
    GO

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```

## <a name="step-5-provision-enclave-enabled-keys"></a>Paso 5: Aprovisionar claves habilitadas para el enclave

En este paso, creará una clave de columna maestra y una clave de cifrado de columna que admiten los cálculos de enclave.

1. Con la instancia de SSMS del paso anterior, en el **Explorador de objetos**, expanda la base de datos y vaya a **Seguridad** > **Claves de Always Encrypted**.
1. Aprovisione una nueva clave maestra de columna habilitada para el enclave:
    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna…** .
    2. Seleccione el nombre de clave maestra de columna: **CMK1**.
    3. Seleccione **Almacén de certificados de Windows (usuario actual o equipo local)** o **Azure Key Vault**.
    4. Seleccione **Permitir cálculos de enclave**.
    5. Si ha seleccionado Azure Key Vault, inicie sesión en Azure y seleccione su almacén de claves. Para obtener más información sobre cómo crear un almacén de claves para Always Encrypted, consulte [Administrar sus almacenes de claves desde Azure Portal](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Seleccione su certificado o su clave de Azure Key Vault si ya existe, o bien haga clic en el botón **Generar certificado** para crear uno.
    7. Seleccione **Aceptar**.

        ![Permitir cálculos de enclave](encryption/media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Cree una nueva clave de cifrado de columnas habilitada para el enclave:

    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna**.
    2. Escriba un nombre para la nueva clave de cifrado de columnas: **CEK1**.
    3. En la lista desplegable **Clave maestra de columna**, seleccione la clave maestra de columna que creó en los pasos anteriores.
    4. Seleccione **Aceptar**.

## <a name="step-6-encrypt-some-columns-in-place"></a>Paso 6: Cifrar algunas columnas en contexto

En este paso, va a cifrar los datos almacenados en las columnas **SSN** y **Salario** dentro del enclave del lado servidor y después podrá probar una consulta SELECT de los datos.

1. Abra una nueva instancia de SSMS y conéctese a la instancia de SQL Server **con** Always Encrypted habilitado para la conexión de base de datos.
    1. Inicie una nueva instancia de SSMS.
    1. En el cuadro de diálogo **Conectar a servidor**, especifique el nombre del servidor, seleccione un método de autenticación e indique sus credenciales.
    1. Haga clic en **Opciones >>** y seleccione la pestaña **Always Encrypted**.
    1. Active la casilla **Habilitar Always Encrypted (cifrado de columna)** y especifique su dirección URL de atestación de enclave (por ejemplo, ht <span>tp://</span>hgs.bastion.local/Attestation).

          ![Conexión al servidor con atestación mediante SSMS](./encryption/media/always-encrypted-enclaves/column-encryption-setting.png)

    1. Seleccione **Conectar**.
    1. Si se le pide que habilite la parametrización para las consultas Always Encrypted, haga clic en **Habilitar**.

1. Con la misma instancia de SSMS (con Always Encrypted habilitado), abra una nueva ventana de consulta y cifre las columnas **SSN** y **Salario** mediante la ejecución de las consultas siguientes.

    ```sql
    USE [ContosoHR];
    GO

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Observe la instrucción ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE para borrar la memoria caché del plan de consulta para la base de datos en el script anterior. Después de modificar la tabla, deberá borrar los planes de todos los lotes y procedimientos almacenados que tengan acceso a la tabla con el fin de actualizar la información de cifrado de los parámetros. 

1. Para comprobar que las columnas **SSN** y **Salario** ahora están cifradas, abra una nueva ventana de consulta en la instancia de SSMS **sin** Always Encrypted habilitado para la conexión de base de datos y ejecute la siguiente instrucción. La ventana de consulta debe devolver valores cifrados de las columnas **SSN** y **Salario**. Si ejecuta la misma consulta mediante la instancia SSMS con Always Encrypted habilitado, verá los datos descifrados.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-7-run-rich-queries-against-encrypted-columns"></a>Paso 7: Ejecutar consultas completas sobre columnas cifradas

Ahora puede ejecutar consultas completas sobre columnas cifradas. Se realizará algún procesamiento de consulta dentro del enclave del lado del servidor. 

1. En la instancia de SSMS **con** Always Encrypted habilitado, asegúrese de que también está habilitada la parametrización para Always Encrypted.
    1. Seleccione **Herramientas** en el menú principal de SSMS.
    2. Seleccione **Opciones...** .
    3. Vaya a **Ejecución de consulta** > **SQL Server** > **Avanzadas**.
    4. Asegúrese de que la opción **Habilitar parametrización para Always Encrypted** esté activada.
    5. Seleccione **Aceptar**.
2. Abra una nueva ventana de consulta y pegue y ejecute la siguiente consulta. La consulta debe devolver los valores de texto no cifrado y las filas que cumplan los criterios de búsqueda especificados.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Pruebe de nuevo la misma consulta en la instancia de SSMS que no tienen habilitado Always Encrypted y observe el error que se produce.

## <a name="next-steps"></a>Pasos siguientes

Después de completar este tutorial, puede ir a uno de los siguientes tutoriales:

- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)
- [Tutorial: Creación y uso de índices en columnas basadas en enclave mediante cifrado aleatorio](./tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)

## <a name="see-also"></a>Consulte también

- [Configuración y uso de Always Encrypted con enclaves seguros](encryption/configure-always-encrypted-enclaves.md)
- [Tutorial: Always Encrypted con enclaves seguros en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
