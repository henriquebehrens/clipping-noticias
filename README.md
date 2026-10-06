# Clipping de Notícias | Henrique Assunção

Painel diário de notícias de política e mercado, gerado automaticamente todo dia às 6h (Brasília) por uma rotina do Claude. O resultado é um único `index.html`, autocontido, com 4 abas, filtros e resumos analíticos.

## Abas

- **Destaques:** Top 10 do dia, com foco macro e político.
- **Brasil:** 10 fontes nacionais, 10 temas (Eleições, Fiscal, Congresso, Mercado etc.).
- **Internacional:** FT, WSJ, NYT e Reuters, 7 temas.
- **Mercados & Empresas:** os 14 veículos, 8 temas setoriais.

## Fontes

Valor, O Globo, Folha, Estadão, CNN Brasil, Metrópoles, Poder360, BCB, FGV/IBRE, IBGE, FT, WSJ, NYT e Reuters. Nenhuma outra fonte entra no painel.

## Como funciona

1. Coleta de `{título, URL}` direto das páginas dos veículos, com janela de 18 horas.
2. Classificação em temas fixos e redação de 3 resumos com links para as matérias.
3. Seleção do Top 10 por relevância para o investidor institucional.
4. Montagem em um template HTML fixo e controle de qualidade automático (links, temas, duplicatas).
5. Entrega do `index.html` e publicação no Cowork.

## Estrutura

```
.
├── index.html   # painel do dia, sobrescrito a cada execução
└── README.md
```

## Aviso

Os cards trazem manchetes e resumos de uma frase, com link para a matéria original. O conteúdo pertence aos respectivos veículos. Uso profissional pessoal; não substitui a leitura das matérias.
