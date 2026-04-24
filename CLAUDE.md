# CLAUDE.md — Blog-Fintechs SAFIE

## O que é este projeto
Blog automatizado em HTML estático, publicado em **fintechs.safie.blog.br**, com artigos gerados diariamente via Claude API.
O blog cobre direito e contabilidade aplicados a fintechs e serviços financeiros digitais, com foco no mercado brasileiro.

## ATENÇÃO: dois domínios completamente diferentes

| Domínio | O que é | Pode alterar? |
|---|---|---|
| safie.com.br | Site institucional da SAFIE | **NUNCA** |
| safie.blog.br | Rede de blogs temáticos | Sim, é este projeto |
| fintechs.safie.blog.br | Este blog específico | Sim |

**NUNCA modifique, acesse para edição ou mencione safie.com.br como destino de qualquer ação de código.**

## Estrutura de pastas

```
Blog-fintechs/
├── config/          # Configurações do blog (blog.json, temas.json, fontes.json)
├── dados/           # Histórico de notícias e controle de consumo
├── templates/       # Templates HTML (artigo, tema)
├── assets/
│   ├── css/         # Estilos (compartilhado com rede SAFIE Blogs)
│   ├── js/          # Scripts (busca, paginação)
│   └── img/         # Imagens e ícones
├── artigos/         # HTMLs gerados de cada artigo
├── temas/           # Páginas de listagem por tema
├── scripts/         # Scripts Python do pipeline
│   ├── buscar_noticia.py
│   ├── gerar_artigo.py
│   └── publicar.py
├── logs/            # Logs diários (não versionados)
├── rodar_diario.sh  # Script orquestrador (chamado pelo launchd às 7h45)
├── sitemap.xml      # Atualizado automaticamente a cada publicação
├── robots.txt
├── .env             # Credenciais (NÃO versionado)
└── .env.template    # Modelo de credenciais (versionado, sem valores reais)
```

## Credenciais necessárias (arquivo .env)
- `ANTHROPIC_API_KEY` — geração de artigos via Claude API
- `GITHUB_TOKEN` — push automático dos artigos
- `GITHUB_REPO` — repositório no formato `lucasm-mantovani/safie-blog-fintechs`

**Nunca hardcode credenciais. Sempre ler de variável de ambiente.**

## Pipeline diário (rodar_diario.sh — executa às 7h45 via launchd)
1. `buscar_noticia.py` — busca notícias via RSS (8 fontes do setor financeiro)
2. `gerar_artigo.py` — gera artigo via Claude API
3. `publicar.py` — gera HTML, atualiza home/sitemap, commit + push GitHub

## Temas cobertos
1. Regulação do Banco Central para fintechs (regulacao-bacen)
2. Open Finance e dados financeiros (open-finance)
3. PIX e meios de pagamento (pix-pagamentos)
4. Crédito digital e BNPL (credito-digital)
5. Compliance e PLD em fintechs (compliance-pld)
6. Tributação de fintechs (tributacao-fintech)
7. Investimento e captação em fintechs (investimento-fintech)
8. Seguros digitais / insurtech (insurtech)

## Regras de SEO e GEO
- Título: máximo 60 caracteres, com palavra-chave principal
- Meta description: máximo 155 caracteres
- Estrutura obrigatória: resumo executivo → contexto jurídico → impacto prático → FAQ (3-5 perguntas)
- Schema.org: BlogPosting + FAQPage em JSON-LD
- URL: `https://fintechs.safie.blog.br/artigos/AAAA-MM-DD-slug-do-artigo`
- Artigos: mínimo 800, máximo 1.500 palavras
- Tom: técnico, direto, sem juridiquês, sem clichês

## Baseado no Blog-Cripto
Este projeto é uma réplica adaptada de ~/CLAUDE/Blog-Cripto.
Ver ~/CLAUDE/Blog-Cripto/REPLICAR.md para o processo completo de infraestrutura.

## Estado atual do projeto (2026-04-23)
- **Fase 1 concluída:** Estrutura de pastas, configs, scripts, templates, páginas HTML
- **Fase 2:** Interface HTML/CSS — usar mesmo estilo do Blog-Cripto (identidade SAFIE compartilhada)
- **Fase 3:** Testar pipeline RSS
- **Fase 4:** Testar geração e publicação do 1º artigo
- **Fase 5:** GitHub + Cloudflare Pages + cron job (launchd 7h45) + DNS
- **Fase 6:** Validação SEO e documentação final

## Próximos passos
1. Criar .env com ANTHROPIC_API_KEY e GITHUB_TOKEN
2. git init + criar repositório safie-blog-fintechs no GitHub
3. Testar: `python3 scripts/buscar_noticia.py --forcar-rss`
4. Testar: `python3 scripts/gerar_artigo.py`
5. Testar: `python3 scripts/publicar.py --sem-git`
6. Conectar Cloudflare Pages ao repositório
7. Configurar launchd (cron 7h45)
8. DNS: CNAME fintechs → safie-blog-fintechs.pages.dev no Registro.br
