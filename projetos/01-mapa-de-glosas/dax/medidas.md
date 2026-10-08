# Medidas DAX: conceitos e exemplos com nomes genéricos

[← Voltar ao projeto](../README.md) · [Etapas e competências](../ETAPAS-DO-PROJETO.md)

A extração analisada contém nove medidas explícitas: cinco somas e quatro percentuais. Os exemplos abaixo preservam a lógica identificada, substituindo nomes de tabela, colunas e medidas por nomes genéricos. Não são um arquivo executável independente nem expõem o esquema interno.

## Valores financeiros

```dax
Valor Faturado = SUM('BaseDemo'[Faturado])
Valor Recebido = SUM('BaseDemo'[Recebido])
Valor Imposto = SUM('BaseDemo'[Imposto])
Valor Glosado = SUM('BaseDemo'[Glosado])
Valor Mantido = SUM('BaseDemo'[Mantido])
```

A SQL prepara os valores por item; as medidas somam esses valores no contexto de filtro do relatório. O recebido documentado é líquido de imposto. Glosa mantida segue a seleção de movimentos descrita no case.

## Percentuais sobre o faturamento

```dax
Percentual Glosa = DIVIDE([Valor Glosado], [Valor Faturado])
Percentual Mantido = DIVIDE([Valor Mantido], [Valor Faturado])
Percentual Imposto = DIVIDE([Valor Imposto], [Valor Faturado])
Percentual Recebido = DIVIDE([Valor Recebido], [Valor Faturado])
```

As expressões calculam a razão entre totais no contexto selecionado. Não calculam a média simples dos percentuais de cada item. O denominador da glosa mantida é o faturado, não o valor glosado. Não foi informado resultado alternativo nas chamadas de `DIVIDE` inspecionadas.

## Outros elementos identificados

- Calendário calculado com atributos temporais e janela móvel de dez anos baseada na data corrente de avaliação.
- Coluna calculada que traduz códigos de categoria em descrições e classifica os demais em um grupo residual.

Os códigos de negócio originais não são reproduzidos. Cobertura do calendário, relacionamentos e aplicação das medidas em cada visual exigem validação específica.

## Limites

Não foram identificadas nessa extração medidas explícitas de recuperação, quantidade de contas ou quantidade de atendimentos. A recuperação bruta existe na SQL e permanece pendente de validação funcional.

Os valores da imagem pública são fictícios e não formam uma base conciliada. A inspeção das expressões confirma sua existência, sem certificar os números da ilustração ou a homologação do relatório.
