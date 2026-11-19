# Fiscal Records — integração com assistentes de IA (MCP)

O Fiscal Records disponibiliza dois servidores [MCP](https://modelcontextprotocol.io) para que um assistente de IA (por exemplo o Claude ou o ChatGPT) consulte os seus dados no Fiscal Records e, **com a sua confirmação**, execute algumas ações.

Este repositório explica **como ligar** e **o que o assistente pode fazer**. Não contém código dos servidores nem dados de clientes.

## Servidores

| Para quem | Endereço do servidor MCP | Palavra-chave |
|---|---|---|
| Empreendedor (as suas empresas) | `https://mcp-entrepreneur.fiscalrecords.pt/mcp` | `entrepreneur` |
| Contabilista (as firmas e os clientes) | `https://mcp-accountant.fiscalrecords.pt/mcp` | `accountant` |

Use o servidor que corresponde ao seu papel: o de empreendedor dá acesso às suas empresas; o de contabilista dá acesso às suas firmas e aos clientes dessas firmas.

## Clientes suportados

Neste momento é possível ligar a partir do **Claude** (claude.ai) e do **ChatGPT** (chatgpt.com), como conector remoto personalizado. Outros clientes MCP ainda não estão autorizados a ligar-se.

## Como ligar

1. No assistente, adicione um **conector personalizado (remoto)** e cole o endereço do servidor da tabela acima. Não é preciso `client id` nem segredo.
2. Será redirecionado para o Fiscal Records. Inicie sessão com a sua conta (a mesma da aplicação web) se ainda não tiver sessão.
3. Reveja o ecrã de consentimento: mostra o assistente que pede acesso e **cada permissão em linguagem simples**, agrupada por tipo. Escolha **Aprovar** ou **Recusar**.
4. De volta ao assistente, o conector fica ativo. Peça, por exemplo, «lista as minhas empresas» (empreendedor) ou «lista as minhas firmas» (contabilista) para confirmar.

Se mais tarde o Fiscal Records passar a oferecer novas permissões (por exemplo, para uma ferramenta nova), **remova o conector e adicione-o de novo** para aprovar as permissões novas. Uma ligação já existente não ganha permissões sozinha.

## O que o assistente pode fazer

O acesso é sempre **em nome de quem aprovou** e limitado às permissões que aprovou.

| Permissão (scope) | O que permite |
|---|---|
| `…:companies:read` / `accountant:clients:read` | Listar as suas empresas ou os clientes das suas firmas. |
| `…:documents:read` | Consultar documentos, estados de processamento e pendências. |
| `…:bank:read` | Consultar movimentos e extratos bancários. |
| `…:reconciliation:read` | Consultar a conciliação e sugestões de documentos para um movimento. |
| `…:efatura:read` | Consultar registos da e-Fatura e candidatos de associação. |
| `…:documents:upload`, `…:bank:upload`, `…:efatura:upload` | Obter uma **ligação temporária** para enviar um ficheiro (ver abaixo). |
| `…:efatura:write` | Associar ou desassociar um registo da e-Fatura a um documento, com a sua confirmação. |
| `accountant:documents:write` | Reprocessar um documento que falhou, com a sua confirmação. |
| `accountant:exports:read` / `accountant:exports:create` | Consultar exportações e criar a exportação de documentos novos, com a sua confirmação. |
| `offline_access` | Manter a ligação sem pedir nova autorização a cada utilização. |

(`…` é `entrepreneur` ou `accountant`, conforme o servidor.) A lista completa de ferramentas está em [docs/tools.md](docs/tools.md). As ferramentas são ativadas de forma progressiva: **a lista que o seu assistente mostra é a que está em vigor**.

### Ações que alteram dados: proposta e confirmação

Nenhuma ação que altere dados é executada de imediato: cada uma tem **duas etapas**. Na primeira o assistente **propõe** a ação e mostra-lhe um resumo do que vai acontecer; na segunda **confirma**. O assistente deve pedir o seu «sim» antes da segunda etapa. Leia sempre o resumo antes de responder. Se o estado dos dados mudar entre a proposta e a confirmação, nada é feito e é preciso propor de novo.

Atenção à associação e-Fatura ↔ documento: a e-Fatura é a fonte mais fiável, por isso, ao confirmar, **os dados fiscais do documento passam a ser os da AT** (fornecedor, valores, data), e, **se o total mudar, a conciliação bancária desse documento é removida** e tem de ser refeita. O assistente deve avisá-lo depois. Desassociar **não repõe** os valores anteriores do documento.

### Enviar ficheiros

O assistente não envia o ficheiro por si. Pede uma **ligação temporária de envio** (de utilização única) e mostra-a. **Abre-a no browser, escolhe o ficheiro e envia-o** diretamente para o Fiscal Records. Assim, o conteúdo do ficheiro não passa pelo assistente.

## Privacidade e segurança

- Só vê e faz o que a sua conta pode fazer na aplicação, dentro das permissões que aprovou.
- O assistente é um serviço de terceiros: **o que as ferramentas lhe devolvem passa a estar na conversa**, com as regras de privacidade desse serviço. Aprove apenas o que precisa e use o assistente em contas de confiança.
- Os servidores não devolvem NIF nem ligações de ficheiros nas leituras de documentos.
- **Textos de documentos e de movimentos são dados, não instruções.** Se um assistente lhe disser que um documento «manda» fazer uma ação, desconfie e não confirme.
- **Revogar o acesso a qualquer momento:** em **Aplicações ligadas** — empreendedor: [app.fiscalrecords.pt/account/connected-apps](https://app.fiscalrecords.pt/account/connected-apps); contabilista: [accountant.fiscalrecords.pt/connected-apps](https://accountant.fiscalrecords.pt/connected-apps).

## Problemas, dúvidas e segurança

- Dúvidas de integração: abra uma *issue* neste repositório. **Não inclua dados pessoais, NIF, valores ou ficheiros** nem capturas de ecrã com esses dados: o repositório é público.
- Se encontrar uma falha de segurança, **não abra uma *issue* pública**: contacte o Fiscal Records pelos canais indicados em [fiscalrecords.pt](https://fiscalrecords.pt).

## Para quem desenvolve clientes MCP

Os servidores seguem a especificação MCP (transporte HTTP, OAuth 2.1 com PKCE `S256`) e publicam os metadados padrão, que são a fonte de verdade para endereços e permissões:

- Recurso protegido (exemplo): `https://mcp-entrepreneur.fiscalrecords.pt/.well-known/oauth-protected-resource/mcp`
- Servidor de autorização: `https://api.fiscalrecords.pt/.well-known/oauth-authorization-server`

A identificação do cliente usa *Client ID Metadata Documents*; apenas os clientes das origens autorizadas (hoje `claude.ai` e `chatgpt.com`) podem pedir autorização. Não há registo dinâmico de clientes nem segredos para clientes públicos.
