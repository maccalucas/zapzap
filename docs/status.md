# Status — 13/11/2026

## O que funciona hoje
- **Página de conexão** servida por HTTPS (nginx atrás de Traefik). Tela de login → botão "Conectar com o Facebook".
- **Login**: e-mail + código de acesso. O banco guarda só o SHA-256 do código. Sucesso devolve uma sessão `payload.assinatura` (HMAC-SHA256, 30 min); erro devolve 401.
- **Trava no backend**: o onboarding recusa qualquer chamada sem sessão válida (testado: execução para em "Validar sessão").
- **Botão**: chama `FB.login` com a configuração de Embedded Signup e `featureType: whatsapp_business_app_onboarding`. O popup abre.
- **Webhook de eventos**: responde o `hub.challenge` com o token certo, 403 com o errado, 200 imediato no POST.
- **Páginas legais** publicadas e cadastradas no app.

## O que impede de ser funcional
O popup da Meta devolve **"não pode integrar clientes no momento"** — inclusive para o número do próprio dono do app. Ordem para destravar:

1. **Verificação da empresa** do portfólio — enviada, em análise (até ~14 dias úteis).
2. **Número de teste** no app — falhou enquanto a verificação está em análise; tentar depois.
3. **Dois vídeos** (envio de mensagem; gestão de template) — roteiro em `app-review.md`.
4. **App Review** de `whatsapp_business_messaging` + `whatsapp_business_management`, no mesmo pacote.
5. **Primeiro onboarding real** e conferência de `is_on_biz_app = true` + `platform_type = CLOUD_API`.

Nada disso é código. Enquanto a Meta analisa, o que dá para construir é o produto.

## Pendências técnicas do laboratório
- Ligar o webhook de eventos ao app e assinar `messages`, `history`, `smb_app_state_sync`, `smb_message_echoes`.
- Processar os eventos (hoje o POST só devolve 200).
- Token guardado em texto puro no Postgres — cifrar antes de ter cliente.
- Login sem limite de tentativas (o código tem 20 caracteres aleatórios).
- Onboarding responde 200 antes de validar a sessão: a página mostra sucesso mesmo quando o backend recusa.
- Ícone 1024×1024 do app ainda não enviado.
- Páginas legais são rascunho.

## O que não existe ainda (o produto)
Caixa de atendimento, contas e usuários (multi-tenant), roteamento de eventos por número, camada de agentes de IA, cobrança. Ver `arquitetura.md`.
