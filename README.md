# PhishGuard AI

**Analisador inteligente de URLs com Python e Google Gemini**

O **PhishGuard AI** é uma aplicação desktop desenvolvida em Python para analisar URLs e identificar possíveis sinais associados a tentativas de phishing.

O sistema combina uma **análise heurística local**, baseada em características da URL, com uma **camada opcional de inteligência artificial utilizando o Google Gemini**, responsável por interpretar os indicadores encontrados e apresentar uma explicação em linguagem natural.

> **Aviso:** o PhishGuard AI é um projeto educacional. A classificação apresentada não garante que uma URL seja segura ou maliciosa.

---

## Objetivo

O projeto foi desenvolvido para aplicar conhecimentos de:

* Python
* Segurança da Informação
* Análise de URLs
* Desenvolvimento de aplicações desktop
* Inteligência Artificial generativa
* Integração com APIs
* Variáveis de ambiente e proteção de credenciais
* Lógica de classificação de riscos

A ideia é transformar conceitos básicos de segurança digital em uma ferramenta simples e acessível para o usuário.

---

## Funcionamento

O funcionamento do sistema ocorre em duas etapas principais:

```text
              URL
               |
               v
       +-----------------+
       | Análise local   |
       | da URL          |
       +--------+--------+
                |
                v
       Indicadores de risco
                |
                v
       Classificação inicial
                |
                v
       +-----------------+
       | Google Gemini   |
       | (opcional)      |
       +--------+--------+
                |
                v
       Explicação para o
            usuário
```

Primeiro, o PhishGuard verifica características da URL sem depender da inteligência artificial.

Depois, caso a API do Gemini esteja configurada, os indicadores encontrados são enviados para a IA, que interpreta os resultados.

---

## Análise da URL

O sistema verifica atualmente alguns indicadores comuns de URLs potencialmente suspeitas.

### Indicadores implementados

* URL inválida
* Presença do caractere `@`
* URL excessivamente longa
* Termos potencialmente suspeitos
* Utilização de `HTTP` em vez de `HTTPS`

### Termos analisados

```text
login
verify
verification
account
password
secure
update
confirm
```

A presença desses termos **não significa que o site seja necessariamente malicioso**. Eles são utilizados apenas como indicadores dentro da análise heurística.

---

## Classificação de risco

A classificação atual é baseada na quantidade de indicadores encontrados:

| Indicadores encontrados | Resultado         |
| ----------------------: | ----------------- |
|                       0 | SEM SINAIS ÓBVIOS |
|                       1 | BAIXO RISCO       |
|                     2–3 | RISCO MÉDIO       |
|               4 ou mais | ALTO RISCO        |

Essa classificação representa apenas o risco aparente de acordo com as regras implementadas no sistema.

---

## Integração com Google Gemini

O projeto possui integração com a **API do Google Gemini**.

A inteligência artificial recebe a URL analisada e os indicadores encontrados pelo sistema.

Ela é instruída a:

1. Avaliar o nível de risco aparente;
2. Explicar a importância dos indicadores;
3. Orientar o usuário sobre como proceder;
4. Informar as limitações da análise.

A IA também é orientada a **não afirmar que uma URL é definitivamente phishing apenas com base nos indicadores heurísticos**.

### Fluxo da IA

```text
URL
 |
 v
Análise heurística
 |
 v
Indicadores
 |
 v
Prompt
 |
 v
Google Gemini
 |
 v
Resposta em português
 |
 v
Interface do PhishGuard
```

A integração utiliza a biblioteca `google-genai`.

---

## Interface

A interface gráfica foi desenvolvida utilizando **CustomTkinter**.

A aplicação possui:

* Título do programa
* Área de conversação
* Campo para inserir URLs
* Botão de análise
* Suporte à tecla `Enter`
* Apresentação dos resultados
* Resposta da inteligência artificial

O formato de conversa foi escolhido para tornar a utilização mais simples e intuitiva.

---

## Tecnologias utilizadas

### Python

Linguagem principal utilizada no desenvolvimento do projeto.

### CustomTkinter

Utilizado para desenvolver a interface gráfica desktop.

### Google GenAI

Utilizado para realizar a comunicação entre o programa e a API do Google Gemini.

### urllib.parse

Biblioteca nativa do Python utilizada para interpretar a estrutura das URLs.

### os

Biblioteca nativa utilizada para acessar a API Key através de uma variável de ambiente.

---

## Estrutura do projeto

```text
PhishGuard-AI/
|
├── app.py
├── ia.py
└── README.md
```

### app.py

Arquivo principal da aplicação.

Responsável por:

* Criar a interface;
* Receber a URL;
* Realizar a análise heurística;
* Identificar indicadores;
* Classificar o risco;
* Apresentar os resultados;
* Chamar o módulo de inteligência artificial.

### ia.py

Responsável pela integração com o Google Gemini.

Contém:

* Recuperação da API Key;
* Criação do cliente Gemini;
* Construção do prompt;
* Envio da solicitação;
* Recebimento da resposta da IA.

---

# Instalação

## 1. Pré-requisitos

É necessário ter instalado:

* Python 3
* pip
* Conexão com a internet para utilizar o Gemini

---

## 2. Instalar as dependências

Abra o terminal na pasta do projeto e execute:

```bash
pip install customtkinter google-genai
```

---

## 3. Configurar o Gemini

Para utilizar a inteligência artificial, é necessário possuir uma API Key do Google Gemini.

Depois de obter a chave, configure uma variável de ambiente.

### Windows PowerShell

```powershell
$env:GEMINI_API_KEY="SUA_CHAVE_AQUI"
```

A chave **não deve ser colocada diretamente no código**.

Também não deve ser publicada no GitHub.

---

## 4. Executar

Na pasta do projeto:

```bash
python app.py
```

A interface gráfica do **PhishGuard AI** será aberta.

---

# Exemplo de utilização

O usuário pode inserir uma URL:

```text
http://exemplo.com/login
```

O sistema poderá identificar:

```text
RISCO MÉDIO

Domínio analisado:
exemplo.com

Indicadores encontrados:

• URL contém o termo suspeito: login
• Conexão não utiliza HTTPS
```

Depois, com o Gemini configurado, a IA poderá explicar por que esses indicadores merecem atenção e quais cuidados o usuário deve tomar.

---

# Segurança da API Key

A chave da API não é armazenada diretamente no código.

O projeto utiliza:

```python
os.getenv("GEMINI_API_KEY")
```

Dessa maneira, a aplicação recupera a chave a partir de uma variável de ambiente.

### Não faça

```python
api_key = "SUA_CHAVE"
```

### Faça

```python
api_key = os.getenv("GEMINI_API_KEY")
```

Isso reduz o risco de expor a chave acidentalmente ao compartilhar o código ou publicar o projeto no GitHub.

---

# Limitações

O PhishGuard AI **não é um antivírus nem um sistema profissional de detecção de phishing**.

A versão atual utiliza principalmente características presentes na própria URL.

Portanto:

* Uma URL maliciosa pode não apresentar os indicadores implementados;
* Uma URL legítima pode apresentar características consideradas suspeitas;
* A presença de palavras como `login` ou `secure` não torna um site malicioso;
* Utilizar HTTPS não garante que um site seja confiável;
* A inteligência artificial pode cometer erros;
* A análise não verifica todo o conteúdo da página;
* A análise não garante a segurança do domínio.

Por esses motivos, o resultado deve ser utilizado como **auxílio à análise**, e não como uma confirmação definitiva.

---

# Possíveis melhorias

Algumas funcionalidades que podem ser implementadas em versões futuras:

* [ ] Integração com VirusTotal
* [ ] Verificação da reputação do domínio
* [ ] Consulta da idade do domínio
* [ ] Identificação de domínios semelhantes
* [ ] Detecção de URLs encurtadas
* [ ] Análise de certificados SSL
* [ ] Sistema de pontuação mais avançado
* [ ] Histórico de análises
* [ ] Exportação dos resultados
* [ ] Testes automatizados
* [ ] Empacotamento como `.exe`
* [ ] Banco de dados de URLs maliciosas conhecidas

---

# Contexto educacional

O PhishGuard AI foi desenvolvido como um projeto de estudo e portfólio com o objetivo de aplicar conhecimentos de **programação, segurança digital e inteligência artificial** em uma aplicação prática.

O projeto demonstra como uma aplicação Python pode combinar:

```text
Programação
     +
Segurança
     +
Interface gráfica
     +
Inteligência Artificial
     =
PhishGuard AI
```

---

# Autor

**Enzo Nogueira**

Projeto desenvolvido para fins educacionais e de portfólio.

---

# Licença

Este projeto foi desenvolvido para fins educacionais.
