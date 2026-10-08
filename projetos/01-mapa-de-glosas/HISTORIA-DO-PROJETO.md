# A história do Mapa de Glosas: da solicitação à homologação

**Michel da S. Carvalho · Analista de Sistemas e Dados**

[← Voltar ao case](README.md) · [Decisões técnicas de cada etapa](ETAPAS-DO-PROJETO.md)

Esta narrativa incorpora o formulário de levantamento e desenvolvimento fornecido pelo autor, confrontado com os arquivos técnicos disponíveis. O documento original permanece fora do repositório público. São omitidos nomes de participantes, empresa, fornecedor do ERP, datas internas, volumes operacionais, identificadores e informações de infraestrutura. Os participantes são apresentados apenas por cargo ou função; a autoria profissional do portfólio é mantida.

## 1. A demanda da gestão

A solicitação partiu da **Gestora de Faturamento, Glosas e Autorização**, com foco no setor de Glosas. A necessidade era centralizar o acompanhamento e facilitar comparações entre períodos, transformando informações distribuídas no ERP Hospitalar em uma visão gerencial acessível.

O relatório precisava atender à gestão e permitir que a equipe de análise do Faturamento investigasse os registros por trás dos indicadores. Minha atuação como responsável pelo BI foi traduzir essas necessidades em regras, dados, medidas e uma apresentação que conectasse a visão consolidada ao detalhe.

**Competência aplicada:** levantamento de requisitos e identificação do público. O ponto de partida foi entender as decisões que o relatório deveria apoiar.

## 2. Das perguntas aos requisitos analíticos

| Pergunta registrada no levantamento | Tradução para a solução |
| --- | --- |
| Quanto recebido, imposto, glosado e mantido representam do faturado? | Medidas de valor e razões com o faturamento como denominador |
| Em quais convênios se concentram as glosas? | Segmentação e comparação por convênio |
| Quais profissionais concentram valores de glosa? | Análise por dimensão profissional, respeitando o contexto de faturamento |
| Quais categorias e serviços apresentam maior glosa? | Classificação e detalhamento por categoria e serviço |
| Como varia o percentual de glosa mantida por mês? | Calendário, data de referência e medida percentual |
| Como os valores se comportam entre anos e períodos? | Comparação temporal dos indicadores |

“Mais glosa” precisa ser interpretado conforme a pergunta: maior valor absoluto e maior percentual sobre o faturamento podem produzir ordenações diferentes. A análise por profissional também não demonstra, isoladamente, a causa da glosa ou a responsabilidade pelo evento.

**Competência aplicada:** transformar perguntas abertas em indicadores com significado, denominador e dimensões definidos.

## 3. Identificação das informações necessárias

O levantamento reuniu seis grupos de informação: atendimento, faturamento, glosas, recebimentos, reapresentações e dimensões de análise. Foi necessário relacionar documentos e movimentos financeiros mantendo a referência ao item de atendimento.

O ambiente operacional possui detalhes necessários à investigação interna. A versão pública compartilha apenas conceitos e dados fictícios, sem reproduzir informações de pacientes, profissionais reais, documentos ou registros da organização.

**Competência aplicada:** análise de sistemas e mapeamento de dados. Conhecer a origem dos valores permite definir quais relacionamentos sustentam cada indicador.

## 4. Desenvolvimento e problemas enfrentados

O formulário registra os seguintes problemas e tratamentos realizados:

| Problema documentado | Tratamento aplicado | Conhecimento demonstrado |
| --- | --- | --- |
| Multiplicação de valores ao combinar movimentos de diferentes granularidades | Agregação prévia e consolidação por item antes da associação final | Cardinalidade e controle da granularidade |
| Mistura entre recebimentos originais e reapresentações | Associação ao documento correspondente e caminhos separados | Rastreabilidade documental |
| Imposto disponível por movimento, sem valor direto por item | Rateio proporcional à participação do item no recebimento | Modelagem de regras financeiras |
| Reapresentações sucessivas | Tratamento de dois níveis e consolidação com UNION ALL | Encadeamento de documentos e operações de conjunto |
| Informações distribuídas no ERP | Consulta organizada em CTEs e junções | Integração relacional e organização da SQL |

A estrutura técnica compreende **12 objetos referenciados, 11 CTEs e 25 JOINs**. Cada etapa, sua granularidade e seus cuidados estão explicados no [detalhamento técnico](ETAPAS-DO-PROJETO.md).

Esses tratamentos descrevem decisões implementadas. A contagem de estruturas não mede, por si só, desempenho, ganho financeiro ou qualidade da homologação.

## 5. Preparação, modelo e medidas

O formulário informa conexão em **modo Importação**, com consumo de SQL nativa Oracle pelo Power Query. Essa distribuição de responsabilidades é compatível com o código M analisado: os tratamentos financeiros estão na SQL, e a consulta M retorna a fonte.

O modelo documentado utiliza uma fato na granularidade de item e uma dimensão calendário. O formulário descreve relacionamento **1:N**, com filtro do calendário para a fato pela data de atendimento. Essa referência organiza os períodos de análise; não deve ser confundida com a data do evento financeiro.

Os arquivos DAX disponíveis confirmam cinco somas financeiras e quatro percentuais sobre o faturamento. Também contêm calendário calculado e classificação de categorias.

**Competência aplicada:** separar responsabilidades entre origem, preparação e modelo, além de construir medidas coerentes com o contexto de filtro.

## 6. Organização da experiência de análise

O formulário descreve as seguintes páginas ou frentes de análise. A descrição documental não substitui uma inspeção da versão atual do relatório em execução.

| Frente documentada | Finalidade | Público por função |
| --- | --- | --- |
| Visão geral | Indicadores financeiros e evolução temporal | Gestora de Faturamento, Glosas e Autorização |
| Análise de glosas | Concentração por convênio, profissional, categoria e serviço | Gestão de Faturamento e Glosas |
| Recebimentos | Valores recebidos, impostos e proporções sobre o faturado | Gestão de Faturamento e Glosas |
| Detalhamento | Investigação da composição dos indicadores | Equipe de análise do Faturamento |
| Recuperação e reapresentação | Acompanhamento do caminho posterior à reapresentação | Gestão de Faturamento e Glosas; indicador pendente de homologação |

As segmentações documentadas incluem período, convênio, profissional, setor, categoria e serviço. Recursos como drill-through, bookmarks, tooltips personalizados e RLS não são apresentados como implementados: o próprio formulário pede confirmação no PBIX.

**Competência aplicada:** hierarquia de informação e organização das análises conforme a necessidade de cada público.

## 7. Evolução do escopo

O formulário registra a ampliação do detalhamento analítico para investigar a composição dos valores e a inclusão de recebimentos, impostos e glosa mantida na mesma visão. Essas alterações constam como aprovadas pela **Gestora de Faturamento e Glosas**.

O tratamento de reapresentações ampliou a análise do ciclo da glosa, mas sua regra de recuperação permanece pendente de homologação final.

**Competência aplicada:** gestão de requisitos e de mudanças, distinguindo ampliação aprovada de funcionalidade ainda em validação.

## 8. Rotina de atualização e manutenção

O uso gerencial foi definido como **mensal**, com **atualização diária**. O formulário informa Power BI Service e gateway para a fonte Oracle, sem confirmação do horário de atualização ou do workspace. Não foram inspecionados agendamentos ou históricos do serviço nesta revisão.

O procedimento documentado prevê testar alterações na consulta, regras, modelo e medidas e compará-las com as informações do ERP antes da publicação definitiva do relatório operacional.

**Competência aplicada:** distinguir frequência de uso de frequência de atualização e planejar a manutenção da solução. Os detalhes de conexão, acesso e infraestrutura permanecem privados.

## 9. Validação e status de entrega

| Item | Situação registrada no formulário |
| --- | --- |
| Faturamento | Validado e em acompanhamento |
| Glosa | Validada e em acompanhamento |
| Glosa mantida | Validada e em acompanhamento |
| Recebimento e imposto | Validados e em acompanhamento |
| Recuperação | Pendente de validação funcional |
| Projeto como um todo | Em homologação, com aceite e data de entrega a confirmar |

O formulário registra conferência da chave composta sem duplicidade na base avaliada. Essa informação não certifica todas as regras financeiras, futuras cargas ou cenários de filtro. Os volumes operacionais e as chaves originais não são divulgados.

O aceite está atribuído à **Gestora de Faturamento, Glosas e Autorização**. A homologação da recuperação envolve o responsável pelo BI e a gestão da área. Não há evidência de aceite definitivo no documento recebido.

**Competência aplicada:** documentar evidências, responsáveis e pendências, distinguindo implementação técnica de aprovação funcional.

## 10. Confronto entre documentação e implementação

Há uma diferença entre a medida de percentual de glosa mantida descrita no formulário e a expressão extraída do modelo: o formulário apresenta resultado alternativo zero em `DIVIDE`, acompanhado de uma observação de validação; a extração disponível não contém esse terceiro argumento. Os [exemplos públicos de medidas](dax/medidas.md) seguem a expressão extraída. A versão vigente deve ser confirmada no PBIX.

Também permanecem pontos de verificação: cobertura do calendário em relação ao histórico, revisão de páginas e interações, horário de atualização e homologação da recuperação. O registro desses limites evita apresentar planejamento ou documentação provisória como comportamento comprovado.

## Minha contribuição profissional

> “Conectei uma necessidade da gestão do Faturamento à construção de uma solução analítica: traduzi perguntas em indicadores, relacionei informações do ERP, organizei cálculos financeiros por item e estruturei a leitura no Power BI. Trabalhei a separação entre recebimento original e recuperação, o rateio proporcional de impostos e a consolidação de movimentos com granularidades diferentes. Também documentei mudanças de escopo, rotina de atualização e critérios de validação. O projeto está registrado em homologação, com a recuperação ainda pendente de validação funcional.”

O valor apresentado no portfólio é a capacidade de percorrer esse processo e explicar suas decisões. Não são atribuídos ganhos financeiros ou percentuais de melhoria sem evidências.
