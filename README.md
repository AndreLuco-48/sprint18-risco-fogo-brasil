# Classificação Experimental do Risco de Fogo no Brasil

Projeto Final — MBA em Ciência de Dados e Inteligência Artificial

## 1. Visão geral

Este projeto investiga a classificação de níveis de risco de fogo
a partir de informações meteorológicas anteriores e características
territoriais históricas de municípios brasileiros.

A solução utiliza aprendizado de máquina como proposta de
desenvolvimento, com classificação em três categorias:
Baixo, Médio e Alto.

A Etapa 1 contempla preparação dos dados, integração das fontes,
análise exploratória e teste de hipótese estatística.

## 2. Problema de negócio

Informações antecipadas sobre condições favoráveis ao fogo na
vegetação podem auxiliar atividades de monitoramento e prevenção.

O objetivo experimental é estimar categorias de um indicador de
risco de fogo para um município e dia, utilizando informações
disponíveis até o dia anterior.

O estudo não constitui uma previsão validada da ocorrência
de incêndios.

## 3. Fontes de dados

Foram utilizadas quatro bases disponibilizadas para o projeto:

- Queimadas: registros de focos detectados em 2024 e 2025.
- BDMEP-2025: medições meteorológicas de janeiro a junho de 2025.
- BDMEP-estacao: cadastro de estações meteorológicas.
- MapBiomas: informações municipais de cobertura e uso da terra
  entre 1985 e 2021.

Como referência espacial complementar, foi utilizada a
Malha Municipal Digital de 2024 do IBGE.

## 4. Preparação e integração

As bases foram investigadas quanto a estrutura, duplicações,
valores ausentes e compatibilidade temporal e geográfica.

Os registros meteorológicos foram consolidados por estação,
data e hora, preservando informações complementares.

Foram construídos indicadores diários de temperatura,
precipitação e umidade.

As medições meteorológicas foram associadas ao dia seguinte
para respeitar o momento estabelecido para a previsão.

Cada município foi associado à estação meteorológica
geograficamente mais próxima, utilizando a distância de Haversine.

As informações territoriais do MapBiomas foram incorporadas
utilizando o último ano disponível, 2021.

## 5. Base consolidada

A versão preparada possui:

- 19.206 observações município-dia.
- 45 colunas.
- Nenhuma combinação município-dia duplicada.
- Nenhuma classificação de risco ausente.

Arquivo: dados/processados/base_consolidada_v2.csv

## 6. Variável-alvo candidata

A variável classe_risco foi construída a partir do maior valor
diário do indicador risco_fogo entre os focos detectados
em cada município.

Categorias adotadas:

- Baixo: 0,00 a 0,40.
- Médio: acima de 0,40 até 0,70.
- Alto: acima de 0,70 até 1,00.

As três categorias representam uma adaptação metodológica
de níveis de risco utilizados como referência pelo INPE.

A variável-alvo permanece sujeita à validação metodológica
definitiva.

## 7. Resultados exploratórios

Distribuição das categorias:

- Baixo: 10.500 observações (54,67%).
- Médio: 1.943 observações (10,12%).
- Alto: 6.763 observações (35,21%).

As análises identificaram, nas categorias superiores de risco:

- Temperaturas médias anteriores mais elevadas.
- Umidade relativa mínima anterior mais baixa.
- Menor precipitação média no dia anterior.

Também foram observadas mudanças relevantes na distribuição
das categorias ao longo dos 43 dias analisados.

Esses resultados representam associações descritivas,
não relações causais.

## 8. Teste de hipótese estatística

Foi investigada a diferença média diária de temperatura
entre as categorias Alto e Baixo.

A análise considerou 42 dias e utilizou regressão com
erros-padrão HAC (Newey-West), devido à autocorrelação temporal.

Resultados principais:

- Diferença média: +1,313 °C.
- Erro-padrão HAC: 0,232.
- Valor-p: inferior a 0,000001.
- Intervalo de confiança de 95%: [0,859; 1,768] °C.

A hipótese nula foi rejeitada ao nível de significância de 5%.

A conclusão permaneceu consistente na análise de sensibilidade
com 1, 3, 5 e 7 defasagens.

## 9. Indicador de desempenho

O KPI principal proposto para a modelagem é o F1-score macro.

Indicadores complementares:

- Recall da classe Alto.
- Precisão e recall por classe.
- Acurácia.
- Matriz de confusão.

A meta quantitativa será definida após a construção
de um modelo de referência.

## 10. Limitações

A base de queimadas contém registros associados a focos
detectados, não representando todos os municípios e dias
sem detecções.

O indicador de risco utilizado como alvo já é calculado
pela fonte original.

A cobertura temporal simultânea das bases é limitada.

A associação meteorológica considera a estação mais próxima,
o que não garante representatividade local.

As características territoriais utilizadas correspondem a 2021,
enquanto as observações de risco são de 2025.

Os resultados não devem ser interpretados como previsão direta
da ocorrência de incêndios.

## 11. Organização do projeto

- dados/originais/: arquivos recebidos.
- dados/processados/: bases consolidadas.
- dados/auxiliares/: referências espaciais e SQLite.
- notebooks/: código, preparação e análises.
- relatorios/: documentos complementares.

Notebook principal:
notebooks/Sprint18_Etapa1_Entrega.ipynb

## 12. Próximas etapas

- Revisar a seleção dos atributos explicativos.
- Validar definitivamente a abordagem da variável-alvo.
- Comparar três modelos de classificação.
- Avaliar desempenho com separação cronológica.
- Investigar interpretabilidade e possíveis vieses.

## 13. Situação

Etapas 1, 2 e 3 — Desenvolvimento analítico concluído.

Revisão de reprodutibilidade e submissão acadêmica pendentes.

## 14. Instruções de reprodução

### Ambiente utilizado

- Python 3.13
- JupyterLab
- pandas
- NumPy
- Matplotlib
- SciPy
- scikit-learn
- GeoPandas
- statsmodels

### Preparação dos dados

Os quatro arquivos originais devem estar disponíveis
na pasta dados/originais/:

- queimadas_2024_2025.csv
- MapBiomas.csv
- BDMEP-estacao.csv
- BDMEP-2025.csv

A malha municipal do IBGE de 2024 deve ser colocada
na pasta dados/auxiliares/.

Esses arquivos não são distribuídos automaticamente
com o repositório.

### Execução

1. Preparar o ambiente Python com as bibliotecas necessárias.
2. Disponibilizar os arquivos nas pastas indicadas.
3. Abrir o notebook Sprint18_Etapa1_Entrega.ipynb.
4. Executar as células em ordem, verificando os resultados.
5. Consultar as bases consolidadas na pasta dados/processados/.

### Observações sobre reprodutibilidade

O notebook contém registros das etapas exploratórias,
incluindo diagnósticos, correções e preparação dos dados.

Algumas células dependem de variáveis ou arquivos
gerados em etapas anteriores.

A reprodução integral deverá ser validada antes
da entrega definitiva.

Os resultados estatísticos apresentados correspondem
ao conjunto de dados e às configurações utilizadas
durante o desenvolvimento.

### Histórico de desenvolvimento

O notebook de entrega apresenta a versão organizada
para avaliação da Etapa 1.

O notebook original contém o histórico completo do
desenvolvimento, incluindo investigações, tentativas
iniciais e correções realizadas durante o projeto.

A reprodutibilidade integral da versão de entrega
ainda está sujeita à validação de execução.

## 15. Estado da reprodutibilidade

A leitura independente da base consolidada v2 foi validada,
confirmando 19.206 observações, 45 colunas, ausência de
duplicações município-dia e integridade da classificação.

A execução integral do notebook de entrega, desde os dados
originais em um ambiente limpo, ainda não foi validada.

As bases originais e os arquivos auxiliares volumosos não
estão incluídos neste repositório. Sua obtenção e organização
são necessárias para reproduzir todas as etapas.

A versão publicada representa o desenvolvimento da Etapa 1,
com suas decisões metodológicas e limitações documentadas.

## 16. Etapa 2 — Modelagem e avaliação

Foram desenvolvidos três modelos de classificação:
Regressão Logística, Árvore de Decisão e Random Forest.

O DummyClassifier foi utilizado como referência.

### Resultados na validação

| Modelo | Acurácia | F1-macro | ROC AUC OvR |
| --- | ---: | ---: | ---: |
| Regressão Logística | 0,5051 | 0,4022 | 0,6238 |
| Árvore de Decisão | 0,6020 | 0,4820 | 0,7119 |
| Random Forest | 0,6795 | 0,5225 | 0,7679 |

O Random Forest apresentou o melhor F1-macro na validação.

### Avaliação no teste

O Random Forest inicial apresentou:

- Acurácia: 0,6363.
- F1-macro: 0,4783.
- Recall da categoria Alto: 0,5618.
- ROC AUC OvR: 0,7061.

O desempenho foi inferior ao observado na validação.

### Análise econômica

Foi realizada uma simulação financeira hipotética,
com redução de 40,22% nas penalidades atribuídas
aos erros do Random Forest em relação ao baseline.

Os valores não representam economia real.

Também foi elaborado um planejamento de teste A/B.
Nenhum experimento operacional foi executado.

## 17. Etapa 3 — Otimização e interpretabilidade

### Otimização com Optuna

Foram realizados 20 experimentos de otimização
do Random Forest.

O F1-macro de validação aumentou de 0,5225 para 0,5383.

Melhores hiperparâmetros:

- n_estimators: 75.
- max_depth: 16.
- min_samples_leaf: 5.
- min_samples_split: 12.

O ganho foi observado na validação, não constituindo
comprovação independente de melhoria de generalização.

### Interpretabilidade SHAP

Os três atributos de maior importância global foram:

1. area_classe_39: 0,0447.
2. temperatura_media: 0,0418.
3. area_classe_3: 0,0333.

A análise identifica contribuições para as previsões
do modelo, sem estabelecer relações causais.

### Análise de vieses

Foram investigadas diferenças de desempenho:

- Entre faixas de distância até a estação meteorológica.
- Entre grupos com diferentes disponibilidades de
  informações meteorológicas.

Foram observadas diferenças no F1-macro e no recall
da categoria Alto.

Os resultados são exploratórios e não constituem
uma auditoria completa de equidade geográfica.

## 18. Notebooks do projeto

- notebooks/Sprint18_Etapa1_Entrega.ipynb
- notebooks/Sprint18_Etapa2_Modelagem.ipynb
- notebooks/Sprint18_Etapa3_Otimizacao_Interpretabilidade.ipynb

## 19. Considerações finais

O projeto desenvolveu um fluxo experimental de
classificação multiclasse, incluindo preparação
de dados, avaliação temporal, otimização,
interpretabilidade e análise de diferenças
de desempenho entre grupos.

Os resultados permanecem restritos aos municípios-dia
representados na base de detecções.

O modelo não constitui uma solução operacional
validada de previsão da ocorrência de incêndios.

## 20. Estado atual do projeto

As três etapas analíticas previstas foram desenvolvidas:

- Etapa 1: preparação, integração e análise exploratória.
- Etapa 2: modelagem, avaliação e planejamento experimental.
- Etapa 3: otimização, interpretabilidade e análise de vieses.

Os notebooks revisados estão disponíveis no repositório.

A reprodução integral do fluxo a partir dos dados originais
ainda não foi comprovada em um ambiente independente.

Os resultados devem ser interpretados considerando as
limitações metodológicas documentadas.
