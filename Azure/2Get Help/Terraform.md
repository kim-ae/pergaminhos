## How to import role assigments
To discover the role assignment ID one must check for the role assignments in the desired scope:
```bash
az role assignment list --scope <scope> --output json > <somename>.json
```
Try to find the role assignment that you want, searching by name or principal id (it depends on how it was created).
Copy the role assignment id and use the following command:
```bash
terraform import 'azurerm_role_assignment.<resource-name-as-in-terraform-file>' <id copied in the previous step>
```