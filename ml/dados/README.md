# Dados reais — INMET / A701

Fonte primária: https://portal.inmet.gov.br/dadoshistoricos
Estação pretendida: A701 — São Paulo — Mirante. Conferir o código no cabeçalho.
Período definido: 2021–2025. Dados horários; agregação diária no notebook.

## Download manual, caso o automático falhe

1. Abra o portal e clique nos anos 2021, 2022, 2023, 2024 e 2025.
2. Extraia de cada ZIP apenas o CSV contendo `_A701_` no nome. Não use a estação de outra cidade ou outra estação de São Paulo.
3. Coloque os cinco CSVs originais em `brutos/`, sem abrir/salvar pelo Excel e sem renomear.
4. No Colab, o caminho padrão é `/content/ml/dados/brutos/`; crie as pastas executando a configuração.
5. Execute a célula 2 de obtenção novamente. Havendo duas cópias do mesmo ano, mantenha apenas uma.

Links diretos fornecidos no portal:
- https://portal.inmet.gov.br/uploads/dadoshistoricos/2021.zip
- https://portal.inmet.gov.br/uploads/dadoshistoricos/2022.zip
- https://portal.inmet.gov.br/uploads/dadoshistoricos/2023.zip
- https://portal.inmet.gov.br/uploads/dadoshistoricos/2024.zip
- https://portal.inmet.gov.br/uploads/dadoshistoricos/2025.zip

CSV: geralmente Latin-1 ou UTF-8, separador `;`, decimais com vírgula, metadados antes do cabeçalho. O código procura o cabeçalho e valida A701. Os cinco CSVs reais estão incluídos no pacote final; os ZIPs nacionais não são necessários. Não há dados sintéticos.

## Tratamento

- Horários interpretados em UTC e convertidos para America/Sao_Paulo.
- Máxima da hora anterior atribuída ao intervalo que terminou; 03 UTC pertence ao fim do dia local anterior.
- Para simplificar as médias diárias, as leituras instantâneas de encerramento também são associadas ao intervalo terminado. Assim o dia D contém leituras de 01:00 de D até 00:00 de D+1, disponíveis na emissão ao encerramento de D. Esse alinhamento é intencional.
- Máxima diária válida somente com 24 máximas horárias; médias auxiliares exigem 20 observações válidas.
- Datas sem observação preservadas na grade diária antes de criar lags/alvo.
- Dados ausentes do alvo não são preenchidos; features auxiliares usam mediana aprendida somente no treino.
- Limites de plausibilidade amplos documentados no notebook; não são critérios oficiais de controle de qualidade do INMET.
- O último dia de 2025 pode estar incompleto por falta das primeiras horas UTC de 2026 e será excluído se não cumprir a cobertura.
- Interrupções por baixa cobertura devem ser investigadas; não reduza o limiar de qualidade só para melhorar métricas.

Após execução: `diario_A701.csv`, `base_modelagem.csv`, `resultados/manifesto_fontes.json` (URLs, arquivos, SHA-256 e data de verificação), `resultados/qualidade_por_ano.csv`. Nos CSVs locais o manifesto registra a origem como pendente de confirmação; o download manual deve vir dos links oficiais.

Dados públicos não implicam licença irrestrita de redistribuição. Preserve a atribuição e os links de origem. Os CSVs brutos estão incluídos e não são excluídos pelo .gitignore. O notebook pode usar essas cópias sem depender do portal. Não substitua a fonte por dados simulados.


## Rastreabilidade da entrega final
Os CSVs com nomes originais foram comparados por SHA-256 com os usados no Colab: os cinco conteúdos são idênticos. O manifesto mantém `arquivo_na_execucao_colab` (com sufixo `(1)`) e aponta `arquivo` para o nome final. As saídas antigas do notebook podem mostrar `(1)`; isso não altera os dados. A origem dos arquivos locais segue descrita como local no manifesto, com os links oficiais para conferência.

A limpeza resultou em 1.730 máximas diárias válidas de 1.826 dias. A modelagem usa 1.351 exemplos de treino e 303 de teste, pois exige D e D+1 válidos e expurga a fronteira. As médias instantâneas incluem o registro de encerramento às 00:00 de D+1; a previsão pressupõe sua disponibilidade.
