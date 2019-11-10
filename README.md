# Azurerm_Kubernetes_Cluster
#### Gerencia um cluster de Kubernetes gerenciado (também conhecido como Serviço de Kubernetes do AKS / Azure)

Você implantara um cluster do AKS (Serviço de Kubernetes do Azure) usando a CLI do Azure. O AKS é um serviço de Kubernetes gerenciado que permite implantar e gerenciar clusters rapidamente. Um aplicativo de vários contêineres que inclui um front-end da Web e uma instância do Redis é executado no cluster. Em seguida, você verá como monitorar a integridade do cluster e dos pods que executam seu aplicativo.

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
Ao criar um cluster do AKS, um segundo grupo de recursos é criado automaticamente para armazenar os recursos do AKS.

> Por que são criados dois grupos de recursos com o AKS?

> O AKS baseia-se em vários recursos de infraestrutura do Azure, incluindo conjuntos de dimensionamento de máquinas virtuais, redes virtuais e Managed disks. Isso permite que você aproveite muitos dos principais recursos da plataforma Azure no ambiente kubernetes gerenciado fornecido pelo AKS. Por exemplo, a maioria dos tipos de máquina virtual do Azure pode ser usada diretamente com AKS e as reservas do Azure podem ser usadas para receber descontos nesses recursos automaticamente.

> Para habilitar essa arquitetura, cada implantação AKS abrange dois grupos de recursos:

> - Você cria o primeiro grupo de recursos. Esse grupo contém apenas o recurso de serviço kubernetes. O provedor de recursos AKS cria automaticamente o segundo grupo de recursos durante a implantação. Um exemplo do segundo grupo de recursos é MC_myResourceGroup_myAKSCluster_eastus. Para obter informações sobre como especificar o nome desse segundo grupo de recursos, consulte a próxima seção.

> - O segundo grupo de recursos, conhecido como grupo de recursos de nó, contém todos os recursos de infraestrutura associados ao cluster. Esses recursos incluem as máquinas virtuais do nó do Kubernetes, rede virtual e armazenamento. Por padrão, o grupo de recursos de nó tem um nome como MC_myResourceGroup_myAKSCluster_eastus. O AKS exclui automaticamente o recurso de nó sempre que o cluster é excluído, portanto, ele só deve ser usado para recursos que compartilham o ciclo de vida do cluster.

```powershell
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```
> [Comando comentado](https://github.com/robertoleao/Azurerm_Kubernetes_Cluster/blob/master/az%20aks%20create)

Após alguns minutos, o comando será concluído e retornará informações no formato JSON sobre o cluster.

##### Imagem colocar

Essa etapa e para Instalar o kubectl localmente

Conectar-se ao cluster
Para gerenciar um cluster do Kubernetes, use kubectl, o cliente de linha de comando do Kubernetes. Se você usar o Azure Cloud Shell, o kubectl já estará instalado. Para instalar o kubectl localmente, use o comando az aks install-cli:

```
az aks install-cli
```

> [Comando comentado](https://github.com/robertoleao/Azurerm_Kubernetes_Cluster/blob/master/az%20aks%20install-cli)

Para configurar o kubectl para se conectar ao cluster do Kubernetes, use o comando az aks get-credentials. Este comando baixa as credenciais e configura a CLI do Kubernetes para usá-las.

```
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

> [Comando comentado](https://github.com/robertoleao/Azurerm_Kubernetes_Cluster/blob/master/az%20aks%20get-credentials)

Para verificar a conexão ao seu cluster, use o comando kubectl get para retornar uma lista de nós do cluster.

```
kubectl get nodes
```

> [Comando comentado](https://github.com/robertoleao/Azurerm_Kubernetes_Cluster/blob/master/kubectl%20get)

A saída de exemplo a seguir mostra o único nó criado nas etapas anteriores. Verifique se o status do nó é Pronto:

##### Imagem colocar



### Executar o aplicativo




