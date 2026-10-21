# Motor de Equivalência Acadêmica Explicável

<p align="justify">Um sistema de inteligência artificial aplicada com explicabilidade de ponta a ponta, projetado para comparar e validar equivalências entre ementas e percursos acadêmicos com auditoria, transparência e possibilidade de revisão humana.</p>

## 1. Inspiração do Projeto

<p align="justify">Este projeto foi inspirado em um desafio técnico compartilhado por recrutadores para a construção de motores de equivalência acadêmica auditáveis.</p>

<p align="center">
  <img src="fig1.png" alt="Imagem de referência do desafio publicado no LinkedIn" width="600px">
</p>

## 2. O Desafio

<p align="justify">Diferentemente de um chatbot tradicional ou de uma comparação baseada apenas em similaridade por cosseno — por exemplo, <code>cosine_similarity = 0.87</code> —, a análise de equivalência acadêmica exige responsabilidade, rastreabilidade e explicabilidade. Cada decisão do motor deve ser auditável e permitir compreender por que determinado resultado foi produzido, como as lacunas de conteúdo e as ementas incompletas são tratadas e de que forma um especialista pode validar ou corrigir a recomendação do sistema por meio de um processo de revisão humana (*Human-in-the-Loop*).</p>

## 3. Arquitetura em Três Camadas

<p align="justify">O motor utiliza uma arquitetura modular composta por três camadas sequenciais. Essa separação organiza as regras de negócio, a análise semântica e a decisão final, facilitando a manutenção, a avaliação e a auditoria do sistema.</p>

### 3.1. Camada 1 — Regras Determinísticas (`layer1_rules.py`)

<p align="justify">Compara as cargas horárias e calcula a sobreposição estrita dos tópicos obrigatórios. A camada retorna pontuações e justificativas estruturadas, podendo, por exemplo, reprovar automaticamente uma equivalência quando a razão entre cargas horárias fica abaixo de limites previamente definidos.</p>

### 3.2. Camada 2 — Similaridade Semântica (`layer2_semantic.py`)

<p align="justify">Utiliza modelos de representações vetoriais multilíngues, como <code>intfloat/multilingual-e5-large</code>, para comparar o significado dos conteúdos. A análise considera tanto a ementa de forma global quanto blocos e tópicos individualizados, produzindo uma matriz detalhada de correspondência entre trechos.</p>

### 3.3. Camada 3 — Juiz com Regras de Segurança (`layer3_judge.py`)

<p align="justify">Aplica regras obrigatórias de segurança e limites de decisão para determinar o resultado final. O veredito é classificado em três categorias: <code>APROVADO</code>, <code>REPROVADO</code> ou <code>ANALISE_HUMANA</code>. A categoria de análise humana permite encaminhar casos incertos ou que exijam avaliação especializada para revisão.</p>

## 4. Interface e Revisão Humana (`app.py`)

<p align="justify">A aplicação disponibiliza uma interface interativa construída com Streamlit, que permite acompanhar as recomendações do motor e registrar decisões de especialistas. A interface reúne validação humana, memória de precedentes e monitoramento das pontuações ao longo do tempo.</p>

<p align="justify"><strong>Validação humana:</strong> especialistas podem revisar a recomendação do modelo, registrar observações qualitativas e salvar a decisão final. Esse processo mantém a participação humana nos casos em que a análise automatizada precisa de confirmação ou correção.</p>

<p align="justify"><strong>Memória de precedentes:</strong> a persistência local com SQLite, por meio do banco <code>precedentes.db</code>, armazena o histórico de decisões e permite formar uma memória institucional consultável para apoiar avaliações futuras.</p>

<p align="justify"><strong>Monitoramento de mudanças:</strong> gráficos interativos permitem acompanhar a variação das pontuações e identificar possíveis mudanças no comportamento do sistema ao longo do tempo, apoiando a investigação de quedas de desempenho ou alterações nos dados analisados.</p>

## 5. Como Executar o Projeto a Partir do Repositório

### 5.1. Pré-requisitos

<p align="justify">Instale o Python 3.10 ou uma versão posterior. Também é necessário ter o Git disponível para clonar o repositório e um terminal para executar os comandos abaixo.</p>

### 5.2. Clonar o Repositório

<p align="justify">Substitua o endereço de exemplo pelo endereço real do repositório e, em seguida, entre na pasta do projeto.</p>

```bash
git clone https://github.com/seu-usuario/motor-equivalencia-explicavel.git
cd motor-equivalencia-explicavel
```

### 5.3. Criar e Ativar um Ambiente Virtual

<p align="justify">O ambiente virtual mantém as dependências do projeto isoladas das demais instalações Python do computador.</p>

```bash
python3 -m venv venv
source venv/bin/activate
```

<p align="justify">No Windows, a ativação pode ser feita com o comando abaixo, executado no Prompt de Comando:</p>

```bat
venv\Scripts\activate
```

### 5.4. Instalar as Dependências

<p align="justify">Com o ambiente virtual ativado, instale os pacotes listados no arquivo <code>requirements.txt</code>.</p>

```bash
pip install -r requirements.txt
```

### 5.5. Iniciar a Aplicação

<p align="justify">Execute o Streamlit a partir da pasta do projeto:</p>

```bash
streamlit run app.py
```

<p align="justify">Após a inicialização, abra o endereço local indicado no terminal. Por padrão, a aplicação costuma ficar disponível em <code>http://localhost:8501</code>.</p>

## 6. Estrutura do Repositório

<p align="justify">A estrutura abaixo resume os principais arquivos do projeto e suas responsabilidades. O banco de dados <code>precedentes.db</code> pode ser criado localmente durante o uso da aplicação, dependendo da implementação.</p>

```text
motor-equivalencia-explicavel/
├── layer1_rules.py       # Regras determinísticas e regras de negócio
├── layer2_semantic.py    # Representações vetoriais e similaridade semântica
├── layer3_judge.py       # Regras de segurança e decisão final
├── api.py                # Orquestração das três camadas
├── app.py                # Interface Streamlit e revisão humana com SQLite
├── requirements.txt      # Dependências do projeto
└── fig.png               # Imagem de referência do desafio
```

## 7. Conclusão

<p align="justify">O Motor de Equivalência Acadêmica Explicável propõe uma abordagem estruturada para comparar ementas e percursos acadêmicos sem depender exclusivamente de uma pontuação de similaridade. A combinação de regras determinísticas, análise semântica e uma camada final de decisão permite considerar tanto critérios objetivos — como carga horária e tópicos obrigatórios — quanto a proximidade de significado entre os conteúdos.</p>

<p align="justify">A interface com revisão humana, o registro de precedentes e o monitoramento das pontuações ampliam a rastreabilidade das decisões e oferecem subsídios para a melhoria contínua do processo. Dessa forma, o projeto busca apoiar avaliações mais consistentes e transparentes, mantendo a decisão humana disponível para casos ambíguos ou que exijam análise especializada.</p>

<p align="justify">Para executar o projeto com sucesso, confirme que o endereço do repositório foi substituído pelo endereço correto e que os arquivos mencionados na estrutura estão presentes. A qualidade das decisões também depende da calibração dos limites, da avaliação dos modelos utilizados e da validação do sistema com ementas reais representativas do contexto acadêmico.</p>
