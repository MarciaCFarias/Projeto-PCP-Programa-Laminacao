# 📋 Requisitos do Processo – Faturamento Troca de Notas

## Regras de Negócio

- **Diretórios e nomenclaturas fixos**  
  - **Caminho de rede da área  
  - **Subdiretórios:**  
    - `Robô_Regras de Programação`  
    - `Robô_Regras de Programação\Programa elaborado pelo Robô`  
  - **Arquivos de saída:**  
    - `Baixa Redução_DD.MM.AAAA_SEQ.xlsx`  
    - `Misto_DD.MM.AAAA_SEQ.xlsx`  
    - `Itens rugosidade brilhante DD.MM.AAAA.xlsx`  
    - `LOG Erro Processamento - Elaboração de Programas.xlsx`  

- **Configuração de filtros nos sistemas legados**  
  - **Sistema X:** selecionar `TF – Tiras a Frio` > `Cilindro` > `Quadro – LE - 2`  
  - **sistema Y:**  
    - Período: Início = data execução – 2 dias; Término = data execução  
    - Unidade Operacional  
    - Equipamento  
  - **Sistema Z:** menu `Controle Material à Frio` > `Estoque – Detalhado`; Situação do Rolo = Resfriado  

- **Classificação e separação de itens**  
  - Rugosidade = **PADRÃO** ou **FOSCA**; **BRILHANTE** isolados em planilha separada  
  - Ordenação de colunas:  
    1. Largura: do maior para o menor  
    2. Espessura: do menor para o maior  
    3. Oleamento LE: ordem alfabética A→Z  
    4. Tempo: do menor para o maior (hora/minuto)  

- **Cenários de alongamento ("Alg Min")**  
  - Baixa Redução: todos os itens com Alg Min < 4 %  
  - Misto: combinação de itens com Alg Min < 4 % e ≥ 4 %  
  - Referência de regras em planilha `Regras – formação de cones`, abas `Somente Itens < 4 %` e `Itens misto < 4 % E ≥ 4 %`  

- **Estrutura das planilhas de saída**  
  - Colunas obrigatórias: Rolo, Sequência, Largura, Espes., Oleamento LE, Tempo, Alg Min, Comp (km), Peso (quando aplicável)  
  - Linhas de cabeçalho adicionais:  
    - Data: `DD/MM/AAAA`  
    - Nº Lista Programa: numeração sequencial diária  
    - Nº Programa PCP: preenchimento manual pelo usuário  
    - Totais: KM Itens rugosidade, KM ≥ 4 %, Peso (cenário misto)  

- **Envio de e-mail ao PCP**  
  - Para área responsável  
  - Assunto: `Lista Programa Nº X (Complemento)`  
  - Corpo padronizado e anexos: todas as planilhas geradas  

- **Governança de alterações**  
  - Qualquer mudança em diretório, nome de coluna, planilha de regras ou tela web exige ajuste via ticket para equipe de Sustentação  

---

## Validações Necessárias

- **Existência de cilindros não cromados**  
  - Pelo menos um registro com coluna `Cromado = N`  
  - Se não houver, registrar log e encerrar execução  

- **Detecção de cilindros reutilizados**  
  - `Reutilizado = 0` → cenário reutilizado (Subprocesso 2)  
  - `Reutilizado ≠ 0` → cenário não reutilizado (Subprocesso 3)  

- **Integridade da exportação de dados**  
  - Planilha gerada no **Sistema X** com dados de `Resumo Produção`  
  - Planilha gerada no **Sistema Y** com `Estoque – Detalhado`  

- **Conformidade das colunas e formatos**  
  - Verificar presença de cabeçalhos esperados: Data de Produção, Cil Trab Sup, Larg., Comp., Rugosidade, Espes., Oleamento LE, Tempo, Alg Min  
  - Confirmar filtros e ordenações aplicados corretamente  

- **Cálculos e fórmulas**  
  - Soma de `Comp.` convertida para quilômetros (÷ 1000)  
  - Soma de rugosidade dentro de faixa 0.60–1.70 para "Total KM Itens com rugosidade"  
  - Soma de peso e KM ≥ 4 % no cenário misto  

- **Isolamento de itens BRILHANTE**  
  - Todos os registros com Rugosidade = BRILHANTE devem ser extraídos para planilha separada  

- **Geração de colunas adicionais**  
  - Sequência, Data, Nº Lista Programa e campos de totais presentes na planilha final  

- **Envio de e-mail**  
  - Verificar anexos e destinatários antes do envio  
  - Garantir sucesso de disparo via Outlook ou registrar falha em log  

- **Registro de exceções**  
  - Todos os erros ou situações não mapeadas devem gerar entrada no `LOG Erro Processamento`  

---

## Saídas Esperadas

- **Planilhas de programas geradas:**  
  - `Baixa Redução_DD.MM.AAAA_SEQ.xlsx`  
  - `Misto_DD.MM.AAAA_SEQ.xlsx`  

- **Planilha de itens com rugosidade brilhante:**  
  - `Itens rugosidade brilhante DD.MM.AAAA.xlsx`  

- **Arquivo de log de erros:**  
  - `LOG Erro Processamento - Elaboração de Programas.xlsx`  
  - Entradas detalhadas contendo Data, Nº Lista Programa, tipo de erro e métricas (peso, KM, rugosidade)  

- **E-mail enviado ao PCP:**  
  - Com todas as planilhas anexadas e corpo padronizado  

- **Organização em diretório compartilhado:**  
  - Todos os arquivos armazenados na rede da área  

---

## Critérios de Sucesso

- Execução automatizada sem necessidade de intervenção manual  
- Geração de todas as planilhas previstas em cada cenário  
- Envio de e-mail concluído sem falhas  
- Tempo de execução dentro do SLA definido (ideal: < 30 min)  
- Percentual de execuções sem erros críticos ≥ 95 %  
- Logs de erro geram informações suficientes para diagnósticos e backup manual  
- Conformidade de diretórios, nomes de arquivos e estrutura de planilhas conforme especificado  

---

**Observação:** melhorias futuras em regras, planilhas ou sistemas legados devem ser ajustados no robô.

