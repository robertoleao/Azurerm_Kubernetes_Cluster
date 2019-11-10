# Azurerm_Kubernetes_Cluster
#### Gerencia um cluster de Kubernetes gerenciado (também conhecido como Serviço de Kubernetes do AKS / Azure)

Você implanta um cluster do AKS (Serviço de Kubernetes do Azure) usando a CLI do Azure. O AKS é um serviço de Kubernetes gerenciado que permite implantar e gerenciar clusters rapidamente. Um aplicativo de vários contêineres que inclui um front-end da Web e uma instância do Redis é executado no cluster. Em seguida, você verá como monitorar a integridade do cluster e dos pods que executam seu aplicativo.

O Azure hospeda o Azure Cloud Shell, um ambiente de shell interativo que pode ser usado por meio do navegador. O Cloud Shell permite usar bash ou PowerShell para trabalhar com serviços do Azure. É possível usar os comandos pré-instalados do Cloud Shell para executar o código neste artigo sem precisar instalar nada no seu ambiente local.

Para executar o código neste artigo no Azure Cloud Shell:
- Inicie o Cloud Shell.
- Clique no botão Copiar no bloco de código para copiá-lo.
- Cole o código na sessão do Cloud Shell com Ctrl+Shift+V no Windows e no Linux ou Cmd+Shift+V no macOS.
- Pressione Enter para executar o código.

Se você optar por instalar e usar a CLI localmente, voce podera instala o CLI do Azure. Se você precisa instalar ou atualizar, consulte    [Instalar a CLI do Azure no site da Microsoft](https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli?view=azure-cli-latest) .

Criar um grupo de recursos
Um grupo de recursos do Azure é um grupo lógico no qual os recursos do Azure são implantados e gerenciados. Ao criar um grupo de recursos, você é solicitado a especificar um local. Essa é a localização na qual os metadados do grupo de recursos são armazenados e na qual os recursos são executados no Azure, caso você não especifique outra região durante a criação de recursos. Crie um grupo de recursos usando o comando az group create.

O exemplo a seguir cria um grupo de recursos chamado myResourceGroup no local eastus.

```powershell
az group create --name myResourceGroup --location eastus

```
> [Comando comentado](https://github.com/robertoleao/Azurerm_Kubernetes_Cluster/blob/master/az%20group%20create)

A seguinte saída de exemplo mostra o grupo de recursos criado com êxito:

##### Imagem colocar

Criar cluster AKS
Use o comando az aks create para criar um cluster do AKS. O exemplo a seguir cria um cluster chamado myAKSCluster com um nó. O Azure Monitor para contêineres também é habilitado usando o parâmetro --enable-addons monitoring. Isso levará vários minutos.

```powershell
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```
Após alguns minutos, o comando será concluído e retornará informações no formato JSON sobre o cluster.

##### Imagem colocar

Conectar-se ao cluster
Para gerenciar um cluster do Kubernetes, use kubectl, o cliente de linha de comando do Kubernetes. Se você usar o Azure Cloud Shell, o kubectl já estará instalado. Para instalar o kubectl localmente, use o comando az aks install-cli:
```
az aks install-cli
```
Para configurar o kubectl para se conectar ao cluster do Kubernetes, use o comando az aks get-credentials. Este comando baixa as credenciais e configura a CLI do Kubernetes para usá-las.
```
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```
Para verificar a conexão ao seu cluster, use o comando kubectl get para retornar uma lista de nós do cluster.
```
kubectl get nodes
```
A saída de exemplo a seguir mostra o único nó criado nas etapas anteriores. Verifique se o status do nó é Pronto:

##### Imagem colocar

### Executar o aplicativo
