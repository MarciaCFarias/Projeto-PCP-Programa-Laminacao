# Análise de Processo: Elaboração de Programas de Laminação



## 1 – Fluxo TO BE

1. **Consultar Cilindros**  
   **Descrição:**  
   - Acessar Sistema legado X via link web.  
   - Selecionar `TF – Tiras a Frio` > `Cilindro` > `Quadro – LE - 2`.  
   - Navegar até `Reserva no Laminador` e validar colunas **Cromado** e **Reutilizado**.  
   - Se todos **Cromados ≠ “N”**, encerrar e registrar log.  
   - Capturar código do primeiro par não cromado e seguir.  

2. **Subprocesso de Estoque**  
   - **Cenário Reutilizado (Subprocesso 2):**  
     - Acessar **Sistema legado Y** > `Produção` > `Resumo Produção` > aplicar filtros de data, unidade e equipamento.  
     - Exportar e abrir planilha Excel, calcular KM total e largura máxima.  
     - Acessar **Sistema legado Z**, extrair estoque e unir dados para aplicação de regras.  
   - **Cenário Não Reutilizado (Subprocesso 3):**  
     - Acessar **Sistema legado W** > `Controle Material à Frio` > `Estoque – Detalhado` > filtrar por **Resfriado**.  
     - Exportar planilha e classificar colunas (rugosidade, largura, espessura, oleamento, tempo).  

3. **Aplicar Regras de Negócio**  
   - Classificar itens por **Alg Min** (< 4 % ou mix) e **Rugosidade**.  
   - Seguir sequência definida nas abas da planilha **Regras - formação de cones**.  

4. **Gerar Planilhas de Programação**  
   - Adicionar colunas **Sequência**, **Data**, **Nº Lista Programa**, e métricas (KM, peso).  
   - Salvar em diretório compartilhado com nomenclatura:  
     - `Baixa Redução_<Data><Seq>` ou  
     - `Misto<Data>_<Seq>`.  

5. **Enviar Planilhas ao PCP**  
   - Disparar e-mail para área.  
   - Assunto com nº da lista e corpo padronizado.  
   - Anexar todas as planilhas geradas na execução.  

6. **Gerar e Salvar Logs**  
   - Registrar no arquivo **LOG Erro Processamento - Elaboração de Programas** eventos de erro e contagens diárias.  
   - Estruturar colunas: Data, Nº Lista, Totais (peso, KM, rugosidade, Alg Min).  

7. **Notificar Exceções**  
   - Em caso de falha no e-mail ou nos sistemas legados, enviar alerta para área que irá analisar a execução.  
   - Incluir instruções para acesso ao log e providências manuais.  

---

## 2 – Melhorias implementadas no processo

- Automação completa do acesso e extração de dados dos sistemas legados.  
- Padronização de filtros, ordenações e nomenclatura de arquivos, reduzindo variações manuais.  
- Aplicação dinâmica de regras de formação de cones via planilha de assets, sem codificação fixa.  
- Geração automática de relatórios e logs consolidados, garantindo rastreabilidade.  
- Envio padronizado de e-mails com anexos, eliminando etapas manuais de comunicação.  
- Potencial de redução das **657 h/ano** atuais para menos de **200 h/ano** de esforço humano.  

---

## 3 – Tratativas em caso de exceções

- **Sem cilindros válidos:** processa até validação, registra log e encerra sem gerar programa.  
- **Alterações telas web:** captura erro, loga mensagem, notifica equipe de sustentação e aguarda correção.  
- **Dados ou colunas ausentes:** interrompe aplicação de regras, atualiza log e solicita ajuste via ticket.  
- **Falha no Outlook/envio:** registra no log de e-mail, mantém arquivos no diretório e alerta área para envio manual.  
- **Erro de conversão ou cálculo:** identifica registro afetado, registra detalhe no log e prossegue no próximo item.  

---

## 4 – KPI

- Tempo médio de ciclo por execução (minutos)  
- Percentual de execuções sem erro (% sucesso)  
- Volume de programas gerados por dia  
- Tempo médio de detecção e correção de erros (horas)  
- Redução de horas manuais **AS-IS vs TO-BE** (horas/ano)  
- SLA de entrega de planilhas (percentual dentro do intervalo de 2 h)  
- Número de tickets de sustentação abertos por alteração em sistemas legados  

---

## 5 – Observações ou destaques

- Processo de alto risco devido a dependência de planilhas e fórmulas; alterações exigem governança.  
- Diretórios e nomenclaturas fixos são pontos críticos de manutenção e devem ser tratados como assets.  
- Recomenda-se evolução para integração via BPMS ou APIs de sistemas legados, reduzindo pontos de falha.  
- Parametrização externa das regras de negócio (assets) facilitará ajustes sem deploy de código.  
- Monitoramento em tempo real e dashboards darão visibilidade operacional e ajudarão na melhoria contínua.  

