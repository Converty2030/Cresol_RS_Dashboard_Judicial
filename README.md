# Converty — Painel Processual (Piloto)

Painel interativo para apresentação de análises processuais a clientes (Credores) da **Converty Recuperadora de Ativos**.

Este é um **projeto piloto** voltado à apresentação executiva de processos em fase de recuperação extrajudicial/judicial de crédito.

---

## Acesso

O painel é protegido por uma senha de acesso fixa (código compartilhado com o cliente).

> ⚠️ A senha é apenas uma camada de conveniência para evitar exposição casual. **Não é um mecanismo de segurança real** — o código do painel roda no navegador do cliente e a senha pode ser lida no código-fonte. Não utilize esse painel como se fosse área restrita de verdade.

---

## Estrutura do Painel

Menu vertical com 7 abas:

1. **Valor da Causa** — com as taxas contratuais (remuneratória, moratória, multa e correção)
2. **Partes** — devedor principal e avalistas (com filtro por devedor)
3. **Garantias** — imóveis e veículos vinculados ao título
4. **Penhoras** — bens indicados e penhoras efetivadas
5. **Sisbajud / CNIB / Renajud** — bloqueios e pesquisas patrimoniais
6. **Status do Andamento** — fase atual e próximos passos
7. **Cronologia dos Fatos** — acionamentos e contatos realizados pela Converty

Cabeçalho com a logomarca da **Converty**.

---

## Filtro por Devedor

O painel permite filtrar as informações por devedor específico (devedor principal ou avalistas), ou exibir **TODOS** simultaneamente.

---

## Arquivos

| Arquivo | Descrição |
|---|---|
| `index.html` | Painel interativo (página única, sem dependências externas) |

---

## Publicação

O painel é um arquivo HTML único, autocontido. Pode ser aberto diretamente em qualquer navegador moderno, ou publicado via **GitHub Pages** para compartilhamento com o cliente.

> 🔒 **Este repositório DEVE ser PRIVADO** enquanto contiver dados reais de processos, partes e CPF/CNPJ — em conformidade com a **LGPD**. Só torne o repositório público em caso de dados inteiramente fictícios/anonimizados.

---

## Status

🟡 **Piloto** — em avaliação. Ajustes em andamento após feedback do cliente.

---

_Converty Recuperadora de Ativos — Departamento Jurídico / Negociações_
