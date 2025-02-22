# Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

![GitHub](https://img.shields.io/github/license/Paucinha/api-ecommerce-dio?style=flat-square)

Neste LAB, aplicaremos técnicas de organização de documentos e conduziremos pesquisas eficientes através da ingestão de dados, seguindo três passos essenciais: ingestão de conhecimento de IA, criação do índice correspondente e exploração dessas funcionalidades. Ao final, ganharemos uma visão prática das potencialidades oferecidas por essas ferramentas na mineração de conhecimento.

**Inteligência Artificial (IA) | Azure Machine Learning**

**Full-Stack | Básico**

## Explorar um índice de pesquisa do Azure AI (IU)

Vamos imaginar que você trabalha para a Fourth Coffee, uma rede nacional de cafés. Você é solicitado a ajudar a construir uma solução de mineração de conhecimento que facilite a busca por insights sobre experiências de clientes. Você decide construir um índice do Azure AI Search usando dados extraídos de avaliações de clientes.

Neste laboratório foi feito:

- Criado recursos do Azure
- Extraido dados de uma fonte de dados
- Enriquecido os dados com habilidades de IA
- Usado o indexador do Azure no portal do Azure
- Consultado o índice de pesquisa
- Revisado os resultados salvos em um Knowledge Store

#### Recursos do Azure necessários

A solução que você criará para o Fourth Coffee requer os seguintes recursos na sua assinatura do Azure:

- Um recurso do Azure AI Search , que gerenciará a indexação e a consulta.
- Um recurso de serviços de IA do Azure , que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.
```
Observação: os recursos do Azure AI Search e dos serviços do Azure AI devem estar no mesmo local!
```
- Uma conta de armazenamento com contêineres de blobs, que armazenarão documentos brutos e outras coleções de tabelas, objetos ou arquivos.

**Criar um recurso *de pesquisa do Azure AI***

1. Entre no [portal do Azure](https://portal.azure.com/#home).

2. Clique no botão **+ Criar um recurso**, pesquise por Azure *AI Search* e crie um recurso do **Azure AI Search** com as seguintes configurações:

- **Assinatura**: *sua assinatura do Azure*.
- **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
- **Nome do serviço**: *Um nome exclusivo*.
- **Localização**: *Escolha qualquer região disponível. Se estiver no leste dos EUA, use “East US 2”*.
- **Nível de preço**: *Básico*

3. Selecione **Revisar + criar** e, depois de ver a resposta **Validação bem-sucedida** , selecione Criar .

4. Após a conclusão da implantação, selecione **Ir para o recurso**. Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

**Criar um recurso de serviços de IA do Azure**

Você precisará provisionar um recurso **de serviços de IA do Azure** que esteja no mesmo local que seu recurso de Pesquisa de IA do Azure. Sua solução de pesquisa usará esse recurso para enriquecer os dados no datastore com insights gerados por IA.

1. Retorne à página inicial do portal do Azure. Clique no botão **＋Criar um recurso** e pesquise por *Azure AI services*. Selecione **criar** um plano **de serviços do Azure AI**. Você será levado a uma página para criar um recurso de serviços do Azure AI. Configure-o com as seguintes configurações:

- **Assinatura**: *sua assinatura do Azure*.
- **Grupo de recursos**: *o mesmo grupo de recursos que seu recurso do Azure AI Search*.
- **Região**: *o mesmo local do seu recurso do Azure AI Search*.
- **Nome**: *Um nome único*.
- **Nível de preço**: *Standard S0*
- **Ao marcar esta caixa, reconheço que li e compreendi todos os termos abaixo**: *Selecionado*

2. Selecione **Revisar + criar**. Após ver a resposta **Validação aprovada**, selecione **Criar**.
3. Aguarde a conclusão da implantação e visualize os detalhes da implantação.

**Criar uma conta de armazenamento**

1. Retorne à página inicial do portal do Azure e selecione o botão **+ Criar um recurso**.
2. Pesquise por *conta de armazenamento* e crie um recurso **de conta de armazenamento** com as seguintes configurações:

- **Assinatura**: *sua assinatura do Azure*.
- **Grupo de recursos**: o mesmo grupo de recursos dos recursos do Azure AI Search e dos serviços do Azure AI .
- **Nome da conta de armazenamento**: *Um nome exclusivo*.
- **Localização**: *Escolha qualquer localização disponível*.
- **Desempenho**: *Padrão*
- **Redundância**: *Armazenamento redundante localmente (LRS)*

3. Clique em **Review** e depois em **Create**. Aguarde a conclusão da implantação e vá para o recurso implantado.
4. Na conta de Armazenamento do Azure que você criou, no painel de menu à esquerda, selecione **Configuração** (em **Configurações**).
5. Altere a configuração de *Permitir acesso anônimo do Blob* para **Habilitado** e selecione **Salvar**.

## Carregar documentos para o armazenamento do Azure

1. No painel de menu à esquerda, selecione **Contêineres**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-1.png)

2. Selecione **+ Container**. Um painel no seu lado direito abre.
3. Insira as seguintes configurações e clique em **Criar**:

- **Nome**: coffee-reviews
- **Nível de acesso público**: Container (acesso de leitura anônimo para containers e blobs)
- **Avançado**: *sem alterações*.

4. Em uma nova aba do navegador, baixe as [avaliações de café compactadas](https://aka.ms/mslearn-coffee-reviews) de https://aka.ms/mslearn-coffee-reviews e extraia os arquivos para a pasta *de avaliações*.

5. No portal do Azure, selecione seu contêiner *coffee-reviews*. No contêiner, selecione **Upload**.

6. No painel **Carregar blob**, selecione **Selecionar um arquivo**.

7. Na janela do Explorer, selecione **todos** os arquivos na pasta de *avaliações*, selecione **Abrir** e, em seguida, selecione **Carregar**.

















## Links Importantes
 
[Explore an Azure AI Search index (UI)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html)

##

Projeto desenvolvido durante o [**Bootcamp Bradesco - Java Cloud Native**](https://www.dio.me/bootcamp/bradesco-java-cloud-native), fornecido pela [**DIO**](https://www.dio.me/)

##

- [By Páucinha](https://github.com/Paucinha)
