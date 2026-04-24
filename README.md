# Painel Judicial Cresol RS — Converty

Painel gerencial interativo (HTML) para apresentação à diretoria, consolidando a posição judicial de 23 devedores, 26 contratos e 44 processos em tramitação.

## Acesso

- **Senha:** `PPR2026`
- Em caso de senha incorreta, o sistema avisa e impede o acesso.
- O painel roda 100% local (basta abrir o `index.html` no navegador) — não há envio de dados externos.

## Estrutura de arquivos (pronta para GitHub)

```
Painel_Dashboard/
├── index.html                 # Página única: login + painel
├── README.md                  # Este arquivo
├── assets/                    # (vazio — reservado para logos/imagens futuras)
└── data/
    ├── dashboard_data.js      # Dados consolidados usados pelo painel
    ├── dashboard_data.json    # Mesma base em JSON puro (referência)
    ├── devedores.json         # Fonte 1: extraído do xlsx Lista_Devedores
    ├── processos.json         # Fonte 2: extraído dos 44 PDFs de processos
    ├── processos_raw.json     # Extração bruta (debug)
    └── perspectivas.json      # Modelo conservador Mai-Dez/26 (score + distribuição mensal)
```

## Conteúdo do painel

Menu vertical com as seguintes abas:

1. **Visão Geral** — KPIs consolidados, Top 10 em atraso, dívida por agência, mix de garantias, exposição (bubble chart) e lista completa.
2. **Financeiro** — Contratos por devedor, dívida global e valor em atraso por operação.
3. **Valor da Causa** — Valor atribuído no processo; há campo `valor_atualizado` editável por processo.
4. **Partes & Avalistas** — Exequente (Cresol RS) e executados (PJ e avalistas PF), por processo.
5. **Garantias** — Garantias declaradas na originação + menções de hipoteca/alienação/aval nos PDFs + tabela gerencial por processo.
6. **Penhoras** — Menções identificadas automaticamente nos autos + campo editável de detalhes.
7. **Sisbajud / Bloqueios** — Contagem de menções Sisbajud/BacenJud + tabela gerencial com valores extraídos dos autos + campo editável para novos valores bloqueados.
8. **Perspectivas (Mai-Dez/26)** — Cronograma conservador de recebimentos e formalização de acordos, mês a mês, com score por processo (tempo no rito, garantia, Sisbajud, sinais de defesa/acordo, penhora). Probabilidade máxima aplicada: 35%. Gráfico empilhado por tipo de solução + tabelas por mês, por tipo, por devedor e detalhamento por processo com metodologia transparente.

### Filtro

Dropdown "Devedor" no topo do painel permite visualizar todos ou apenas um devedor específico. O filtro afeta todas as abas simultaneamente.

## Como editar informações

Para enriquecer as abas dinâmicas (Penhoras detalhadas, Bloqueios Sisbajud com valores, Valor atualizado):

1. Abra `data/processos.json` em um editor.
2. Localize o processo desejado pelo `numero_processo`.
3. Edite o bloco `"editaveis"`:
   ```json
   "editaveis": {
     "valor_atualizado": "95.000,00",
     "garantias_detalhes": ["Matrícula 12.345 — Imóvel Rua X", "Veículo Placa ABC-1234"],
     "penhoras_detalhes": ["Penhora no rosto dos autos em 12/03/2025 - R$ 45.000"],
     "sisbajud_bloqueios": [
       {"data": "15/04/2026", "valor": "12.500,00", "observacao": "Bloqueio parcial"}
     ],
     "observacoes": "Acordo em tratativa com o advogado do executado."
   }
   ```
4. Regenere o `dashboard_data.js` (opcional — veja próxima seção) ou atualize-o manualmente usando o mesmo conteúdo.

### Regerar dashboard_data.js (Python)

```bash
python3 - <<'PY'
import json
d = json.load(open('data/devedores.json',encoding='utf-8'))
p = json.load(open('data/processos.json',encoding='utf-8'))
# ... (código de merge disponível nos artefatos do projeto)
PY
```

## Como publicar no GitHub

1. Crie um repositório privado (dados sensíveis) — ex: `cresol-rs-painel-judicial`.
2. Faça upload da pasta **`Painel_Dashboard/`** inteira (arrastar no GitHub web, ou via git CLI).
3. (Opcional) Habilite **GitHub Pages** em *Settings → Pages → Deploy from branch main /root*. O painel ficará disponível em `https://<usuario>.github.io/<repo>/Painel_Dashboard/`.
4. A senha `PPR2026` permanece embutida no código — para uso interno apenas. Para controle mais robusto, migrar para autenticação server-side.

## Dados extraídos automaticamente

- **Lista_Devedores/Devedores.xlsx**: 26 contratos consolidados em 22 devedores únicos.
- **Processos/ (44 PDFs)**: extração via `pdftotext` das primeiras 8 páginas (partes, valor da causa, classe) + contagem de palavras-chave (Sisbajud, BacenJud, Penhora, Bloqueio, Hipoteca, Alienação Fiduciária, Aval) em todo o documento.

### Principais indicadores (referência)

| Indicador | Valor |
|---|---|
| Dívida Global Total | R$ 12.263.462,46 |
| Valor em Atraso | R$ 2.063.933,07 |
| % em Atraso | 16,83% |
| Devedores únicos | 22 |
| Processos ativos | 44 |
| Agências envolvidas | 10 |
| **Previsão Conservadora Mai-Dez/26** | **R$ 733.620,35** |
| **Sisbajud já bloqueado (autos)** | **R$ 76.287,53** |

### Modelo de Perspectivas — Resumo técnico

Score por processo (0-140), considerando:

- **Tempo no rito** (25 pts): ≥18m: +25 · 12-18m: +18 · 6-12m: +12 · 3-6m: +6
- **Garantia** (30 pts): Hipoteca: +30 · Alienação Fiduciária: +25 · Penhor: +18 · Aval: +8
- **Sisbajud já bloqueado** (30 pts): >R$20k: +30 · >R$5k: +22 · >0: +14
- **Sinais de defesa** (-20 a 0): embargos/impugnação/agravo robusta = penalidade
- **Sinais de acordo** (0 a +20): proposta/acordo/parcelamento identificado
- **Penhora/Leilão** (0 a +25): leilão designado, penhora efetuada, revelia

Probabilidade conservadora: ≥80 → 35% · 60-80 → 22% · 40-60 → 13% · 20-40 → 7% · <20 → 3%.
Valor esperado limitado ao teto de **1,2 × rateio do atraso** do devedor (evita superestimativa).
Distribuição mensal por tipo (Acordo concentra cedo, Excussão/Leilão no 2º semestre).

## Limitações e observações

- A extração automática de **Partes/Avalistas** capturou dados em 23 dos 44 processos (52%). Os demais podem exigir consulta manual aos PDFs (geralmente por formatação atípica).
- Detalhes **qualitativos** de penhoras e bloqueios Sisbajud (valores, datas) precisam ser preenchidos manualmente nos campos `editaveis` — o painel mostra a contagem automática e trechos extraídos para contexto.
- O painel é estático (HTML/JS puro): qualquer navegador moderno abre. Não depende de servidor.

---

**Uso interno — Converty Recuperadora de Ativos**
Gestão de Negociações · Carteira Cresol Rio Grande do Sul
