![TEOLOGOS](assets/banner.jpg)

App: https://teologos.streamlit.app

TEOLOGOS é um sistema de perguntas e respostas teológicas baseado em RAG (geração aumentada por recuperação). Cada personagem responde apenas com os textos recuperados de sua própria base documental e cita a referência exata usada, por exemplo *ST I, q.2, a.3* no caso de São Tomás ou o parágrafo correspondente do Catecismo. Quando o tema não é tratado diretamente nas fontes, a resposta diz isso e, se possível, aplica os princípios do autor ao caso, deixando claro que é uma aplicação e não uma citação.

O app está em beta e aberto para cadastro.

## Personagens e fontes

- **São Tomás de Aquino**: Suma Teológica e Suma contra os Gentios, tradução inglesa dos Dominicanos (1920, domínio público), indexadas por questão e artigo.
- **Santo Agostinho**: Confissões e A Cidade de Deus, organizadas em fichas próprias de consulta.
- **Padre (Catecismo da Igreja Católica)**: texto do Catecismo, Bíblia na tradução de Antônio Pereira de Figueiredo (1778, domínio público) e a Catena Aurea de São Tomás de Aquino, que reúne comentários dos Padres da Igreja sobre os Evangelhos.

## Arquitetura

1. A pergunta é convertida em vetor pelo modelo de embeddings `intfloat/multilingual-e5-small` (sentence-transformers).
2. O vetor alimenta uma busca híbrida (semântica e textual) no Supabase, com extensão pgvector, executada por funções RPC específicas para cada personagem.
3. Os trechos recuperados, junto com a passagem completa de onde vieram, formam o contexto enviado ao modelo de linguagem.
4. A resposta é gerada pelo Groq, usando o modelo aberto `openai/gpt-oss-120b`, com o Gemini (`gemini-3.5-flash` e `gemini-3.5-flash-lite`) como alternativa quando o Groq está indisponível.
5. Um filtro remove qualquer citação ou referência bíblica que não tenha vindo da busca, para evitar referências inventadas.
6. Cada personagem tem voz própria via Google Cloud Text-to-Speech (vozes Chirp 3 HD), com as vozes do Gemini como reserva quando a cota mensal se esgota.

O app roda no Streamlit Community Cloud, a partir do repositório de deploy (separado deste).

## Projeto pessoal

TEOLOGOS é um projeto independente de Pedro Wolf. Este repositório reúne as informações públicas do projeto; o código do app roda em um repositório separado.
