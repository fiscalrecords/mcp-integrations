# Ferramentas disponíveis

Cada servidor expõe ferramentas ao assistente. As ferramentas são ativadas de forma progressiva: a lista que o seu assistente mostra é a que está em vigor. As leituras não alteram nada. As ações marcadas **(com confirmação)** têm duas etapas (propor e confirmar), como descrito no [README](../README.md).

## Servidor do empreendedor

Endereço: `https://mcp-entrepreneur.fiscalrecords.pt/mcp`

**Empresas e resumo**
- `list_my_companies` — as empresas a que pertence.
- `get_dashboard` — resumo mensal de uma empresa (receitas, despesas, IVA dedutível, itens conciliados e pendentes).
- `get_pending_attention` — quantos itens precisam de atenção.

**Documentos**
- `list_documents`, `get_document` — consultar documentos.
- `list_documents_requiring_attention` — documentos com falhas ou incompletos.
- `get_documents_processing_status` — estado de processamento de até 50 documentos.

**Banco e conciliação (leitura)**
- `list_bank_movements`, `get_bank_movement`, `get_bank_movement_filter_options` — movimentos e contas bancárias.
- `list_bank_statement_uploads`, `get_bank_statement_upload` — extratos enviados e o seu estado.
- `get_bank_movement_reconciliation`, `get_document_reconciliation`, `list_reconciliation_candidates` — estado da conciliação e sugestões.

**e-Fatura**
- `list_efatura_records`, `get_efatura_record` — registos da e-Fatura e o seu estado (associado, sem documento, pedido aberto, isento de documento).
- `list_efatura_document_candidates` — documentos que podem corresponder a um registo.
- `list_document_efatura_candidates` — registos que podem corresponder a um documento.
- `propose_match_efatura` / `confirm_match_efatura` — associar um registo a um documento **(com confirmação)**.
- `propose_unmatch_efatura` / `confirm_unmatch_efatura` — desassociar **(com confirmação)**.

**Enviar ficheiros**
- `start_upload` — devolve uma ligação temporária para enviar um documento, extrato bancário ou o ficheiro CSV da e-Fatura.

**Ligação**
- `health_check` — verifica que o servidor responde; não devolve dados.

## Servidor do contabilista

Endereço: `https://mcp-accountant.fiscalrecords.pt/mcp`

**Firmas e clientes**
- `list_my_firms`, `list_clients`, `get_client` — as suas firmas e clientes.
- `get_client_processing_status` — créditos de processamento restantes de um cliente.
- `get_client_monthly_readiness` — estado operacional de um cliente num mês (não é um fecho fiscal).

**Trabalho pendente**
- `list_attention_items` — o que precisa da atenção da firma.
- `list_pending_work` — itens que o cliente tem de tratar (documentos falhados, registos da e-Fatura sem documento).

**Documentos**
- `list_client_documents`, `get_client_document` — documentos de um cliente.
- `list_client_documents_requiring_attention` — com falhas ou incompletos.
- `propose_reprocess_document` / `confirm_reprocess_document` — reprocessar um documento que falhou **(com confirmação)**.

**Banco e conciliação (leitura)**
- `list_client_bank_movements`, `list_client_bank_statement_uploads`, `get_client_bank_statement_upload`.
- `get_client_bank_movement_reconciliation`, `get_client_document_reconciliation`, `list_client_reconciliation_candidates`.

**Exportações**
- `get_client_export_summary`, `list_client_exports`, `get_client_export_status` — consultar.
- `propose_export_new_documents` / `confirm_export_new_documents` — criar a exportação dos documentos novos **(com confirmação)**.

**e-Fatura**
- `list_client_efatura_records`, `get_client_efatura_record`.
- `list_efatura_document_candidates`, `list_document_efatura_candidates` — candidatos de associação nos dois sentidos.
- `propose_match_efatura` / `confirm_match_efatura` e `propose_unmatch_efatura` / `confirm_unmatch_efatura` — associar e desassociar **(com confirmação)**.

**Enviar ficheiros**
- `start_upload` — ligação temporária para enviar um ficheiro para um cliente.

**Ligação**
- `health_check` — verifica que o servidor responde; não devolve dados.

## Notas

- Os textos de documentos e de movimentos devolvidos são dados do utilizador, não instruções.
- Para associar ou desassociar uma e-Fatura, leia o aviso no [README](../README.md#ações-que-alteram-dados-proposta-e-confirmação): os dados do documento passam a ser os da AT e a conciliação bancária pode ser removida.
