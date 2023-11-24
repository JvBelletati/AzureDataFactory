# AzureDataFactory

## Projeto
Nos últimos anos, a cidade de Boston nos Estados Unidos, teve um aumento notável nas solicitações de serviços não emergenciais através do número 311. Cada chamada representa um(a) morador(a) buscando atendimento, que é um aspecto da cidade a ser melhorado. O período de 2015 a 2020, em especial, trouxe um volume crescente dessas solicitações.

Diante desse cenário, o governo de Boston reconheceu a necessidade de um tratamento mais sistemático desses dados. A missão é criar um data lake eficiente. Antes de qualquer análise ou processamento, o foco é garantir uma ingestão correta e organizada dos dados. Assim, Boston estará preparada para desvendar o significado por trás dessas solicitações e tomar decisões mais informadas.

### Acesso aos dados de requisições de serviço 311
As requisições de serviço 311 refletem os pedidos não emergenciais feitos por cidadãos(ãs) da cidade de Boston ao longo dos anos.

- 311 Service Requests - 2015: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/c9509ab4-6f6d-4b97-979a-0cf2a10c922b/download/311_service_requests_2015.csv
- 311 Service Requests - 2016: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/b7ea6b1b-3ca4-4c5b-9713-6dc1db52379a/download/311_service_requests_2016.csv
- 311 Service Requests - 2017: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/30022137-709d-465e-baae-ca155b51927d/download/311_service_requests_2017.csv
- 311 Service Requests - 2018: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/2be28d90-3a90-4af1-a3f6-f28c1e25880a/download/311_service_requests_2018.csv
- 311 Service Requests - 2019: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/ea2e4696-4a2d-429c-9807-d02eb92e0222/download/311_service_requests_2019.csv
- 311 Service Requests - 2020: https://data.boston.gov/dataset/8048697b-ad64-4bfc-b090-ee00169f2323/resource/6ff6a6fd-3141-4440-a880-6f60a37fe789/download/script_105774672_20210108153400_combine.csv

## Primeiro Passo - Configurar recursos na Azure
Para iniciar o projeto, o primeiro passo é criar uma conta na Azure, já que a maioria dos recursos necessários para a construção do pipeline estão disponíveis nessa plataforma.

Com a conta criada, podemos configurar um alerta de gastos para monitorar os custos e também criar um grupo de recursos que será utilizado para gerenciar os recursos relacionados a esse projeto.

## Criar uma conta de armazenamento
O governo de Boston já utiliza o Blob Storage como repositório para dados provenientes de várias aplicações. Devido a esse requisito, foi solicitado que também fosse utilizado o Blob Storage como fonte primária para a ingestão dos dados compactados.

Para dar início a este processo, é essencial criar uma conta de armazenamento na Azure. Uma vez configurada, nossa próxima etapa será extrair e armazenar os dados compactados no Blob Storage.

## Extrair e compactar os dados
Os dados que precisamos armazenar estão disponíveis no site: [311 Service Requests - Analyze Boston](https://data.boston.gov/dataset/311-service-requests)

Para extrair esses dados e compactá-los para posteriormente salvá-los na nossa conta de armazenamento da Azure. Para fazer esse processo de extração e compactação, vamos utilizar o Python.

## Salvar os dados no Blob Storage
Com os dados extraídos e compactados, a próxima etapa é fazer o upload desses dados para a conta de armazenamento na Azure.

Para isso podemos continuar usando Python para essa tarefa, o que nos proporciona uma experiência de desenvolvimento consistente.

Para nos ajudar com essa tarefa, a Microsoft Azure fornece documentações detalhadas que podemos consultar, sendo elas:
- https://learn.microsoft.com/pt-br/azure/storage/blobs/storage-blob-python-get-started?tabs=azure-ad
- https://learn.microsoft.com/pt-br/azure/storage/blobs/storage-blob-upload-python#upload-a-block-blob-from-a-local-file-path

## Criar e estruturar o Data Lake
Para armazenar, organizar e preparar nossos dados para futuras análises e processamentos, optamos pela tecnologia Azure Data Lake Storage Gen 2, uma solução robusta e eficiente da Microsoft Azure. A implementação deste Data Lake ocorre através da criação de uma conta de armazenamento, otimizada para grandes volumes de dados e análises avançadas.

Uma vez estabelecido nosso Data Lake, adotaremos uma abordagem em camadas para organizar nossos dados. Esta estrutura, composta pelas camadas bronze, silver e gold, nos permitirá uma progressão lógica e eficiente desde os dados brutos até os insights refinados e prontos para uso.

## Configurar o Data Factory
Os nossos dados, atualmente compactados, estão armazenados no Blob Storage. A próxima etapa é transferi-los para a camada bronze do Data Lake, no qual armazenamos dados em seu estado original.

Como podemos ter atualizações e novos conjuntos de dados chegando regularmente, é essencial que essa transferência seja eficiente e facilmente repetível.

Para alcançar isso, vamos utilizar o Azure Data Factory. Usando essa ferramenta, construiremos um pipeline que cuidará do processo de mover os dados do Blob Storage diretamente para a camada bronze do Data Lake. Isso não só automatiza o processo, mas também garante consistência conforme novos dados são adicionados.

## Criar um pipeline para copiar os dados do Blob para o Data Lake
Nós precisamos construir um pipeline que realize o processo de cópia dos nossos arquivos que estão no Blob Storage para a camada bronze do nosso Data Lake. Durante esse processo de cópia, os dados, que atualmente estão compactados, devem ser salvos de forma descompactada no Data Lake.

A razão por trás disso é simples: enquanto os dados compactados são ótimos para economizar espaço e otimizar transferências, eles não são a melhor escolha para análises rápidas. Assim, ao descompactarmos os dados durante a transferência, garantimos que eles estejam prontos para uma análise imediata no Data Lake, otimizando o tempo de resposta para as equipes e eliminando etapas adicionais.

