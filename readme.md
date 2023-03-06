# Desafio do Processo Seletivo Desenvolvedor Python

Versão: 1.4

## Instruções

O propósito desse desafio é simular uma pequena parte de algumas atividades desenvolvidas nas tarefas diárias da equipe. Sua tarefa é desenvolver uma pequena ferramenta para auxiliar os gestores na operação de uma empresa de entregas. Considere que as unidades de distribuição operam todos os dias da semana, sem interrupção. Para realizar a tarefa, foram fornecidos um conjunto de dados descrevendo a localização das centrais de distribuição, os municípios atendidos pela empresa, as distâncias entre os municípios e os dados dos pedidos.

A ferramenta escrita deve realizar as seguintes tarefas:
- Ler e tratar os dados, padronizando seu tratamento no código
- Ler os dados do diretório "dados", com a mesma estrutura e organização atual (incluindo os pedidos em .zip) - utilizaremos esse diretório para substituir os dados e testar sua solução
- Processar cada arquivo de pedido individualmente e fornecer um resultado unificado (não analise todos os pedidos de uma única vez)
- Determinar os seguintes dados para cada HUB:
    - Total de Pedidos Alocados: total de pedidos originalmente no hub
    - Total de Pedidos Redirecionados Recebidos: total de pedidos recebidos de outros hubs para entrega
    - Total de Pedidos Redirecionados Enviados: total de pedidos cujo destino não é coberto por essa unidade de distribuição, mas são cobertos por outra unidade
    - Total de Pedidos Não Entregues: total de pedidos que não puderam ser entregues diretamente ou redirecionados para outra unidade de distribuição
    - Total de dias de entrega: dias necessários para entregar todos os pedidos alocados e recebidos por redirecionamento
    - Distância total da frota: considere a soma da distância de ida e volta para cada pedido entregue

- Gerar o output, no formato abaixo:
    - Arquivo csv com encoding "cp1252", separado por ";" e decimais com ","
    - Colunas: "Nome HUB", "Total de Pedidos Alocados", "Total de Pedidos Redirecionados Recebidos", "Total de Pedidos Redirecionados Enviados", "Total de Pedidos Não Entregues", "Total de dias de entrega", "Distância total da frota"

## Bônus
Desenvolver cada uma das tarefas dessa seção é uma parte opcional do desafio, criada com o intuito de testar os limites do seu conhecimento e como você pode lidar com uma tarefa mais complexa e incomum. Ainda que não consiga cumprir todas as etapas, recomendamos tentar realizar elas ao máximo da sua capacidade. As respostas de cada desafio devem estar organizadas da seguinte forma no seu respositório de resposta: desafios/tarefa_n, onde n é a tarefa sendo respondida.

### Tarefa 1
Faça a análise de complexidade Big-O de tempo e espaço do seu código e documente seus resultados.

### Tarefa 2
Faça um profile de tempo e memória do seu código, gerando gráficos de chama para a execução dele. Sugerimos que a análise de memória seja realizada com a biblioteca bloomberg/memray.

### Tarefa 2b
Realize apenas após ter completado as tarefas 1 e 2: compare os resultados obtidos pelo profiling com os resultados da sua análise de complexidade.

### Tarefa 3
Otimize a realocação de pedidos, de forma que ao ser realocado, a distância total "km_rota" seja mínima para cada pedido realocado (compute a distância total a partir da unidade original do pedido até a entrega).

### Tarefa 4
Implemente processamento dos arquivos de forma paralela com a standard lib do python. Considere criar um worker/cpu core. Importante: o resultado deve ser um único arquivo de output.

## Descrição dos Dados
### Municípios
| Município - UF                 |   COD mun |
|:-------------------------------|----------:|
| Caetanos - BA                  |   2905156 |
| Durande - MG                   |   3123528 |
| Sao Jose do Herval - RS        |   4318465 |
| Santo Antonio do Sudoeste - PR |   4124400 |
| Ivatuba - PR                   |   4111605 |

### Distâncias
|     org |    dest | km_linear          | km_rota      |
|--------:|--------:|:-------------------|:-------------|
| 1100015 | 1100015 | mesma cidade       | mesma cidade |
| 1100015 | 1100098 | 170.44938201864124 | 145.0        |
| 1100015 | 1100130 | 326.2857946406476  | 388.0        |
| 1100015 | 1100148 | 71.24773383984993  | 61.0         |
| 1100015 | 1100296 | 63.64018959020441  | 26.0         |

Colunas:
- org: código do município de origem
- dest: código do município de destino
- km_linear: comprimento do arco na superfície da terra, ligando os pontos org e dest
- km_rota: comprimento da rota terrestre (estrada) ligando os municípios

> Para origem e destino na mesma cidade considere a distância de 10km.

Infelizmente ao extrair os dados do excel, o estagiário cometeu um erro e trocou alguns valores de distâncias...
Será preciso trocar os valores de "km_linear" e "km_rota" sempre que "km_linear" > "km_rota".


### Hubs
| Nome HUB                        |   COD inicial |   COD final |   Capacidade Entrega |
|:--------------------------------|--------------:|------------:|---------------------:|
| HUB Anori Primário              |       1039451 |     5425086 |                  523 |
| CD Caracol Primário             |       1260883 |     2774732 |                  317 |
| TP Patos do Piaui Secundário    |       1256429 |     4124588 |                  181 |
| CD Entre Rios de Minas Auxiliar |       3075667 |     3687187 |                  257 |
| CD Nova Santa Helena Primário   |       3458663 |     5342929 |                  211 |

Colunas:
- Nome HUB: nome da unidade de distribuição
- COD inicial: código do primeiro município atendido pela unidade
- COD final: código do último município atendido pela unidade
- Capacidade Entrega: quantidade de pedidos que podem ser enviados pela unidade em um dia

> A coluna "Nome HUB" está sempre no formato `f"{tipo} {municipio} {classe}"`, utilize essa informação para identificar os códigos dos municípios de cada hub.

### Dados dos pedidos
|   Id Pedido |     COD org |    COD dest |
|------------:|------------:|------------:|
|           0 |      312391 |      410030 |
|           1 |      312391 |      311430 |
|           2 |      350491 |      120043 |
|           3 |      251081 |      353570 |
|           4 |      316671 |      410754 |


## Forma de entrega:
- Submeta o código em um repositório público, sem os dados
- A execução deve ser realizada a partir do arquivo main.py na raiz do projeto
- Caso julgue necessário, qualquer documentação extra da sua solução deve ser colocada
- Formate seu código com a biblioteca `black` utilizando a configuração padrão

### Dicas para ter uma boa avaliação:
- Crie um código limpo e bem organizado em módulos e classes coerentes
- Lembre-se que outras pessoas devem ler o seu código e serem capazes de entendê-lo de forma rápida
- Considere e aplique boas práticas que conhecer
- Prefira sempre um código claro e auto explicativo
- Utilize laços com cautela, preferindo operações pandas e numpy sempre que possível
- Antes de começar, `python -m this`
- Acompanhe esse repositório para eventuais atualizações na descrição da tarefa
- Qualquer dúvida entre em contato com nosso RH

