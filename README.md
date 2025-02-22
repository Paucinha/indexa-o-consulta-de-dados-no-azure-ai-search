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

:exclamation: **Observação:** os recursos do Azure AI Search e dos serviços do Azure AI devem estar no mesmo local!

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

4. Em uma nova aba do navegador, baixe as [avaliações de café compactadas](https://aka.ms/mslearn-coffee-reviews) de `https://aka.ms/mslearn-coffee-reviews` e extraia os arquivos para a pasta *de avaliações*.

5. No portal do Azure, selecione seu contêiner *coffee-reviews*. No contêiner, selecione **Upload**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-3.png)

6. No painel **Carregar blob**, selecione **Selecionar um arquivo**.

7. Na janela do Explorer, selecione **todos** os arquivos na pasta de *avaliações*, selecione **Abrir** e, em seguida, selecione **Carregar**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-container-upload-files.png)

8. Após o upload ser concluído, você pode fechar o painel **Upload blob**. Seus documentos agora estão no seu contêiner de armazenamento *coffee-reviews*.

## Indexar os documentos

Depois de ter os documentos no armazenamento, você pode usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um *assistente Importar dados*. Com esse assistente, você pode criar automaticamente um índice e um indexador para fontes de dados com suporte. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

1. No portal do Azure, navegue até seu recurso do Azure AI Search. Na página **Visão geral**, selecione **Importar dados**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/azure-search-wizard-1.png)

2. Na página **Connect to your data**, na lista **Data Source**, selecione **Azure Blob Storage**. Preencha os detalhes do armazenamento de dados com os seguintes valores:

- **Fonte de dados**: Armazenamento de Blobs do Azure
- **Nome da fonte de dados** : coffee-customer-data
- **Dados a extrair**: Conteúdo e metadados
- **Modo de análise**: Padrão
- **String de conexão**: *Selecione **Choose an existing connection**. Selecione sua conta de armazenamento, selecione o contêiner **coffee-reviews** e clique em **Select**.
- **Autenticação de identidade gerenciada**: Nenhuma
- **Nome do contêiner**: *esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente*.
- **Pasta Blob**: *deixe em branco*.
- **Descrição**: Avaliações das cafeterias Fourth Coffee.

3. Selecione **Avançar: Adicionar habilidades cognitivas (opcional)**.

4. Na seção **Anexar serviços de IA**, selecione seu recurso de serviços de IA do Azure.
5. Na seção **Adicionar enriquecimentos**:

- Altere o **nome do Skillset** para **coffee-skillset**.
- Marque a caixa de seleção **Habilitar OCR e mesclar todo o texto no campo merged_content**.

:exclamation: **Observação:** é importante selecionar **Habilitar OCR** para ver todas as opções de campo enriquecido.

- Certifique-se de que o **campo Dados de origem** esteja definido como **merged_content*.
- Altere o **nível de granularidade de enriquecimento** para **Páginas (blocos de 5000 caracteres)**.
- Não selecione *Habilitar enriquecimento incremental*
- Selecione os seguintes campos enriquecidos:

![campos enriquecidos](https://github.com/Paucinha/indexar-consulta-de-dados-no-azure-ai-search/blob/master/campos%20enriquecido.png)

6. Em **Salvar enriquecimentos em um armazenamento de conhecimento**, selecione:

- Projeções de imagem
- Documentos
- Páginas
- Frases-chave
- Entidades
- Detalhes da imagem
- Referências de imagem

:exclamation: **Observação:** Um aviso solicitando uma **sequência de conexão de conta de armazenamento** é exibido. ![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-enrichments-warning.png)

7. Selecione **Choose an existing connection (Escolha uma conexão existente)**. Escolha a conta de armazenamento que você criou anteriormente.

:exclamation: a. Clique em **+ Contêiner** para criar um novo contêiner chamado **knowledge-store** com o nível de privacidade definido como **Privado** e selecione **Criar**.

              b. Selecione o contêiner **de armazenamento de conhecimento** e clique em **Selecionar** na parte inferior da tela.

8. Selecione **Azure blob projections: Document**. Uma configuração para *Container name* com o contêiner *knowledge-store* preenchido automaticamente é exibida. Não altere o nome do contêiner.

9. Selecione **Próximo: Personalizar índice de destino**. Altere o **nome do índice para coffee-index**.

10. Certifique-se de que a **Chave** esteja definida como **metadata_storage_path**. Deixe **o nome do Suggester** em branco e **o modo Search** preenchido automaticamente.

11. Revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão. Os nomes de campo que precisam ser marcados como *filtráveis* ​​incluem: content, locations, keyphrases, sentiment, merged_content, text, layoutText, imageTags, imageCaption.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-customize-index.png)

12. Selecione **Avançar: Criar um indexador**.

13. Altere o **nome do indexador para coffee-indexer**.

14. Deixe a **programação** definida como **Uma vez**.

15. Expanda as **opções Avançadas**. Certifique-se de que a opção **Base-64 Encode Keys** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

16. Selecione **Enviar** para criar a fonte de dados, conjunto de habilidades, índice e indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:

- Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
- Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
- Mapeia os campos extraídos para o índice.

17. Retorne à sua página de recursos do Azure AI Search. No painel esquerdo, em **Search Management**, selecione **Indexers**. **Selecione o coffee-indexer** recém-criado. Aguarde um minuto e selecione ↻ Refresh até que o **Status** indique sucesso.

18. Selecione o nome do indexador para ver mais detalhes.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-search-indexer-success.png)

![Indexador](https://github.com/Paucinha/indexar-consulta-de-dados-no-azure-ai-search/blob/master/Indexador.png)

## Consultar o índice

Use o Search explorer para escrever e testar consultas. O Search explorer é uma ferramenta incorporada ao portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search explorer para escrever consultas e revisar resultados em JSON.

1. Na página Visão geral do serviço de pesquisa, selecione **Explorador de pesquisa** na parte superior da tela.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/5-exercise-screenshot-7.png)

2. Observe como o índice selecionado é o *coffee-index* que você criou. Abaixo do índice selecionado, altere a visualização para **JSON view**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/search-explorer-query.png)

No campo **do editor de consulta JSON**, copie e cole:

```json
{
    "search": "*",
    "count": true
}
```

1. Selecione **Search**. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo **@odata.count**. O índice de pesquisa deve retornar um documento JSON contendo os resultados da sua pesquisa.

2. Agora vamos filtrar por localização. No campo **JSON query editor**, copie e cole:

```json
{
 "search": "locations:'Chicago'",
 "count": true
}
```

3. Selecione **Search**. A consulta pesquisa todos os documentos no índice e filtra por avaliações com um local em Chicago. Você deve ver `3` no `@odata.count` campo.

4. Agora vamos filtrar por sentimento. No campo **JSON query editor**, copie e cole:

```json
{
 "search": "sentiment:'negative'",
 "count": true
}
```

5. Selecione **Search**. A consulta pesquisa todos os documentos no índice e filtra por avaliações com um sentimento negativo. Você deve ver `1` no `@odata.count` campo.

:exclamation: **Nota** Veja como os resultados são classificados por `@search.score`. Esta é a pontuação atribuída pelo mecanismo de busca para mostrar o quão próximos os resultados correspondem à consulta fornecida.

6. Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da avaliação?

## Revise o repositório de conhecimento

Vamos ver o poder do armazenamento de conhecimento em ação. Quando você executou o assistente Import data , você também criou um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, você encontrará os dados enriquecidos extraídos por habilidades de IA persistem na forma de projeções e tabelas.

1. No portal do Azure, volte para sua conta de armazenamento do Azure.

2. No painel de menu à esquerda, selecione **Containers**. Selecione o contêiner **knowledge-store**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-0.png)

3. Você verá uma lista de pastas. Há uma pasta para todos os metadados de cada documento de revisão. **Selecione qualquer uma das pastas**. Dentro da pasta, clique no arquivo *objectprojection.json**.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-1.png)

4. Selecione **Editar** para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-2.png)

5. Selecione o breadcrumb do blob de armazenamento no canto superior esquerdo da tela para retornar aos Contêineres da conta de armazenamento .

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-4.png)

![alt text](https://github.com/Paucinha/indexar-consulta-de-dados-no-azure-ai-search/blob/master/breadcrumb-%20blob.png)

6. Em *Containers*, selecione o container *coffee-skillset-image-projection*. Selecione qualquer um dos itens.

![alt text](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-5.png)

7. Selecione qualquer um dos arquivos *.jpg*. Selecione **Editar** para ver a imagem armazenada do documento. Observe como todas as imagens dos documentos são armazenadas dessa maneira.

![https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png)

![alt text](https://github.com/Paucinha/indexar-consulta-de-dados-no-azure-ai-search/blob/master/coffee-skillset-image-projection.png)

8. Selecione o breadcrumb do blob de armazenamento no canto superior esquerdo da tela para retornar aos *Contêineres* da conta de armazenamento.

9. Selecione **Storage browser** no painel esquerdo e selecione **Tables**. Há uma tabela para cada entidade no índice. Selecione a tabela *coffeeSkillsetKeyPhrases*.

Veja as frases-chave que o repositório de conhecimento conseguiu capturar do conteúdo nas avaliações. Muitos dos campos são chaves, então você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.

## Links Importantes
 
[Explore an Azure AI Search index (UI)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html)

[Azure AI Search service page](https://learn.microsoft.com/pt-br/azure/search/)

##

Projeto desenvolvido durante o [**Bootcamp Bradesco - Java Cloud Native**](https://www.dio.me/bootcamp/bradesco-java-cloud-native), fornecido pela [**DIO**](https://www.dio.me/)

##

- [By Páucinha](https://github.com/Paucinha)
