# zapzap

SaaS de atendimento por WhatsApp, no molde do Chatwoot, com dois diferenciais:

1. **Conexão por coexistência** — o cliente liga o próprio número à API oficial (Embedded Signup da Meta) e continua usando o app WhatsApp Business no celular.
2. **Agentes de IA plugáveis** — o canal aceita um agente por trás (ZAIA primeiro, outros provedores depois), com handoff para humano.

> Estado: **laboratório**. O fluxo de onboarding está montado e testado até onde a Meta permite; o produto (caixa de atendimento, multi-tenant, camada de agentes) ainda não existe.

## Onde estamos

| Peça | Estado |
|---|---|
| Página de conexão com login (`lab/pagina/`) | no ar |
| Páginas legais (privacidade, termos, exclusão de dados) | no ar, rascunho |
| Login por e-mail + código, sessão HMAC de 30 min (`lab/n8n/n8n-conectar-login.json`) | testado |
| Onboarding: valida sessão → troca `code` → grava token → `subscribed_apps` → sync contatos e histórico → confere status (`lab/n8n/n8n-coexistencia-onboard.json`) | pronto, nunca rodou de verdade |
| Webhook de eventos da Meta: GET de verificação + POST 200 (`lab/n8n/n8n-webhook-eventos.json`) | no ar, sem processamento |
| App Meta inscrito como Independent Tech Provider | feito |
| Verificação da empresa na Meta | **em análise** — é o bloqueio |
| App Review das permissões `whatsapp_business_*` | não iniciado (`docs/app-review.md`) |

Detalhe em [`docs/status.md`](docs/status.md).

## Documentação

- [`docs/status.md`](docs/status.md) — o que existe, o que falta para ser funcional
- [`docs/coexistencia.md`](docs/coexistencia.md) — como o onboarding funciona e as armadilhas já pagas
- [`docs/arquitetura.md`](docs/arquitetura.md) — do laboratório ao SaaS e decisões em aberto
- [`docs/app-review.md`](docs/app-review.md) — roteiro dos vídeos do App Review

## Segredos

Nada sensível vive aqui. Tokens, app secret, segredo do HMAC, códigos de acesso e credenciais de banco ficam fora do repositório. Os arquivos de `lab/` usam placeholders (`SEU_APP_ID`, `<HMAC_SECRET_64_HEX>`, `<CRED_ID>`).
