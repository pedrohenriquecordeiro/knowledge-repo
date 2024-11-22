# Modelos de Linguagem com a SDK do Vertex AI

O **Vertex AI Generative AI Studio**, do Google Cloud, permite que você utilize modelos de linguagem como o **Gemini** diretamente no Python. 
Esses modelos são ideais para uma ampla gama de tarefas, como criação de conteúdo, interações por chat, resumos de texto e outras aplicações.

Vamos explorar na prática a **classe GenerativeModel** da SDK do Vertex AI, explicando como usar os métodos `generate_content` e `start_chat` para geração de textos.

## Principais Modelos Disponíveis

O Vertex AI oferece os seguintes modelos de linguagem:

### 1. **Text-Bison**
Focado em geração de texto, excelente para criar redações, resumos ou qualquer outro conteúdo baseado em texto.

### 2. **Chat-Bison**
Desenvolvido para interações contínuas, simula um chatbot ou assistente virtual.

### 3. **Codey**
Especializado em compreensão e geração de código, é ideal para ajudar em tarefas de programação.

### 4. **Gemini**
O mais avançado, combina capacidades de texto e conversa, com contextualização aprimorada e flexibilidade para diversas aplicações. Atualmente é um modelo multimodal, tem a capacidade de receber além de texto audio, imagens e videos.

---

## Pré-requisitos

### Configuração inicial
Antes de começar, você precisará configurar o seu ambiente:

1. **Instale a SDK do Vertex AI**:
   ```
   # Instala a biblioteca necessária para usar o Vertex AI
   pip install google-cloud-aiplatform
   ```

2. **Autentique-se no Google Cloud**:
   Certifique-se de estar autenticado no projeto correto:
   ```
   # Autenticação com o Google Cloud
   gcloud auth application-default login
   ```

3. **Configure o projeto e região no código**:
   ```
   # Inicializa o ambiente do Vertex AI com o ID do projeto e a localização
   from google.cloud import aiplatform

   aiplatform.init(project="seu-projeto-id", location="us-central1")
   ```

---

## Configurações de Geração e Segurança

Ao utilizar os modelos, você pode personalizar o comportamento de geração com os seguintes parâmetros:

- **`GenerationConfig`**: 
  - **temperature**: Controla a criatividade do modelo (valores mais altos = respostas mais criativas).
  - **max_output_tokens**: Define o limite máximo de palavras ou tokens na saída.
  - **top_k** e **top_p**: Ajustam como os tokens mais prováveis são selecionados.

- **`SafetySettings`**: 
  - Restringe respostas inadequadas ou sensíveis ao ajustar categorias e limiares.

Exemplo de configuração:
```
# Configuração dos parâmetros de geração
generation_config = {
    "temperature": 0.7,  # Ajuste de criatividade
    "max_output_tokens": 256,  # Limite de 256 tokens na resposta
    "top_k": 40,  # Seleciona os 40 tokens mais prováveis
    "top_p": 0.8,  # Seleção baseada em probabilidade cumulativa
}

# Configuração de segurança para evitar respostas inadequadas
safety_settings = [
    {"category": "harm_category", "threshold": 1},  # Categoria de dano em nível 1
    {"category": "violence", "threshold": 2},  # Categoria de violência em nível 2
]
```

---

## Classe `GenerativeModel`
A classe GenerativeModel é a base para trabalhar com modelos de linguagem generativa disponíveis na plataforma. 
Essa classe fornece uma interface para tarefas de geração de texto e interação por chat, permitindo integrar funcionalidades como a criação de conteúdos, 
respostas a prompts e diálogos contextuais diretamente em aplicações Python. 

### Método 1: `generate_content`

Esse método é usado para gerar respostas únicas e diretas a partir de um prompt, como resumos ou explicações.

#### Exemplo

```
from vertexai.preview.language_models import TextGenerationModel

# Carrega o modelo de geração de texto
model = TextGenerationModel.from_pretrained("text-bison")

# Define o prompt que será enviado ao modelo
prompt = "Explique os benefícios da energia solar de forma resumida."

# Faz a chamada ao modelo com as configurações de geração
response = model.predict(
    prompt,  # Texto de entrada
    temperature=0.7,  # Criatividade moderada
    max_output_tokens=256,  # Limite de 256 tokens na saída
    top_k=40,  # Seleciona os 40 tokens mais prováveis
    top_p=0.8  # Usa probabilidade cumulativa para a escolha dos tokens
)

# Exibe o texto gerado pelo modelo
print("Resposta Gerada:")
print(response.text)
```

#### Saída esperada
```
# Exemplo de resposta gerada pelo modelo
A energia solar é sustentável, reduz custos com eletricidade e contribui para a diminuição das emissões de carbono, promovendo um futuro mais limpo e ecológico.
```

---

### Método 2: `start_chat`

#### O que é?
Esse método é usado para interações mais ricas e contextuais, mantendo um histórico da conversa. Perfeito para chatbots ou assistentes interativos.

#### Exemplo de Uso

```
from vertexai.preview.language_models import ChatModel

# Carrega o modelo de chat
chat_model = ChatModel.from_pretrained("chat-bison")

# Inicializa um chat com um contexto inicial
chat = chat_model.start_chat(
    context="Você é um assistente especializado em tecnologia.",  # Define o contexto do assistente
    examples=[  # Exemplos para ajudar o modelo a entender o estilo das respostas
        {"input": "O que é IA?", "output": "IA é a inteligência artificial, que simula capacidades humanas."}
    ]
)

# Envia uma mensagem ao modelo e recebe uma resposta
response = chat.send_message(
    "Pode me explicar o que é aprendizado de máquina?",  # Pergunta ao modelo
    temperature=0.7,  # Criatividade moderada
    max_output_tokens=256,  # Limite de 256 tokens na resposta
    top_k=40,  # Seleciona os 40 tokens mais prováveis
    top_p=0.8  # Usa probabilidade cumulativa para seleção
)

# Exibe a resposta gerada pelo modelo
print("Resposta do Chat:")
print(response.text)
```

#### Saída esperada
```
# Exemplo de resposta gerada pelo modelo no chat
O aprendizado de máquina é uma área da inteligência artificial que permite que sistemas aprendam e melhorem automaticamente com base em dados, sem serem explicitamente programados.
```

---

## Diferenças Entre os Métodos

| Aspecto              | `generate_content`                                   | `start_chat`                                    |
|----------------------|------------------------------------------------------|-----------------------------------------------|
| **Propósito**        | Geração de texto único com base em um prompt.        | Conversas contínuas com contexto persistente. |
| **Contexto**         | Não guarda histórico.                                | Histórico é mantido para interações futuras.  |
| **Uso Ideal**        | Textos únicos, como respostas, resumos ou redações.  | Diálogos interativos ou assistentes virtuais. |

---

## Considerações Finais

A SDK do **Vertex AI** é uma ferramenta poderosa para integrar modelos de linguagem generativa às suas aplicações. A combinação de métodos como `generate_content` e `start_chat` com a flexibilidade dos modelos **Gemini**, **Text-Bison** e **Chat-Bison** permite criar soluções robustas para várias necessidades.

Experimente os exemplos, ajuste as configurações e aproveite ao máximo o potencial dessa tecnologia!
