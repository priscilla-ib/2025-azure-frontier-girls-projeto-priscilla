# Personal Trainer Condropatia AI
Agente criado no Azure AI Foundry, utilizando o modelo GPT-4.1, projetado para montar treinos seguros e personalizados para pessoas com condropatia patelar, ajustar a progressão automaticamente conforme feedback e, opcionalmente, enviar treinos por e-mail via Logic App — tudo low-code e com baixíssimo custo.
________________________________________
# Objetivo do Projeto
Este projeto tem como objetivo entregar um agente capaz de:
•	Criar treinos seguros, progressivos e adaptados para condropatia patelar
•	Recolher feedback simples do usuário (dor e intensidade)
•	Ajustar automaticamente o treino conforme necessidade
•	Consumir pouquíssimos tokens (GPT-4.1 + persistência resumida)
•	Operar totalmente low-code, sem necessidade de programação
•	Enviar o treino por e-mail automaticamente após cada ciclo
•	Ser facilmente implantado, mantido e expandido
________________________________________
#  Funcionalidades Principais
•	Coleta de dados iniciais (nome, idade, peso, dor, frequência, local)
•	Geração automática do treino
•	Feedback guiado
•	Ajuste automático do treino
•	Persistência resumida do histórico (economia de tokens)
•	Linguagem motivadora e segura
•	Envio automático de treino por e-mail (opcional)
•	Fluxo de conversação controlado (reduz custo e garante precisão)
________________________________________
# Configuração Técnica no Azure AI Foundry
•	Modelo: GPT-4.1 (version:2025-04-14)
•	Temperatura: 1
•	Ferramentas:
o	Enviaemail_treino (Action via Logic App)
•	Modo: Conversational Agent (low-code)
________________________________________
# Prompt do Sistema (para o agente no foundry)
Você é um personal trainer especializado em condropatia patelar.
Seu papel é montar treinos seguros, claros e progressivos.

REGRAS PRINCIPAIS:
1. Nunca sugerir exercícios de impacto ou que causem dor no joelho.
2. Priorizar fortalecimento de quadríceps, glúteos, glúteos médio e core.
3. Ajustar o treino com base no feedback: dor, facilidade ou dificuldade.
4. Exibir treinos sempre organizados (dias → exercícios → séries/reps).
5. Usar tom motivador, amigável e leve.
6. Manter no contexto persistente apenas um resumo breve do usuário, treino e feedback.
7. Sempre finalizar perguntando: “Deseja receber o treino por e-mail?”
8. Se o usuário disser “sim”, chamar a ação enviaemail_treino com:
   - email: e-mail informado pelo usuário
   - treino: treino formatado
9. Se faltarem dados, solicitar apenas:
   Nome,idade, peso, dor (0–10), frequência semanal, ambiente (casa/acad).
10. Se houver dor em algum exercício, substituir por variação isométrica segura.
11. Você não responde perguntas sobre qualquer outro assunto.

Objetivo: ajudar o usuário a evoluir com segurança e sem dor.
________________________________________
# Fluxo esperado da conversa com o Agente
1. Coleta de Dados
Perguntas:
•	Nome
•	Idade
•	Peso
•	Frequência semanal
•	Nível de dor (0–10)
•	Ambiente (casa ou academia)
•	E-mail para envio do treino (opcional)
________________________________________
2. Geração do Treino
Exemplo:
- Segunda-feira
•	Ponte de Glúteo — 3×10
•	Cadeira extensora leve — 3×12
•	Prancha — 30s
- Quarta-feira
•	Bicicleta leve — 10 min
•	Mobilidade de quadril — 2×10
•	Ponte unilateral — 3×8
- Sexta-feira
•	Isometria na parede — 3×30s
•	Alongamento posterior — 2×30s
•	Prancha lateral — 2×20s
________________________________________
3. Feedback
Perguntas:
•	Houve dor? (sim/não)
•	Como classificaria a intensidade? (fácil / moderado / difícil)
________________________________________
4. Ajuste Automático
Regras:
•	Se dor → reduzir carga, diminuir amplitude, trocar exercício
•	Se fácil → +1 série ou progressão
•	Se difícil → remover série ou regressão leve
________________________________________
5. Envio por E-mail (opcional)
O agente pergunta:
“Deseja receber este treino por e-mail?”
•	Se sim → chama a ação enviaemail_treino
•	Se não → prossegue normalmente
________________________________________
## Fluxo do Agente (Diagrama)
\```mermaid
flowchart TD
  A[Coleta de dados] --> B[Geração do treino]
  B --> C[Feedback do usuário]
  C --> D{Dor ou dificuldade?}
  D -->|Não| E[Pergunta sobre e-mail]
  D -->|Sim| F[Ajusta treino automaticamente]
  E --> G{Enviar e-mail?}
  G -->|Sim| H[Chama enviaemail_treino]
  G -->|Não| I[Fim]
\```
________________________________________
# Integração: Envio Automático de Treino por E-mail
Para permitir que o agente envie o plano de treino diretamente por e-mail, foi criada uma integração entre o Azure AI Foundry e um Logic App com conector do Outlook. A ação utilizada pelo agente chama enviaemail_treino.
________________________________________
# Estrutura do arquivo com prints (Evidências):
1.0 Modelo criado
2.0 Configurando Logic App Action
3.0 Visão no Logic App
4.0 Conversa com o agente 
4.1 Teste do action enviaemail_treino
4.2 Novo teste de e-mail após feedback do treino
4.3 Testes do e-mail enviado
5.0 E-mails enviados
5.1 E-mails recebidos
5.1.1 E-mail detalhado recebido
