# Pedizap - OpenDelivery API Reference

Documentação e coleção de requisições da API **OpenDelivery** utilizada pela integração da Pedizap, cobrindo o fluxo de autenticação, gestão de merchants, pedidos, eventos e webhooks entre a Ordering Application e o parceiro de delivery.

## Estrutura do projeto

```
.
├── index.html                 # Página de referência da API (Scalar), lê openapi.yaml
├── openapi.yaml                # Especificação OpenAPI 3.0 consolidada
├── opencollection.yml          # Configuração da coleção (formato Bruno)
├── environments/                # Variáveis de ambiente (baseUrl, etc.)
├── authentication/               # Requisições de obtenção de token (OAuth)
├── merchant/                    # Endpoints de merchant (status, horários, webhook de atualização)
├── orders/                       # Ciclo de vida do pedido (confirmar, despachar, cancelar, rastrear...)
├── events/                        # Consumo/confirmação de eventos (polling)
└── webhook/                       # Endpoints de webhook (notificação de pedido, tracking, código de entrega)
```

Cada pasta contém um `folder.yml` (metadados da pasta) e arquivos `.yml` por requisição, no formato de coleção do [Bruno](https://www.usebruno.com/) (`opencollection`).

## Visualizando a documentação

A referência interativa é renderizada com [Scalar](https://github.com/scalar/scalar) a partir do `openapi.yaml`. Para visualizar localmente, sirva a pasta com qualquer servidor estático, por exemplo:

```bash
npx serve .
# ou
python3 -m http.server 8080
```

Depois acesse `index.html` no navegador.

> Note que `index.html` aponta para `url: '/docs/opendelivery/openapi.yaml'`. Ajuste esse caminho conforme o path onde o projeto for hospedado, ou sirva o projeto nessa mesma rota.

## Usando a coleção de requisições

Os arquivos `.yml` (fora o `openapi.yaml`) seguem o formato de coleção do Bruno. Para usá-los:

1. Abra o [Bruno](https://www.usebruno.com/) e importe esta pasta como uma coleção.
2. Selecione o ambiente desejado em `environments/` (Development ou Production) e configure a variável `baseUrl`.
3. Rode a requisição **Get Access Token** (`authentication/`) para obter o token — o script `after-response` já salva o token na variável `token` para reuso nas demais chamadas.

## Autenticação

A API utiliza OAuth2 client credentials via `POST {{baseUrl}}/oauth/token`, retornando `access_token`, `token_type` e `expires_in`.

## Principais fluxos

- **Merchant**: consulta de dados, status/horário de funcionamento e atualização de endpoint via webhook.
- **Orders**: ciclo completo do pedido — confirmação, preparo, pronto para retirada, despacho, entrega, cancelamento (solicitação/aceite/negação) e validação de código de entrega.
- **Events**: polling de novos eventos e confirmação (acknowledge) de recebimento.
- **Webhook**: endpoints que a Pedizap expõe para receber notificações (novo pedido, tracking, código de entrega).

## Referências

- [OpenDelivery (ABRASEL)](https://abrasel-nacional.github.io/opendelivery/)
