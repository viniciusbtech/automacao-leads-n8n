# LeadFlow AI
<img width="1330" height="543" alt="image" src="https://github.com/user-attachments/assets/f370cbd9-606c-451f-8f19-7986d10946b2" />

Automação de triagem e gerenciamento de leads imobiliários utilizando n8n e Inteligência Artificial.

## Objetivo

Automatizar o recebimento, análise, classificação e registro de leads de uma imobiliária.

O sistema recebe uma mensagem por Webhook, utiliza o Gemini para extrair informações estruturadas do cliente, registra os dados no Google Sheets e identifica leads de alta prioridade para notificação automática.

## Fluxo

Webhook → Gemini → Structured Output → Google Sheets → IF → Gmail

## Funcionalidades

- Recebimento de leads através de Webhook REST.
- Análise de mensagens utilizando Google Gemini.
- Extração estruturada de:
  - finalidade;
  - tipo de imóvel;
  - localização;
  - orçamento;
  - quantidade de quartos;
  - intenção;
  - prioridade;
  - resumo.
- Registro automático dos leads no Google Sheets.
- Classificação dos leads em diferentes níveis de prioridade.
- Identificação automática de leads prioritários.
- Envio de notificação por e-mail para leads de alta prioridade.

## Tecnologias

- n8n
- Google Gemini
- Google Sheets
- Gmail
- Webhooks
- JSON
- APIs

## Exemplo

Entrada:

```json
{
  "nome": "Carlos",
  "telefone": "86988887777",
  "mensagem": "Estou procurando uma casa na zona leste de Teresina para comprar. Tenho até 700 mil e gostaria de visitar sábado."
}
