# Synyster Vampires — Game Design Document

> **Versão:** 0.3 (sistemas RPG: habilidades, mordida, inventário/loot)
> **Data:** 31/10/2026
> **Status:** Pré-produção — conceito em definição
> Seções marcadas com ❓ indicam decisões de design em aberto.

---

## 1. Visão Geral

### 1.1 Elevator Pitch
> *"Vampire Survivors ao contrário: você é o vampiro. Cace para não morrer, sobreviva até o amanhecer, e faça a próxima noite ainda mais mortífera."*

### 1.2 Conceito Central
Um **survivor-like invertido** com visão top-down em mapa medieval, onde o jogador controla um **vampiro que precisa caçar para sobreviver**. Enquanto em Vampire Survivors você foge de hordas e fica mais forte com o tempo, aqui você **perde vida constantemente** e precisa ativamente caçar presas (humanos, animais) para se manter vivo até o amanhecer — enquanto suas presas aprendem, fogem e lutam contra você.

### 1.3 Pilares de Design
1. **Caça, não sobrevivência passiva** — o jogador é o predador; o ritmo é ofensivo, não defensivo
2. **Sangue é tudo** — recurso único que funciona como vida, experiência e progressão
3. **Presas inteligentes** — humanos com comportamentos sociais distintos que se adaptam aos padrões do jogador
4. **Cada noite é um capítulo** — progressão contínua entre noites, criação de narrativa emergente ("minha run")
5. **RPG de verdade** ✅ — muitos atributos, habilidades ativas e passivas, builds; a profundidade de RPG é prioridade, não enfeite

### 1.5 Fora de Escopo (decidido)
- ❌ Gerenciamento de exércitos/unidades (legendas do mockup eram ilustrativas)
- ❌ Expansão de domínio/território
- O foco é **um vampiro caçando** — sem camada de estratégia/gestão

### 1.4 Informações do Projeto
| Item | Definição |
|---|---|
| Gênero | Survivor-like / Action roguelite (invertido) |
| Câmera | 3D estilo RTS (câmera elevada) com **shader de pixelização** ✅ |
| Plataforma alvo | PC (Steam) primeiro ❓ |
| Engine | **Godot** ✅ |
| Equipe | **Solo + desenvolvimento assistido por IA** ✅ (direção criativa humana; implementação majoritariamente via Claude Code) |
| Público-alvo | Fãs de Vampire Survivors, Halls of Torment, roguelites |

---

## 2. Gameplay Core

### 2.1 Loop de Jogo (Core Loop)

```
ANOITECER → Caçar presas → Beber sangue → Ganhar upgrades
    ↑                                      ↓
    ←── Sobreviver ao amanhecer ← Humanos reagem e se adaptam
    ↓
PRÓXIMA NOITE (mantém upgrades) → dificuldade/mundo escalam
```

### 2.2 O Ciclo do Sangue (mecânica central) ✅ DEFINIDO
- **Recurso único:** vida e progressão são o mesmo medidor de sangue
- O vampiro **perde sangue continuamente** (a "sede") — vida decaindo = pressão constante para caçar
- Beber sangue de presas **restaura o medidor**
- **Progressão por marcos:** ao atingir cada **marco de sangue total obtido** (acumulado), o jogador escolhe **1 entre 3 upgrades**
  - O sangue **não é gasto** ao ganhar upgrade — o marco é um gatilho, não um custo (gastar aceleraria a morte do vampiro)
- **Marcos contam o acumulado da run inteira** ✅ — a curva de poder é contínua entre noites, combinando com os upgrades persistentes
- **Cura por Roubo de Vida/Vampirismo conta para os marcos** ✅ — ⚠️ *atenção no balanceamento: builds de cura aceleram progressão; monitorar no playtest*
- ❓ Valores da curva de marcos a definir (ex.: 100 → 300 → 600 → 1000... — balancear no protótipo)
- ❓ O medidor de sangue atual tem teto? (afeta se dá para "estocar" vida)

### 2.3 A Noite e o Amanhecer
- **Estrutura de run: INFINITA** ✅ — noites se sucedem com dificuldade crescente, estilo arcade/score
- **Duração da noite: 10–15 minutos** ✅ (valor exato a ajustar no playtest)
- Um relógio/indicador de céu mostra a aproximação do amanhecer
- **Score da run = noites sobrevividas** ✅ — simples e legível; contador exibido como **"Noite 1, Noite 2..."** ✅
- Ao amanhecer, o vampiro dorme → **a run continua na próxima noite mantendo upgrades**
- ❓ O que muda entre noites? (propostas abaixo)
  - Humanos mais alertas/armados a cada noite que passa
  - População do mapa diminui (o vampiro está esvaziando a região) → escassez força risco maior
  - Autoridades locais reagem: caçadores de vampiros aparecem a partir da noite N

### 2.4 Amanhecer e Esconderijo ✅ DEFINIDO
- **Mecânica de esconderijo ativa:** nos últimos minutos da noite o jogador precisa voltar a um **esconderijo** antes do sol nascer
- Ser pego pela luz do amanhecer fora do esconderijo → **morte / fim da run** ❓ (ou dano massivo? — a ajustar no playtest)
- Cria a decisão de risco clássica do fim de noite: *"mais uma caçada ou volto?"*
- ❓ Quantos esconderijos existem no mapa? São fixos ou descobertos/escolhidos pelo jogador?

### 2.5 Condições de Derrota
- Medidor de sangue chega a zero (sede constante, dano recebido, **sangramento próprio**) → **fim da run**
- Pego pela luz do amanhecer fora do esconderijo → **fim da run**

### 2.6 Combate / Caça ✅ DEFINIDO
- **Ataques manuais** — o jogador controla ativamente os golpes do vampiro
- **Habilidades ativas em slots (1–9)** ✅ — o vampiro tem múltiplas habilidades ativas com cooldown, além de habilidades passivas (ver §5.2)

### 2.7 Controles ✅ DEFINIDO
| Entrada | Ação |
|---|---|
| **WASD** | Movimento |
| **Clique esquerdo** | Ataque básico (direção do mouse) |
| **Clique direito** | Canalização de absorção de sangue no chão |
| **1–9** | Slots de habilidades ativas — **cada slot configurável** (jogador atribui qual habilidade vai em qual tecla) ✅ |
| **E** | **Mordida direta** (na presa próxima) ✅ |

- Esquema estilo ARPG (Diablo/Path of Exile) — coerente com o pilar "RPG de verdade"
- ❓ Suporte a controle/gamepad? (WASD+mouse sugere PC-first; registrar decisão depois)

### 2.8 Sistema de Mordida ✅ DEFINIDO
- **Tecla E** na presa próxima → tentativa de mordida
- **Sucesso probabilístico:** calculado a partir da **Força do vampiro vs. Força do alvo**
- **Quantidade de sangue sugada:** também deriva da comparação de Força (vantagem = gole maior)
- **Consequência da falha:** a presa se desvencilha, **grita e alerta a vizinhança** (gera rumor/pânico) ❓ proposta — falha precisa custar algo, senão é só spam de E

#### Conceito de Força (proposta ❓ a validar)
- **Força é um atributo que existe nos dois lados:** vampiro (melhorável via upgrades/RPG) e cada classe de humano (fixa por classe, com variação individual leve)
- Escala sugerida: camponês fraco → guarda forte → caçador de vampiros fortíssimo (mordida quase impossível diretamente)
- **Fórmula base proposta:**
  - `chance de sucesso = clamp(50% + (Força_vampiro − Força_alvo) × 5%, 5%, 95%)`
  - `sangue sugado = porção fixa % × (1 + bônus por vantagem de Força)`
- **Modificadores de situação** (recompensam caça inteligente):
  - Alvo **dormindo** → sucesso automático ou bônus massivo
  - Alvo **sangrando/enfraquecido** → Força efetiva reduzida
  - Ataque **por trás/furtivo** → bônus
  - Alvo **em alerta/pânico** → mais difícil (resiste, se debate)
- Implicação de build: investir em Força abre presas "premium" (guardas, mercadores) como fonte de sangue rica; builds de baixa Força dependem de exsanguinação + canalização

#### Sistema de Sangramento (mecânica assinatura)
- **Todo golpe — tanto no vampiro quanto em alvos — pode causar sangramento**
- Alvos sangrando deixam **rastro/poças de sangue no chão**
- **Morte por exsanguinação:** se 100% do sangue de um alvo vazar, ele morre
- O sangue derramado **permanece no chão** como recurso coletável
- ⚠️ **O próprio vampiro também sangra ao ser golpeado** — e o sangue perdido do vampiro **não pode ser reabsorvido** (só sangue de origem externa é consumível). Ser atingido = perda líquida de recurso

#### As duas formas de se alimentar
| Método | Como funciona | Trade-off |
|---|---|---|
| **Mordida direta** | Instantânea quando próximo da presa; **drena sem matar** ✅ | Exige chegar no corpo-a-corpo com a presa viva |
| **Canalização de absorção** | Absorve sangue derramado no chão dentro de um **range** (atributo melhorável via upgrades) | O vampiro **pode andar** enquanto canaliza, mas **não pode atacar nem usar habilidades** |

#### Beber sem matar ✅
- A mordida **drena a presa sem matá-la**; matar exige mais golpes ou exsanguinação total
- Presa mordida sobrevive, **foge em pânico e espalha rumores** → decisão de design emergente: deixar vivo alimenta o medo (e a adaptação da IA), matar silencia mas gera corpo/investigação
- ❓ Presa mordida fica "marcada"/enfraquecida? Pode ser caçada de novo?

#### Sangue no chão ✅
- **Poças de sangue evaporam após X segundos** — evita "fazendas de sangue" e força o jogador a colher rápido, mantendo o ritmo da caça
- ❓ Tempo exato de decaimento (balancear no protótipo; upgrade pode aumentá-lo?)

#### Implicações de design
- **Eficácia do sangue por fonte** ✅: humano = 1.0, animal = 0.5 — `sangue obtido = valor base × eficácia da fonte × modificadores do vampiro` (ver `NPCS.md` §2.4)
- Combate cria "economia de sangue no cenário": lutar sujo espalha recurso pelo chão, e o jogador decide **quando parar de atacar para colher**
- Canalizar em área aberta durante perseguição = decisão de risco (sem ataques disponíveis)
- Upgrades de range de absorção viram build defensiva interessante

---

## 3. Mundo e Ambientação

### 3.1 Setting
- Mapa **medieval** — vilas, florestas, fazendas, igrejas, castelo
- **Mapa procedural por run** ✅ — cada run gera um território novo (rejogabilidade), e o mapa **persiste durante as noites daquela run**
- Consequência de design: a **memória adaptativa da IA é por run** — os humanos aprendem seus padrões ao longo das noites daquela run específica, dentro do mesmo território

### 3.2 A Sociedade Humana (simulação leve)
> 📎 **Stat blocks completos de todos os NPCs (humanos e animais): ver `NPCS.md`**
> Todos os mobs usam a ficha universal de 21 atributos (§8.3).
Humanos têm **classes sociais** com rotinas e comportamentos distintos.
**Escopo alvo: 8+ classes no lançamento** ✅ — com rollout faseado (ver nota de risco abaixo):

#### Fase 1 — Protótipo/Vertical Slice (4 classes)
| Classe | Comportamento em paz | Reação ao vampiro | Notas de design |
|---|---|---|---|
| **Camponês** | Trabalha em campos, anda em grupos | Foge em pânico, alerta outros | Presa "fácil", baixo valor de sangue |
| **Guarda** | Patrulha rotas fixas | Persegue e ataca o vampiro | Primeira ameaça real |
| **Sacerdote** | Fica na igreja, abençoa áreas | Repelente/zona proibida ao vampiro | Cria zonas de risco no mapa |
| **Animais** (porcos, cavalos, cães) | Pastam/dormem | Fogem; cães alertam donos | Fonte de sangue fraca mas segura |

#### Fase 2 — Alpha (adiciona)
| Classe | Comportamento em paz | Reação ao vampiro | Notas de design |
|---|---|---|---|
| **Mercador** | Viaja entre pontos de comércio | Contrata guardas, foge rápido | Alvo de alto valor, bem protegido |
| **Caçador de Vampiros** | Aparece após rumores/mortes | Cria armadilhas, aprende padrões | "Boss recorrente" emergente |

#### Fase 3 — Lançamento (adiciona, propostas ❓)
| Classe | Comportamento em paz | Reação ao vampiro | Notas de design |
|---|---|---|---|
| **Ferreiro** | Forja durante a noite | Forja armas de prata para os guardas | Escalar ameaça no tempo |
| **Nobre** | Fica no castelo, muito protegido | Ordena caçadas, aumenta recompensa | Alvo de "final de run" simbólico |
| **Bandido** | Ataca outros humanos à noite | Pode "competir" com o vampiro ou aliar-se | Moralmente cinzento — caçar bandidos não gera rumores? |
| **Ermitã/Bruxa** | Vive isolada na floresta | Oferece "pactos" ou é presa rara | Encontro especial |

> ⚠️ **Nota de risco (dev solo + IA):** com a implementação assistida por IA, o gargalo deixa de ser tempo de codificação e passa a ser **coerência de design, integração entre sistemas e playtest/balanceamento** — que continuam 100% humanos. O rollout faseado continua valendo: cada fase só avança se a anterior estiver divertida. Nunca cortar as 4 classes da Fase 1 — elas já sustentam o jogo.

### 3.3 IA Adaptativa (diferencial do jogo)
Os humanos **aprendem com os padrões do jogador**:
- Jogador sempre ataca no mesmo ponto do mapa → humanos **evitam a área** ou montam **emboscadas** ali
- Jogador sempre usa a mesma tática → guardas/caçadores desenvolvem **contra-medidas** (armadilhas, patrulhas cruzadas, iscas)
- Mortes geram **rumores**: humanos ficam mais agrupados, toque de recolher, menos presas fáceis nas ruas
- **Granularidade: memória por run** ✅ (consequência do mapa procedural por run) — os humanos esquecem tudo quando uma nova run começa; dentro da run, a memória acumula noite a noite
- ❓ Como comunicar isso ao jogador? (feedback visual é essencial — ex.: humanos sussurrando, cartazes de procurado, mudança de rotas visível)

---

## 4. Personagens Jogáveis (Vampiros)

Jogo projetado para **múltiplos vampiros controláveis**, cada um com **passivas únicas** que definem estilos de gameplay distintos.

### 4.1 Vampiros do lançamento ✅ (catálogo completo em `CATALOGOS.md` §4)
| Vampiro | Fantasia | Passiva única |
|---|---|---|
| **Varkosh, a Fera** | Brutamontes | Mordidas em comuns sempre sucedem; mortes explodem em sangue |
| **Lamia, a Sombra** | Furtiva | Invisível parada; mordida por trás garantida e silenciosa |
| **Mordecai, Senhor dos Morcegos** | Controle de área | Sangue de animais ×2; canalização +30% |
| **Anciã Valdora** | RPG/progressão | 4 opções de upgrade; +raridade; Fome +25% |

- **Quantidade no lançamento: 4** ✅
- **Desbloqueio por progressão** ✅ (proposta: 0/5/10/15 noites — validar)

---

## 5. Progressão e Upgrades

### 5.1 Upgrades em Run
- Gatilho: acumular X sangue bebido → escolha 1 de 3 ❓
- Categorias propostas ❓:
  - **Ofensivas** — dano, velocidade de ataque, garras em área
  - **Predação** — velocidade de bebida, sangue extra por presa, detectar presas à distância
  - **Sobrevivência** — sede mais lenta, regeneração, resistência a alho/prata/luz
  - **Manipulação** — hipnose, medo em área, disfarce humano temporário
  - **Evolutivas** — estilo VS: armas/poderes que evoluem em combos

### 5.2 Sistema RPG — Atributos e Habilidades ✅ DEFINIDO
> "Acima de tudo, o jogo é um RPG com muitos atributos."

#### Atributos ✅ (lista base)
| # | Atributo | Definição |
|---|---|---|
| 1 | **Força** | Poder muscular: levantar/empurrar objetos, dano corpo-a-corpo, e governa a mordida (§2.8) |
| 2 | **Velocidade de Ataque** | 1.00 = 1 ataque básico por segundo |
| 3 | **Velocidade de Movimento** | Valor absoluto (padrão League of Legends) |
| 4 | **Roubo de Vida** | % — cura equivalente a % do dano causado **apenas pelo ataque básico** |
| 5 | **Vampirismo** | % — cura equivalente a % do dano causado **por qualquer fonte** |
| 6 | **Sorte** | Governa drops (ambos os estágios, §5.3) |
| 7 | **Aceleração de Habilidade** | % — reduz o tempo de recarga de **tudo** que tiver cooldown |
| 8 | **HP** | Pontos de vida atuais; desce ao tomar dano, sobe ao receber cura — **é o medidor de sangue** (§2.2) |
| 9 | **Vida Máxima** | % — aumenta o teto de HP (100 HP + 10% = 110) |
| 10 | **Mana** | Recurso de magia; habilidades que a usam consomem custo definido por habilidade |
| 11 | **Mana Máxima** | % — aumenta o teto de mana |
| 12 | **Regeneração de Mana** | x por segundo |
| 13 | **Fome** | Quanto de HP o vampiro perde por segundo (a sede, §2.2) |
| 14 | **Range de Absorção** | Alcance da canalização de sangue no chão (§2.6) |
| 15 | **Velocidade de Canalização** | Rapidez de absorção das poças de sangue |
| 16 | **Resistência** | Redução de dano recebido (eixo defensivo) ✅ |
| 17 | **Poder Vampírico** | Escala o dano/efeito de habilidades mágicas (o "AP" do jogo) ✅ |
| 18 | **Furtividade** | Reduz o raio de detecção dos humanos (builds de caçador silencioso) ✅ |
| 19 | **Crítico** | Chance de crítico + dano crítico ✅ |
| 20 | **Redução de Cura** | % — reduz curas obtidas por **todas as fontes** ✅ |
| 21 | **Alcance** | Distância máxima do **ataque básico** da entidade ✅ |

**Lista oficial: 21 atributos** ✅

#### Tipos de ataque básico ✅
| Tipo | Como funciona | Escala com |
|---|---|---|
| **Corpo a corpo** | Golpe direto até a distância do Alcance | Força |
| **À distância** | Lança **projétil** (físico ou mágico) até a distância do Alcance | Força (físico) ou Poder Vampírico (mágico) |

- Cada entidade declara seu tipo de ataque básico (vampiro: corpo a corpo; Caçador de Vampiros: besta = físico à distância; Sacerdote: mágico à distância)
- **Habilidades de área:** cada habilidade AoE declara seus **próprios atributos de área/alcance** nos seus dados (raio, cone, comprimento) — não usam o atributo global Alcance ✅

#### Nota de design — Redução de Cura
- Pensado para **desafios e trade-offs**: debuffs de inimigos (ex.: ataque sagrado do sacerdote?), itens com custo (ofensivo forte com −cura), modificadores de noite
- ❓ Decisão importante: Redução de Cura afeta também **mordida e canalização** (que são "cura" do medidor de sangue)?
  - Se afetar: atributo vira ameaça existencial (toca na sobrevivência base, não só em cura de combate)
  - Proposta: afetar, mas com fontes de Redução de Cura sempre telegrafadas e evitáveis

#### Regras do sistema de atributos ✅
- **Escalonamento estilo LoL:** habilidades, efeitos de itens e tudo que aplica efeito pode **escalar com atributos diferentes** (vida máxima, mana, velocidade de movimento, etc.) — cada habilidade/efeito declara suas razões de escala
- **Atributos dinâmicos:** todos podem ser afetados por efeitos de qualquer fonte — itens, passivas, ativas conjuradas, efeitos de inimigos — positivos (buffs) e negativos (debuffs)
- **Habilidades ativas:** ✅ *definição formal:* dependem do jogador apertar o botão para a ação acontecer. Ocupam **9 slots (teclas 1–9), cada um configurável pelo jogador**, têm cooldown; obtidas/evoluídas via upgrades e marcos
- **Habilidades passivas:** ✅ *definição formal:* estão **sempre rodando** — surtem efeito automaticamente, sem input (salvo passivas condicionais, que seguem ativas mas só disparam numa situação específica, ex.: "ao ficar abaixo de 30% de sangue...")
- UI deve **demarcar claramente ativa vs. passiva**, com **tooltip ao passar o mouse** mostrando detalhes da skill
- ❓ Limite de slots de ativas/passivas por run? (força escolhas de build)
- ❓ Atributos sobem por marco de sangue, por upgrade, ou ambos?

### 5.3 Inventário, Itens e Loot ✅ DEFINIDO
- O jogo possui **inventário e itens colecionáveis durante a run**
- **Inventário não-volumoso** ✅ — lista simples de slots, sem gerenciamento de grade/espaço (não é jogo de Tetris de mochila)

#### Modelagem de item ✅
Cada item tem dois eixos independentes:
- **Origem** (como foi obtido): drop de NPC, baú/local, ❓ outros futuros
- **Função** (o que faz):
  - **Consumível** — uso único (frascos de sangue, buffs temporários)
  - **Equipável** — bônus passivo enquanto equipado
  - **Valuável** ✅ — só converte em moeda de meta-progressão (joias, talheres de prata...)
  - **Chave/especial** ✅ — abre baús trancados, cofres, áreas do mapa

#### Raridade ✅ (aplica-se a itens, aprimoramentos E habilidades)
| Raridade | Tier | Cor ✅ |
|---|---|---|
| **Anêmico** | Comum | Cinza pálido |
| **Sangrento** | Raro | Vermelho vivo |
| **Sanguinário** | Muito raro | Bordô |
| **Carniceiro** | Lendário | Vermelho-dourado brilhante |

*(Gradiente de "saturação de sangue" — anêmico é literalmente sangue fraco. Identidade temática própria, fugindo do padrão cinza/azul/roxo/dourado)*

#### Lógica de drop ✅ (rolagem em 2 estágios)
```
1. NPC abatido → teste de drop (ex.: 1% base, modificado pela classe do NPC e pela Sorte do vampiro)
2. Passou → teste de raridade (pesos Anêmico > Sangrento > Sanguinário > Carniceiro, modificado pela Sorte)
3. Raridade definida → sorteio estatístico do item específico dentro daquela raridade (loot table ponderada)
```
- **Sorte atua nos dois estágios** ✅ — com peso maior no estágio 1

#### Fontes de loot
| Fonte | Regra |
|---|---|
| **Vítimas** | Chance de drop = **Sorte do vampiro + chance de drop da classe do NPC** (ex.: mercador dropa mais e melhor que camponês) |
| **Locais do mapa** | Baús, armazéns, despensas — conteúdo randomizado; locais característicos (igreja, castelo, ferreiro) têm loot tables temáticas |

#### Loot tables por classe/local (proposta ❓)
| Origem | Exemplos de drops |
|---|---|
| Camponês | Comida (sangue mínimo?), moedas, itens comuns |
| Guarda | Armas/equipamento, moedas |
| Sacerdote | Itens sagrados — ❓ perigosos de carregar? (item amaldiçoado = risco/benefício) |
| Mercador | Moedas em quantidade, itens raros de comércio |
| Nobre | Itens raros/únicos |
| Baú de armazém | Consumíveis, materiais |

#### Questões de inventário ❓
- ❓ Itens persistem entre as noites da run? (proposta: sim, como os upgrades)
- ❓ Limite de slots do inventário? Equipáveis têm slots próprios (amuleto, anéis...)?
- ❓ Vasculhar locais gera ruído/alerta? (proposta: sim — lootear a casa de alguém à noite é arriscado)

#### Implicação de design
- Loot cria **terceiro eixo de decisão** na noite: caçar (sangue) × lootear (itens) × voltar ao esconderijo (tempo)
- **Frasco de sangue** como consumível resolve o problema de "run morrer por azar de posicionamento" — válvula de segurança do jogador

### 5.4 Meta-Progressão entre Runs ✅ DEFINIDO
- **Meta-progressão completa:**
  - **Moeda permanente** entre runs (ex.: "sangue ancestral" ❓ nome/moeda a definir) gasta em melhorias permanentes
  - **Desbloqueáveis:** novos vampiros jogáveis, mapas/modificadores, via conquistas
- Upgrades da noite persistem apenas dentro da run; meta-progressão é o que sobrevive à morte

---

## 6. Direção de Arte e Áudio

### 6.1 Identidade Visual ✅ (definida por mockup de referência)
- 📎 **Arquivo de referência:** `referencia_visual.png` (raiz do projeto)
- **Arte: 3D estilo RTS com shader de pixelização** ✅ — renderizado em 3D com câmera elevada/isométrica, com shader que dá cara de pixel art
- **Profundidade 3D + estética de sprites:** câmera livre com sensação de profundidade real, mantendo leitura retrô
- **Cenário:** vila medieval à noite — casas de madeira/pedra, igreja com torre, cemitério, praça com fonte, cercas, floresta ao redor

### 6.2 Paleta e Iluminação ✅
- **Contraste quente/frio como linguagem central:**
  - Azul escuro/roxos = noite, frio, perigo exterior
  - Laranja/amarelo quente = tochas, janelas acesas, "civilização" e segurança dos humanos
  - **Vermelho = vampiro e sangue** — aura do personagem, trilhas de sangue, partículas, lua de sangue
- Iluminação dinâmica 3D vende o ciclo da noite: o amanhecer deve ser *sentido* na tela antes de chegar
- **Lua de sangue** como elemento icônico recorrente (key art, telas, eventos?)

### 6.3 Elementos visuais-chave do mockup
- Vampiro com **anel/aura vermelha no chão** — casa perfeitamente com o indicador de range de canalização (§2.6)
- **Partículas vermelhas** para trilhas de sangue e sangramento
- Humanos visualmente distinguíveis por classe (roupas/silhueta)

### 6.4 Áudio
- Trilha medieval sombria
- Feedback sonoro da sede: batimento cardíaco acelerando quando o sangue baixa
- ❓ Tom geral: gótico sombrio puro ou com humor negro sutil?

---

## 7. UI/UX

### 7.1 HUD ✅ (definido pelo mockup de referência)
- **Canto superior esquerdo:** retrato do vampiro + barra de **SANGUE** (valor atual/máximo, ex.: 180/200) + nível
- **Abaixo do retrato:** barra de **9 slots de habilidades (1–9)** — **ativas e passivas bem demarcadas** visualmente; **tooltip ao hover** com detalhes da skill ✅
- **Topo central:** ícone de lua + **"Noite N"** + timer regressivo até o amanhecer
- **Canto superior direito:** contadores — moeda de meta-progressão, progresso ao próximo marco de upgrade, contador de vítimas ✅ + **minimapa**
- **Canto inferior esquerdo:** objetivos/notificações (ex.: rumores, alertas da vila, eventos da noite)
- **No mundo:** anel de range de canalização no chão, trilhas de sangue em partículas

### 7.2 Telas
- Seleção de vampiro (com passivas e stats visíveis)
- Tela de escolha de upgrade (1 de 3, ao atingir marco)
- Resumo de noite ("Noite 3 sobrevivida — 12 vítimas — a vila está em pânico")
- Tela de meta-progressão entre runs
- ❓ Tela/painel de atributos completo estilo RPG?

---

## 8. Escopo Técnico

### 8.1 Riscos técnicos identificados
1. **IA adaptativa** é o maior risco — começar simples (estados: calmo/alerta/pânico + memória de locais de ataque) antes de padrões complexos
2. Simulação de sociedade (rotinas) — manter leve; ilusão de vida > simulação real
3. Performance com muitos NPCs na tela

### 8.2 Fluxo de desenvolvimento assistido por IA
Com a implementação via Claude Code, este GDD passa a ter **dupla função**: documento de design *e* especificação de implementação. Consequências práticas:
- **Decisões devem ser registradas de forma precisa e inequívoca** — ambiguidade no GDD vira ambiguidade no código
- Cada mecânica nova deve entrar no GDD **antes** de ser implementada, com valores iniciais explícitos (mesmo que provisórios)
- Manter **tabela de atributos/balanceamento** no GDD (vida de presas, dano, ranges, timers) como fonte única da verdade para ajustes
- Playtest e balanceamento continuam humanos: reservar tempo por fase só para isso
- Versionar o projeto com Git desde o primeiro commit (segurança para iterações grandes via IA)

### 8.3 Princípios de arquitetura ✅ DEFINIDO
1. **Tudo é configurável por dados, nada hardcoded:** todo item, habilidade, passiva, NPC e atributo vive em **arquivos de dados editáveis** (Godot: Custom Resources `.tres` — editáveis no editor, sem tocar em código). Balanceamento e manutenção = editar dados, nunca lógica
2. **Ficha universal de atributos:** ✅ **TODO mob** (vampiro, humanos, animais) usa a **mesma ficha de 21 atributos** (§5.2). Atributos irrelevantes para um mob simplesmente ficam em valor padrão (Fome = 0 em humanos, Sorte = 0 em animais...)
   - Consequência: qualquer efeito/buff/debuff funciona em qualquer entidade — um único sistema, zero casos especiais
3. **Estrutura de dados sugerida:**
```
projeto/
└── data/
    ├── atributos/      ← definição dos 20 atributos (nome, tipo, min/max, descrição)
    ├── vampiros/       ← 1 .tres por vampiro jogável
    ├── npcs/           ← 1 .tres por classe de humano/animal
    ├── habilidades/    ← 1 .tres por habilidade ativa/passiva
    ├── itens/          ← 1 .tres por item
    └── loot_tables/    ← rolagens de drop por classe/raridade
```

### 8.4 Roadmap sugerido
| Fase | Entrega |
|---|---|
| **Protótipo** | 1 vampiro, 1 mapa pequeno, sede + sangramento + mordida/canalização + as 4 classes da Fase 1 com fuga/perseguição básica. Pergunta a responder: *caçar é divertido?* |
| **Vertical Slice** | 1 noite completa: marcos/upgrades, esconderijo ao amanhecer, adaptação de IA v1, shader de pixelização aplicado |
| **Alpha** | Multi-noites, 2+ vampiros, meta-progressão, conteúdo completo de sistemas |
| **Beta/Lançamento** | Polimento, balanceamento, áudio, Steam page |

---

## 9. Perguntas em Aberto (resumo)

**Decisões consolidadas ✅**
| # | Pergunta | Decisão |
|---|---|---|
| 1 | Sangue: recurso único ou separado? | Recurso único, progressão por marcos (sem gasto) |
| 2 | Ataques automáticos ou manuais? | Manuais |
| 3 | Bebida: instantânea ou canalizada? | Ambas: mordida instantânea + canalização de sangue no chão |
| 4 | Run infinita ou campanha? | Run infinita |
| 5 | Duração da noite? | 10–15 minutos |
| 6 | Esconderijo ao amanhecer? | Sim, com risco de morte fora dele |
| 7 | Marcos contam o quê? | Acumulado da run inteira |
| 8 | Mordida mata? | Não — drena; matar é separado |
| 9 | Sangue no chão decai? | Sim, evapora após X segundos |
| 10 | Score da run? | Noites sobrevividas |
| 11 | Mapa? | Procedural por run, persistente entre as noites da run |
| 12 | Meta-progressão? | Completa (moeda permanente + desbloqueáveis) |
| 13 | Classes de humanos? | 8+ no lançamento, rollout faseado 4→6→8+ |
| 14 | Engine? | Godot |
| 15 | Arte? | 3D estilo RTS com shader de pixelização |
| 16 | Equipe? | Solo |

**Restam apenas ajustes finos (definir no protótipo) ❓**
1. Valores da curva de marcos de sangue / teto do medidor
2. Tempo de decaimento das poças de sangue
3. Amanhecer fora do esconderijo: morte instantânea ou dano massivo?
4. Quantos esconderijos por mapa — fixos ou descobertos?
5. Presa mordida fica marcada/re-caçável?
6. Escalonamento exato entre noites (curva de dificuldade)
7. Moeda de meta-progressão: nome e fontes de ganho
8. ~~Lista inicial de upgrades/habilidades~~ → **Catálogos criados** (`CATALOGOS.md`) — validar conteúdo
9. ~~Conceitos dos vampiros~~ → **4 vampiros definidos** ✅ (`CATALOGOS.md` §4)
10. ~~Lista completa de atributos~~ → **13 atributos base definidos** ✅; validar os 6 sugeridos (§5.2)
11. ~~Limite de slots~~ → **9 slots de ativas configuráveis** ✅; passivas têm limite? ❓
12. Atributos sobem por marco, por upgrade, ou ambos?
13. Tela de atributos/personagem completa estilo RPG?
14. ~~Tecla da mordida~~ → **E, com sucesso probabilístico por Força** ✅
15. Validar proposta do conceito de Força (§2.8): fórmula, modificadores, consequência da falha
16. Suporte a gamepad?

---

*Próximos passos: detalhar o protótipo (o que entra, o que fica de fora) e brainstorm de upgrades e vampiros.*
