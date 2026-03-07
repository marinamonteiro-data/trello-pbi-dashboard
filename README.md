# trello-pbi-dashboard
Dashboard de KPIs de equipes com integração via API do Trello e Power BI.

📊 Dashboard de Produtividade: Integração Trello API & Power BI
Análise de KPIs de Equipes baseada em Fluxos de Trabalho (Checklists)

📌 Contexto do Projeto
Este projeto nasceu da necessidade de mensurar a eficiência operacional em um ambiente de gestão ágil. Utilizando a API do Trello, desenvolvi um pipeline de dados que extrai informações detalhadas de cartões e, especificamente, o progresso de checklists, transformando dados qualitativos de gestão em métricas quantitativas de desempenho.

🧠 Abordagem Metodológica

Diferente de dashboards comuns de "status", esta solução aplica conceitos de:
 - Análise de Fluxo (Throughput): Medição da vazão de itens concluídos por período.
 - Identificação de Gargalos: Aplicação de lógica de Lead Time para detectar etapas onde o processo perde tração.

🛠️ Arquitetura Técnica
 - Extração (ETL): Conexão via Web.Contents no Power Query para consumo dos endpoints da API do Trello (Boards, Cards e Checklists).
 - Tratamento de Dados: Uso de Linguagem M para expansão de estruturas JSON aninhadas e tratamento de paginação/limites da API.
 - Modelagem: Estrutura de dados otimizada para performance, garantindo a integridade dos relacionamentos 1:N entre cartões e sub-itens.
 - DAX Avançado: Criação de medidas para cálculo de taxas de conclusão (%).

📈 Principais KPIs Desenvolvidos
 - Índice de Conclusão de Checklists (ICC): Relação percentual entre itens planejados vs. executados.
 - Distribuição de Carga: Visualização do volume de micro-tarefas por responsável (em andamento)
 - Monitoramento de Backlog: Envelhecimento dos cartões sem movimentação de checklist (em andamento).

Este modelo foi adaptado para fluxos de fechamento contábil, garantindo que cada etapa do checklist de conformidade seja auditável em tempo real.
