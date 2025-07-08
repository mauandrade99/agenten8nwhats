
# Agente de WhatsApp Multimodal com n8n e OpenAI

Este é um workflow para n8n que implementa um agente de conversação inteligente para WhatsApp. O agente é capaz de processar mensagens de texto, áudio e imagem, utilizando os modelos da OpenAI (GPT-4o Mini para texto e visão, e Whisper para áudio) para gerar respostas contextuais e informadas.

## ✨ Principais Funcionalidades

- **Recepção Multimodal**: Aceita mensagens de texto, áudio e imagem via WhatsApp.
- **Transcrição de Áudio**: Converte mensagens de áudio em texto usando a API Whisper da OpenAI.
- **Análise de Imagem**: Descreve o conteúdo de imagens enviadas usando o modelo de visão GPT-4o Mini.
- **Inteligência Artificial**: Utiliza um Agente LangChain com o modelo `gpt-4o-mini` para processar as informações e formular respostas.
- **Memória de Conversa**: Mantém o histórico da conversa com cada usuário, permitindo respostas contextuais.
- **Ferramenta de Pesquisa**: O agente tem acesso à **Wikipedia** para buscar informações externas.

## 🚀 Guia Completo: Instalação e Inicialização do Ambiente

Esta seção é o seu guia definitivo para instalar as dependências, iniciar todos os serviços e configurar a conexão entre eles.

### Pré-requisitos

*   **Node.js (versão LTS)**: Essencial para rodar o n8n.
*   **Docker Desktop**: Para executar o container da Evolution API.
*   **Conta ngrok**: Para expor o webhook do n8n para a internet.
*   **Conta da OpenAI**: Com uma chave de API válida.
*   **Evolution API**: Já configurada em uma pasta com seu `docker-compose.yml`.

### Parte 1: Instalação (Apenas na Primeira Vez)

Se estiver configurando o ambiente do zero:

*   **Node.js**: Baixe a versão **LTS** do [site oficial do Node.js](https://nodejs.org/) e instale.
*   **n8n**: Após instalar o Node.js, abra um terminal (PowerShell/CMD) e execute:
    ```bash
    npm install -g n8n
    ```
*   **Docker Desktop**: Baixe e instale a partir do [site oficial do Docker](https://www.docker.com/products/docker-desktop/).
*   **ngrok**: Crie uma conta no [site do ngrok](https://ngrok.com/), baixe o executável e coloque-o em uma pasta de fácil acesso (ex: `C:\ngrok`).

### Parte 2: Passo a Passo para Iniciar o Ambiente

Para colocar tudo no ar, você precisará de **três terminais** abertos. Siga os passos na ordem:

####  терминал 1: Iniciar a Evolution API (Docker)

1.  Abra um terminal (PowerShell ou CMD).
2.  Navegue até a pasta do seu projeto da Evolution API.
    ```bash
    # Exemplo:
    cd C:\caminho\para\evolution-api
    ```
3.  Inicie o container em segundo plano:
    ```bash
    docker-compose up -d
    ```

####  терминал 2: Iniciar o ngrok (A Ponte para a Internet)

1.  Abra um **novo** terminal.
2.  Navegue até a pasta onde você salvou o ngrok.
    ```bash
    cd\Program Files (x86)\
    ```
3.  **(Apenas na primeira vez)** Configure seu token de autenticação:
    *   **Como encontrar seu authtoken:** Faça login na sua conta no site do ngrok. Vá para o dashboard e clique em "Your Authtoken" no menu lateral. Copie o token.
    *   Execute o comando no terminal:
    ```bash
    ngrok config add-authtoken 2wVEXa17V4guFLfmQJdYeezakyg_4CZok51kGbtkqAnqRoiX3
    ```
4.  Inicie o túnel HTTP para a porta padrão do n8n (5678):
    ```bash
    ngrok http 5678
    ```
5.  O ngrok exibirá uma URL pública na linha `Forwarding` (ex: `https://abcd-1234.ngrok-free.app`). **Copie esta URL.** Ela é essencial para o próximo passo.
6.  Deixe este terminal **aberto**. Se fechá-lo, o túnel será desfeito.

####  терминал 3: Iniciar o n8n (O Cérebro)

1.  Abra um **terceiro** terminal.
2.  Navegue até a pasta de dados do n8n, onde seus workflows estão salvos.
    ```bash
    cd \users\users\.n8n
    ```
3.  Inicie o n8n:
    ```bash
    npx n8n start
    ```
4.  Deixe este terminal **aberto**.

### Parte 3: Passo Final e Crucial - Configurar o Webhook

Agora, você precisa dizer à Evolution API para onde enviar as mensagens.

1.  **Acesse o Manager da Evolution API** no seu navegador: `http://localhost:8080/manager/login` e faça o login.

2.  Encontre a sua instância/sessão do WhatsApp e vá para as configurações dela.

3.  **Localize o campo "Webhook"**.

4.  **Construa a URL Completa do Webhook:**
    *   Pegue a URL que você copiou do ngrok (ex: `https://abcd-1234.ngrok-free.app`).
    *   Adicione o caminho do webhook do n8n no final. No seu workflow, o caminho é `wapptrigger`. O n8n espera o caminho no formato `/webhook/SEU_PATH`.
    *   Sua URL final será: **`URL_DO_NGROK/webhook/wapptrigger`**
    *   Exemplo: `https://abcd-1234.ngrok-free.app/webhook/wapptrigger`

5.  **Cole esta URL completa no campo "Webhook"** da Evolution API e salve as alterações.

**Pronto!** Seu ambiente está totalmente configurado. Novas mensagens enviadas para seu número de WhatsApp serão encaminhadas pela Evolution, passarão pelo túnel do ngrok e chegarão ao seu workflow no n8n.
