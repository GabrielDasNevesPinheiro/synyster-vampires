# Synyster Vampires — Especificação do Protótipo

> **Versão:** 1.0 | **Engine:** Godot 4.x | **Linguagem:** GDScript
> **Pergunta que o protótipo responde:** *caçar é divertido?*
> Documentos-fonte: `GDD_Synyster_Vampires.md` (sistemas), `CATALOGOS.md` (conteúdo), `NPCS.md` (mobs)
> ⚠️ Todos os números são **v0** — existem para o jogo rodar; balanceamento vem do playtest.

---

## 1. Escopo — o que ENTRA

| Sistema | Detalhe |
|---|---|
| Vampiro genérico | Movimento WASD, ataque básico (clique esq.), canalização (clique dir.), mordida (E) |
| Ficha universal | 21 atributos, sistema de efeitos buff/debuff mínimo |
| Sede | Fome drena HP continuamente |
| Sangramento | Golpes causam sangramento; sangue vaza em poças no chão; 100% vazado = morte; poças evaporam |
| Alimentação | Mordida (E, probabilística por Força) + canalização (absorve poças, range definido, pode andar, não pode atacar) |
| 4 NPCs Fase 1 | Camponês, Guarda, Sacerdote + 1 animal (Porco) |
| IA mínima | Estados: calmo → alerta → pânico/fuga/combate; grito de alerta |
| 1 noite | Timer, 1 mapa pequeno fixo (feito à mão), esconderijo |
| Marcos de sangue | 2–3 upgrades de teste (escolha 1 de 3) |
| HUD mínimo | Barra de sangue, timer da noite, prompt de upgrade |

## 2. Escopo — o que NÃO ENTRA (não construir agora)

❌ Shader de pixelização · ❌ mapa procedural · ❌ loot/inventário/itens · ❌ **interiores de construções** (casas são sólidas; NPC "dormir" = despawna na porta) · ❌ habilidades ativas/passivas (além das de teste) · ❌ IA adaptativa/memória · ❌ multi-noites · ❌ meta-progressão · ❌ 4 vampiros · ❌ áudio final · ❌ menus completos

> **Rotina noturna no protótipo (versão simplificada do GDD §3.2):** camponeses começam na rua e, na segunda metade da noite, vão para casa e despawnam; guardas patrulham a noite toda. Interiores completos ficam para depois do protótipo.

> Arte: **placeholders** (sprites simples/formas). O visual do mockup vem na Vertical Slice.

---

## 3. Valores iniciais (v0)

### 3.1 Vampiro do protótipo
| Atributo | Valor | Atributo | Valor |
|---|---|---|---|
| HP / Vida Máx | 200 | Vel. Ataque | 1.0 |
| Força | 20 | Vel. Movimento | 340 |
| Fome | **1,5 HP/s** | Alcance (ataque) | 60px |
| Range de Absorção | 150px | Vel. Canalização | 20 sangue/s |
| Resistência | 0 | Demais atributos | padrão/0 |

### 3.2 Sistemas
| Sistema | Valor v0 |
|---|---|
| Sangramento | 5 HP/s por 6s (por aplicação); chance base 30% por golpe |
| Poça de sangue | evapora em 20s; absorção instantânea por proximidade dentro do range ao canalizar |
| Mordida (E) | `chance = clamp(50 + (Força_v − Força_a) × 5, 5, 95)%`; sucesso drena 50% do HP atual do alvo; falha = alvo grita (alerta 20m) |
| Modificadores de mordida | alvo sangrando: Força efetiva ×0,5 · alvo dormindo/parado por trás: +30% · em pânico: −20% |
| Dano ataque básico | 100% Força = 20 |
| Duração da noite | **600s (10 min)** |
| Amanhecer | fora do esconderijo nos últimos 0s = morte; aviso aos 120s ("o céu clareia") |
| Marcos de sangue | 150 → 400 → 800 (acumulado na noite, para teste) |
| Upgrades de teste | +5 Força · +20 Vida Máx · +50 Range de Absorção (escolha 1 de 3) |

### 3.3 NPCs (de NPCS.md, Fase 1)
| NPC | HP | Força | Vel.Mov | Detecção | Comportamento |
|---|---|---|---|---|---|
| Camponês | 45 | 5 | 290 (+30% pânico) | 6m | Trabalha/anda; foge e grita |
| Guarda | 130 | 15 | 310 | 12m | Patrulha; persegue 15s; ataque corpo a corpo (dano 12) |
| Sacerdote | 60 | 3 | 280 | 8m | Fica na igreja; zona abençoada −30% cura (raio 8m, telegrafada) |
| Porco | 30 | 0 | 250 | 3m | Pasta; foge |

População do mapa teste: 12 camponeses, 4 guardas, 1 sacerdote, 4 porcos.

---

## 4. Arquitetura Godot

### 4.1 Estrutura de pastas
```
projeto/
├── project.godot
├── data/                    ← RECURSOS .tres (tudo configurável, GDD §8.3)
│   ├── atributos/
│   ├── npcs/                ← campones.tres, guarda.tres, sacerdote.tres, porco.tres
│   ├── upgrades/            ← 3 upgrades de teste
│   └── vampiros/            ← vampiro_prototipo.tres
├── scenes/
│   ├── main.tscn
│   ├── mundo/mapa_teste.tscn
│   ├── entidades/vampiro.tscn
│   ├── entidades/npc.tscn   ← cena única; dados vêm do .tres
│   ├── sistemas/poca_sangue.tscn
│   └── ui/hud.tscn
└── scripts/
    ├── sistemas/            ← atributos, efeitos, sangue, ia, tempo
    └── entidades/
```

### 4.2 Decisões-chave
- **Data-driven:** `NPCData.gd`, `AtributoData.gd`, `UpgradeData.gd` como `Resource`; a cena `npc.tscn` é genérica e recebe o `.tres` — **nenhum valor no código**
- **Ficha de atributos:** um `AttributeSheet.gd` (Dictionary de 21 atributos + lista de modificadores com fonte) usado por vampiro E NPCs
- **Sistema de efeitos:** `Effect` = {atributo, operação, valor, duração, fonte}; buff/debuff mesmo pipeline
- **IA:** máquina de estados simples por NPC (`Calmo → Alerta → Pânico/Combate → Fuga`), sinais globais de "grito de alerta" propagando por raio
- **Poças de sangue:** cena própria com `Area2D`/equivalente, valor de sangue, timer de evaporação; canalização consulta poças no range
- **Sangramento:** componente `BleedComponent` (dano/s, duração, emissão de poças no rastro)
- **Tempo da noite:** autoload `GameTime` com sinal `amanhecer_proximo` (120s) e `amanheceu`

### 4.3 Controles (GDD §2.7)
WASD move · LMB ataque básico · RMB canalizar (segurar) · E mordida · F interagir (esconderijo) · (1–9 sem função no protótipo)

**Mira (estilo LoL):** ataque básico e habilidades disparam na direção/coordenada do mouse; se o cursor estiver além do Alcance, o disparo é limitado ao alcance máximo naquela direção (clamp).

**Decisões menores registradas:** câmera segue o vampiro (zoom fixo no protótipo) · corpos de vítimas persistem e continuam sangrando até secar · Resistência única cobre dano físico e mágico (simplificação v0).

---

## 5. Marcos de implementação (ordem)

| Marco | Entrega | Critério de aceite |
|---|---|---|
| **M0** | Projeto Godot + Git + pastas | Roda vazio, repositório commitado |
| **M1** | Vampiro andando no mapa vazio (câmera segue) | WASD suave, colisão com cenário |
| **M2** | AttributeSheet + Fome drenando HP + HUD barra | HP cai 1,5/s; morte aos 0 |
| **M3** | Ataque básico + sangramento + poças + evaporação | Golpe no alvo gera rastro e poças que somem |
| **M4** | Mordida (E) + canalização (RMB) | Drenar cura; poças absorvidas no range; fórmula de chance funcionando |
| **M5** | NPCs: camponês (foge/grita), porco | Caçada funciona; alerta propaga |
| **M6** | Guarda + sacerdote | Perigo real; zona de −cura funciona |
| **M7** | Timer da noite + esconderijo + amanhecer | Sobreviver à noite completa é possível |
| **M8** | Marcos de sangue + 3 upgrades | Escolha 1-de-3 aparece e aplica |
| **M9** | **PLAYTEST** | Responder: *caçar é divertido?* |

> Regra: um marco por sessão de trabalho; commit a cada marco verde.

---

## 6. Perguntas do playtest (M9)
1. Caçar é divertido? (a pergunta-mestre)
2. A Fome de 1,5/s pressiona sem sufocar?
3. Mordida probabilística é tensa ou frustrante?
4. Canalizar enquanto anda é fluido?
5. Os 10 min passam rápido (bom) ou arrastado (ruim)?

**Critério para avançar à Vertical Slice:** respostas 1 e 3 positivas.

---

## 7. Mapa teste (layout sugerido)
Vila pequena ~100×100m: praça central com fonte, 6–8 casas, igreja no canto nordeste (zona do sacerdote), pocilga ao sul (porcos), esconderijo (cripta) no canto oposto à igreja. Iluminação placeholder: tochas pontuais.
