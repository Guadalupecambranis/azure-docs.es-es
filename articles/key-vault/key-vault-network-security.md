---
ms.assetid: ''
title: Configuración de firewalls y redes virtuales de Azure Key Vault
description: Instrucciones detalladas paso a paso para configurar los firewalls y las redes virtuales de Azule Key Vault
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: d95ede3b6e99d6791a2642c6059281dedca3fcf2
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423167"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Configuración de firewalls y redes virtuales de Azure Key Vault

En este artículo se describen las instrucciones paso a paso para configurar los firewalls y las redes virtuales de Azure Key Vault para restringir el acceso al almacén de claves. Los [puntos de conexión de servicio de red virtual para Key Vault](key-vault-overview-vnet-service-endpoints.md) le permiten restringir el acceso a la red virtual especificada y a un conjunto de rangos de direcciones IPv4 (versión 4 del protocolo de Internet).

> [!IMPORTANT]
> Una vez que las reglas del firewall están en vigor, los usuarios solo pueden realizar operaciones del [plano de datos](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) de Key Vault cuando las solicitudes se originan desde redes virtuales o rangos de direcciones IPv4 permitidos. Esto también se aplica al acceso de Key Vault desde Azure Portal. Aunque los usuarios pueden ir a un almacén de claves desde Azure Portal, es posible que no pueda enumerar las claves, los secretos o los certificados si su equipo cliente no está en la lista de dispositivos permitidos. Esto también afecta al selector de Key Vault de otros servicios de Azure. Los usuarios podrían ver la lista de almacenes de claves, pero no enumerar las claves si las reglas del firewall limitan su equipo cliente.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Así es cómo puede configurar firewalls y redes virtuales de Key Vault mediante Azure Portal:

1. Vaya al almacén de claves que quiere proteger.
2. Seleccione **Firewalls y redes virtuales**.
3. Haga clic en **Redes seleccionadas** en **Permitir el acceso desde**.
4. Para agregar redes virtuales existentes a las reglas de firewall y de red virtual, haga clic en **+ Agregar redes virtuales existentes**.
5. En la nueva hoja que se abrirá, seleccione la suscripción, las redes virtuales y las subredes a las cuales desea otorgar acceso a este almacén de claves. Si las redes virtuales y subredes que seleccionó no tienen puntos de conexión de servicio habilitados, confirme que desea habilitar los puntos de conexión de servicio y seleccione **Habilitar**. Esta operación podría tardar hasta 15 minutos en surtir efecto.
6. En **Redes IP**, agregue rangos de direcciones IPv4; para ello, escriba los rangos de direcciones IPv4 en la [notación CIDR (enrutamiento de interdominios sin clases)](https://tools.ietf.org/html/rfc4632) o en las direcciones IP individuales.
7. Seleccione **Guardar**.

Asimismo, puede también agregar nuevas redes virtuales y subredes y habilitar los puntos de conexión de servicio de las redes virtuales y subredes recién creadas. Para ello, haga clic en **+ Agregar nueva red virtual**. A continuación, siga las indicaciones.

## <a name="use-the-azure-cli-20"></a>Uso de la CLI de Azure 2.0

Estos son los pasos para configurar firewalls y redes virtuales de Key Vault mediante la CLI de Azure 2.0:

1. [Instale la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) e [inicie sesión](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Se muestra la lista de reglas de redes virtuales disponibles. Si no ha establecido ninguna regla para este almacén de claves, la lista estará vacía.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Habilite un punto de conexión de servicio para Key Vault en una red virtual y subred existentes.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Agregue una regla de red para una red virtual y subred.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Agregue un rango de direcciones IP desde el que se va a permitir el tráfico.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Si cualquier servicio de confianza necesita obtener acceso a este almacén de claves, establezca la opción `bypass` en `AzureServices`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Active las reglas de red estableciendo la acción predeterminada en `Deny`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Uso de Azure PowerShell

Estas son las instrucciones para configurar los firewalls y las redes virtuales de Key Vault mediante PowerShell:

1. Instale la última versión de [Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) e [inicie sesión](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Se muestra la lista de reglas de redes virtuales disponibles. Si no ha establecido ninguna regla para este almacén de claves, la lista estará vacía.
   ```PowerShell
   (Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Habilite el punto de conexión de servicio para Key Vault en una red virtual y subred existentes.
   ```PowerShell
   Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
   ```

4. Agregue una regla de red para una red virtual y subred.
   ```PowerShell
   $subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Agregue un rango de direcciones IP desde el que se va a permitir el tráfico.
   ```PowerShell
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Si cualquier servicio de confianza necesita obtener acceso a este almacén de claves, establezca la opción `bypass` en `AzureServices`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Active las reglas de red estableciendo la acción predeterminada en `Deny`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Referencias

* Comandos de la CLI de Azure 2.0: [az keyvault network-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Cmdlets de Azure PowerShell: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Add-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule) y [Update-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Pasos siguientes

* [Puntos de conexión de servicio de red virtual para Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Protección de Key Vault](key-vault-secure-your-key-vault.md)