# LeadFlow AI
<img width="1330" height="543" alt="image" src="https://github.com/user-attachments/assets/f370cbd9-606c-451f-8f19-7986d10946b2" />


Automação inteligente para triagem, classificação e gerenciamento de leads imobiliários utilizando **n8n, Google Gemini, Google Sheets e Gmail**.

O projeto simula um fluxo real de atendimento de uma imobiliária, automatizando desde o recebimento da mensagem de um potencial cliente até o registro das informações e a notificação de leads considerados prioritários.

---

## Objetivo

Em processos comerciais, mensagens de potenciais clientes normalmente chegam em linguagem natural e precisam ser analisadas manualmente antes de serem registradas em planilhas ou sistemas.

O LeadFlow AI automatiza essa etapa.

A solução:

- recebe informações do cliente por Webhook;
- utiliza Inteligência Artificial para interpretar a mensagem;
- transforma texto não estruturado em dados estruturados;
- registra automaticamente o lead no Google Sheets;
- classifica o nível de prioridade;
- identifica leads prioritários;
- envia uma notificação automática por e-mail para o responsável.

---

## Arquitetura


flowchart TD
    A[Webhook] --> B[Basic LLM Chain]
    B --> C[Google Gemini]
    C --> D[Structured Output Parser]
    D --> E[Google Sheets]
    E --> F{Prioridade Alta?}
    F -->|Sim| G[Gmail - Notificação]
    F -->|Não| H[Fim do fluxo]
Fluxo simplificado


Webhook
   ↓
Google Gemini
   ↓
Structured Output Parser
   ↓
Google Sheets
   ↓
IF - Prioridade é Alta?
   ├── Sim → Gmail
   └── Não → Finaliza
Funcionamento
1. Recebimento do lead

O workflow possui um Webhook HTTP responsável por receber as informações iniciais do cliente.

Exemplo de requisição:

{
  "nome": "Carlos",
  "telefone": "86988887777",
  "mensagem": "Boa tarde. Estou procurando uma casa na zona leste de Teresina para comprar. Tenho até 700 mil e queria uma casa com pelo menos 3 quartos. Se tiver alguma disponível gostaria de visitar sábado."
}
2. Processamento com Inteligência Artificial

A mensagem é enviada ao Google Gemini.

O modelo analisa a linguagem natural utilizada pelo cliente e identifica informações relevantes para o atendimento imobiliário.

Entre os dados extraídos estão:

finalidade;
tipo de imóvel;
localização;
orçamento;
número de quartos;
intenção do cliente;
prioridade;
resumo do atendimento.
3. Saída estruturada

Para evitar respostas em texto livre, o workflow utiliza um Structured Output Parser.

Exemplo de saída:

{
  "finalidade": "Compra",
  "tipo_imovel": "Casa",
  "localizacao": "Zona leste de Teresina",
  "orcamento": 700000,
  "quartos": 3,
  "intencao": "Comprar casa e agendar visita",
  "prioridade": "Alta",
  "resumo": "Cliente procura casa para comprar na zona leste de Teresina, com até 700 mil e 3 quartos, e deseja visitar no sábado."
}

Dessa forma, uma mensagem escrita livremente pelo cliente é transformada em dados que podem ser utilizados por outros sistemas.

Entrada
"Estou procurando uma casa na zona leste de Teresina para comprar.
Tenho até 700 mil e queria uma casa com pelo menos 3 quartos.
Gostaria de visitar sábado."
Saída
Finalidade: Compra
Tipo de imóvel: Casa
Localização: Zona leste de Teresina
Orçamento: R$ 700.000
Quartos: 3
Intenção: Comprar casa e agendar visita
Prioridade: Alta
4. Registro automático dos leads

Depois do processamento, os dados são enviados automaticamente para uma planilha no Google Sheets.

Estrutura utilizada:

Data	Nome	Telefone	Finalidade	Tipo de imóvel	Localização	Orçamento	Quartos	Intenção	Prioridade	Resumo	Status
11/09/2026	Carlos	86988887777	Compra	Casa	Zona Leste de Teresina	700000	3	Comprar casa e agendar visita	Alta	Cliente procura casa...	Novo

Assim, todos os leads ficam centralizados em uma estrutura padronizada.

5. Classificação de prioridade

O projeto utiliza uma regra de negócio para classificar os leads.

Alta prioridade

Exemplos:

cliente deseja realizar uma visita;
informou orçamento;
possui características do imóvel definidas;
demonstra intenção clara de compra ou aluguel;
possui necessidade mais imediata.
Média prioridade

Exemplos:

demonstra interesse real;
ainda está pesquisando opções;
informou apenas parte das características desejadas.
Baixa prioridade

Exemplos:

mensagem muito genérica;
apenas curiosidade;
poucas informações para iniciar um atendimento comercial.
6. Automação baseada em regra de negócio

Depois do registro no Google Sheets, um nó IF verifica:

prioridade == "Alta"

O workflow então possui dois caminhos:

Alta prioridade
       ↓
Notificação automática

Média/Baixa prioridade
       ↓
Lead permanece registrado
       ↓
Fim

Isso permite tratar automaticamente leads comercialmente mais relevantes.

7. Notificação automática

Quando um lead é classificado como de alta prioridade, o sistema envia automaticamente um e-mail pelo Gmail.

Exemplo:

🚨 NOVO LEAD DE ALTA PRIORIDADE

Cliente: Carlos
Telefone: 86988887777

Finalidade: Compra
Tipo de imóvel: Casa
Localização: Zona Leste de Teresina
Orçamento: R$ 700.000
Quartos: 3

Intenção:
Comprar casa e agendar visita

Prioridade:
Alta

Resumo:
Cliente procura casa para comprar na zona leste de Teresina,
com orçamento de até R$ 700 mil e deseja realizar uma visita.

Dessa forma, um responsável comercial poderia ser informado imediatamente quando surgir um potencial cliente com maior intenção de negócio.

Tecnologias utilizadas
Tecnologia	Utilização
n8n	Orquestração e automação do workflow
Google Gemini	Interpretação das mensagens dos clientes
Structured Output Parser	Conversão da resposta da IA em estrutura padronizada
Webhook / HTTP	Entrada de dados no workflow
JSON	Estrutura de comunicação entre os componentes
Google Sheets	Registro e organização dos leads
Gmail	Notificação automática de leads prioritários
Conceitos aplicados

Durante o desenvolvimento foram utilizados conceitos de:

automação de processos;
Inteligência Artificial generativa;
integração entre sistemas;
APIs e Webhooks;
processamento de linguagem natural;
transformação de dados não estruturados;
JSON;
regras condicionais;
lógica de negócio;
organização de dados;
integração com serviços Google.
Estrutura do repositório
LeadFlow-AI/
│
├── workflow/
│   └── leadflow-workflow.json
│
├── docs/
│   ├── workflow.png
│   ├── google-sheets.png
│   └── email-notificacao.png
│
└── README.md
Workflow

O arquivo exportado do n8n está disponível em:

workflow/leadflow-workflow.json

Para importar:

Abra o n8n.
Crie um novo workflow.
Escolha a opção de importar workflow.
Selecione o arquivo leadflow-workflow.json.
Configure suas próprias credenciais.
Segurança

Nenhuma credencial deve ser armazenada no repositório.

O projeto não publica:

Gemini API Key;
credenciais Google;
tokens de autenticação;
senhas;
dados privados de contas.

As integrações devem ser configuradas diretamente pelo sistema de credenciais do n8n.

Limitações atuais

Esta versão representa um MVP para estudo e demonstração de automação.

Atualmente:

os leads entram através de Webhook;
a persistência é realizada em Google Sheets;
a classificação utiliza um modelo de linguagem;
a notificação é realizada por Gmail;
não existe ainda integração direta com CRM;
não existe autenticação própria na API de entrada;
não existe integração ativa com WhatsApp.
Possíveis evoluções

Algumas melhorias planejadas:

integração com WhatsApp Business;
utilização de WhatsApp como canal de entrada dos leads;
integração com CRM;
atribuição automática de leads para corretores;
prevenção de leads duplicados;
acompanhamento do status do atendimento;
dashboard de métricas comerciais;
histórico de interações;
tratamento de erros e retries;
logs e observabilidade;
validação de telefone e dados do cliente.

Uma possível evolução da arquitetura seria:

WhatsApp
    ↓
n8n
    ↓
Gemini
    ↓
Classificação do lead
    ↓
CRM / Banco de Dados
    ↓
Distribuição automática
    ↓
Corretor responsável
Exemplo de caso de uso

Um cliente envia:

"Estou procurando uma casa na zona leste de Teresina para comprar.
Tenho até 700 mil, preciso de pelo menos três quartos e gostaria de
visitar alguma opção sábado."

Sem automação:

Pessoa lê a mensagem
↓
Interpreta os dados
↓
Abre planilha
↓
Preenche os campos
↓
Avalia a importância do lead
↓
Avisa outro funcionário

Com o LeadFlow AI:

Mensagem recebida
↓
IA interpreta
↓
Dados são estruturados
↓
Planilha é preenchida
↓
Prioridade é avaliada
↓
Responsável é notificado
Objetivo do projeto

Este projeto foi desenvolvido como exercício prático de automação de processos empresariais utilizando Inteligência Artificial, explorando a integração de modelos de linguagem com ferramentas utilizadas em rotinas comerciais e administrativas.


### O que eu colocaria como imagens

Além do README, tire **três screenshots bons**:

`workflow.png` — mostrando o fluxo inteiro:

```text
Webhook → LLM → Sheets → IF → Gmail

google-sheets.png — mostrando uma linha preenchida automaticamente.

email-notificacao.png — mostrando o e-mail que chegou.

Isso faz muita diferença no GitHub, porque uma pessoa consegue entender o projeto sem precisar instalar o n8n.

E há uma coisa que eu evitaria: não coloque a tentativa de Twilio/WhatsApp como funcionalidade atual. Coloque apenas em Possíveis evoluções, porque assim seu GitHub fica 100% fiel ao que realmente está funcionando.
