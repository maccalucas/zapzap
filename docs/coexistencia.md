# Coexistência via Embedded Signup em app próprio

Coexistência = o mesmo número funciona no app WhatsApp Business e na Cloud API. A Meta exige Embedded Signup com `featureType: whatsapp_business_app_onboarding`, webhooks inscritos e sincronização em até 24 h.

## Fluxo
1. Cliente faz login na página e clica em conectar.
2. Popup da Meta: escolhe o portfólio, informa o número, escaneia o QR pelo app.
3. A página recebe o `code` (expira em ~30 s) e, por `postMessage`, `waba_id` e `phone_number_id`. Envia tudo ao backend junto com a sessão.
4. Backend:
   - valida a sessão;
   - `GET /oauth/access_token?client_id&client_secret&code` → token;
   - **grava o token antes de qualquer outra coisa**;
   - `POST /{waba_id}/subscribed_apps`;
   - **não** chama `/register` — o número já está registrado;
   - `POST /{phone_number_id}/smb_app_data` com `sync_type: smb_app_state_sync` (contatos) e depois `history`. **Cada um só uma vez**; repetir exige offboard e refazer tudo;
   - `GET /{phone_number_id}?fields=is_on_biz_app,platform_type` → esperado `true` + `CLOUD_API`.

## Configuração do app Meta
- App do tipo **Empresa**, com os produtos WhatsApp e Login do Facebook para Empresas.
- Configurações → Básico: domínio do app, URLs de privacidade, termos e exclusão de dados, categoria, ícone.
- Login do Facebook para Empresas → Configurações: URI de redirecionamento OAuth e domínio permitido do SDK JS = URL da página; "Entrar com o SDK do JavaScript" = Sim.
- WhatsApp → "Torne-se um Provedor de Tecnologia" → Independent Tech Provider.

## Armadilhas já pagas
- **"Criar configuração" só oferece a variação "Geral".** A de cadastro incorporado sai por *Configurações → Criar do modelo → "Configuração do cadastro incorporado do WhatsApp com token de expiração em 60 dias"*. A variação não pode ser trocada depois.
- **Não roda nem no próprio número** sem inscrição como Tech Provider **e** verificação da empresa. Só aceitar os termos não libera.
- A tela de bloqueio não gera `code`: o sync não é gasto. O botão dela só compartilha contato.
- A versão da Graph no código de exemplo do painel é a que vale; conferir antes de fixar.
- n8n com `executionOrder: v1` executa primeiro o ramo de cima: a gravação do token fica acima do sync para sobreviver a uma falha dele.
- `PUT` em workflow ativo do n8n com nó incompleto devolve 400 **mas grava os nós**.
- O campo de ícone do painel só aceita o seletor nativo de arquivos.

## Limites da coexistência (documentação da Meta — não testados aqui)
- Histórico: ~6 meses, importado uma única vez.
- O app precisa ser aberto a cada ~14 dias.
- No app deixam de existir listas de transmissão, mensagens temporárias e visualização única.
- WhatsApp para Windows e WearOS não funcionam como aparelho vinculado.
- Mensagens enviadas pelo app são gratuitas; só o que sai pela API é cobrado, no cartão da WABA do cliente.
