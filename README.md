# Criar Azure Function com .NET Isolado (dotnet-isolated 8.0)

Este guia descreve os passos para criar uma Azure Function no plano de Consumo (Consumption Plan) utilizando o runtime **dotnet-isolated 8.0**, no Resource Group `estudos`, localizado na região **Brazil South**.

---

## Variáveis Utilizadas

Antes de começar, defina as seguintes variáveis no **PowerShell**:

```powershell
$RG = "estudos"                      # Nome do Resource Group
$LOCATION = "brazilsouth"            # Região da Function App
$FUNCTION_APP_NAME = "first-function-http-dio"   # Nome da Function App
$STORAGE_ACCOUNT_NAME = "stfirstfunctionhttpdio" # Nome da Storage Account
```

---

## Provisionamento

### 1. Criar o Resource Group (se ainda não existir)
Crie o Resource Group na região especificada:

```powershell
az group create --name $RG --location $LOCATION
```

---

### 2. Criar a Conta de Armazenamento
Crie uma conta de armazenamento exclusiva para a Function App:

```powershell
az storage account create `
  --name $STORAGE_ACCOUNT_NAME `
  --location $LOCATION `
  --resource-group $RG `
  --sku Standard_LRS
```

> **Nota:** O nome da Storage Account deve ser único em todo o Azure, apenas com letras minúsculas e números, e ter entre 3 e 24 caracteres.

---

### 3. Criar a Azure Function App
Crie a Function App no plano de Consumo (Consumption Plan) com o runtime `dotnet-isolated` e a versão 4 do Azure Functions:

```powershell
az functionapp create `
  --name $FUNCTION_APP_NAME `
  --storage-account $STORAGE_ACCOUNT_NAME `
  --resource-group $RG `
  --consumption-plan-location $LOCATION `
  --runtime dotnet-isolated `
  --functions-version 4
```

---

### 4. Conclusão
Se tudo estiver configurado corretamente, você verá uma mensagem de sucesso indicando que a Function App foi criada. O endpoint da função será algo como:

```
https://first-function-http-dio.azurewebsites.net/api/HttpExample
```

---
## Excluir Recursos para Evitar Custos

Após concluir os testes e estudos, você pode apagar todos os recursos criados para evitar custos desnecessários. Execute os comandos abaixo:

### 1. Apagar a Function App
```powershell
az functionapp delete --name $FUNCTION_APP_NAME --resource-group $RG
```

### 2. Apagar a Conta de Armazenamento
```powershell
az storage account delete --name $STORAGE_ACCOUNT_NAME --resource-group $RG --yes
```

### 3. Apagar o Resource Group (Remove todos os recursos dentro dele)
```powershell
az group delete --name $RG --yes --no-wait
```

> **Atenção:** O comando acima apaga todo o Resource Group e seus recursos. Certifique-se de que não há outros recursos importantes no grupo antes de executá-lo.

---


## Observações

1. Certifique-se de que o nível de autorização da função esteja configurado como `Anonymous` no arquivo `function.json` (ou no código) para tornar o endpoint público.
2. Utilize ferramentas como **Postman** ou **cURL** para testar o endpoint público.

---
