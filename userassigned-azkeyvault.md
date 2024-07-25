# Managed-Identities

# Testando User Assigned Managed Identity no Azure

## Introdução
Este guia fornece um passo a passo para criar e testar uma User Assigned Managed Identity no Azure. Vamos criar uma Managed Identity, associá-la a uma VM, configurar permissões no Azure Key Vault e testar o acesso ao Key Vault a partir da VM.

## Passo 1: Criar uma User Assigned Managed Identity

1. **Acessar o Portal do Azure:**
   - Abra o [Portal do Azure](https://portal.azure.com).

2. **Criar a Managed Identity:**
   - No menu de navegação à esquerda, clique em "Identities" em "Entra ID".
   - Clique em "Add" e selecione "User Assigned".
   - Preencha os detalhes:
     - **Nome:** Nome da sua Managed Identity.
     - **Região:** Selecione a região adequada.
     - **Grupo de Recursos:** Selecione um grupo de recursos existente ou crie um novo.
   - Clique em "Review + create" e, em seguida, em "Create".

## Passo 2: Associar a Managed Identity a uma VM

1. **Selecionar a VM:**
   - No Portal do Azure, vá para "Virtual Machines" e selecione a VM à qual deseja associar a Managed Identity.

2. **Habilitar a Managed Identity na VM:**
   - No menu da VM, vá para "Identity" em "Settings".
   - Selecione a guia "User assigned".
   - Clique em "Add" e selecione a Managed Identity que você criou.
   - Clique em "Save".

## Passo 3: Atribuir Permissões no Azure Key Vault

1. **Acessar o Azure Key Vault:**
   - No Portal do Azure, vá para "Key Vaults" e selecione o Key Vault que deseja acessar.

2. **Atribuir Permissões:**
   - No menu do Key Vault, vá para "Access policies".
   - Clique em "Add Access Policy".
   - Em "Configure from template", selecione "Key, Secret & Certificate Management".
   - Em "Select principal", clique em "None selected" e procure pela Managed Identity criada.
   - Selecione a Managed Identity e clique em "Select".
   - Clique em "Add" e, em seguida, em "Save".

## Passo 4: Testar o Acesso ao Key Vault a partir da VM

1. **Conectar-se à VM:**
   - Conecte-se à VM via SSH (Linux) ou RDP (Windows).

2. **Instalar o Azure CLI (se não estiver instalado):**
   - **Linux:**
     ```bash
     curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
     ```
   - **Windows:** Baixe e instale o [Azure CLI](https://aka.ms/installazurecliwindows).

3. **Logar-se no Azure:**
   - No terminal ou prompt de comando da VM, faça login no Azure:
     ```bash
     az login
     ```

4. **Obter um Token de Acesso:**
   - Use o comando `az` para obter um token de acesso:
     ```bash
     az account get-access-token --resource https://vault.azure.net
     ```

5. **Acessar um Segredo no Key Vault:**
   - Utilize o token para acessar um segredo no Key Vault:
     ```bash
     VAULT_NAME=<SeuNomeDoKeyVault>
     SECRET_NAME=<SeuNomeDoSegredo>
     az keyvault secret show --vault-name $VAULT_NAME --name $SECRET_NAME
     ```

## Conclusão
Se você seguiu todos os passos corretamente, deve ser capaz de acessar os segredos no Azure Key Vault usando a Managed Identity atribuída pelo usuário. Esta abordagem elimina a necessidade de gerenciar manualmente as credenciais, melhorando a segurança e a facilidade de gerenciamento.

---

Este guia pode ser armazenado em um repositório GitHub para referência futura.
