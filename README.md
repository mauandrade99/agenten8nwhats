
# Agente de WhatsApp Multimodal com n8n e OpenAI

Este é um workflow para n8n que implementa um agente de conversação inteligente para WhatsApp. O agente é capaz de processar mensagens de texto, áudio e imagem, utilizando os modelos da OpenAI (GPT-4o Mini para texto e visão, e Whisper para áudio) para gerar respostas contextuais e informadas.

## 🚀 Guia Completo: Instalação e Inicialização do Ambiente

Esta seção é o seu guia definitivo para instalar as dependências, iniciar todos os serviços e configurar a conexão entre eles.

### Pré-requisitos

*   **Node.js (versão LTS)**: Essencial para rodar o n8n.
*   **Docker e Portainer**: Para gerenciar o ambiente da Evolution API.
*   **Conta ngrok**: Para expor o webhook do n8n para a internet.
*   **Conta da OpenAI**: Com uma chave de API válida.

### Parte 1: Instalação (Apenas na Primeira Vez)

Se estiver configurando o ambiente do zero:

*   **Node.js**: Baixe a versão **LTS** do [site oficial do Node.js](https://nodejs.org/) e instale.
*   **n8n**: Após instalar o Node.js, abra um terminal (PowerShell/CMD) e execute:
    ```bash
    npm install -g n8n
    ```
*   **Docker/Portainer**: Assumindo que seu ambiente Docker com Portainer já está configurado.
*   **ngrok**: Crie uma conta no [site do ngrok](https://ngrok.com/), baixe o executável e coloque-o em uma pasta de fácil acesso (ex: `C:\ngrok`).

### Parte 2: Passo a Passo para Iniciar o Ambiente

Para colocar tudo no ar, você precisará de **dois terminais** e do seu **navegador**.

####  Passo 1: Iniciar o Stack da Evolution API (Portainer)

1.  Abra seu navegador e acesse a interface do **Portainer** (ex: `http://localhost:9000`).
2.  No menu lateral, vá para **"Stacks"**.
3.  Localize o stack chamado `evov2`.
4.  Certifique-se de que o status dele é **"active"** ou **"running"**. Se não estiver, selecione-o e use os controles na parte superior para iniciá-lo ("Start"). Isso iniciará todos os quatro contêineres (`evolution_v2`, `postgres_db`, `pgadmin`, `redis_service`) de uma só vez.

    > **Nota sobre Credenciais:** Se precisar verificar o usuário e senha do Evolution Manager, clique no stack `evov2`, vá para a aba **"Editor"** e procure as variáveis `AUTHENTICATION_USERNAME` e `AUTHENTICATION_PASSWORD`.

####  Passo 2: Iniciar o ngrok (A Ponte para a Internet)

1.  Abra um terminal (PowerShell ou CMD).
2.  Navegue até a pasta onde você salvou o ngrok.
    ```bash
    cd \ngrok
    ```
3.  **(Apenas na primeira vez)** Configure seu token de autenticação (pegue no seu dashboard do ngrok):
    ```bash
    ngrok config add-authtoken SEU_TOKEN_AQUI
    ```
4.  Inicie o túnel HTTP para a porta padrão do n8n (5678):
    ```bash
    ngrok http 5678
    ```
5.  O ngrok exibirá uma URL pública (`Forwarding`). **Copie esta URL** (ex: `https://abcd-1234.ngrok-free.app`).
6.  Deixe este terminal **aberto**.

####  Passo 3: Iniciar o n8n (O Cérebro)

1.  Abra um **segundo** terminal.
2.  Navegue até a pasta de dados do n8n.
    ```bash
    cd \users\users\.n8n
    ```
3.  Inicie o n8n:
    ```bash
    npx n8n start
    ```
4.  Deixe este terminal **aberto**.

### Parte 3: Configuração Final do Webhook

1.  Acesse o Manager da Evolution API no seu navegador: `http://localhost:8080/manager/login` e faça o login.
2.  Encontre sua instância do WhatsApp e vá para as configurações.
3.  No campo **"Webhook"**, cole a URL completa construída com o endereço do ngrok e o caminho do seu workflow:
    *   **URL final:** `URL_DO_NGROK/webhook/wapptrigger`
    *   **Exemplo:** `https://abcd-1234.ngrok-free.app/webhook/wapptrigger`
4.  Salve as alterações.

**Tudo pronto!** Seu ambiente está completo, configurado e pronto para operar.
