# SVI Doctor — Landing Page

Landing page do braço médico da SVI Company. Método P.U.L.S.O para médicos especialistas que faturam R$80k+/mês.

## Stack

- HTML + CSS + JavaScript vanilla (zero build)
- Supabase (Postgres) para armazenamento de leads + RPC com senha
- Fontes: Playfair Display + DM Sans
- Meta Pixel + Google Ads (via `config.js`)
- Schema.org ProfessionalService + Service + FAQPage + Reviews
- Hospedagem: Vercel (deploy estático)

## Arquivos

| Arquivo | O que é |
|---|---|
| `index.html` | LP principal com estrutura voxr.ai adaptada (nav, hero com form 4-step, marquee, stats, 5 pilares P.U.L.S.O, 30/60/90, 6 diferenciais, 3 cases reais, 11 FAQs, CTA final, footer) |
| `admin.html` | Painel de leads (senha: `svidoctor123`) com filtros por ICP, status, especialidade, período; detalhe do lead; export CSV |
| `obrigado.html` | Thank-you para lead que **passou no ICP** (CTA pra WhatsApp + fluxo de 4 passos) |
| `obrigado-conteudo.html` | Thank-you para lead **fora do ICP** (oferece conteúdo gratuito) |
| `politica-de-privacidade.html` | LGPD-compliant |
| `termos-de-uso.html` | Termos com ressalvas sobre depoimentos e CFM |
| `config.js` | Credenciais Supabase, senha admin, IDs de tracking |
| `robots.txt` | Liberação explícita pros AI bots (ChatGPT, Perplexity, Claude, Gemini, Copilot) |
| `llms.txt` | Contexto da SVI Doctor para AI crawlers |
| `sitemap.xml` | Sitemap das páginas públicas |
| `vercel.json` | Config Vercel (cache, clean URLs, admin noindex) |

## URLs

| URL | Descrição |
|---|---|
| `/` | LP principal |
| `/obrigado` | Thank-you ICP (aprovado) |
| `/obrigado-conteudo` | Thank-you fora ICP |
| `/admin` | Painel de leads (senha `svidoctor123`) |
| `/politica-de-privacidade` | LGPD |
| `/termos-de-uso` | Termos |
| `/llms.txt` | Contexto AI |

## Supabase

- Projeto: `qvkfcvcqlfamyzgqgnrq.supabase.co`
- Tabela: `public.svi_doctor_leads`
- RLS: `anon` só faz INSERT; leitura/update/delete **somente via RPC com senha**
- ICP scoring automático via trigger `svi_doctor_leads_before_upsert`

### Scoring ICP (tier)

| Tier | Score | O que significa |
|---|---|---|
| **ideal** | 80–100 | Faturamento R$80k+, 2+ anos, investe regularmente, tem secretária |
| **intermediario** | 55–79 | Faturamento R$40-80k OU estrutura parcial |
| **fora** | 0–54 | Início de carreira ou sem estrutura mínima |

Leads **ideal + intermediário** vão para `obrigado.html`.
Leads **fora** vão para `obrigado-conteudo.html`.

### Trocar a senha do admin

```sql
CREATE OR REPLACE FUNCTION public.svi_doctor_verify_admin(admin_secret text)
RETURNS boolean LANGUAGE plpgsql SECURITY DEFINER SET search_path = public AS $$
BEGIN RETURN admin_secret = 'NOVA_SENHA_AQUI'; END;
$$;
```

Depois atualize `ADMIN_PASSWORD` em `config.js`.

## Deploy Vercel

```bash
cd "/Users/joao/Documents/CLAUDE CODE N8N/SVI-DOCTOR-LP"
vercel          # primeiro deploy
vercel --prod   # produção
```

Custom domain (quando sair): `lp.svidoctor.com.br` → Vercel Settings → Domains → CNAME para `cname.vercel-dns.com`.

## Antes de ativar os ads

- [ ] Preencher `META_PIXEL_ID` em `config.js`
- [ ] Preencher `GOOGLE_GTAG_ID` + `GOOGLE_CONVERSION_LABEL` em `config.js`
- [ ] Criar imagens: `images/hero-bg.jpg` (opcional, fundo hero) + `images/og-image.jpg` (1200×630)
- [ ] Testar submissão de formulário e ver se aparece em `/admin`
- [ ] Validar rich snippets em https://search.google.com/test/rich-results
- [ ] Trocar WhatsApp hardcoded se o número mudar (hoje: `5594991858282`)
- [ ] Coletar autorização formal dos 3 médicos (Brenno, Daniel, Felipe) para uso dos depoimentos
- [ ] Gravar vídeo-depoimentos oficiais dos 3 médicos (roadmap de 30 dias do diagnóstico)

## Identidade visual

| | Hex |
|---|---|
| Dourado | `#C9A227` |
| Dourado claro | `#E6BA3F` |
| Preto | `#000000` |
| Cinza escuro | `#0a0a0a` / `#111111` |
| Cream (fundo claro) | `#f5f2ec` |

Fontes: Playfair Display (títulos) + DM Sans (corpo).

## Time e contato

- **Time:** João (CEO), Ruan (sócio), Pedro (prospecção), José Ricardo (operação), Sarah Campos (social media)
- **Redenção — PA**
- WhatsApp: (94) 99185-8282
- Instagram: [@svimedicos](https://www.instagram.com/svimedicos)
