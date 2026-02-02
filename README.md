# PowerBI-Project-Data-Visualization
Este projeto apresenta uma análise detalhada de performance de vendas, focando na transformação de dados brutos em inteligência estratégica. O objetivo foi criar um ecossistema de dados que permitisse monitorar faturamento, comportamento de clientes e desempenho regional através de visualizações dinâmicas e interativas.

Este projeto apresenta um dashboard desenvolvido no Power BI, focado no monitoramento de Vendas e Percentual de Faturamento, projetado para oferecer uma visão clara sobre a saúde financeira e a eficiência logística de uma operação global.

O Problema de Negócio

A gestão precisava responder a perguntas críticas de forma rápida:

- Qual o impacto real dos descontos no faturamento líquido?

- Quais categorias de produtos dominam o faturamento vs. volume de vendas?

- Como a sazonalidade trimestral afeta o fluxo de caixa?

- O nosso Lead Time médio está dentro da meta operacional?

A Solução

Para endereçar essas questões, desenvolvi uma solução visual em Dark Mode para facilitar a leitura prolongada e o foco nas métricas-chave. O dashboard integra:

KPIs de Performance: Visibilidade imediata de Faturamento Bruto (R$ 1,00M), Faturamento Líquido e Descontos.

Análise Temporal: Gráfico de tendência por trimestres para identificar picos e vales de demanda.

Geolocalização: Mapa interativo para análise de expansão territorial.

Produtos: Detalhamento por categoria para otimização de estoque.

Dataset
Para o desenvolvimento deste projeto, serão utilizadas três planilhas, que em conjunto permitem atender às necessidades de análise da empresa. A seguir, estão descritos os nomes das tabelas e a explicação de cada atributo presente em cada uma delas.

detalhe_pedido;

Esta tabela contém informações detalhadas sobre os produtos incluídos em cada pedido

1. preco → valor unitário de cada produto.

2. quantidade → quantidade de unidades do produto em cada pedido.

3. desconto → valor de desconto aplicado ao produto.

pedido;

Esta tabela armazena informações gerais relacionadas aos pedidos realizados.

1. data_pedido → data em que o pedido foi efetuado.

categoria;

Esta tabela apresenta informações sobre a classificação dos produtos.

1. descricao → descrição da categoria ou do produto.

Preparando

Para dar início ao desenvolvimento deste dashboard, será realizada a conexão com os dados armazenados no banco de dados DB_ERP_COMERCIAL, hospedado em um servidor local (SQL Server). Essa etapa é fundamental para garantir o acesso às informações necessárias para a análise e construção dos indicadores.

A criação do dashboard será orientada por perguntas de negócio previamente definidas, que refletem as principais necessidades da gestão em relação à tomada de decisão

É importante destacar que, devido à utilização da versão limitada (gratuita) do programa, algumas ferramentas e funcionalidades avançadas não estarão disponíveis. Dessa forma, o desenvolvimento do dashboard será realizado respeitando essas limitações, utilizando apenas os recursos acessíveis nessa versão, sem comprometer a clareza e a qualidade das análises apresentadas.

Conexão com o banco de dados
1.1 Nesta etapa, é realizada a conexão com o banco de dados DB_ERP_COMERCIAL, hospedado em um servidor local SQL Server, com o objetivo de acessar as tabelas e informações necessárias para a construção do dashboard.

(./resources/img1.png)

1.2 Nesta etapa, serão selecionadas todas as tabelas do banco de dados, mesmo que apenas algumas sejam utilizadas inicialmente. Essa decisão visa facilitar futuras expansões do dashboard, garantindo que os dados já estejam disponíveis caso sejam necessários posteriormente.

Press enter or click to view image in full size

Após a conexão com o banco de dados, serão definidos o tamanho do dashboard e o tema visual, de forma a proporcionar uma melhor organização das informações e uma experiência visual mais clara e objetiva.

Press enter or click to view image in full size

Indicadores de Performance (KPI Cards)

Pergunta a ser respondida: Qual o impacto real dos descontos no faturamento líquido?

Cards de indicadores financeiros

Foram criados cards de indicadores-chave com o objetivo de fornecer uma visão imediata do desempenho financeiro. Esses cartões concentram informações estratégicas ao apresentar, de forma integrada, o Total de Vendas, o Faturamento Líquido e o Total de Descontos, permitindo uma leitura clara da relação de causa e efeito entre receita bruta, política de descontos e resultado final.

Press enter or click to view image in full size

Medidas criadas (DAX)

Cada card foi construído a partir de medidas específicas, garantindo flexibilidade e reutilização em outros visuais do dashboard:

Vendas Total:

DAX
Vendas Total =
SUMX(
    detalhe_pedido,
    detalhe_pedido[preco] * detalhe_pedido[quantidade]
)
Desconto Total:

DAX
Desconto Total =
SUMX(
    detalhe_pedido,
    detalhe_pedido[preco]
        * detalhe_pedido[quantidade]
        * detalhe_pedido[desconto]
)
Faturamento Líquido

DAX
Faturamento Líquido =
[Vendas Total] - [Total Desconto]
Press enter or click to view image in full size

A visualização centraliza o Total de Vendas (R$ 1.003.506,97), o Faturamento Líquido (R$ 936.088,33) e o Total de Descontos (R$ 67.418,64), facilitando a compreensão do impacto direto dos descontos sobre a receita final da empresa.

Become a member
Essa abordagem permite à gestão avaliar a eficiência da política de descontos, identificar possíveis excessos e validar se a margem líquida está sendo preservada em relação à receita bruta, apoiando decisões mais assertivas.

Eficiência Logística (Lead Time)

Pergunta a ser respondida: O nosso Lead Time médio está dentro da meta operacional?

Construção do visual

Foi criada uma medida em DAX para calcular o Lead Time médio, considerando a diferença em dias entre a data do pedido e a data de entrega:

DAX
Lead Time =
AVERAGEX(
    pedido,
    DATEDIFF(pedido[data_pedido], pedido[data_entrega], DAY)
)
Press enter or click to view image in full size

A medida foi utilizada na criação de um card de KPI, destacando o Lead Time médio de 27,88 dias como um indicador crítico da operação

Esse indicador funciona como uma métrica de controle da eficiência logística, permitindo avaliar o nível de serviço prestado, identificar gargalos operacionais e apoiar decisões que impactam diretamente a satisfação do cliente e a agilidade da cadeia de suprimentos.

Sazonalidade Trimestral (Vendas por Trimestre)

Pergunta a ser respondida: Como a sazonalidade trimestral afeta o fluxo de caixa?

Etapas

Utilizou-se a tabela pedido, com a coluna data_pedido para a hierarquia de tempo (trimestres).
Como valor, foi aplicada a medida Total de Vendas
O visual escolhido foi um gráfico de linha com marcadores, facilitando a análise da evolução ao longo do ano.
Press enter or click to view image in full size

O gráfico evidencia o menor faturamento no 2º trimestre (R$ 152K) e a recuperação até o pico no 4º trimestre (R$ 336K), oferecendo suporte ao planejamento financeiro, antecipação de sazonalidades e ajustes estratégicos de vendas e custos operacionais.

Mix de Produtos e Volume (Percentual de Quantidade e Total de Vendas)

Pergunta respondida: Quais categorias de produtos dominam o faturamento vs. volume de vendas?

Etapas

Foi utilizado um gráfico de rosca para representar a distribuição do volume de vendas por categoria.
A análise usou a coluna descricao da tabela categoria e a coluna quantidade da tabela detalhe_pedido.
Para complementar a visualização, foi criado um card exibindo o total de produtos vendidos (38.682 unidades), obtido pela soma da quantidade.
Press enter or click to view image in full size

O visual destaca a concentração do volume em Laticínios (18,31%) e Bebidas (17%), permitindo identificar categorias com maior giro de estoque e apoiando decisões relacionadas a mix de produtos, reposição e estratégia comercial.

Mix de Produtos e Volume(Valor Total de Vendas e Percentual de Vendas)

O gráfico complementa a análise do mix de produtos ao apresentar o faturamento e porcentagem do valor total de vendas por categoria.

Etapas

Utilizou-se a coluna descricao da tabela categoria como eixo categórico.
A medida Total de Vendas foi aplicada para representar o valor absoluto do faturamento (barras).
A mesma medida foi utilizada para exibir a participação percentual no faturamento total (linha).
Press enter or click to view image in full size

Essa visualização permite comparar volume financeiro e participação relativa entre categorias, apoiando decisões estratégicas sobre precificação, foco comercial e priorização do portfólio de produtos.

Distribuição Geográfica (Total de Vendas por País)

Pergunta a ser respondida: Quais regiões apresentam maior maturidade comercial?

Etapas

Foi utilizado um mapa de calor (Bubble Map) para representar a distribuição geográfica das vendas.
A análise considerou a coluna pais da tabela endereco.
A medida Total de Vendas foi aplicada para definir o volume financeiro, representado pelo tamanho das bolhas.
Press enter or click to view image in full size

O visual destaca a maior concentração de vendas na América do Norte e América do Sul, facilitando a identificação de mercados consolidados e oportunidades de expansão geográfica, apoiando decisões estratégicas de crescimento.

Ajuste de Design do Dashboard
Com as visualizações definidas, inicia-se a etapa de ajuste de design e personalização dos gráficos. Nesse momento, o foco é garantir clareza, padronização visual e melhor experiência de leitura, tornando os insights mais acessíveis para a gestão.

Foram aplicados ajustes como:

Padronização de cores, fontes e estilos, alinhados ao tema do dashboard;
Organização do layout para facilitar a leitura e comparação entre indicadores;
Destaque visual para KPIs críticos, direcionando a atenção para os dados mais relevantes;
Ajustes em rótulos, títulos e legendas para maior objetividade.
Press enter or click to view image in full size

Conclusão
O dashboard desenvolvido cumpre o objetivo de responder às principais perguntas de negócio da gestão, transformando dados operacionais em informações visuais claras e acionáveis.

Mesmo utilizando a versão gratuita da ferramenta, foi possível construir uma solução analítica eficiente, explorando boas práticas de modelagem, criação de medidas e visualização de dados.

O resultado final oferece suporte à tomada de decisão estratégica, permitindo avaliar desempenho financeiro, eficiência operacional, comportamento sazonal, mix de produtos e maturidade geográfica dos mercados, além de servir como base para futuras evoluções e aprofundamentos analíticos.

Obrigado.
