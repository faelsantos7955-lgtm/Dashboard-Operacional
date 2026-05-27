# Dashboard Operacional J&T Express

Dashboard operacional em **HTML único** para acompanhamento de **Absenteísmo** e **Custo Operacional** dos PAs (Pontos de Atendimento) da J&T Express.

> 🌐 **Acesso:** [faelsantos7955-lgtm.github.io/Dashboard-Operacional](https://faelsantos7955-lgtm.github.io/Dashboard-Operacional/)

---

## 📋 Visão geral

- **Single-page** — apenas `index.html`, sem backend, sem servidor.
- **Dados locais** — cada usuário seleciona a pasta no PC dele que tem os Excel/CSV.
- **Login com hash** — senhas armazenadas como SHA-256, não em texto puro.
- **Multi-mês** — joga vários arquivos numa pasta, dashboard agrega tudo e oferece filtro por mês.
- **Persistência** — Chrome/Edge lembram a pasta entre sessões (1 clique pra reativar).

## ✨ Features

### Aba Absenteísmo
- 4 KPIs principais: % Geral, Meta, Efetivo Total, PAs Acima da Meta
- Comparativo automático ▲/▼ vs mês anterior
- 🔮 Projeção fim do mês (quando o mês ainda está em andamento)
- 🔴 Banner de alerta com PAs que ultrapassaram meta nos últimos 7 dias
- Gráfico de barras % por PA + status visual
- **Drill-down por colaborador** — clica no nome de um PA → abre ranking dos piores colaboradores
- Exportar Excel e Relatório PDF executivo

### Aba Custo Operacional
- 4 KPIs principais: Custo/Pacote, Custo Total, Desvio Forecast, Demanda
- Comparativo automático ▲/▼ vs mês anterior
- 🔮 Projeção fim do mês
- ⚠️ Banner de alerta com PAs acima de R$/pct média geral +30%
- Gráficos: Composição de custo por PA, Demanda vs Forecast
- Exportar Excel e Relatório PDF executivo

### Aba Organograma
- Estrutura completa: PA → Coordenador → Supervisor → Líder → Assistente
- Filtro por supervisor

---

## 📂 Formato dos arquivos de dados

O usuário coloca os arquivos numa pasta qualquer e o dashboard reconhece pelo **nome do arquivo**.

### Absenteísmo (`*frequencia*.xlsx` ou `*abs*.xlsx`)
Aba obrigatória: **`Frequência`**

Estrutura matriz (1 linha por colaborador, colunas = dias do mês):

| Colaborador | PA | Mes | Ano | 01\nDom | 02\nSeg | ... | 31\nTer | P | F+FJ+FI | A | FE | ABS% |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Fulano | PA BARUERI-SP | Abril | 2026 | P | F | ... | P | 28 | 1 | 0 | 0 | 3.4% |

**Códigos de status:**
- `P` = Presente
- `F` = Falta (genérica)
- `FJ` = Falta Justificada
- `FI` = Falta Injustificada
- `A` = Atestado
- `FE` = Férias
- (vazio) = dia futuro ou não trabalhado

### Custo Operacional (`*custo*.xlsx`)
Aba obrigatória: **`DADOS_CUSTO`**

Estrutura tabular (1 linha por PA × dia):

| Data | PA | Responsável | Demanda Coletada (Pcts) | Forecast (Pcts) | % Desvio Forecast | Qtd Colaboradores | Custo Mão de Obra (R$) | Custo Frete Veículo (R$) | Custo Total (R$) | Custo Final por Pacote (R$) |
|---|---|---|---|---|---|---|---|---|---|---|
| 01/04/2026 | PA BARUERI-SP | LIDER T1 | 5000 | 6000 | -16.7% | 5 | 1600 | 500 | 6000 | 1.20 |

---

## 🚀 Como usar

1. Acesse a URL do dashboard
2. Faça login (peça suas credenciais ao administrador)
3. Clique em **"Selecionar Pasta"** no topo
4. Escolha a pasta no seu PC que contém os arquivos `.xlsx` de absenteísmo e custo
   - Pode ter vários meses na mesma pasta (ex: `frequencia_abril.xlsx`, `frequencia_maio.xlsx`)
5. O dashboard carrega automaticamente — abre no mês mais recente

### Trocar de mês
Use o dropdown **Mês** no topo de cada aba.

### Investigar um PA específico
Na aba Absenteísmo, na tabela "Detalhamento", **clique no nome do PA** (em vermelho com 🔍) — abre modal com os colaboradores ordenados por % de absenteísmo.

### Gerar relatório executivo
Botão vermelho **📄 Relatório PDF** em cada aba — gera PDF A4 com KPIs, top 10 piores PAs e análise automática.

---

## 🔐 Usuários

| Usuário | Permissão |
|---|---|
| `admin` | Acesso total (todos os PAs) |
| `lider1` a `lider8` | Acesso restrito a um PA específico |

**Senhas:** solicite ao administrador. As senhas são armazenadas como hash SHA-256 — não há como recuperar a senha original a partir do código.

### Trocar/adicionar usuário

1. Gere o hash SHA-256 da nova senha em [emn178.github.io/online-tools/sha256](https://emn178.github.io/online-tools/sha256.html)
2. Edite `index.html` na constante `USERS`:
   ```js
   'novouser': { hash: 'abc123...', nome: 'Nome', pa: 'PA Exemplo' },
   ```
3. Commit + push.

---

## 🛠️ Stack técnico

- **HTML/CSS/JS puro** — sem framework, sem build, sem dependências locais
- **Chart.js 4.4.1** — gráficos
- **PapaParse 5.4.1** — CSV
- **SheetJS 0.18.5** — XLSX
- **jsPDF + autotable** — geração de PDF
- **File System Access API** — persistência da pasta (Chrome/Edge)
- **Web Crypto API** — hash SHA-256 nativo das senhas
- **IndexedDB** — armazena handle da pasta entre sessões

## 🌐 Compatibilidade

| Browser | Suporte completo | Persistência da pasta |
|---|---|---|
| Chrome | ✅ | ✅ |
| Edge | ✅ | ✅ |
| Opera/Brave | ✅ | ✅ |
| Firefox | ✅ | ❌ (precisa selecionar a pasta toda vez) |
| Safari | ✅ | ❌ |

---

## 📦 Deploy

Servido via **GitHub Pages** direto do branch `main`. Qualquer push atualiza a URL pública automaticamente.

Para hospedar uma cópia própria:
1. Fork deste repositório
2. Settings → Pages → Source: `main` branch, root
3. Acesse `https://SEU_USER.github.io/Dashboard-Operacional/`

---

## 📝 Licença

Uso interno J&T Express.
