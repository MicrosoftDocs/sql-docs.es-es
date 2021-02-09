---
title: Identifique el Azure SQL Database SKU correcto para la base de datos local (Data Migration Assistant) | Microsoft Docs
description: Aprenda a usar Data Migration Assistant para identificar la SKU de Azure SQL Database adecuada para la base de datos local.
ms.custom: ''
ms.date: 05/06/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.openlocfilehash: 2c82514c12ec3ba496e288454d87750e0c91a5f8
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835680"
---
# <a name="identify-the-right-azure-sql-databasemanaged-instance-sku-for-your-on-premises-database"></a>Identificación de la SKU de Azure SQL Database/Instancia administrada adecuada para la base de datos local

La migración de bases de datos a la nube puede ser complicada, especialmente cuando se intenta seleccionar el mejor destino y la SKU de Azure Database para la base de datos. Nuestro objetivo con el Migration Assistant de base de datos (DMA) es ayudar a abordar estas preguntas y facilitar la migración de la base de datos proporcionando estas recomendaciones de SKU en una salida fácil de utilizar.

Este artículo se centra en la característica de recomendaciones de SKU de Azure SQL Database de DMA. Azure SQL Database y Azure SQL Instancia administrada tienen varias opciones de implementación, entre las que se incluyen:

- Base de datos única
- Grupos elásticos
- de SQL DB

La característica de recomendaciones de SKU le permite identificar el mínimo recomendado Azure SQL Database base de datos única o la SKU de Azure SQL Instancia administrada en función de los contadores de rendimiento recopilados de los equipos que hospedan las bases de datos. La característica proporciona recomendaciones relacionadas con el plan de tarifa, el nivel de proceso y el tamaño máximo de los datos, así como el costo estimado al mes. También ofrece la posibilidad de aprovisionar masivamente bases de datos únicas e instancias administradas para todas las bases de datos recomendadas.

> [!NOTE]
> Esta funcionalidad está actualmente disponible solo a través de la interfaz de la línea de comandos (CLI).

Las siguientes son instrucciones para ayudarle a determinar las recomendaciones de la SKU y aprovisionar las bases de datos únicas correspondientes o las instancias administradas de Azure mediante DMA.

## <a name="prerequisites"></a>Requisitos previos

- Descargue e instale la versión más reciente de [DMA](https://aka.ms/get-dma). Si ya tiene una versión anterior de la herramienta, ábrala y se le pedirá que actualice el DMA.
- Asegúrese de que el equipo tiene la [versión 5,1](https://www.microsoft.com/download/details.aspx?id=54616) o posterior de PowerShell, que es necesaria para ejecutar todos los scripts. Para obtener información sobre cómo averiguar qué versión de PowerShell está instalada en el equipo, consulte el artículo [Descargar e instalar Windows PowerShell 5,1](/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-5-1).
  > [!NOTE]
  > Para recopilar información de la máquina, el script de recopilación de datos usa el cmdlet Get-WmiObject, que quedó en desuso en PowerShell 6. Para ejecutar este script en PowerShell 6 o 7, debe reemplazar los cmdlets de WMI por los cmdlets de CIM más recientes.
- Asegúrese de que el equipo tiene instalado el módulo de Azure PowerShell. Para obtener más información, vea el artículo [instalar el módulo de Azure PowerShell](/powershell/azure/install-az-ps?view=azps-1.8.0&preserve-view=true).
- Compruebe que el archivo de PowerShell **SkuRecommendationDataCollectionScript.ps1**, que es necesario para recopilar los contadores de rendimiento, está instalado en la carpeta DMA.
- Asegúrese de que el equipo en el que va a realizar este proceso tenga permisos de administrador en el equipo que hospeda las bases de datos de.

## <a name="collect-performance-counters"></a>Recopilar contadores de rendimiento

El primer paso del proceso es recopilar los contadores de rendimiento de las bases de datos. Puede recopilar los contadores de rendimiento mediante la ejecución de un comando de PowerShell en el equipo que hospeda las bases de datos. DMA proporciona una copia de este archivo de PowerShell, pero también puede usar su propio método para capturar los contadores de rendimiento del equipo.

No es necesario realizar esta tarea para cada base de datos individualmente. Los contadores de rendimiento recopilados de un equipo se pueden usar para recomendar la SKU para todas las bases de datos hospedadas en el equipo.

1. En la carpeta DMA, busque el archivo de PowerShell SkuRecommendationDataCollectionScript.ps1. Este archivo es necesario para recopilar los contadores de rendimiento.

    ![Archivo de PowerShell que se muestra en la carpeta DMA](../dma/media/dma-sku-recommend-data-collection-file.png)

2. Ejecute el script de PowerShell con los siguientes argumentos:
    - **ComputerName**: el nombre del equipo que hospeda las bases de datos de.
    - **OutputFilePath**: la ruta de acceso del archivo de salida para guardar los contadores recopilados.
    - **CollectionTimeInSeconds**: la cantidad de tiempo durante la que desea recopilar datos del contador de rendimiento. Capture los contadores de rendimiento durante al menos 40 minutos para obtener una recomendación significativa. Cuanto mayor sea la duración de la captura, más precisa será la recomendación. Asegúrese también de que las cargas de trabajo se ejecutan en las bases de datos deseadas para permitir recomendaciones más precisas.
    - **DbConnectionString**: la cadena de conexión que apunta a la base de datos maestra hospedada en el equipo desde el que va a recopilar los datos del contador de rendimiento.

    Esta es una invocación de ejemplo:

    ```
    .\SkuRecommendationDataCollectionScript.ps1
     -ComputerName Foobar1
     -OutputFilePath D:\counters2.csv
     -CollectionTimeInSeconds 2400
     -DbConnectionString Server=localhost;Initial Catalog=master;Integrated Security=SSPI;
    ```

    Una vez ejecutado el comando, el proceso generará un archivo que incluye los contadores de rendimiento en la ubicación especificada. Este archivo se puede usar como entrada para la siguiente parte del proceso, lo que proporcionará recomendaciones de SKU para las opciones de base de datos única y de instancias administradas.

## <a name="use-the-dma-cli-to-get-sku-recommendations"></a>Uso de la CLI de DMA para obtener recomendaciones de SKU

Use el archivo de salida de los contadores de rendimiento que creó como entrada para este proceso.

En el caso de la opción de base de datos única, DMA proporcionará recomendaciones para el Azure SQL Database plan de tarifa de base de datos única, el nivel de proceso y el tamaño de datos máximo para cada base de datos del equipo. Si tiene varias bases de datos en el equipo, también puede especificar las bases de datos para las que desea obtener recomendaciones. DMA también le proporcionará el costo mensual estimado para cada base de datos.

En el caso de la instancia administrada, las recomendaciones admiten un escenario de elevación y desplazamiento. Como resultado, DMA le proporcionará recomendaciones para el plan de tarifa de Azure SQL Instancia administrada, el nivel de proceso y el tamaño máximo de los datos para el conjunto de bases de datos del equipo. De nuevo, si tiene varias bases de datos en el equipo, también puede especificar las bases de datos para las que desea obtener recomendaciones. DMA también le proporcionará el costo mensual estimado para la instancia administrada.

Para usar la CLI de DMA para obtener recomendaciones de SKU, en el símbolo del sistema, ejecute dmacmd.exe con los siguientes argumentos:

- **/Action = SkuRecommendation**: escriba este argumento para ejecutar evaluaciones de SKU.
- **/SkuRecommendationInputDataFilePath**: la ruta de acceso al archivo de contador recopilado en la sección anterior.
- **/SkuRecommendationTsvOutputResultsFilePath**: la ruta de acceso para escribir los resultados de salida en formato TSV.
- **/SkuRecommendationJsonOutputResultsFilePath**: la ruta de acceso para escribir los resultados de salida en formato JSON.
- **/SkuRecommendationHtmlResultsFilePath**: ruta de acceso para escribir los resultados de salida en formato HTML.

Además, seleccione uno de los argumentos siguientes:

- Impedir actualización de precios
  - **/SkuRecommendationPreventPriceRefresh**: si se establece en true, evita que se produzca la actualización del precio y asume los precios predeterminados. Use si se ejecuta en modo sin conexión. Si no usa este parámetro, debe especificar los siguientes parámetros para obtener los precios más recientes en función de una región especificada.
- Obtener los precios más recientes
  - **/SkuRecommendationCurrencyCode**: la moneda en la que se muestran los precios (por ejemplo, "USD").
  - **/SkuRecommendationOfferName**: el nombre de la oferta (por ejemplo, "MS-AZR-0003P"). Para obtener más información, consulte la página Detalles de la [oferta de Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) .
    - **/SkuRecommendationRegionName**: el nombre de la región (por ejemplo, "westus").
    - **/SkuRecommendationSubscriptionId**: el identificador de la suscripción.
    - **/AzureAuthenticationTenantId**: el inquilino de autenticación.
    - **/AzureAuthenticationClientId**: el identificador de cliente de la aplicación de AAD que se usa para la autenticación.
    - Una de las siguientes opciones de autenticación:
      - Interactive
        - **AzureAuthenticationInteractiveAuthentication**: establézcalo en true para una ventana emergente de autenticación.
      - Basado en certificados
        - **AzureAuthenticationCertificateStoreLocation**: se establece en la ubicación del almacén de certificados (por ejemplo, "CurrentUser").
        - **AzureAuthenticationCertificateThumbprint**: se establece en la huella digital del certificado.
      - Basado en token
        - **AzureAuthenticationToken**: se establece en el token de certificado.

> [!NOTE]
> Para obtener el ClientId y el TenantId para la autenticación interactiva, debe configurar una nueva aplicación de AAD. Para obtener más información sobre la autenticación y la obtención de estas credenciales, en el artículo [Microsoft Azure ejemplos de código de la API de facturación: API de RateCard](https://github.com/Azure-Samples/billing-dotnet-ratecard-api), siga las instrucciones que aparecen en **paso 1: configuración de una aplicación cliente nativa en el inquilino de AAD**.

Por último, hay un argumento opcional que puede usar para especificar las bases de datos para las que desea recomendaciones: 

- **/SkuRecommendationDatabasesToRecommend**: una lista de bases de datos para las que se van a hacer recomendaciones. Los nombres de las bases de datos distinguen mayúsculas de minúsculas y deben (1) encontrarse en Input. csv, (2) cada una de ellas entre comillas dobles y (3) separadas por un solo espacio entre los nombres (por ejemplo,/SkuRecommendationDatabasesToRecommend = "Database1" "Database2" "Database3"). Omitir este parámetro garantiza que se proporcionen recomendaciones para todas las bases de datos de usuario identificadas en el archivo Input. csv.  

A continuación se muestran algunas invocaciones de ejemplo:

**Ejemplo 1: obtener recomendaciones con precios predeterminados. Use cuando se ejecute en modo sin conexión o cuando no disponga de credenciales de autenticación.**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationPreventPriceRefresh=true
```

**Ejemplo 2: obtener recomendaciones con los precios más recientes para la región especificada (por ejemplo, "UKWest").**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationCurrencyCode=USD
/SkuRecommendationOfferName=MS-AZR-0044p
/SkuRecommendationRegionName=UKWest
/SkuRecommendationSubscriptionId=<Your Subscription Id>
/AzureAuthenticationInteractiveAuthentication=true
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId>
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

**Ejemplo 3: obtener recomendaciones para bases de datos específicas (por ejemplo, "TPCDS1G, EDW_3G, TPCDS10G").**

```
.\DmaCmd.exe /Action=SkuRecommendation 
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv" 
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv" 
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json" 
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html" 
/SkuRecommendationCurrencyCode=USD 
/SkuRecommendationOfferName=MS-AZR-0044p 
/SkuRecommendationRegionName=UKWest 
/SkuRecommendationSubscriptionId=<Your Subscription Id> 
/SkuRecommendationDatabasesToRecommend=“TPCDS1G” “EDW_3G” “TPCDS10G” 
/AzureAuthenticationInteractiveAuthentication=true 
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId> 
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

En el caso de las recomendaciones de base de datos única, el archivo de salida TSV tendrá el siguiente aspecto:

![Archivo de base de solo base de PowerShell que se muestra en la carpeta DMA](../dma/media/dma-sku-recommend-single-db-recommendations.png)

En el caso de recomendaciones de instancia administrada, el archivo de salida TSV tendrá el siguiente aspecto:

![Archivo de instancia administrada de PowerShell que se muestra en la carpeta DMA](../dma/media/dma-sku-recommend-mi-recommendations.png)

A continuación se muestra una descripción de cada columna del archivo de salida.

- **DatabaseName** : el nombre de la base de datos.
- **MetricType** : nivel de rendimiento recomendado.
- **MetricValue** : SKU recomendada.
- **PricePerMonth** : el precio estimado al mes para la SKU correspondiente.
- **RegionName** : el nombre de la región de la SKU correspondiente. 
- **IsTierRecommended** : se crea una recomendación de SKU mínima para cada nivel. A continuación, se aplica la heurística para determinar el nivel correcto de la base de datos. Esto refleja el nivel recomendado para la base de datos. 
- **ExclusionReasons** : este valor está en blanco si se recomienda un nivel. Para cada nivel que no se recomienda, se proporcionan las razones por las que no se ha seleccionado.
- **AppliedRules** : notación breve de las reglas que se aplicaron.

El nivel final recomendado (es decir, **MetricType**) y el valor (es decir, **MetricValue**): se encuentra donde la columna **IsTierRecommended** es true, refleja la SKU mínima necesaria para que las consultas se ejecuten en Azure con una tasa de éxito similar a las bases de datos locales. En el caso de Azure SQL Instancia administrada, DMA actualmente admite recomendaciones para el 8vcore que se usa con más frecuencia para 40vcore SKU. Por ejemplo, si la SKU mínima recomendada es S4 para el nivel estándar, la elección de S3 o inferior hará que las consultas agoten el tiempo de espera o no se ejecuten.

El archivo HTML contiene esta información en un formato gráfico. Proporciona un medio descriptivo para ver la recomendación final y aprovisionar la siguiente parte del proceso. En la sección siguiente se muestra más información sobre la salida HTML.

## <a name="provision-recommended-skus-to-azure"></a>Aprovisionamiento de SKU recomendados en Azure

Con tan solo unos clics, puede usar las recomendaciones identificadas para aprovisionar las SKU de destino en Azure en las que puede migrar las bases de datos. Puede usar el archivo HTML para especificar la suscripción de Azure. Elija el plan de tarifa, el nivel de proceso y el tamaño máximo de los datos de las bases de datos. y genere un script para aprovisionar las bases de datos. Puede ejecutar este script mediante PowerShell.

Puede realizar este proceso en un único equipo, o puede realizarlo en varios equipos para determinar las recomendaciones de SKU a escala. DMA lo convierte actualmente en una experiencia sencilla y escalable al admitir todo el proceso a través de la interfaz de la línea de comandos.

Para introducir información de aprovisionamiento y realizar cambios en las recomendaciones, actualice el archivo HTML como se indica a continuación.

**Para recomendaciones de base de datos única**

![Azure SQL Database pantalla de recomendaciones de SKU](../dma/media/dma-sku-recommend-single-db-recommendations1.png)

1. Abra el archivo HTML y escriba la siguiente información:
    - **Identificador de suscripción** : el identificador de suscripción de la suscripción de Azure en la que desea aprovisionar las bases de datos.
    - **Grupo de recursos** : el grupo de recursos en el que desea implementar las bases de datos. Especifique un grupo de recursos que exista.
    - **Región** : la región en la que se aprovisionan las bases de datos. Asegúrese de que la suscripción admite la región seleccionada.
    - **Nombre del servidor** : el servidor de Azure SQL Database en el que desea implementar las bases de datos. Si escribe un nombre de servidor que no existe, se creará.
    - **Nombre de usuario de administrador** : nombre de usuario del administrador del servidor. Asegúrese de que el nombre de inicio de sesión cumple los requisitos siguientes: su nombre de inicio de sesión no debe contener un identificador de SQL o un nombre de sistema típico (por ejemplo, admin, Administrator, SA, root, dbmanager, LoginManager, etc.) o un usuario o rol de base de datos integrado (por ejemplo, DBO, Guest, Public, etc.). El nombre de inicio de sesión no debe incluir caracteres no alfanuméricos (incluidos espacios en blanco, caracteres Unicode). el nombre de inicio de sesión no debe empezar por números ni símbolos

    - **Contraseña de administrador** : la contraseña de administrador del servidor. La contraseña debe tener una longitud de 8 caracteres como mínimo.
La contraseña no debe tener más de 128 caracteres de longitud.
La contraseña debe contener caracteres de tres de las siguientes categorías: letras en mayúsculas del alfabeto inglés, letras en minúscula del alfabeto inglés, números (0-9) y caracteres no alfanuméricos (!, $, #, %, etc.).
La contraseña no puede contener todo o parte del nombre de inicio de sesión. (Parte de un nombre de inicio de sesión se define como tres caracteres alfanuméricos consecutivos).

2. Revise las recomendaciones para cada base de datos y modifique el plan de tarifa, el nivel de proceso y el tamaño máximo de los datos según sea necesario. Asegúrese de anular la selección de las bases de datos que no quiera aprovisionar actualmente.

3. Seleccione **generar script de aprovisionamiento**, guarde el script y ejecútelo en PowerShell.

    Este proceso debe crear todas las bases de datos que ha seleccionado en la página HTML.

**Recomendaciones de Azure SQL Instancia administrada**

![Pantalla de recomendaciones de SKU de Azure SQL MI](../dma/media/dma-sku-recommend-mi-recommendations1.png)

1. Abra el archivo HTML y escriba la siguiente información:
    - **Identificador de suscripción** : el identificador de suscripción de la suscripción de Azure en la que desea aprovisionar las bases de datos.
    - **Grupo de recursos** : el grupo de recursos en el que desea implementar las bases de datos. Especifique un grupo de recursos que exista.
    - **Región** : la región en la que se aprovisionan las bases de datos. Asegúrese de que la suscripción admite la región seleccionada.
    - **Nombre de instancia** : la instancia de Azure SQL instancia administrada a la que desea migrar las bases de datos. El nombre de instancia solo puede contener letras minúsculas, números y '-', pero no puede comenzar ni terminar con '-' ni tener más de 63 caracteres.
    - **Nombre de usuario de administrador de instancia** : nombre de usuario de administrador de instancia. Asegúrese de que el nombre de inicio de sesión cumple los requisitos siguientes: su nombre de inicio de sesión no debe contener un identificador de SQL o un nombre de sistema típico (por ejemplo, admin, Administrator, SA, root, dbmanager, LoginManager, etc.) o un usuario o rol de base de datos integrado (por ejemplo, DBO, Guest, Public, etc.). El nombre de inicio de sesión no debe incluir caracteres no alfanuméricos (incluidos espacios en blanco, caracteres Unicode). el nombre de inicio de sesión no debe empezar por números ni símbolos

    - **Contraseña de administrador de instancia** : la contraseña de administrador de la instancia. La contraseña debe tener una longitud de 16 caracteres como mínimo.
La contraseña no debe tener más de 128 caracteres de longitud.
La contraseña debe contener caracteres de tres de las siguientes categorías: letras en mayúsculas del alfabeto inglés, letras en minúscula del alfabeto inglés, números (0-9) y caracteres no alfanuméricos (!, $, #, %, etc.).
La contraseña no puede contener todo o parte del nombre de inicio de sesión. (Parte de un nombre de inicio de sesión se define como tres caracteres alfanuméricos consecutivos).

    - **Nombre de la red virtual** : el nombre de la red virtual con la que se debe aprovisionar la instancia administrada. Escriba un nombre de red virtual existente.
    - **Nombre de subred** : el nombre de subred en el que se debe aprovisionar la instancia administrada. Escriba un nombre de subred existente.

2. Revise las recomendaciones para cada instancia y modifique el plan de tarifa, el nivel de proceso y el tamaño máximo de los datos según sea necesario. Aunque las recomendaciones están limitadas actualmente a las SKU de 8vcore a 40vcore, todavía existe la opción de aprovisionar SKU 64vcore y 80vcore si se desea. Asegúrese de anular la selección de las instancias que no desea aprovisionar actualmente.

    Este proceso debe crear todas las bases de datos que ha seleccionado en la página HTML.

    > [!NOTE]
    > La creación de instancias administradas en una subred (especialmente por primera vez) puede tardar varias horas en completarse. Después de ejecutar el script de aprovisionamiento a través de PowerShell, puede comprobar el estado de la implementación en Azure portal.

## <a name="next-step"></a>Paso siguiente

- Para obtener una lista completa de los comandos para ejecutar DMA desde la CLI, consulte el artículo [ejecución de Data Migration Assistant desde la línea de comandos](./dma-commandline.md).
