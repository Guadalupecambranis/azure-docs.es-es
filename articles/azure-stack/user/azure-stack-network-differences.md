---
title: Diferencias y consideraciones de redes de Azure Stack | Microsoft Docs
description: Aprenda sobre las diferencias y consideraciones al trabajar con redes en Azure Stack.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.date: 01/25/2019
ms.topic: article
ms.service: azure-stack
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: f0e3eca0c38a47e7107e52464e889580dd0b1b8a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243579"
---
# <a name="considerations-for-azure-stack-networking"></a>Consideraciones para las redes de Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Las redes de Azure Stack tienen muchas de las características proporcionadas por las redes de Azure. Sin embargo, hay algunas diferencias clave que debe conocer antes de implementar una red de Azure Stack.

En este artículo se proporciona información general sobre las consideraciones únicas para las redes de Azure Stack y sus características. Para obtener información acerca de las diferencias de alto nivel entre Azure y Azure Stack, consulte el artículo [Key considerations](azure-stack-considerations.md) (Consideraciones clave).

## <a name="cheat-sheet-networking-differences"></a>Hoja de referencia rápida: diferencias de redes

| Servicio | Característica | Azure (global) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | DNS multiinquilino | Compatible | Todavía no se admite |
|  | Registros AAAA de DNS | Compatible | No compatible |
|  | Zonas DNS por suscripción | 100 (valor predeterminado)<br>Puede aumentarse a petición. | 100 |
|  | Conjuntos de registros DNS por zona | 5000 (valor predeterminado)<br>Puede aumentarse a petición. | 5000 |
|  | Servidores de nombres para la delegación de zona | Azure proporciona cuatro servidores de nombres para cada zona de usuario (inquilino) que se crea. | Azure Stack proporciona dos servidores de nombres para cada zona de usuario (inquilino) que se crea. |
| Virtual Network | Emparejamiento de redes virtuales de Azure | Conecte dos redes virtuales de la misma región mediante la red troncal de Azure. | Todavía no se admite |
|  | Direcciones IPv6 | Puede asignar una dirección IPv6 como parte de la [configuración de la interfaz de red](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions). | Se admite solo IPv4. |
|  | Plan de DDoS Protection | Compatible | Todavía no se admite. |
|  | Configuraciones de direcciones IP del conjunto de escalado | Compatible | Todavía no se admite. |
|  | Servicios de acceso privado (subred) | Compatible | Todavía no se admite. |
|  | Puntos de conexión de servicio | Admitido para la conexión interna (sin conexión a Internet) a los servicios de Azure. | Todavía no se admite. |
| Se admite solo IPv4. | Directivas de puntos de conexión de servicio | Compatible | Todavía no se admite. |
|  | Túneles de servicio | Compatible | Todavía no se admite.  |
| Grupos de seguridad de red | Reglas de seguridad aumentada | Compatible | Todavía no se admite. |
|  | Reglas de seguridad eficaz | Compatible | Todavía no se admite. |
|  | Grupos de seguridad de la aplicación | Compatible | Todavía no se admite. |
| Puertas de enlace de red virtual | Puerta de enlace de VPN de punto a sitio | Compatible | Todavía no se admite. |
|  | Puerta de enlace de VNET a VNET | Compatible | Todavía no se admite. |
|  | Tipo de puerta de enlace de red virtual | Azure admite VPN<br> ExpressRoute <br> Red de Hyper | Azure Stack admite solo el tipo VPN en este momento. |
|  | SKU de puerta de enlace de VPN | Compatibilidad con SKU básicas, GW1, GW2, GW3, alto rendimiento estándar, ultra alto rendimiento. | Compatibilidad con SKU básicas, estándar y de alto rendimiento. |
|  | Tipo VPN | Azure admite tanto la basada en directivas como la basada en rutas. | Azure Stack admite solo la basada en rutas. |
|  | Configuración de BGP | Azure admite la configuración de dirección de emparejamiento BGP y de peso del mismo nivel. | La dirección de emparejamiento BGP y el peso del mismo nivel se configuran automáticamente en Azure Stack. No hay ninguna manera de que el usuario configure estas opciones con sus propios valores. |
|  | Sitio de puerta de enlace predeterminado | Azure admite la configuración de un sitio predeterminado para la tunelización forzada. | Todavía no se admite. |
|  | Cambio del tamaño de la puerta de enlace | Azure permite cambiar el tamaño de la puerta de enlace después de la implementación. | No se permite cambiar el tamaño. |
|  | Configuración activa/activa | Compatible | Todavía no se admite. |
|  | UsePolicyBasedTrafficSelectors | Azure admite el uso de selectores de tráfico en función de directivas con las conexiones de puerta de enlace basadas en rutas. | Todavía no se admite. |
| Equilibrador de carga | SKU | Se admiten los equilibradores de carga Estándar y Básico. | Solo se admite el equilibrador de carga Básico.  La propiedad SKU no se admite. |
|  | Zones | Availability Zones se admite. | Todavía no se admite |
|  | Las reglas NAT de entrada admiten los puntos de conexión de servicio | Azure permite especificar puntos de conexión de servicio para las reglas NAT de entrada. | Azure Stack no admite aún puntos de conexión de servicio, por lo que estos no se pueden especificar. |
|  | Protocolo | Azure permite especificar GRE o ESP. | En Azure Stack no se admite la clase de protocolo. |
| Dirección IP pública | Version de dirección IP pública | Azure admite tanto IPv6 como IPv4 | Se admite solo IPv4. |
| Interfaz de red | Obtención de tabla de rutas efectivas | Compatible | Todavía no se admite. |
|  | Obtención de ACL efectivas | Compatible | Todavía no se admite. |
|  | Habilitar Accelerated Networking | Compatible | Todavía no se admite. |
|  | reenvío de IP | Deshabilitado de forma predeterminada.  Se puede habilitar. | No se puede alternar esta opción.  Está activada de forma predeterminada. |
|  | Grupos de seguridad de la aplicación | Compatible | Todavía no se admite. |
|  | Etiqueta de nombre DNS interno | Compatible | Todavía no se admite. |
|  | Versión de dirección IP privada | Se admiten tanto IPv6 como IPv4. | Se admite solo IPv4. |
| Network Watcher | Funciones de supervisión de red de inquilinos de Network Watcher | Compatible | Todavía no se admite. |
| CDN | Perfiles de Content Delivery Network | Compatible | Todavía no se admite. |
| puerta de enlace de aplicaciones | Equilibrio de carga de nivel 7 | Compatible | Todavía no se admite. |
| Traffic Manager | Dirija el tráfico entrante para obtener un rendimiento y una confiabilidad óptimos de las aplicaciones. | Compatible | Todavía no se admite. |
| ExpressRoute | Configure una conexión privada rápida con los servicios en la nube de Microsoft desde su infraestructura local o una instalación de colocalización. | Compatible | Compatibilidad para conectar Azure Stack a un circuito de Express Route. |

## <a name="next-steps"></a>Pasos siguientes

[DNS en Azure Stack](azure-stack-dns.md)
