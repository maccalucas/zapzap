# App Review — roteiro dos vídeos (Tech Provider)

Dois vídeos, um por permissão. Nunca reusar o mesmo vídeo. Tela inteira, 1080p, sem cortes, 1–2 min cada. Legenda/descrição em **inglês**.

Pré-requisitos: número de teste (ou próprio) no app, token com as duas permissões, `app-review.postman_collection.json` importada no Postman com as variáveis preenchidas, celular espelhado na tela (QuickTime → Nova Gravação de Filme → iPhone).

## Vídeo 1 — `whatsapp_business_messaging`
1. Mostrar o painel do app (nome e ID visíveis).
2. Postman → request **1. Enviar mensagem** → Send → resposta 200 com `messages[0].id`.
3. Celular espelhado: a mensagem chega no WhatsApp.
4. Responder do celular e mostrar o evento no webhook (execução do n8n).

Descrição (EN): *"Macca Tecnologia is a Tech Provider. Our platform sends WhatsApp messages on behalf of business customers who onboard through Embedded Signup. This video shows our app sending a template message through the Cloud API and the message being received in the WhatsApp client."*

## Vídeo 2 — `whatsapp_business_management`
1. Postman → **2. Listar modelos** → 200.
2. Postman → **3. Criar modelo** → 200 com `id` e `status`.
3. WhatsApp Manager → Modelos de mensagem: o modelo novo aparece na lista.
4. Opcional: mostrar `https://conectar.macca.ia.br/` e o popup do cadastro incorporado abrindo.

Descrição (EN): *"We use whatsapp_business_management to create and manage message templates and to read WhatsApp Business Account assets for the customers who onboard to our platform. This video shows our app listing and creating a template and the template appearing in WhatsApp Manager."*

## Antes de submeter
- Submeter as **duas permissões no mesmo pacote** (uma primeiro trava a outra).
- Management exige ≥1 chamada real de API antes; o contador leva até 24 h.
- URLs de privacidade, termos e exclusão já estão no app (13/11/2026).
- Falta no app: ícone 1024×1024.
