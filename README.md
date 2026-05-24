# Conteúdo Extra — VeritaPlugin

Repositório de artefatos complementares ao Trabalho de Conclusão de Curso (TCC) **VeritaPlugin: Uma Extensão de Navegador para Detecção de Golpes Digitais no Facebook**, desenvolvido na Universidade Federal Fluminense (UFF).

> Site oficial do projeto: [verita-plugin-web-site.vercel.app](https://verita-plugin-web-site.vercel.app/)

---

## Estrutura do Repositório

```
.
├── Dataset Interno/
│   ├── Golpes Dataset Interno.xlsx
│   └── Prints/
│       ├── Advanced-fee Scam/
│       ├── Catfishing/
│       ├── Fake News/
│       ├── Fake Shopping/
│       ├── Giveaway Scam/
│       ├── HashJacker/
│       ├── Identity Theft/
│       ├── Phishing/
│       └── ... (demais categorias)
├── Dataset Treinamento/
│   ├── v1.xlsx
│   ├── v2.xlsx
│   └── v3.xlsx
├── Datasets Externos/
│   └── Golpes Datasets Externos.xlsx
└── Formulários/
    ├── Avaliação do VeritaPlugin.csv
    └── VeritaPlugin - Avaliação das Justificativas Jurídicas.csv
```

---

## Dataset Interno

### BrScamsFacebook

O dataset `Golpes Dataset Interno.xlsx` foi construído a partir de conteúdos reais publicados no Facebook — posts, comentários e anúncios —, coletados por meio de personas criadas para navegar e observar a plataforma em condições autênticas de uso.

As categorias foram definidas a partir das subcategorias propostas pelo artigo de referência e adaptadas à realidade brasileira. Os dados foram estruturados com as colunas: **Categoria**, **Subcategoria**, **Texto**, **Link do post**, **Usuário**, **Perfil** e **Explicação**. Após filtragem de qualidade, **64 instâncias** foram retidas.

### Prints

A pasta `Prints/` contém capturas de tela dos conteúdos coletados, organizadas por categoria de golpe:

| Categoria | Descrição |
|---|---|
| Advanced-fee Scam | Golpes de taxa antecipada |
| Catfishing | Criação de perfis falsos para enganar |
| Fake News | Desinformação digital |
| Fake Shopping | Lojas virtuais falsas |
| Giveaway Scam | Sorteios fraudulentos |
| HashJacker | Sequestro de hashtags |
| Identity Theft | Roubo de identidade |
| Phishing | Phishing e roubo de dados |
| Romance Baiting / Honey Trap / LoveGuru | Golpes românticos |
| Social Engineering / Social Spamming / Sockpuppets | Engenharia social |
| Tech Support Scam | Suporte técnico falso |
| Gaslighting | Manipulação psicológica |
| Seguro | Conteúdo seguro (classe negativa) |

---

## Dataset Treinamento

A pasta `Dataset Treinamento/` contém as três versões do dataset utilizadas no ajuste fino supervisionado do modelo, correspondentes às três iterações de desenvolvimento do prompt RAG.

| Arquivo | Versão | Descrição |
|---|---|---|
| `v1.xlsx` | Prompt v1 — Sem RAG | Prompt direto com nome da categoria e lista estática de leis (GPT-5). Problemas: explicações genéricas, risco de alucinação legal. |
| `v2.xlsx` | Prompt v2 — Com RAG | Incorporação de RAG para enriquecimento do contexto jurídico (GPT-4o). Melhora nas explicações, mas com cobertura jurídica incompleta. |
| `v3.xlsx` | Prompt v3 — RAG Aprimorado | Versão definitiva: ampliação da base jurídica (LGPD, arts. 138–140 CP), validação por tokens (`_tokens_da_lei()`), princípios jurídicos explícitos no system prompt e mecanismos de sanitização de texto. |

---

## Datasets Externos

O arquivo `Golpes Datasets Externos.xlsx` consolida dados de seis fontes externas utilizadas para compor o dataset de treinamento final (**450 instâncias**, 75 por categoria, distribuição balanceada).

| ID | Fonte | Categoria | Descrição |
|---|---|---|---|
| Externo 1, 5 e 7 | [ScamWarners](https://www.scamwarners.com/) (Web Scraping) | Múltiplas | Dados extraídos via scraper modular com `cloudscraper` e tradução automática (`deep-translator`). Scraper: [brunanoroes/scraping-scamwarners](https://github.com/brunanoroes/scraping-scamwarners) |
| Externo 2 | SMS Spam Collection (Kaggle) | Phishing | Mensagens SMS classificadas como spam/ham. Ferramenta de pré-processamento com tradução: [brunanoroes/tradutor-dataset-phishing](https://github.com/brunanoroes/tradutor-dataset-phishing) |
| Externo 3 | Fake News Portuguese (Kaggle) | Desinformação | ~20.000 notícias falsas e 2.000 verdadeiras em português. Amostragem desbalanceada, priorizando exemplos de desinformação. |
| Externo 4 | sec-fraudimabr (Nascimento et al., 2023) | Golpes Financeiros | Mensagens de WhatsApp e Telegram rotuladas, lidas via `pickle.load()`. Extrator: [brunanoroes/extrator-dataset-processed_texts](https://github.com/brunanoroes/extrator-dataset-processed_texts) |
| Externo 6 | Fake Reviews Dataset (Kaggle) | Lojas Virtuais Falsas | ~40.000 avaliações de produtos (reais vs. geradas por computador). Amostra de 150 registros traduzida para português. Código: [brunanoroes/tratamento-dados-golpes-lojas-falsas](https://github.com/brunanoroes/tratamento-dados-golpes-lojas-falsas) |

As categorias finais do dataset de treinamento são:

- Golpes Financeiros Ilusórios
- Fraudes em Lojas Virtuais Falsas
- Golpes de Relacionamento Romântico
- Golpes de Desinformação Digital
- Phishing e Roubo de Dados
- Conteúdo Seguro

---

## Formulários de Avaliação

### Avaliação das Justificativas Jurídicas

`VeritaPlugin - Avaliação das Justificativas Jurídicas.csv`

Avalia a qualidade das explicações, bases legais e ações recomendadas geradas pelo pipeline RAG v3 — versão definitiva do sistema. O objetivo foi verificar se os textos produzidos são juridicamente pertinentes, corretos e adequados para orientar um usuário leigo diante de situações de golpe digital no Facebook.

### Avaliação de Usabilidade (SUS)

`Avaliação do VeritaPlugin.csv`

Respostas do questionário baseado na **System Usability Scale (SUS)**, aplicado durante o experimento controlado com a *Facebook Replica v1.0* — ambiente web simulado que expôs todos os participantes aos mesmos cenários de teste (posts, itens à venda e interações no chat).

O protocolo seguiu uma abordagem não diretiva com duas etapas comparativas:
1. Exploração sem auxílio tecnológico (percepção de risco base)
2. Navegação assistida pelo VeritaPlugin

O método **Think Aloud** foi adotado como principal instrumento de observação. A SUS é composta por 10 afirmações (alternando enunciados positivos e negativos) avaliadas em escala de 5 pontos.

---

## Sobre o VeritaPlugin

O VeritaPlugin é uma extensão de navegador que detecta e classifica golpes digitais no Facebook, gerando explicações jurídicas e ações recomendadas ao usuário por meio de um pipeline RAG integrado à API da OpenAI.

Para mais informações sobre o projeto, acesse o [site oficial](https://verita-plugin-web-site.vercel.app/).

---

**Autora:** Bruna Assis Norões  
**Instituição:** Universidade Federal Fluminense (UFF)
