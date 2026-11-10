# Refresca SP — Inteligência Artificial & Machine Learning

Experimento isolado da Global Solution CitizenTech: prever a temperatura máxima do dia seguinte em São Paulo e avaliar um indicador didático de dia muito quente.

## Equipe

| Integrante | RM |
|---|---|
| Matheus | 555177 |
| Gustavo A. | 559098 |
| Nicolas | 556617 |
| Gustavo C. | 556289 |
| Igor Soos | 556010 |

## Resultado principal

O notebook foi executado com dados reais do INMET, estação A701 (São Paulo — Mirante), de 2021 a 2025. A floresta reduziu o MAE de teste em aproximadamente **9,8%** frente à persistência, mas **não sinalizou nenhum dos cinco dias com máxima ≥35 °C**. O indicador não está apto ao uso operacional.

| Modelo | Conjunto | MAE (°C) | RMSE (°C) | R² |
|---|---|---:|---:|---:|
| Random Forest | Treino | 1,403 | 1,973 | 0,803 |
| Persistência | Treino | 2,420 | 3,439 | 0,402 |
| Random Forest | Teste | 2,295 | 3,050 | 0,556 |
| Persistência | Teste | 2,543 | 3,528 | 0,407 |

Melhores parâmetros: 200 árvores, max_depth=None e min_samples_leaf=5. Busca de 12 combinações de 3 hiperparâmetros, com 3 folds temporais só no treino. MAE de validação: 2,352 °C; persistência: 2,368 °C. Teste reservado em 2025, sem ajuste de limiar pelo teste.

## Arquivos para avaliação

- [Notebook executado, com saídas](ml/refresca_sp_ml.ipynb)
- [Relatório ML](ml/RELATORIO_ML.md)
- [Fonte, tratamento e instruções dos dados](ml/dados/README.md)
- [Dependências](ml/requirements.txt)
- [Resultados numéricos](ml/resultados/metricas.csv) e [indicador de calor](ml/resultados/alertas.json)

A pasta `ml/figuras/` contém quatro gráficos; `ml/dados/brutos/` contém os cinco CSVs; `ml/resultados/` contém busca, folds, previsões, manifesto de fontes e modelo. Os CSVs e o modelo são incluídos no Git. Não é necessário executar o modelo para consultar os resultados salvos.

## Executar no Colab

1. Abra https://colab.research.google.com/ e envie `ml/refresca_sp_ml.ipynb`.
2. As saídas científicas já estão salvas. Para reproduzir, execute a célula de instalação, que não modifica o IPython do Colab. Se solicitado, reinicie a sessão e prossiga pela seção 1.
3. Na seção 2, selecione os cinco CSVs de `ml/dados/brutos/` do pacote. Se já existirem na sessão, o upload será dispensado. Abrir só o notebook não transfere os CSVs automaticamente.
4. Execute as etapas seguintes em ordem. CPU é suficiente. A busca faz 36 ajustes e um refit.
5. Salve o notebook com as saídas. A última célula gera um ZIP dos arquivos de `ml/`, mas não captura automaticamente o notebook aberto no Colab. Baixe também o `.ipynb` pelo menu Arquivo.

Se a sessão ainda estiver afetada pela instalação antiga de IPython, use uma nova sessão limpa antes de reproduzir. Não execute comandos de atualização de IPython no Colab.

## Executar localmente no Windows

A execução original usou Python 3.13.18, NumPy 2.4.6, pandas 2.2.3 e scikit-learn 1.6.1. Use Python 3.13 e abra o terminal na raiz `refresca_sp`:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r ml/requirements.txt
.\.venv\Scripts\python.exe -m jupyter notebook
```

Abra `ml/refresca_sp_ml.ipynb` e use o kernel da venv. Os CSVs já estão na pasta: o upload é dispensado. Os intervalos de versões compatíveis de Matplotlib/Joblib estão documentados no requirements. O modelo joblib foi produzido com scikit-learn 1.6.1; prefira essa versão ao carregá-lo.

## Revisão final e reprodutibilidade

As saídas das etapas de dados, preparação, busca, métricas e alertas foram preservadas da execução original do Colab.

A regra de 35 °C é didática e não define oficialmente onda de calor. Uma estação não representa todos os bairros; somente cinco eventos extremos chegaram ao teste após os filtros. O MAE é erro médio, não garantia por previsão. Mais limitações claras no relatório.

## Compartilhamento e entrega

O Colab mantém salvo os resultados das execuções dos códigos.

- **Link do Colab:** (https://colab.research.google.com/drive/1yw7_ZODMBrZW_QkJiriNP3TOlG-8aCXt?usp=sharing)
- **Vídeo no YouTube:** adicionar após gravar.
