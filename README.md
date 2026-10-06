# Clipping de Notícias | Henrique Assunção

Painel diário de notícias para análise política e de mercado, gerado automaticamente todos os dias às 6h (horário de Brasília) por uma rotina agendada do Claude (Cowork). O resultado é um único arquivo HTML autocontido (`index.html`), com navegação por abas, filtros e resumos analíticos.

## O que o painel entrega

O painel tem **4 abas fixas**, nesta ordem:

| Aba | Conteúdo |
|---|---|
| **Destaques** (aba inicial) | Top 10 do dia: as 10 notícias mais relevantes entre as outras três abas, com foco macro/político (eleições, fiscal, mercados em sentido macro, geopolítica e macroeconomia global). Não é uma vitrine setorial. |
| **Brasil** | Notícias de 10 fontes nacionais, classificadas em 10 temas, com resumo analítico do dia. |
| **Internacional** | Notícias de FT, WSJ, NYT e Reuters, classificadas em 7 temas, com resumo próprio. |
| **Mercados & Empresas** | Notícias de mercado financeiro e de empresas dos 14 veículos, classificadas em 8 setores/temas, com resumo próprio. |

Cada aba de conteúdo tem um resumo expansível ("mood do dia"), filtros por tema e fonte, e cards com título (link direto para a matéria), resumo de uma frase, veículo, horário e temas.

## Fontes

Apenas as fontes abaixo são usadas; nenhum agregador, buscador ou outro veículo entra no painel.

- **Brasil (10):** Valor, O Globo, Folha de S.Paulo, Estadão, CNN Brasil, Metrópoles, Poder360, Banco Central (BCB), FGV/IBRE e IBGE.
- **Internacional (4):** Financial Times, Wall Street Journal, New York Times e Reuters (somente manchetes e resumos públicos, sem login).
- **Mercados & Empresas:** os 14 veículos acima, navegando nas seções de economia, negócios e mercado.

## Taxonomia

- **Brasil:** Eleições, Política, Congresso, Fiscal, Economia, Geopolítica, Master, Commodities, Inflação, Mercado.
- **Internacional:** Guerra & Conflitos, Estados Unidos, Europa, China, Japão, Reino Unido, Economia Global.
- **Mercados & Empresas:** Mercado & Câmbio, Bancos & Financeiro, Tecnologia & IA, Varejo & Consumo, Indústria & Aviação, Energia & Commodities, Agronegócio, Fusões & Aquisições.

## Como funciona

1. **Coleta:** a rotina navega nas páginas de cada veículo pelo Chrome (sem RSS) e extrai pares `{título, URL}` diretamente do DOM, de modo que cada notícia aponta para o link real da matéria. URLs nunca são construídas a partir do título; sem link capturado, a notícia é descartada.
2. **Janela temporal:** últimas 18 horas (em Brasília). No Brasil a janela é rígida, usando a data da URL ou a meta tag de publicação. No Internacional e em Mercados & Empresas é best-effort, usando horário relativo ou a posição na listagem quando há paywall.
3. **Classificação e resumos:** cada notícia recebe temas das listas fixas; os três resumos são escritos em prosa, com links inline restritos às notícias do próprio dia, sem repetir o nome do veículo a cada frase.
4. **Top 10:** seleção entre as notícias já coletadas, ordenada por importância para um investidor institucional (fiscal, juros e câmbio, política com risco de mercado, bancos centrais, geopolítica).
5. **Montagem:** os dados são injetados em um template HTML fixo (layout largo, 4 abas, CSS e JavaScript inline).
6. **Controle de qualidade:** script que valida os arrays de dados, as fontes e os temas permitidos, URLs duplicadas, referências do Top 10, links dos resumos, ausência de `localStorage`/`sessionStorage` e a estrutura do HTML.
7. **Entrega:** o `index.html` é enviado no chat e uma cópia (sem as tags de documento) é publicada como artifact "Clipping de Notícias | Henrique Assunção" no Cowork, que mantém sempre a edição mais recente.

## Estrutura do projeto

```
.
├── index.html   # painel do dia (standalone, sobrescrito a cada execução)
└── README.md
```

O `index.html` é um documento completo (`<!DOCTYPE html>` até `</html>`) e abre sozinho em qualquer navegador. A única dependência externa é a fonte do Google Fonts (Archivo, Inter e IBM Plex Mono); sem ela, o navegador usa fontes padrão.

## Estrutura de dados (dentro do `index.html`)

Três arrays JavaScript, um por aba de conteúdo, com objetos no formato:

```js
{ t: "título", r: "resumo de uma frase", u: "URL direta da matéria", j: "veículo", h: "horário", tm: ["tema1", "tema2"] }
```

- `NOTICIAS` (Brasil), `NOTICIAS_INTL` (Internacional) e `NOTICIAS_MERC` (Mercados & Empresas).
- `TOP10`: lista de 10 itens `{ u, tab }`, em que `u` é uma URL que já existe em um dos três arrays e `tab` é `"brasil"`, `"intl"` ou `"merc"`.
- A mesma notícia pode aparecer em mais de uma aba sob ângulos diferentes, mas nunca duas vezes no mesmo array.

## Regras editoriais

- Descartar esportes, celebridades, entretenimento, loterias, clima local, polícia local sem dimensão política e conteúdo patrocinado.
- Fatos, números e datas vêm do texto ou da meta tag capturados das páginas, nunca inventados.
- Em dia dominado por um tema (por exemplo, uma eleição), a concentração nesse tema é aceitável; não se força espalhamento artificial.
- Se não houver fato novo na janela para um tema (por exemplo, Master ou Japão), a seção do resumo pode ser omitida ou registrar a ausência, com nota na coleta.

## Aviso

Os cards trazem apenas manchetes, linhas finas e resumos de uma frase, com link para a matéria original. O conteúdo pertence aos respectivos veículos; o painel é de uso profissional pessoal e não substitui a leitura das matérias. Resumos baseados em manchetes não têm acesso a conteúdo sob paywall.
