# 🧠 LeidsonVect-SemanticLite

### Embeddings semânticos com PHP, SQLite e uma arquitetura leve.

**Transforme texto em vetores. Construa aplicações inteligentes. Simplifique sua infraestrutura.**

O **LeidsonVect-SemanticLite** é um projeto experimental de processamento semântico que disponibiliza embeddings de texto por meio de uma API desenvolvida em PHP, utilizando vocabulários vetoriais pré-computados e armazenados em bancos SQLite.

A proposta é explorar uma alternativa leve para aplicações que precisam trabalhar com representações vetoriais de texto sem executar um modelo neural a cada requisição.

🌐 **Experimente a demonstração:** https://embeddings.leidson.pro/

---

## 🚀 Por que conhecer o LeidsonVect-SemanticLite?

Aplicações modernas precisam compreender relações entre textos, encontrar conteúdos relevantes e identificar padrões que vão além da correspondência exata de palavras.

O LeidsonVect-SemanticLite explora uma abordagem diferente: **utilizar vetores previamente calculados para transformar consultas textuais em representações numéricas de forma eficiente.**

Com o processamento vetorial realizado em PHP e os dados armazenados em SQLite, o projeto busca reduzir a complexidade operacional de determinados cenários de processamento semântico.

### ✨ Principais características

- ⚡ **Processamento leve:** consulta vetores previamente armazenados, sem executar inferência neural durante a vetorização.
- 📦 **Armazenamento SQLite:** vocabulários vetoriais organizados em bancos de dados locais.
- 🔢 **Vetores de diferentes dimensões:** suporte previsto para modelos de 384 e 768 dimensões.
- 🔌 **API HTTP:** integração com aplicações web e outros serviços.
- 🐘 **PHP nativo:** processamento integrado ao ecossistema PHP, sem exigir Python para executar a etapa de consulta vetorial.
- 🔍 **Exploração semântica:** base para experimentos com busca por similaridade, recuperação de informações e organização de conteúdos.
- 🧪 **Laboratório interativo:** demonstrações de aplicações semânticas disponíveis no site do projeto.

> **A ideia central:** separar o custo de preparação dos vetores do processamento das consultas, aproveitando um vocabulário previamente calculado.

---

## 🧩 O que são embeddings?

Embeddings são representações numéricas de informações, como palavras, frases ou documentos.

Em sistemas que utilizam embeddings semanticamente consistentes, os vetores podem ser comparados para estimar relações entre conteúdos.

Essa propriedade permite desenvolver aplicações como mecanismos de busca por similaridade, sistemas de recomendação, agrupamento de documentos e recuperação contextual de informações.

O LeidsonVect-SemanticLite utiliza um vocabulário vetorial pré-computado para produzir representações de frases por meio da combinação dos vetores das palavras reconhecidas.

**Importante:** a qualidade semântica dos resultados depende do modelo de origem, da composição do vocabulário e do método de geração dos vetores. O desempenho e a qualidade devem ser avaliados conforme a aplicação.

---

## 🛠️ Como funciona?

A arquitetura básica pode ser representada pelo seguinte fluxo:

```text
              APLICAÇÃO CLIENTE
                     |
                     | Texto de entrada
                     v
                API HTTP / PHP
                     |
                     v
          Tokenização e normalização
                     |
                     v
            Consulta ao SQLite
                     |
                     v
        Recuperação dos vetores conhecidos
                     |
                     v
          Combinação dos vetores
                     |
                     v
             Embedding da frase
                     |
                     v
              Resposta JSON
```

O fluxo permite reutilizar vetores já armazenados, evitando recalcular individualmente a representação neural de cada palavra durante uma consulta.

Na implementação atual, a representação da frase é calculada pela média dos vetores válidos encontrados no vocabulário.

---

## 💡 Aplicações possíveis

### ⚡ 1. Cache semântico para LLMs

Evite chamadas desnecessárias a modelos generativos quando uma pergunta puder ser associada a uma consulta anterior.

Uma arquitetura de cache semântico pode comparar a representação de uma nova pergunta com as perguntas armazenadas e reutilizar uma resposta quando a similaridade atingir um limiar adequado.

**Possíveis benefícios:**
- Redução de chamadas repetidas a APIs de IA.
- Menor latência em consultas atendidas pelo cache.
- Potencial redução de custos operacionais.

O resultado depende da qualidade dos embeddings, do mecanismo de comparação e dos critérios utilizados para aceitar uma resposta em cache.

### 🔍 2. Busca semântica

Desenvolva mecanismos de recuperação de informações que considerem a proximidade entre representações vetoriais.

Aplicações possíveis:
- Busca de documentos.
- Pesquisa em bases de conhecimento.
- Localização de artigos e conteúdos.
- Recuperação de informações corporativas.

A busca por similaridade vetorial precisa ser implementada sobre os embeddings e os documentos indexados. A simples geração de um vetor não constitui, isoladamente, um mecanismo completo de busca semântica.

### 🎯 3. Sistemas de recomendação

Compare vetores de produtos, artigos ou outros elementos para identificar itens com representações próximas.

A abordagem pode servir como componente de sistemas de recomendação, desde que os vetores utilizados sejam adequados ao domínio e que a similaridade seja validada com dados representativos.

### 📊 4. Classificação e agrupamento

Utilize embeddings como entrada para algoritmos de classificação ou clustering.

Exemplos:
- Agrupamento de tickets de suporte.
- Organização de documentos por assunto.
- Identificação de conteúdos semelhantes.
- Exploração de coleções textuais.

Essas aplicações exigem componentes adicionais para realizar a classificação ou o agrupamento propriamente dito.

---

## 🔌 API de embeddings

O serviço disponibiliza uma interface HTTP para solicitar a vetorização de textos.

**Demonstração online:** https://embeddings.leidson.pro/

### Exemplo de endpoint

```http
GET /api/v1/768/vectorize?frase=representacao%20semantica
```

A dimensão selecionada determina o banco SQLite utilizado pela API.

Modelos previstos na implementação:

| Dimensão | Arquivo |
|---|---|
| 384D | `weights/weights_384.db` |
| 768D | `weights/weights_768.db` |

### Exemplo de resposta

```json
{
  "sucesso": true,
  "engine": "LeidsonVect-SemanticLite-768D",
  "prompt": "representacao semantica",
  "tokens_encontradas": 2,
  "dimensoes": 768,
  "embedding": [0.012, -0.034, 0.056]
}
```

*Exemplo ilustrativo, com valores abreviados. Uma resposta real contém o vetor completo.*

A API também pode informar quando nenhuma palavra válida é encontrada no vocabulário.

Consulte a implementação e a documentação do serviço para conhecer os parâmetros e os requisitos de autenticação disponíveis.

---

## 🧱 Tecnologias

| Tecnologia | Função |
|---|---|
| PHP | API e processamento textual |
| SQLite | Armazenamento do vocabulário vetorial |
| PDO | Acesso aos bancos de dados |
| JSON | Comunicação com clientes HTTP |
| Python | Preparação e geração dos dados vetoriais no pipeline de desenvolvimento |

A arquitetura separa a preparação dos dados do processamento das consultas, permitindo que os vetores sejam gerados previamente e consumidos por uma aplicação PHP.

---

## 🔬 Um projeto de experimentação

O LeidsonVect-SemanticLite também explora uma questão interessante de engenharia de software:

**Até onde podemos simplificar a infraestrutura de uma aplicação semântica quando os vetores já estão disponíveis?**

Essa abordagem pode ser útil em ambientes nos quais executar modelos neurais em produção não seja necessário ou desejável para cada consulta.

Ao mesmo tempo, ela apresenta limitações importantes: o vocabulário precisa estar preparado previamente, palavras desconhecidas podem não contribuir para a representação, e a média de vetores de palavras não equivale necessariamente a um embedding contextual de frases produzido por um modelo neural.

O objetivo é investigar possibilidades, construir ferramentas práticas e avaliar os resultados em cenários reais.

---

## 🗺️ Possíveis evoluções

O projeto pode evoluir em diferentes direções:

- [ ] Melhorar o tratamento de palavras ausentes no vocabulário.
- [ ] Implementar mecanismos eficientes de busca por similaridade.
- [ ] Desenvolver índices vetoriais para coleções maiores.
- [ ] Avaliar diferentes estratégias de composição dos vetores.
- [ ] Criar ferramentas de comparação e análise de embeddings.
- [ ] Ampliar os recursos de monitoramento e diagnóstico da API.
- [ ] Realizar benchmarks reproduzíveis de latência, memória e qualidade semântica.

Esses itens representam possibilidades de desenvolvimento, não necessariamente funcionalidades já implementadas.

---

## 🤝 Contribuições

Ideias, sugestões, testes e contribuições são bem-vindos.

Se você trabalha com PHP, processamento de linguagem natural, embeddings, busca semântica ou inteligência artificial, este projeto pode oferecer um ponto de partida para experimentação e desenvolvimento.

Contribuições que ajudem a avaliar a qualidade dos vetores, melhorar a API ou ampliar os casos de uso são especialmente interessantes.

---

## 🌐 Conheça o projeto

**Site oficial:** https://embeddings.leidson.pro/

Explore a demonstração, conheça as possibilidades dos embeddings e experimente novas formas de integrar processamento vetorial a aplicações web.

---

**LeidsonVect-SemanticLite**  
*Vetores semânticos. Infraestrutura enxuta. Novas possibilidades para aplicações inteligentes.*
