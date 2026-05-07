# 03-roboshop-ansible-v1

## Azure CLI Setup

- Import the Microsoft repository key (for RHEL 10 and CentOS Stream 10):
  ```bash
  sudo rpm --import https://packages.microsoft.com/keys/microsoft-2025.asc
  ```

- Add the packages-microsoft-com-prod repository (for RHEL 10):
  ```bash
  sudo dnf install -y https://packages.microsoft.com/config/rhel/10/packages-microsoft-prod.rpm
  ```

- Install Azure CLI:
  ```bash
  sudo dnf install azure-cli
  ```

- Login to Azure CLI:
  ```bash
  az login
  ```
  This command authenticates you with Azure and allows Terraform to manage Azure resources on your behalf.

## Script to create VMs using az cli tool

Use the following Azure CLI script to create the required virtual machines:

```bash
VM_NAMES=("frontend" "mysql" "catalogue" "mongodb" "user" "valkey" "cart" "shipping" "rabbitmq" "payment" "notification" "orders" "ratings")

for NAME in "${VM_NAMES[@]}"
do
  echo "Starting creation of VM: $NAME"
  az vm create \
    --resource-group dnmrk-est-rg \
    --name "$NAME" \
    --location denmarkeast \
    --image "/subscriptions/e95ed2ec-55a5-49ac-a41d-51cb0ac50b67/resourceGroups/dnmrk-est-rg/providers/Microsoft.Compute/galleries/rhel10/images/1.0.0" \
    --size Standard_B1s \
    --admin-username azureuser \
    --admin-password "YourComplexPassword123!" \
    --authentication-type password \
    --os-disk-size-gb 64 \
    --os-disk-delete-option delete \
    --nic-delete-option delete \
    --security-type TrustedLaunch \
    --enable-secure-boot true \
    --enable-vtpm true \
    --zone 1 \
    --no-wait

done
```


