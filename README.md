## main.parameters.dev.json  

以下のコマンドでmain.bicepの環境変数などの情報保管をおこなう
```sh
az deployment group create --template-file main.bicep --parameters main.parameters.dev.json
```

### keyvalue 
クラウドのアーキテクチャーにおいて シークレット変数を クラウドで管理する
コマンドから作成可能

キーコンテナとシークレットを作成する
```sh
keyVaultName='YOUR-KEY-VAULT-NAME'
read -s -p "Enter the login name: " login
read -s -p "Enter the password: " password

az keyvault create --name $keyVaultName --location westus3 --enabled-for-template-deployment true
az keyvault secret set --vault-name $keyVaultName --name "sqlServerAdministratorLogin" --value $login --output none
az keyvault secret set --vault-name $keyVaultName --name "sqlServerAdministratorPassword" --value $password --output none
```

### キーコンテナーのリソースIDを取得する
```sh
az keyvault show --name $keyVaultName --query id --output tsv
```

### main.parameters.dev.json ファイル
以下にシークレット変数を格納できる
```sh
"keyVault": {
    "id": "YOUR-KEY-VAULT-RESOURCE-ID"
},
```
