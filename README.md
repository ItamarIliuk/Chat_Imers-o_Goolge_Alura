# Chat + Google Gemini + Salva Conversas em Arquivo

Este projeto é resultado da atividade da aula 04 da Imersão Google + Alura sobre Gemini e Agentes. O notebook permite criar um chat interativo com modelos Gemini do Google, registrar o histórico das conversas e salvar as interações em arquivo de texto.

## Como utilizar este notebook

### 1. Pré-requisitos

- Conta Google para utilizar o Colab.
- Chave de API do Google Gemini (Google AI Studio).
- Python 3 (o notebook roda diretamente no Google Colab).

### 2. Instale as dependências

No Colab, execute a primeira célula para instalar a biblioteca necessária:

```python
!pip install google-genai
```

### 3. Configure sua chave de API

O notebook utiliza o `userdata` do Colab para armazenar a variável de ambiente `GOOGLE_API_KEY`. Para isso, insira sua chave de API do Google Gemini conforme abaixo:

```python
import os
from google.colab import userdata

os.environ['GOOGLE_API_KEY'] = userdata.get('GOOGLE_API_KEY')
```

> **Dica:** No Colab, vá em “Editar > Preferências de usuário” para adicionar sua chave de API.

### 4. Inicialize o cliente Gemini

```python
from google import genai

client = genai.Client()
```

### 5. Liste os modelos disponíveis (opcional)

Você pode listar os modelos Gemini disponíveis:

```python
for model in client.models.list():
    print(model.name)
```

### 6. Escolha um modelo

Por exemplo:

```python
model = "gemini-2.0-flash"
```

### 7. Gere uma resposta simples

```python
resposta = client.models.generate_content(model=model,
                                         contents="Quem é a empresa por trás dos modelos Gemini?")
print(resposta.text)
```

### 8. Crie e utilize um chat interativo

```python
chat = client.chats.create(model=model)

# Envie uma mensagem
resposta = chat.send_message("Boa tarde, como vai você?")
print(resposta.text)
```

Você pode continuar enviando mensagens para o chat normalmente.

### 9. Personalize o comportamento do assistente (opcional)

É possível definir instruções de sistema para o assistente:

```python
from google.genai import types

chat_config = types.GenerateContentConfig(
    system_instruction="Você é um assistente pessoal e sempre responde de forma sucinta."
)
chat = client.chats.create(model=model, config=chat_config)
```

### 10. Salve as interações em arquivo

O notebook fornece uma função para extrair e salvar a última interação (pergunta e resposta) no arquivo `interações_chatbot.txt`:

```python
def extrair_e_salvar_ultima_interacao(historico_chat, nome_arquivo="interações_chatbot.txt"):
    # Veja a implementação completa da função no notebook
    ...
```

Ao final de cada interação, chame a função para salvar o diálogo:

```python
historico_atual = chat.get_history()
extrair_e_salvar_ultima_interacao(historico_atual)
```

### 11. Chat em loop com salvamento automático

O notebook inclui um laço para perguntas e respostas, salvando automaticamente cada interação:

```python
prompt = input("Digite uma pergunta: ")

while prompt != "fim":
    resposta = chat.send_message(prompt)
    print("Resposta: ", resposta.text)
    historico_atual = chat.get_history()
    extrair_e_salvar_ultima_interacao(historico_atual)
    prompt = input("Digite uma pergunta: ")
```

As perguntas e respostas ficam salvas no arquivo `interações_chatbot.txt`.

---

## Observações

- Caso deseje mudar o estilo de resposta do assistente, basta alterar a configuração do `system_instruction`.
- Para mais detalhes sobre a biblioteca [google-genai](https://pypi.org/project/google-genai/), consulte a documentação oficial.
- O notebook pode ser executado tanto no Google Colab quanto localmente (desde que as dependências estejam instaladas e a chave de API esteja válida).

---
