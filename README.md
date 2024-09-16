az vm list-skus --location <region> --resource-type virtualMachines --output json > vm_skus.json
az vm list-skus --location <region> --resource-type virtualMachines --output table
curl -X GET "https://prices.azure.com/api/retail/prices?currencyCode='USD'&$filter=serviceFamily eq 'Compute'" -H "accept: application/json"
import subprocess
import requests
import json

# List of regions to get VM pricing
regions = [
    "eastus2", "westus", "centralus", "northcentralus", "southcentralus", "northeurope", "westeurope",
    "eastasia", "southeastasia", "japaneast", "japanwest", "australiaeast", "australiasoutheast", "australiacentral",
    "brazilsouth", "southindia", "centralindia", "westindia", "canadacentral", "canadaeast", "westus2",
    "westcentralus", "uksouth", "ukwest", "koreacentral", "koreasouth", "francecentral", "southafricanorth",
    "uaenorth", "switzerlandnorth", "germanywestcentral", "norwayeast", "jioindiawest", "westus3", "swedencentral",
    "qatarcentral", "polandcentral", "italynorth", "israelcentral", "spaincentral", "mexicocentral"
]

# Azure Retail Prices API endpoint
pricing_url = "https://prices.azure.com/api/retail/prices?currencyCode=USD&$filter=serviceFamily eq 'Compute'"

# Get the VM SKUs for each region using Azure CLI and store them in a list
def get_vm_skus(region):
    command = ["az", "vm", "list-skus", "--location", region, "--resource-type", "virtualMachines", "--output", "json"]
    result = subprocess.run(command, capture_output=True, text=True)
    return json.loads(result.stdout)

# Fetch prices from Azure Retail Prices API
def get_vm_prices():
    response = requests.get(pricing_url)
    if response.status_code == 200:
        return response.json()['Items']
    else:
        print("Error fetching prices from Azure Retail API:", response.status_code)
        return []

# Match the SKU with the price
def find_vm_price(vm_sku, prices):
    for price in prices:
        if vm_sku['name'] == price['armSkuName']:
            return price['unitPrice']
    return None

# Main function to list SKUs and fetch pricing
def get_vm_costs():
    prices = get_vm_prices()
    if not prices:
        return

    # Iterate through each region and list the SKUs
    for region in regions:
        print(f"Fetching VM SKUs for region: {region}")
        skus = get_vm_skus(region)
        for sku in skus:
            vm_name = sku['name']
            price = find_vm_price(sku, prices)
            if price:
                print(f"VM: {vm_name}, Region: {region}, Price: ${price} per hour")
            else:
                print(f"VM: {vm_name}, Region: {region}, Price: Not available")

if __name__ == "__main__":
    get_vm_costs()
python azure_vm_costs.py

