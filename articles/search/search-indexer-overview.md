---
title: 'Los indizadores para rastrear orígenes de datos durante la indexación: Azure Search'
description: Rastree Azure SQL Database, Azure Cosmos DB o Azure Storage para extraer los datos utilizables en búsquedas y rellenar un índice de Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8dae593dea36944f8db037803c0dfac68cbac7c8
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384911"
---
# <a name="indexers-in-azure-search"></a>Indexadores de Azure Search

Un *indexador* de Azure Search es un rastreador (crawler) que extrae datos y metadatos utilizables en búsquedas de un origen de datos de Azure externo y rellena un índice basado en las asignaciones de un campo a otro entre el índice y su origen de datos. Este enfoque se denomina a veces "modelo de extracción", porque el servicio extrae datos sin que sea preciso escribir código que inserte datos en un índice.

Los indexadores se basan en tipos de orígenes de datos o plataformas, y hay un indexador individual para SQL Server en Azure, Cosmos DB, Azure Table Storage, Blob Storage, etc.

Puede utilizar un indexador como único medio para la ingesta de datos. o bien utilizar una combinación de técnicas que incluyan el uso de un indexador para cargar solo algunos de los campos en el índice.

Puede ejecutar los indexadores a petición o con una programación de actualización periódica de datos que se ejecuta con una frecuencia de incluso cada quince minutos. Actualizaciones más frecuentes requieren un modelo de inserción que actualiza simultáneamente los datos en Azure Search y su origen de datos externo.

## <a name="approaches-for-creating-and-managing-indexers"></a>Enfoques para crear y administrar indexadores

Puede crear y administrar indexadores mediante estos enfoques:

* [Portal &gt; Asistente para la importación de datos ](search-import-data-portal.md)
* [API de REST de servicio](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Inicialmente, un nuevo indexador se anuncia como una característica en versión preliminar. Las características en versión preliminar se introdujeron en las API (REST y. NET) y luego se integraron en el portal después de aprobarse para disponibilidad general. Si va a evaluar un nuevo indexador, debe pensar en escribir código.


<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Orígenes de datos admitidos

Los indexadores rastrean los almacenes de datos en Azure.

* [SQL de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md) 
    * Tenga en cuenta que Azure Table Storage no es compatible con [Cognitive Search](cognitive-search-concept-intro.md)


## <a name="basic-configuration-steps"></a>Pasos básicos de configuración
Los indexadores pueden ofrecer características que son exclusivas del origen de datos. En este sentido, algunos aspectos de la configuración de orígenes de datos o indexadores varían según el tipo de indexador. No obstante, todos los indexadores comparten composición básica y requisitos. Más adelante, se explican los pasos que son comunes a todos los indexadores.

### <a name="step-1-create-a-data-source"></a>Paso 1: Creación de un origen de datos
Un indexador extrae datos de un *origen de datos* que contiene información, por ejemplo, una cadena de conexión y, posiblemente, credenciales. Para crear el recurso, llame a la API de REST [Create Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source) o [clase DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource).

Los orígenes de datos se configuran y administran independientemente de los indexadores que los usan, lo que significa que varios indexadores pueden usar un origen de datos para cargar más de un índice a la vez.

### <a name="step-2-create-an-index"></a>Paso 2: Creación de un índice
Un indexador automatizará algunas tareas relacionadas con la ingesta de datos, pero la creación de un índice no suele ser una de ellas. Como requisito previo, debe tener un índice predefinido con campos que coincidan con los del origen de datos externo. Para más información sobre la estructura de un índice, consulte [Create an Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) [Creación de un índice (API de REST de Azure Search)] o [clase Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Para obtener ayuda con las asociaciones de campo, consulte [Asignaciones de campos en los indexadores de Azure Search](search-indexer-field-mappings.md).

> [!Tip]
> Aunque los indexadores no pueden generar un índice, el Asistente para **importar datos** del portal puede ayudarle. En la mayoría de los casos, el asistente puede inferir un esquema de índice de los metadatos existentes en el origen y presentar un esquema de índice preliminar que se pueda modificar en línea mientras el asistente esté activo. Una vez que el índice se crea en el servicio, la mayoría de las posteriores modificaciones se limitan a agregar nuevos campos. Tenga en cuenta al asistente para crear índices, pero no para revisarlos. Para obtener conocimientos prácticos, recorra el [tutorial del portal](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Paso 3: Creación y programación del indizador
La definición del indexador es una construcción que especifica el índice, el origen de datos y una programación. Un indexador puede hacer referencia a un origen de datos desde otro servicio, siempre que ese origen de datos pertenezca a la misma suscripción. Para más información sobre la estructura de un indexador, consulte [Crear el indizador (API de REST de Azure Search)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene el concepto básico, el paso siguiente consiste en revisar los requisitos y las tareas específicos de cada tipo de origen de datos.

* [Azure SQL Database o SQL Server en una máquina virtual de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexación de blobs CSV con el indexador de blobs de Azure Search](search-howto-index-csv-blobs.md)
* [Indexación de blobs JSON con el indexador de blobs de Azure Search](search-howto-index-json-blobs.md)
