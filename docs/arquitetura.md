# Do laboratório ao SaaS

O laboratório é n8n + página estática + um único tenant. O produto precisa de:

- **Multi-tenant**: conta → usuários → números. Token por número, cifrado em repouso.
- **Autenticação de verdade**, com limite de tentativas e recuperação de acesso.
- **Ingestão de eventos**: um webhook para todos os tenants, roteado por `phone_number_id`. Campos: `messages`, `history`, `smb_app_state_sync`, `smb_message_echoes`.
- **Caixa de atendimento**: conversas, atribuição, status, notas internas. É a máquina de estado que faz humano e agente não operarem às cegas na mesma conversa.
- **Camada de agentes**: um adaptador por provedor. ZAIA primeiro (caminho síncrono para resposta única; assíncrono com callback para conversa).
- **Handoff**: humano assumiu — no painel **ou respondendo pelo app do celular** (`smb_message_echoes`) — a IA para naquela conversa.
- **Janela de 24 h e templates**: fora da janela, só template aprovado.
- **Idempotência** por id de mensagem: a Meta reenvia eventos.

## Decisões em aberto
1. **Construir do zero ou partir do Chatwoot** (fork/extensão)? O Chatwoot CE não tem coexistência nem gestão de template pela API; pesar o custo de manter fork contra o de construir a caixa.
2. **Stack**: Next.js + Supabase é o candidato. O n8n fica como cola ou sai do caminho crítico?
3. **Posicionamento**: concorrente de plataformas de atendimento ou motor por trás de um serviço?
4. **Preço e cobrança da Meta**: como Tech Provider não há linha de crédito; o cartão é do cliente.

## Primeiro passo sugerido
Registrar as decisões 1 e 2 e portar o onboarding do n8n para código: validar sessão → trocar `code` → gravar token cifrado → `subscribed_apps` → sync. É a única parte já desenhada e testada até onde a Meta deixou.
