# Synyster Vampires — Catálogo de NPCs (Humanos e Animais)

> **Versão:** 0.1 (proposta inicial para validação)
> Todos os valores são **balanceamento v0** — ajustar no playtest.
> **Princípio (GDD §8.3):** todo mob usa a **ficha universal de 21 atributos**. As tabelas mostram só os atributos relevantes; os demais ficam no valor padrão (ver §1).
> Referência do vampiro base: HP 200, Força 20, Vel. Movimento 340, Vel. Ataque 1.00.
> **HP do NPC = seu volume total de sangue** — quando 100% vaza (sangramento) ou é drenado, o NPC morre (GDD §2.6).

---

## 1. Template universal de atributos (todos os mobs)

| Atributo | Padrão NPC | Atributo | Padrão NPC |
|---|---|---|---|
| Força | por classe | Mana Máxima | por classe |
| Velocidade de Ataque | por classe | Regeneração de Mana | 0 |
| Velocidade de Movimento | por classe | Fome | 0 (só vampiros) |
| Roubo de Vida | 0% | Range de Absorção | 0 (só vampiros) |
| Vampirismo | 0% | Vel. de Canalização | 0 (só vampiros) |
| Sorte | 0 | Resistência | por classe |
| Aceleração de Habilidade | 0% | Poder Vampírico ("Poder") | por classe |
| HP / Vida Máxima | por classe | Furtividade | por classe |
| Mana | por classe | Crítico | 0% |
| Alcance | por classe/tipo de ataque | Redução de Cura | 0% |

> **Tipo de ataque básico:** cada NPC declara `corpo a corpo` ou `à distância` (projétil físico/mágico) — ver GDD §5.2. Ex.: Guarda = corpo a corpo; Caçador = besta (físico à distância); Sacerdote = magia sagrada (mágico à distância).

> **Nota:** humanos usam "Poder Vampírico" como atributo genérico de **Poder** (dano mágico/habilidades deles — ex.: magia sagrada do Sacerdote escala com ele). Mesma ficha, leitura por contexto.

---

## 2. Humanos — Fase 1 (Protótipo/Vertical Slice)

### 2.1 Camponês 🌾
*Presa base do jogo. Abundante, fraco, previsível — até deixar de ser.*
| Atributo | Valor | | Atributo | Valor |
|---|---|---|---|---|
| HP (sangue) | 45 | | Resistência | 0 |
| Força | 5 | | Furtividade | 0 |
| Vel. Ataque | 0.8 (dano 4) | | Raio de detecção | 6m |
| Vel. Movimento | 290 (+30% em pânico) | | Drop (estágio 1) | 1% |

- **Comportamento:** trabalha em grupos de 2–5; foge em pânico ao ver o vampiro ou um corpo; **grita alertando a vizinhança** (raio 20m)
- **Mordida:** fácil (Força 5 vs. fórmula §2.8)
- **Loot table:** Talheres de Prata, moedas, Frasco de Sangue (raro)
- **Papel de design:** ensina o jogador a caçar; em pânico coletivo, vira "reação em cadeia"

### 2.2 Guarda 🛡️
*Primeira ameaça real. Luta, persegue, coordena.*
| Atributo | Valor | | Atributo | Valor |
|---|---|---|---|---|
| HP (sangue) | 130 | | Resistência | 10 |
| Força | 15 | | Furtividade | 0 |
| Vel. Ataque | 1.0 (dano 12) | | Crítico | 5% |
| Vel. Movimento | 310 | | Raio de detecção | 12m |
| Drop (estágio 1) | 4% | | | |

- **Comportamento:** patrulha rotas fixas; investiga gritos; persegue o vampiro por até 15s; chama reforços se sobreviver a um encontro
- **Mordida:** difícil diretamente (Força 15) — enfraquecer com sangramento primeiro
- **Loot table:** equipáveis Anêmico/Sangrento, moedas, Chave Enferrujada
- **Papel de design:** parede de progressão — Força alta = alvo premium de médio/late game

### 2.3 Sacerdote ✝️
*Zona proibida ambulante. Contra-medida de cura.*
| Atributo | Valor | | Atributo | Valor |
|---|---|---|---|---|
| HP (sangue) | 60 | | Resistência | 5 |
| Força | 3 | | Poder | 20 |
| Vel. Ataque | 0.5 (dano sagrado 10) | | Furtividade | 0 |
| Vel. Movimento | 280 | | Raio de detecção | 8m |
| Drop (estágio 1) | 3% | | | |

- **Comportamento:** fica na igreja; abençoa zonas (área com **Redução de Cura de 30%** para o vampiro, telegrafada visualmente); dano sagrado aplica debuff de −cura por 5s
- **Mordida:** fácil (Força 3), mas chegar perto é o problema
- **Loot table:** Rosário Profanado (raro), Relíquia (muito raro), itens sagrados
- **Papel de design:** aplica o atributo #20 (Redução de Cura) no jogo; cria geografia de risco

### 2.4 Animais 🐷
*Fonte de sangue segura, de baixo valor — com exceções.*
| Animal | HP (sangue) | Vel. Movimento | Detecção | Traço especial |
|---|---|---|---|---|
| **Porco** | 30 | 250 | 3m | Nenhum — presa tutorial |
| **Galinha** | 5 | 260 | 2m | "Petisco"; aos grupos, caos cômico |
| **Cavalo** | 70 | 380 (fuga) | 6m | **Coice: dano 15 + empurrão** se abordado por trás |
| **Cão** | 20 | 330 | **14m (olfato)** | **Late: alerta todos os humanos em 25m; ignora 50% da Furtividade** ⚠️ |

- **Eficácia do sangue por fonte ✅ DEFINIDO:**
  - Humano = **1.0** (eficácia plena)
  - Animal = **0.5** (50% de eficácia)
  - Fórmula: `sangue obtido = valor base × eficácia da fonte × modificadores do vampiro`
  - Implementado via campo `valor_sangue_modificador` no `.tres` de cada NPC (§6); modificadores do vampiro (ex.: passiva do Mordecai ×2 em animais → 0.5 × 2 = 1.0) aplicados no consumo
- **Papel de design:** cão é o **counter da build furtiva** — Lamia odeia cães; Mordecai ama todos

---

## 3. Humanos — Fase 2 (Alpha)

### 3.1 Mercador 💰
*Alvo de alto valor, bem protegido, pernas rápidas.*
| Atributo | Valor | | Atributo | Valor |
|---|---|---|---|---|
| HP (sangue) | 55 | | Resistência | 2 |
| Força | 4 | | Vel. Movimento | 320 (foge rápido) |
| Raio de detecção | 10m | | Drop (estágio 1) | **8%** |

- **Comportamento:** viaja entre pontos de comércio com escolta de 1–2 guardas; ao ser atacado, **paga/contrata** guardas próximos para defendê-lo (buff neles: +10% dano por 1 noite)
- **Loot table:** Relógio de Bolso, moedas em quantidade, itens Sangrento+
- **Papel de design:** caça de risco/recompensa — a escolta é o preço do loot

### 3.2 Caçador de Vampiros 🎯
*O predador do predador. "Boss recorrente" emergente.*
| Atributo | Valor | | Atributo | Valor |
|---|---|---|---|---|
| HP (sangue) | 220 | | Resistência | 15 |
| Força | 18 | | Furtividade | 8 (ele também se esconde) |
| Vel. Ataque | 1.1 (dano 20) | | Crítico | 15% |
| Vel. Movimento | 330 | | Raio de detecção | **16m** |
| Drop (estágio 1) | **15%** (raridade ↑) | | | |

- **Comportamento:** aparece a partir da noite 3–5 ❓ após mortes suficientes; investiga **locais dos seus ataques anteriores** (a IA adaptativa personificada); monta **armadilhas** (estacas, luz sagrada); usa besta à distância
- **Mordida:** quase impossível (Força 18) — exige enfraquecimento total
- **Loot table:** equipáveis Sanguinário+, chaves especiais
- **Papel de design:** a memória da IA com rosto; matá-lo é o clímax de uma noite

---

## 4. Humanos — Fase 3 (Lançamento)

### 4.1 Ferreiro 🔨
*Ameaça que escala com o tempo.*
| HP | Força | Vel. Ataque | Detecção | Drop |
|---|---|---|---|---|
| 90 | 12 | 0.9 (dano 10) | 6m | 3% |
- **Comportamento:** a cada noite, forja **armas de prata** → todos os guardas ganham +2 dano permanente (por run)
- **Papel de design:** relógio de dificuldade com endereço — eliminá-lo cedo custa caro (bem protegido), ignorá-lo custa mais

### 4.2 Nobre 👑
*O prêmio máximo da vila.*
| HP | Força | Vel. Movimento | Detecção | Drop |
|---|---|---|---|---|
| 70 | 4 | 300 | 10m | **12%** (raridade ↑↑) |
- **Comportamento:** nunca sai do castelo; escolta pesada (3–4 guardas); se morto → **pânico total + reforços na noite seguinte**
- **Loot table:** Joias da Nobreza, Chave Mestra do Castelo, itens Carniceiro possíveis

### 4.3 Bandido 🗡️
*O outro predador da noite.*
| HP | Força | Furtividade | Crítico | Detecção | Drop |
|---|---|---|---|---|---|
| 95 | 10 | **10** | 10% | 8m | 5% |
- **Comportamento:** ataca **outros humanos** à noite; **matá-lo não gera rumores** (a vila agradece, sem saber); pode competir pelas mesmas presas que você
- **Papel de design:** caça "ética" sem custo de adaptação da IA; encontros de 3ª parte (bandido × guarda × você)

### 4.4 Bruxa 🌙
*Encontro especial da floresta.*
| HP | Poder | Resistência | Detecção | Drop |
|---|---|---|---|---|
| 70 | 25 | 5 | 12m | 6% (mágicos) |
- **Comportamento:** isolada na floresta; **oferece "pactos"** ❓ (buff poderoso por preço sombrio — ex.: −Vida Máxima permanente na run por +Poder); hostil se atacada
- **Papel de design:** escolha narrativa/mecânica única por run; conteúdo "secreto"

---

## 5. Tabela-resumo (economia de sangue da vila)

| NPC | Sangue | Dificuldade de mordida | Perigo | Valor de loot | Fase |
|---|---|---|---|---|---|
| Camponês | 45 | ★ | ★ | ★ | 1 |
| Guarda | 130 | ★★★ | ★★★ | ★★ | 1 |
| Sacerdote | 60 | ★ (mas protegido) | ★★ | ★★ | 1 |
| Porco/Galinha | 30/5 | ★ | — | — | 1 |
| Cavalo | 70 | ★★ | ★ | — | 1 |
| Cão | 20 | ★★ | ★★ (alerta) | — | 1 |
| Mercador | 55 | ★★ | ★★★ (escolta) | ★★★★ | 2 |
| Caçador | 220 | ★★★★★ | ★★★★★ | ★★★★ | 2 |
| Ferreiro | 90 | ★★★ | ★★★ | ★★ | 3 |
| Nobre | 70 | ★★ | ★★★★★ (castelo) | ★★★★★ | 3 |
| Bandido | 95 | ★★★ | ★★★ | ★★ | 3 |
| Bruxa | 70 | ★★ | ★★★★ | ★★★ | 3 |

---

## 6. Estrutura de dados por NPC (para implementação)

Cada NPC = 1 recurso `.tres` com:
```
id, nome, fase, ficha_atributos[21], tipo_ataque_basico, raio_deteccao,
comportamento_id, chance_drop_estagio1, loot_table_id,
valor_sangue_modificador,
tracos_especiais[] (ex.: "latido_alerta", "zona_abencoada", "coice")
```
Comportamentos são scripts de IA separados e reutilizáveis (patrulha, fuga, pânico, investigar, montar_armadilha...).
