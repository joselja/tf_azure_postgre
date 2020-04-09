## How create an Azure PostgreSQL Database

This Terraform module creates a Azure PostgreSQL Database.

## Usage

```hcl
resource "azurerm_resource_group" "rg" {
    name     = "Dev5"
    location = "northeurope"
}

module "postgresql" {
    source              = "Path"

    resource_group_name = "${azurerm_resource_group.rg.name}"
    location            = "${azurerm_resource_group.rg.location}"

    server_name = "sampleserver"
    sku_name = "GP_Gen5_2"
    sku_capacity = 2
    sku_tier = "GeneralPurpose"
    sku_family = "Gen5"

    storage_mb = 6120
    backup_retention_days = 7
    geo_redundant_backup = "Disabled"

    administrator_login = "login"
    administrator_password = "password"

    server_version = "9.5"
    ssl_enforcement = "Enabled"

    db_names = ["my_db1", "my_db2"]
    db_charset = "UTF8"
    db_collation = "English_United States.1252"

    firewall_rule_prefix = "firewall-"
    firewall_rules = [
        {name="test1", start_ip="10.0.0.5", end_ip="10.0.0.8"},
        {start_ip="127.0.0.0", end_ip="127.0.1.0"},
    ]

    vnet_rule_name_prefix = "postgresql-vnet-rule-"
    vnet_rules = [
        {name="subnet1", subnet_id="<subnet_id>"}
    ]

    tags = {
        Environment = "Demo",
        CostCenter = "Cheaper",
    }

    postgresql_configurations = {
        backslash_quote = "on",
    }
}
```
