# PROJETO 4 - FICHA TÉCNICA

# Ficha Técnica: Projeto Análise de Dados 4

## Título do Projeto: Atrasos de Voo

---

## Objetivo

Uma startup  está projetando um aplicativo de monitoramento de voos para facilitar o público no acompanhamento de suas viagens. Buscando compreender melhor quais são os principais fatores de atrasos nos voos, a startup buscou a DataLab para analisar as movimentações aéreas de Janeiro de 2023 nos Estados Unidos e a partir disso ajudá-los a traçar estratégias de melhoria no app. 

Inicialmente, a startup levantou quatro hipóteses para validarmos:

- O clima é o principal motivo de atrasos? (PARCIALMENTE REFUTADO)
- Voos mais longos apresentam maior risco de atrasos na chegada? (REFUTADA)
- Companhias com mais voos tem maior risco de atraso? (PARCIALMENTE CONFIRMADA)
- Voos partindo na madrugada apresentam menor risco de atraso? (CONFIRMADA)

Dessa forma, temos como objetivo principal analisar os diferentes fatores que contribuem para os atrasos de voo e traçar estratégias que podem ser implementadas pelo app para uma melhor visualização dos usuários de possíveis maiores riscos de atraso de seus voos. 

---

## Equipe

Débora Vasconcellos e Mayara Alves

---

## Ferramentas e Tecnologias

- Google Colab
- Power BI

Linguagem: Python

---

## Processamento e Análises

---

### Limpeza dos dados

***Tabela “flights_202301”***:

Identificamos:
9978 nulos na variável “DEP_TIME”,

9982 nulos na variável “DEP_DELAY”,

10197 nulos na variável “WHEELS_OFF”,

10519 nulos na variável “WHEELS_ON”,

10519 nulos na variável “ARR_TIME”,

11640 nulos na variável “ARR_DELAY”,

11640 nulos na variável “ELAPSED_TIME”,

11640 nulos na variável “AIR_TIME”,

1 nulo na variável “CRS_ELAPSED_TIME”.
Solucionamos:
Esses casos optamos pela exclusão das linhas nulas, já que diante das perguntas de negócio precisávamos de todos os horários para compreender as dinâmicas de voo e seus possíveis atrasos.

Identificamos:

422124 nulos na variável “DELAY_DUE_CARRIER”,

422124 nulos na variável “DELAY_DUE_WEATHER”,

422124 nulos na variável “DELAY_DUE_NAS”,

422124  nulos na variável “DELAY_DUE_SECURITY”,

422124 nulos na variável “DELAY_DUE_LATE_AIRCRAFT”.

Solucionamos: 

Optamos por substituir os valores nulos pelo valor 0, por entendermos que os valores nulos estariam relacionados aos voos que não tiveram registro de tempo de atraso por esses fatores. 

Remoção de colunas:

CANCELLED 

CANCELLATION_CODE

TAXI_IN

TAXI_OUT

WHEELS_ON

WHEELS_OUT

DIVERTED

Optamos por retirar essas colunas, por não estarem relacionadas as perguntas de negócio que iremos responder no decorrer da análise.

Transformação de variáveis:  

CRS_DEP_TIME
DEP_TIME'
WHEELS_OFF
WHEELS_ON'
CRS_ARR_TIME
ARR_TIME

Optamos pela transformação dessas variáveis pois estavam como INT64 e FLOAT64, e queremos utilizar elas como data, dessa forma alteramos para o formato de hh:mm que é OBJECT. Após a transformação, identificamos 358 casos em que a hora estava registrada como 24:00 e convertemos para 00:00. 

Transformação tipo de variáveis FLOAT64 para INT64:

'DEP_DELAY',
'ARR_DELAY',
'CRS_ELAPSED_TIME',
'ELAPSED_TIME',
'AIR_TIME',
'DELAY_DUE_CARRIER',
'DELAY_DUE_WEATHER',
'DELAY_DUE_NAS',
'DELAY_DUE_SECURITY',
'DELAY_DUE_LATE_AIRCRAFT'

Optamos pela transformação devido o caráter inteiro dos números dessas variáveis já que medem os minutos de duração dos atrasos, tempo de voo e tempo no ar, sendo desnecessária a leitura de decimais.  

**Tabela “AIRLINE_CODE_DICTIONARY”**
Convertemos os nomes das colunas para posterior identificação na união das tabelas

Code': 'AIRLINE_CODE', 'Description': 'AIRLINE_NAME’.

**Tabela “DOT_CODE_DICTIONARY”**

Convertemos os nomes das colunas para posterior identificação na união das tabelas

'Code': 'DOT_CODE', 'Description': 'DOT_NAME’ 

---

### Análise Exploratória, Novas Variáveis e Risco Relativo

### Variáveis Gerais

***Tabela “flight_analysis”*** 

Após unificação das tabelas anteriores, criamos uma nova tabela com todas as informações, além de termos criado novas variáveis:

***“MAIN_CAUSE_DELAY”***: variável categórica indicando qual foi o principal motivo de atraso (ou ausência de atraso) dos voos registrados. 

***“DELAY_FLAG”***: variável binária indicando 0 ausência de qualquer tipo de justificativa de atraso e 1 indicando que houve qualquer tipo de atraso.

***“HAS_DELAY”***: variável binária indicado 0 ausência de atrasos e 1 indicando atrasos, entendendo como atrasos voos que tiveram mais de 10 minutos na variável DEP_DELAY. 

***“FLIGHT_ROUTE”***: concatenação das cidades de origem e destino para visualização das rotas.

***Tabelas auxiliares:***

***Tabela “tabela_dest_city”***

Agrupamos as seguintes variáveis pela unidade de análise “cidade destino (DEST_CITY)”:

***“TOTAL_FLIGHTS”***: contagem total de voos por destino.

***“TOTAL_DEP_DELAY”***: soma da diferença em minutos da saída programada para a saída real de todos os voos por destino.

***“TOTAL_ARR_DELAY”***: soma da diferença em minutos da chegada programada para a chegada real de todos os voos por destino.

***“TOTAL_DELAY_DUE_CARRIER”***: soma dos minutos de atrasos causados pela companhia aérea de todos os voos por destino.

***“TOTAL_DELAY_DUE_WEATHER”***: soma dos minutos de atrasos causados pelo clima de todos os voos por destino.

“***TOTAL_DELAY_DUE_NAS***”: soma dos minutos de atrasos do Sistema Aéreo Nacional 

“***TOTAL_DELAY_DUE_SECURITY***”:  soma dos minutos de atrasos por segurança.

“***TOTAL_DELAY_DUE_LATE_AIRCRAFT***”: soma dos minutos de atrasos devido a problemas na aeronaves.

“***TOTAL_DISTANCE***”: soma da distância entre os aeroportos.

“***TOTAL_AIR_TIME***”: soma do tempo de voo no ar em minutos.

***“TOTAL_HAS_DELAY”***: soma de atrasos, entendendo como atrasos voos que tiveram mais de 10 minutos na variável DEP_DELAY. 

***Tabela “tabela_origin_city”***

Agrupamos as seguintes variáveis pela unidade de análise “cidade origem (ORIGIN_CITY)”:

***“TOTAL_FLIGHTS”***: contagem total de voos por origem.

***“TOTAL_DEP_DELAY”***: soma da diferença em minutos da saída programada para a saída real de todos os voos por origem.

***“TOTAL_ARR_DELAY”***: soma da diferença em minutos da chegada programada para a chegada real de todos os voos por origem.

***“TOTAL_DELAY_DUE_CARRIER”***: soma dos minutos de atrasos causados pela companhia aérea de todos os voos por origem.

***“TOTAL_DELAY_DUE_WEATHER”***: soma dos minutos de atrasos causados pelo clima de todos os voos por origem.

“***TOTAL_DELAY_DUE_NAS***”: soma dos minutos de atrasos causados pelo Sistema Aéreo Nacional 

“***TOTAL_DELAY_DUE_SECURITY***”: soma dos minutos de atrasos por segurança.

“***TOTAL_DELAY_DUE_LATE_AIRCRAFT***”: soma dos minutos de atrasos devido a problemas na aeronaves.

“***TOTAL_DISTANCE***”: soma da distância entre os aeroportos.

“***TOTAL_AIR_TIME***”: soma do tempo de voo no ar em minutos.

***“TOTAL_HAS_DELAY”***: soma de atrasos, entendendo como atrasos voos que tiveram mais de 10 minutos na variável DEP_DELAY. 

***Tabela “tabela_airline_name”***

Agrupamos as seguintes variáveis pela unidade de análise “nome da companhia aérea (AIRLINE_NAME)”:

***“TOTAL_FLIGHTS”***: contagem total de voos por companhia.

***“TOTAL_DEP_DELAY”***: soma da diferença em minutos da saída programada para a saída real de todos os voos por companhia.

***“TOTAL_ARR_DELAY”***: soma da diferença em minutos da chegada programada para a chegada real de todos os voos por companhia.

***“TOTAL_DELAY_DUE_CARRIER”***: soma dos minutos de atrasos causados pela companhia aérea.

***“TOTAL_DELAY_DUE_WEATHER”***: soma dos minutos de atrasos causados pelo clima de todos os voos por companhia.

“***TOTAL_DELAY_DUE_NAS***”: soma dos minutos de atrasos causados pelo Sistema Aéreo Nacional 

“***TOTAL_DELAY_DUE_SECURITY***”: soma dos minutos de atrasos por segurança.

“***TOTAL_DELAY_DUE_LATE_AIRCRAFT***”: soma dos minutos de atrasos devido a problemas na aeronaves.

“***TOTAL_DISTANCE***”: soma da distância entre os aeroportos.

“***TOTAL_AIR_TIME***”: soma do tempo de voo no ar em minutos.

***“TOTAL_HAS_DELAY”***: soma de atrasos, entendendo como atrasos voos que tiveram mais de 10 minutos na variável DEP_DELAY. 

---

### Risco Relativo

Efetuamos a divisão dos quartis e, em seguida, o cálculo do risco relativo das variáveis abaixo:

CRS_ELAPSED_TIME - ATRASO NA PARTIDA

0    0.890142
1    0.946024
2    1.057305
3    1.109881

ELAPSED_TIME - ATRASO NA PARTIDA

0    0.855027
1    0.953301
2    1.074131
3    1.120793

AIR_TIME - ATRASO NA PARTIDA

0    0.867519
1    0.963486
2    1.075029
3    1.096855

DISTANCE - ATRASO NA PARTIDA

0    0.864634
1    0.951556
2    1.066337
3    1.118851

DISTANCE - ATRASO NA CHEGADA

0    0.913450
1    0.967090
2    1.047581
3    1.072735

CAT_CRS_DEP_TIME - ATRASO NA PARTIDA

Afternoon     1.145789
Evening       1.301113
Late Night/Early Morning    0.561376
Morning       0.725227

CAT_CRS_ARR_TIME - ATRASO NA CHEGADA

Afternoon     0.996792
Evening       1.287510
Late Night/Early Morning    1.440561
Morning       0.618939

MAIN_CAUSE_DELAY
Aircraft                     3.952349
Carrier                      3.669671
National Aviation Service    1.595006
Security                     3.902573
Weather                      3.819766
Without Delay                0.374594

---

### Regressão Linear

A regressão Linear é uma ferramenta estatística que nos auxilia na previsibilidade de fenômenos, compreendendo que uma variável influencia no comportamento de outra. Com os dados de atraso na partida (independente), podemos prever com muita assertividade o atraso da chegada (dependente). Dessa forma, essa informação pode ser útil para que no aplicativo seja informado um horário atualizado ao passageiro do seu novo horário de chegada.

R de Pearson = 0.9663163069527376, p-value = 0.0, r² = 0.9288684710692274

![Untitled](PROJETO%204%20-%20FICHA%20TE%CC%81CNICA%2028798f31b6ec4b6b87c63d2d0850e9b5/Untitled.png)

---

### Regressão Logística

Pensamos em utilizar a técnica de regressão logística para auxiliar na previsibilidade de atrasos nos voos. 

Quando aplicamos a técnica inicialmente, utilizamos as variáveis:

“DELAY_DUE_CARRIER”, “DELAY_DUE_WEATHER”, “DELAY_DUE_LATE_AIRCRAFT”, “DELAY_DUE_SECURITY”, “DELAY_DUE_NAS”. 

O modelo obteve um bom resultado preditivo, entretanto compreendemos que a natureza dessas variáveis não seriam preditivas pois elas seriam as justificativas de atrasos já ocorridos. Dessa forma, optamos por retirá-las e desenvolvendo um segundo modelo.

Como resultado, optamos por incluir apenas três variáveis independentes em nosso modelo: 'DAY_OF_WEEK_BINARY' (1 para os dias da semana com maior risco de atrasos, 0 para os com menor risco ), 'CAT_CRS_DEP_TIME_BINARY' (1 para os turnos com maior risco, 0 para os com menor risco) e 'FLIGHT_ROUTE_BINARY' (1 para rotas com taxa de 50% de atraso, 0 para as com taxa menor que 50%), com a variável dependente sendo 'HAS_DELAY'. Devido ao desbalanceamento dos dados, aplicamos a técnica SMOTE. 

Devido as limitações do banco, como: 

- Conter apenas os registros do mês de janeiro de 2023, ou seja, ausência de uma série histórica;
- Ausência de dados meteorológicos (velocidade do vento, precipitação, etc.);
- Ausência de dados sobre o tamanho/porte dos aeroportos.

Concluímos que a nossa regressão não atingiu a acuracidade necessária para ser utilizada de modo a ter uma melhor predição. Sendo assim, recomendamos que sejam captados os dados destacados acima  para um melhor desempenho do modelo.  

True Positive: 28125
False Positive: 64397
False Negative: 11629
True Negative: 54009
Acurácia: 0.519309559939302
Precisão: 0.3039817556905385
Recall: 0.7074759772601499
F1-score: 0.4252472103782999
AUC ROC: 0.6059685615850862
Log Loss: 17.325839609009993

![Untitled](PROJETO%204%20-%20FICHA%20TE%CC%81CNICA%2028798f31b6ec4b6b87c63d2d0850e9b5/Untitled%201.png)

![Untitled](PROJETO%204%20-%20FICHA%20TE%CC%81CNICA%2028798f31b6ec4b6b87c63d2d0850e9b5/Untitled%202.png)

---

## Resultados e Conclusões

### Hipótese 1  **|  Risco Relativo**

HIPÓTESE 1: O clima é o principal motivo de atrasos?

O clima é um importante fator a se considerar quando pensamos no transporte aéreo. As condições climáticas são determinantes para que o avião tenha a circunstância adequada para voo. 

Observando os dados disponíveis, percebemos que o clima não é o principal motivo de atrasos. Sendo o motivo mais frequente o atraso decorrente da preparação/liberação das aeronaves. O clima se encontra no terceiro maior risco de atrasos com as variáveis disponíveis. 

Entretanto, apesar dos minutos registrados de atrasos decorrentes de questões climáticas não serem os maiores, entendemos que a questão necessita de uma melhor avaliação. Com o cruzamento de dados de precipitação, pressão atmosférica, velocidade do vento e condição do tempo em uma séria histórica para avaliar o quanto tais indicadores interferem.

Além disso, é necessário ter maior compreensão sobre o preenchimento dos dados de justificativa de atrasos por minuto, pois ao observarmos um outlier identificamos uma possível incongruência no preenchimento dos campos.

O caso foi do voo FL-NUMBER (369) da American Airlines do dia 03 de janeiro de 2023 com rota Santa Ana (CA) - Nova Iorque (NY) que registrou 2554 minutos de atraso. Os minutos de atraso foram justificados como tendo sido referentes a própria operadora do voo. Entretanto, entre os dias 01 e 03 de janeiro o estado da Califórnia passou por uma tempestade. O que nos deixou o questionamento de se de fato todos os minutos de atraso foram por conta da operadora como registrado.

Além disso, as mudanças climáticas tem afetado cada vez mais os nossos dias, com a presença de fenômenos mais intensos. Há exemplo de tempestades mais fortes e mais longas, queimadas causadas por secas extremas e que comprometem a visibilidade aérea, maior incidência de furacões, nevascas mais rigorosas, entre outras catástrofes. 

Dessa forma, interpretamos que a hipótese está PARCIALMENTE REFUTADA.

Fonte: [Lessons learned from Northern California's deadly storms in January 2023 (kcra.com)](https://www.kcra.com/article/northern-california-january-2023-storms-response/46301535)

[Another effect of climate change? More flight delays and cancellations. - CBS News](https://www.cbsnews.com/news/climate-change-flight-delays-and-cancellations-travel/)

[Aeroporto de Porto Alegre: quem deve pagar por danos de inundação no local, fechado há 70 dias - BBC News Brasil](https://www.bbc.com/portuguese/articles/cz7eyw7g314o)

---

### Hipótese 2  **|  Correlação de Spearman**

HIPÓTESE 2: Voos mais longos apresentam maior risco de atrasos na chegada?

A partir do cálculo de risco relativo, percebemos que havia uma diferença muito pequena  (1.072735) que não justifica uma interpretação de que há maior risco. Além disso, realizamos o teste de correlação de Spearman (optamos por esse teste devido a ausência de normalidade nos dados) que apontou uma ausência de correlação entre as variáveis DISTANCE e ARR_DELAY (diferença do tempo real e previsto na chegada do voo). 

Correlação de Spearman: -0.0019264521639428727
Valor-p: 0.16188413082814462

Dessa forma, a hipótese foi REFUTADA.

---

### **Hipótese 3  |  Teste de Mann-Whitney U | Taxa de Atrasos**

HIPÓTESE 3: Companhias com mais voos tem maior risco de atraso?

Realizamos o teste de Mann-Whitney para verificar se companhias com mais voos têm mais atrasos que aquelas que possuem menos voos. Dessa forma, podermos determinar se há diferença estatisticamente significativa. Abaixo estão os resultados obtidos.

Teste de Mann-Whitney: 0.0

Valor-p: 0.001465

Há diferença estatisticamente significativa. Entretanto, apesar das companhias com mais voos terem mais atrasos, questionamos se esse maior número refletia proporcionalmente entre companhias maiores e menores. Dessa forma, calculamos a taxa média e obtivemos o seguinte resultado:

Taxa de atrasos por operadora % (decrescente):

| AIRLINE_NAME | TAXA DE ATRASO |  |
| --- | --- | --- |
| Frontier Airlines Inc.  | 40.590895 |  |
| Spirit Air Lines | 33.164699 |  |
| JetBlue Airways |  29.750196 |  |
| Allegiant Air  | 29.676811 |  |
| Southwest Airlines Co. | 28.226864 |  |
| United Air Lines Inc.  | 26.562333 |  |
| Hawaiian Airlines Inc. | 25.000000 |  |
| Delta Air Lines Inc. | 24.144372 |  |
| American Airlines Inc.   | 23.887059 |  |
| Alaska Airlines Inc. | 22.903043 |  |
| SkyWest Airlines Inc. |  22.251850 |  |
|  Envoy Air   | 21.609823 |  |
| Endeavor Air Inc.  | 20.899201 |  |
| Republic Airline  |  14.807268 |  |
| PSA Airlines Inc.  | 14.427959 |  |

Companhias Aéreas por Ordem Decrescente de Total de Voos

| AIRLINE NAME | TOTAL VOOS | TOTAL ATRASOS |
| --- | --- | --- |
| Southwest Airlines Co.  | 108999 |  30767 |
| Delta Air Lines Inc.  | 74419 |  17968  |
| American Airlines Inc | 73454 |  17546   |
| United Air Lines Inc. | 56102 | 14902 |
| SkyWest Airlines Inc. | 48378 | 10765 |
| Republic Airline | 24049 | 3561  |
| JetBlue Airways | 22978 | 6836  |
| Spirit Air Lines | 21348 | 7080 |
| Alaska Airlines Inc | 19421 | 4448 |
| Envoy Air | 18325 | 3960  |
| Endeavor Air Inc | 16637 | 3477 |
| PSA Airlines Inc. | 15165 | 2188 |
| Frontier Airlines Inc. | 12828 | 5207 |
| Allegiant Air | 8478 | 2516 |
| Hawaiian Airlines Inc. |  6616 | 1654  |

Sendo assim, apesar das operadoras com mais voos terem mais atrasados, proporcionalmente isso não se reflete por isso optamos por PARCIALMENTE CONFIRMAR.

---

### **Hipótese 4  |  Teste Z-Score**

HIPÓTESE 4: Voos partindo na madrugada apresentam menor risco de atraso?

Optamos por realizar o teste Z, já que o Z-Score é uma medida que indica quantos desvios padrão uma estatística se encontra de distância da média. Dessa forma, o utilizamos para verificar se existe uma diferença significativa entre as proporções de atrasos durante a madrugada e fora desse período . Abaixo estão os resultados ob

Proporção de atrasos na madrugada: 0.14
Proporção de atrasos fora da madrugada: 0.26
Z-Score: -31.80
Valor-p: 0.0000

Esse valor de Z-Score nos informa que a diferença entre as duas proporções é muito grande e está muito longe da média esperada se não houvesse de fato uma diferença entre os horários. E com o p-valor de 0,000 essa diferença é estatisticamente significativa entre as proporções de atraso durante a madrugada e fora dela. Sendo assim, a hipótese foi CONFIRMADA.

---

### Sugestões e Conclusão

Atrasos geralmente estão relacionados a questões operacionais das próprias companhias aéreas, liberação/preparação das aeronaves para embarque, autorização do sistema aéreo, condições climáticas e questões de segurança que envolvem tanto o funcionamento da aeronave quanto fatores de interação social (por exemplo, passageiros portando objetos não autorizados). 

Em janeiro de 2023, os Estados Unidos registraram 527 mil voos nacionais, sendo  128 mil deles registrados com atraso em suas partidas (média de 63 minutos de atraso). O transporte aéreo é fundamental para a sociedade atual e demanda extrema organização e preparo tanto dos aeroportos, quanto dos próprios passageiros que irão embarcar e se deslocar para outras localidades. 

Diante dos dados analisados, compreendemos que para uma aplicativo de monitoramento de voos seja interessante ter acesso à alguns dados, como:

- Dados Meteorológicos em Tempo Real: o clima é o terceiro fator de maior risco de atrasos. Dessa forma, é necessário que o aplicativo monitore e informe os passageiros sobre possíveis atrasos decorrentes do clima e se preparem para eventuais transtornos durante o embarque.
- Dados de Rotas: algumas rotas apresentam maiores taxas de atraso como, por exemplo, Philadelphia, PA - Bangor, ME,  San Francisco, CA - Hayden, CO, Denver, CO - Albany, NY, Newark, NJ - Oakland, CA  e West Palm Beach/Palm Beach, FL - Los Angeles, CA, as rotas citadas não possuem o maior número bruto de atrasos, mas demonstraram uma alta taxa de atrasos. Com isso, passageiros com voos marcados para rotas com taxa de atraso acima de 50% poderiam ser alertados sobre possíveis riscos e aconselhados para terem maior atenção.
- Horário de partida - “Best Time to Travel”:  Confirmamos que voos com partida na madrugada têm menor incidência de atrasos. Portanto, o aplicativo pode indicar, como recomendação aos clientes, que optem por voos nesse horário para minimizar possíveis contratempos. Voos com saída à noite apresentam um maior risco de atraso. Sugerimos que o aplicativo envie notificações indicando ao passageiro que tenha atenção para eventuais mudanças no horário de partida.
- Dias de semana - “Day With Delay”: Observamos que voos com partida nas segundas e quartas-feiras de janeiro de 2023 possuem maior risco de atraso. Dessa forma, o aplicativo pode alertar o passageiro para eventuais atrasos em voos programados nesses dias da semana. Salientamos que a informação corresponde ao mês de janeiro, pois no dia 11  de janeiro de 2023 (quarta-feira) ocorreu uma falha no sistema aéreo estadunidense que resultou na suspensão de todos os voos por uma hora e meia. O que pode ter enviesado os dados para um maior risco nas quartas.
- Companhia aérea: Sugerimos que o aplicativo informe aos clientes sobre as companhias aéreas que têm mais atrasos e o tempo médio desses atrasos. Além disso, também avisar aos clientes as companhias que geralmente cumprem o horário programado de saída ou que até mesmo possuam tendência para saídas mais adiantadas. Indicando através de notificações que o passageiro se adiante no check-in e posicionamento para o embarque naquelas companhias que tendencialmente saem mais cedo.
- Tempo real de atualização dos embarques: Recomendamos que, com base nos minutos computados de atraso, o aplicativo forneça uma previsão atualizada do horário de chegada ao destino. Isso permitirá aos clientes planejar  com maior precisão eventuais mudanças em seu ponto de chegada, como reagendar transporte particular, notificar local de estadia sobre possíveis atrasos no check-in, entre outras coisas.

Fonte:

[https://g1.globo.com/mundo/noticia/2023/01/12/falha-que-paralisou-voos-nos-eua-revela-sistemas-de-trafego-aereo-obsoletos-no-pais.ghtml](https://g1.globo.com/mundo/noticia/2023/01/12/falha-que-paralisou-voos-nos-eua-revela-sistemas-de-trafego-aereo-obsoletos-no-pais.ghtml)

* Consideramos atrasos voos que tiveram uma diferença entre tempo estimado e tempo real de partida maior que 10 minutos. 

---

## Limitações/Próximos Passos

O banco se restringe as informações referentes a janeiro de 2023, limitando as possibilidades de análise histórica dos dados. Com isso, não podemos afirmar com rigor estatístico que os turnos, dias da semana e principais motivos de atraso possuem os mesmos riscos em todos os meses e anos. Seria interessante para a construção de uma série histórica, dados pelo menos de 1 ano ou um intervalo de 3 anos, para comparação. Também está ausente informações presentes na literatura como fundamentais para predição de atrasos, necessitando o uso de recursos como API.

API:

No banco de dados fornecido, não havia informações sobre longitude, latitude, temperatura, velocidade do vento e precipitação. Para suprir essas lacunas, buscamos uma API que pudesse fornecer esses dados. Conseguimos obter dados de longitude e latitude através de uma API gratuita(LocationIQ). No entanto, após inúmeras buscas, não encontramos uma API gratuita que disponibilizasse dados históricos de temperatura e precipitação.

Identificamos uma API que oferece esses dados (Open Weather), mas o custo é de £140, o que ultrapassa o orçamento disponível no momento. Para futuras análises mais detalhadas e precisas do banco de dados, recomendamos alocar um orçamento para adquirir esses dados históricos.

Também buscamos uma API(Aviation Edge) que fornecesse informações sobre o tamanho dos aeroportos, com o objetivo de verificar se os atrasos poderiam estar correlacionados com esse fator. Infelizmente, não encontramos uma API gratuita que disponibilizasse esses dados.

Fonte: 

[Open Access proceedings Journal of Physics: Conference series (iop.org)](https://iopscience.iop.org/article/10.1088/1757-899X/1024/1/012107/pdf)

[EstudoAtrasoVoos.pdf (ufu.br)](https://repositorio.ufu.br/bitstream/123456789/30528/5/EstudoAtrasoVoos.pdf)

[Prediction of flight departure delays caused by weather conditions adopting data-driven approaches | Journal of Big Data | Full Text (springeropen.com)](https://journalofbigdata.springeropen.com/articles/10.1186/s40537-023-00867-5)

---

## Links de interesse

**Google Colab:** 

[https://colab.research.google.com/drive/1nadgon0ZQ8fKcyO_iHxSO4MJKWs32vR7?usp=sharing](https://colab.research.google.com/drive/1nadgon0ZQ8fKcyO_iHxSO4MJKWs32vR7?usp=sharing) (Débora)

[https://colab.research.google.com/drive/1nrIkfJLu989in_UiaY2wh_BhFwGPnIDG?authuser=1#scrollTo=CCnDjUqK7qyR](https://colab.research.google.com/drive/1nrIkfJLu989in_UiaY2wh_BhFwGPnIDG?authuser=1#scrollTo=CCnDjUqK7qyR) (Mayara)

**Dashboard Power BI**: 

https://drive.google.com/file/d/1pYSeDk_RlhJqtyYw0UycIfjZ6fQya0Ix/view?usp=sharing (Débora)

[https://drive.google.com/file/d/1FTop_IAayA3DNnPMnoLEhVQv1RC9iwuo/view](https://drive.google.com/file/d/1FTop_IAayA3DNnPMnoLEhVQv1RC9iwuo/view) (Mayara)

**Slide**:

[https://www.canva.com/design/DAGLBOA3KJU/ejm-KMQksAYOgFFpnpfuZQ/edit?utm_content=DAGLBOA3KJU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton](https://www.canva.com/design/DAGLBOA3KJU/ejm-KMQksAYOgFFpnpfuZQ/edit?utm_content=DAGLBOA3KJU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

**Video**:

https://www.loom.com/share/7fabca3f57834881b148cbd00ff05a8f?sid=bd72e598-5179-45b1-98d3-3e9d597327e0



---

---