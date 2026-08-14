# MHM Solution — Site institucional

Site da **MHM Solution LTDA** (crédito consignado) — página única, estática e autocontida
(fontes e imagens embutidas; só consome link de WhatsApp por fora).

**Ainda não publicado.** Domínio e app no Coolify a definir.

## Como mexer no site

A fonte de verdade é o **`src/`**. O `index.html` da raiz é **gerado** — não edite ele à mão.

```bash
python3 build.py           # src/ -> index.html
python3 build.py --check   # confere se o index.html está em dia com o src/

python3 -m http.server 8097   # preview em http://127.0.0.1:8097/index.html
```

O que fica onde está em [`src/README.md`](src/README.md). Resumo:

- `src/component.js` — lógica da página, incluindo o **simulador de empréstimo**
- `src/page.html` — a marcação das 6 telas
- `src/props.json` — telefone, 0800, WhatsApp, e-mail
- `src/assets/` — imagens, fontes e o runtime do framework

## Paleta

Extraída dos pixels do logo original.

| Papel | Cor |
|---|---|
| Fundo / header | `#0E0A08` (preto quente) · `#070505` no rodapé |
| Superfície escura | `#17120D` |
| Dourado (fundo, botões) | `#F4C958` → `#C9A227` |
| Dourado (texto sobre claro) | `#8A6A14` |
| Dourado (texto sobre escuro) | `#F0D48A` |
| Texto sobre claro | `#17120D` · secundário `#6B6155` |
| WhatsApp | `#158746` |

Os dois tons de dourado para texto não são decoração: dourado claro sobre branco não passa em
contraste. A paleta foi validada com auditoria WCAG nas 6 telas — **0 reprovações** (AA).
Ao mexer nas cores, rode a auditoria de novo antes de publicar.

## Deploy

Build via **Dockerfile**: uma etapa `python:3.12-alpine` roda o `build.py`, e o nginx serve o
resultado. Ou seja, **o deploy sempre sai do `src/`** — um `index.html` desatualizado no git
não chega em produção.

```bash
docker build -t mhm-site .
docker run -p 8080:80 mhm-site   # http://localhost:8080
```

### Ambiente

- **Host:** a definir (mesmo padrão da irmã MMS: Coolify em `admin.axisnetworks.com.br`,
  servidor `195.182.200.204`, deploy automático por webhook na branch `main`)
- **App:** ainda não criado
- **Domínio:** a definir

## Histórico

Nasceu em 2026-08-14 a partir do site da empresa irmã **MMS Consignados**
(`idavinunes/mms-site-demo`), reaproveitando estrutura, build e layout. Foram trocadas a marca
e a paleta (marinho → preto + dourado), e **removidas as credenciais que pertencem à MMS**:
selo RA1000 do Reclame Aqui, os três depoimentos de clientes e os números de atendimento.
Ver "Pendências" abaixo.

## Pendências (precisam da MHM)

- **Contatos reais** — `src/props.json` está com telefone, 0800, WhatsApp e e-mail de exemplo.
- **Logo em boa qualidade** — o atual foi extraído de um JPEG de WhatsApp por máscara de
  luminância. Fica bom sobre preto, mas tem ruído de compressão. Ideal: vetor ou PNG com fundo
  transparente.
- **Prova social própria** — depoimentos e selos, se a MHM tiver.
- **Números da empresa** — as três estatísticas do topo hoje são fatos do produto (84x, 35%,
  R$ 0) justamente por não serem da MHM; podem virar números reais quando existirem.
- **Bancos parceiros** — a esteira de logos (BB, Caixa, Itaú, Bradesco, Santander) veio da MMS;
  confirmar quais são os da MHM.
- **Tagline** — "Sua vida financeira mais leve" é a da MMS; confirmar se as irmãs compartilham.
- **Disclaimer** — o rodapé diz que a MHM atua como correspondente/assessoria e não empresta
  diretamente. Confirmar que descreve corretamente a MHM.
