# 🤖 Sr. Doutor - Assistente Farmacêutico Interativo com Google Gemini e ADK

A inspiração desse projeto é para auxiliar as pessoas mais idosas e que possuem dificuldades para ler as bulas dos medicamentos.
A ideia inicial era tirar uma foto da receita e o assitente identificaria o medicamento e posologia. Uma vez esses dados capturados as respostas seriam dadas escritas e por voz. A sequência seria a seguinte :
  1- Confirmar os medicamentos;
  2- Confirmar a posologia;
  3- Confirmar a data e hora de início do tratamento;
  4- Criar alertas na agenda do Google para lembrar o horário e a medicação a ser tomada;(Google Agenda)
  5- Buscar a Indicação, Contraindicação, Efeitos Colaterais, Advetências e Nome Genérico e dando a opção do usuário abrir ou não essa opção; (Google Search)
  6- Solicitar o CEP do usuário para indicar as farmácias mais próximas para compra do medicamento; (Google Maps)
  7- Fazer um levantamento de preços aproximado para informar como referência apenas. (Google Search)

  Como sou iniciante em tudo isso, não consegui criar tudo isso, mas acredito que tenho o início de um agente bem interessante que poderá chegar na ideia original com um bom desesenvolvimento.

Segue descrição do que foi realizado e está disponível para teste.
  
Este projeto demonstra a criação de um assistente virtual farmacêutico interativo utilizando a API Google Gemini através do SDK `google-generativeai` e o Agent Development Kit (ADK) do Google para a estrutura de agentes. O assistente é capaz de fornecer informações detalhadas sobre medicamentos, incluindo indicações, contraindicações, efeitos colaterais, e também oferece sugestões sobre como encontrar locais de compra e estimativas de preço.

## 🌟 Funcionalidades Principais

*   **Consulta Interativa:** O assistente guia o usuário através de um fluxo de conversa natural.
*   **Informações Detalhadas de Medicamentos:**
    *   Principal Indicação
    *   Contraindicações Principais
    *   Efeitos Colaterais Comuns
    *   Advertências Importantes e Efeitos Graves
    *   Nome Genérico
*   **Assistência para Compra (Sugestões):**
    *   Pergunta a localização do usuário (CEP/Cidade/Bairro).
    *   Sugere como o usuário pode encontrar farmácias próximas usando aplicativos de mapas e sites de grandes redes.
    *   Oferece dicas sobre como pesquisar preços online, com ressalvas sobre a variabilidade.
*   **Uso da Ferramenta `google_search` (Experimental):**
    *   O agente é instruído a usar a ferramenta `google_search` (se ativada e configurada) para tentar encontrar nomes de farmácias próximas à localização informada pelo usuário.
    *   *Nota: A eficácia e precisão desta funcionalidade dependem da qualidade dos resultados da busca e da capacidade do LLM de extrair informações relevantes.*
*   **Construído com Google Gemini:** Utiliza o poder dos modelos de linguagem generativa do Google.
*   **Estrutura com Google ADK:** Emprega o Agent Development Kit para gerenciamento do agente, sessões e execução.

## 🛠️ Tecnologias Utilizadas

*   **Python 3.x**
*   **Google Gemini API:** Através da biblioteca `google-generativeai`.
*   **Google Agent Development Kit (ADK):** Biblioteca `google-adk`.
*   **Jupyter Notebook / Google Colab:** Ambiente de desenvolvimento e execução do script.
*   **Bibliotecas Auxiliares:** `os`, `textwrap`, `IPython.display`, `datetime`, `warnings`.

## 🚀 Como Executar o Projeto (Ambiente Colab)

1.  **Pré-requisitos:**
    *   Uma conta Google.
    *   Uma API Key da Google Gemini. Você pode obtê-la no [Google AI Studio](https://aistudio.google.com/app/apikey).

2.  **Configurar API Key no Colab:**
    *   Abra o notebook no Google Colab.
    *   No painel esquerdo, clique no ícone de chave (🔑 Secrets).
    *   Adicione uma nova "secret" com o nome `GOOGLE_API_KEY` e cole sua API Key como valor. Certifique-se de que o acesso ao notebook está habilitado para esta secret.

3.  **Abrir o Notebook:**
    *   Faça o upload do arquivo `.ipynb` para o seu Google Drive e abra-o com o Google Colab, ou clone este repositório e abra o notebook diretamente do GitHub no Colab.

4.  **Executar a Célula Principal:**
    *   O projeto está contido em uma única célula de código principal no notebook.
    *   Selecione "Ambiente de execução" > "Reiniciar ambiente de execução" para garantir um estado limpo.
    *   Execute a célula de código. Ela cuidará de:
        *   Instalar as bibliotecas necessárias (`google-generativeai`, `google-adk`).
        *   Configurar a API Key.
        *   Definir o agente e suas instruções.
        *   Iniciar o loop de interação com o usuário.

5.  **Interagir com o Assistente:**
    *   Siga os prompts do assistente no output da célula.
    *   Forneça os nomes dos medicamentos quando solicitado.
    *   Responda às perguntas do agente (ex: "sim/não", CEP/cidade).
    *   Digite "sair" para encerrar a conversa.

## 📝 Estrutura do Código Principal

O script no notebook é organizado nas seguintes seções:

1.  **Instalação das Bibliotecas:** Uso de `%pip install` para `google-generativeai` e `google-adk`.
2.  **Imports e Configuração da API Key:** Importação dos módulos necessários e configuração da API Key (obtida do `userdata` do Colab e definida como variável de ambiente).
3.  **Seleção do Modelo LLM:** Definição do `MODEL_ID` a ser usado (ex: `gemini-1.5-flash-latest`).
4.  **Funções Auxiliares:**
    *   `to_markdown()`: Para formatar a saída do agente para exibição no Colab.
5.  **Definição do Agente:**
    *   Criação da instância `Agent` do ADK.
    *   Definição detalhada das `instructions` que guiam o comportamento e o fluxo de conversa do agente.
    *   Inclusão da ferramenta `google_search` (se ativada).
6.  **Interação com o Usuário:**
    *   Configuração do `Runner` e `InMemorySessionService` do ADK.
    *   Loop principal `while` para a conversa:
        *   Coleta a entrada do usuário (`input()`).
        *   Envia a mensagem para o `runner.run()`.
        *   Processa os eventos retornados pelo runner para extrair a resposta textual do agente.
        *   Lida com eventos de chamada de ferramenta (`tool_code`) se a busca estiver ativa.
        *   Exibe a resposta do agente formatada.

## 🧠 Detalhes das Instruções do Agente

As instruções do agente são o "cérebro" da sua lógica de conversação. Elas são cuidadosamente elaboradas para cobrir:
*   O fluxo de confirmação dos medicamentos.
*   A pergunta sobre o desejo de ver detalhes.
*   O formato para apresentar os detalhes dos medicamentos (indicação, contraindicações, etc.).
*   A transição para perguntar sobre a localização do usuário.
*   As instruções para usar a ferramenta `google_search` para farmácias (com foco em nomes e, se possível, referências de endereço).
*   A forma de apresentar as sugestões de preço e a ressalva obrigatória.
*   O ciclo de "Posso ajudar com mais alguma coisa?".

## 💡 Possíveis Melhorias e Próximos Passos

*   **Integração Robusta com API de Mapas:** Para obter endereços precisos de farmácias, integrar com uma API como Google Places seria o ideal. Isso exigiria:
    *   Obtenção e configuração de uma API Key para o serviço de mapas.
    *   Criação de uma ferramenta customizada no ADK para chamar essa API.
    *   Tratamento dos resultados da API (JSON) para extrair endereços.
*   **Interface de Usuário Mais Amigável:** Em vez de um notebook Colab, o agente poderia ser integrado a uma interface web ou um aplicativo de mensagens usando frameworks como Flask, Streamlit, ou plataformas de chatbot.
*   **Gerenciamento de Estado de Sessão Persistente:** Atualmente, usa-se `InMemorySessionService`. Para um agente de produção, um serviço de sessão persistente (ex: banco de dados) seria necessário.
*   **Refinamento Contínuo das Instruções:** Testar com mais usuários e casos de uso para aprimorar a clareza e robustez das instruções do LLM.
*   **Tratamento de Erros Mais Detalhado:** Adicionar mais blocos `try-except` para lidar com falhas específicas de rede, API, ou da ferramenta de busca.
*   **Logging:** Implementar logging para registrar interações e possíveis erros para depuração e análise.

## ⚠️ Limitações e Considerações

*   **Precisão da Informação Médica:** O assistente fornece informações baseadas no conhecimento do LLM e, se aplicável, em buscas na web. **ESSAS INFORMAÇÕES NÃO SUBSTITUEM O ACONSELHAMENTO DE UM PROFISSIONAL DE SAÚDE (MÉDICO OU FARMACÊUTICO).** Sempre consulte um profissional para questões médicas.
*   **Disponibilidade e Preços de Medicamentos:** As informações sobre onde comprar e preços são estimativas e sugestões. A disponibilidade real e os preços exatos devem ser sempre verificados diretamente com as farmácias.
*   **Limites de Quota da API:** O uso da API Gemini está sujeito a limites de taxa. Uso excessivo pode levar a erros `429 RESOURCE_EXHAUSTED`.
*   **Qualidade da Busca na Web:** Se a ferramenta `google_search` for usada, a qualidade e relevância dos resultados da busca podem variar, afetando a capacidade do agente de extrair informações úteis sobre farmácias.

## 🤝 Contribuições

Contribuições para este projeto são bem-vindas! Sinta-se à vontade para sugerir melhorias, correções de bugs ou novas funcionalidades.

---
