# Synyster Vampires — Catálogos de Conteúdo

> **Versão:** 0.1 (proposta inicial para validação)
> Todos os valores numéricos são **balanceamento v0** — ajustar no playtest (ver BALANCEAMENTO.md quando criado).
> Escalonamento segue o modelo LoL (§5.2 do GDD): cada habilidade declara com qual(is) atributo(s) escala.
> Lembrete: **HP = medidor de sangue**. Habilidades com custo de HP queimam o próprio sangue — risco/recompensa temático de magia vampírica.

---

## 1. Habilidades Ativas (slots 1–9)

| # | Nome | Efeito | Custo | Recarga | Escala com | Raridade |
|---|---|---|---|---|---|---|
| A1 | **Garras Rasgantes** | Golpe em arco à frente; aplica sangramento | — | 4s | Força (100%) | Anêmico |
| A2 | **Salto Sombrio** | Dash curto na direção do mouse; atravessa humanos | — | 6s | — | Anêmico |
| A3 | **Espinho de Sangue** | Projétil de sangue coagulado; alto dano à distância | 8% HP | 5s | Poder Vampírico (120%) | Anêmico |
| A4 | **Grito do Predador** | Humanos ao redor entram em pânico e fogem (não gritam alerta) | 20 mana | 18s | Poder Vampírico (raio) | Sangrento |
| A5 | **Marca da Presa** | Marca um alvo: visível através de paredes; +15% chance de mordida e +20% dano contra ele por 30s | 10 mana | 12s | — | Sangrento |
| A6 | **Névoa Sanguínea** | Névoa em área: humanos dentro têm detecção zerada e ficam lentos | 25 mana | 20s | Poder Vampírico (duração) | Sangrento |
| A7 | **Dominação** | Hipnotiza um humano comum: ele caminha até você por 5s (mordida garantida se alcançar) | 30 mana | 25s | Poder Vampírico (duração) | Sanguinário |
| A8 | **Forma de Morcego** | 3s: vira enxame, +80% velocidade de movimento, não pode atacar nem ser atacado | 15% HP | 22s | — | Sanguinário |
| A9 | **Enxame Famélico** | Morcegos atacam em área por 4s, aplicando sangramento em massa | 35 mana | 30s | Poder Vampírico (100%) | Sanguinário |
| A10 | **Banquete Cruel** | Execução em alvo abaixo de 25% HP: morte instantânea, drena 100% do sangue restante | 10% HP | 40s | Força (limiar de execução) | Carniceiro |

**Notas de design:**
- Kit inicial do vampiro genérico do protótipo: A1 + A2 (ataque em área + mobilidade)
- A4 é a resposta à falha de mordida (§2.8): pânico sem grito = controle de dano
- A5 + A7 formam o eixo "caçador planejador"; A8/A9 o eixo "senhor dos morcegos"
- ❓ Habilidades evoluem (níveis 1→5) ou só existem em 4 raridades? Proposta: **raridade define o tier da habilidade** (A1 anêmica → A10 carniceira), sem níveis internos no protótipo

---

## 2. Habilidades Passivas

| # | Nome | Efeito | Tipo | Raridade |
|---|---|---|---|---|
| P1 | **Garras Afiadas** | Ataques básicos têm +15% de chance de causar sangramento | Permanente | Anêmico |
| P2 | **Metabolismo Acelerado** | +1,5 de Regeneração de Mana | Permanente | Anêmico |
| P3 | **Sangue Frio** | Poças de sangue demoram 50% mais para evaporar | Permanente | Anêmico |
| P4 | **Aristocracia Vampírica** | +10% de Sorte | Permanente | Anêmico |
| P5 | **Instinto Assassino** | +20% de dano contra alvos sangrando | Condicional | Sangrento |
| P6 | **Digestão Lenta** | −20% de Fome (dreno por segundo) | Permanente | Sangrento |
| P7 | **Eco da Caça** | Abates reduzem em 1s as recargas ativas | Condicional | Sangrento |
| P8 | **Última Ceia** | Abaixo de 30% de HP: +25% Vel. de Movimento e +15% Roubo de Vida | Condicional | Sanguinário |
| P9 | **Sede Insaciável** | +25% de Fome, mas +35% de sangue obtido de todas as fontes | Trade-off | Sanguinário |
| P10 | **Frenesi do Carniceiro** | Abates consecutivos (janela de 5s) dão +4% Vel. de Ataque, acumula 10× | Condicional | Carniceiro |

**Notas de design:**
- P8 e P10 são "condicionais": sempre ligadas, disparam em situação específica (definição do §5.2)
- P9 é a passiva de trade-off: acelera progressão (marcos!) ao custo de sobrevivência — decisão de build real
- P3 + canalização = build "agricultor de sangue"; P5 + A1/A9 = build de sangramento
- ❓ Quantas passivas simultâneas por run? Proposta: **6 slots de passivas** (ativas têm 9, passivas 6)

---

## 3. Itens (primeira leva)

### 3.1 Consumíveis
| Nome | Efeito | Raridade |
|---|---|---|
| **Frasco de Sangue** | Restaura 25% do HP | Anêmico |
| **Bomba de Alho** | Área que repele humanos por 8s (ferramenta de fuga/controle) | Anêmico |
| **Frasco de Sangue Puro** | Restaura 50% do HP | Sangrento |
| **Pergaminho de Recolhimento** | Teleporta ao esconderijo (2s de conjuração) — a "válvula de escape" do amanhecer | Sangrento |
| **Coração Engarrafado** | Restaura 100% do HP + imune a sangramento por 30s | Sanguinário |
| **Elixir do Carnificina** | 100% HP, zera recargas, +20% todos os atributos ofensivos por 20s | Carniceiro |

### 3.2 Equipáveis
| Nome | Efeito | Raridade |
|---|---|---|
| **Anel do Servo** | +5% Sorte | Anêmico |
| **Botas Silenciosas** | +8% Furtividade | Anêmico |
| **Medalhão de Caça** | +8 Força, +8% Furtividade | Sangrento |
| **Rosário Profanado** | +20 Poder Vampírico, **−15% de cura** (trade-off com Redução de Cura; drop temático do Sacerdote) | Sangrento |
| **Manto do Corvo** | +20% Furtividade; +10% Vel. de Movimento | Sanguinário |
| **Presa de Lendário** | Mordidas falhas não geram grito de alerta; +10% chance de mordida | Sanguinário |
| **Coroa do Rei Vampiro** | +15 a todos os atributos ofensivos; +10% Vida Máxima | Carniceiro |

### 3.3 Valuáveis (convertem em moeda meta)
| Nome | Valor | Raridade | Fonte típica |
|---|---|---|---|
| **Talheres de Prata** | Baixo | Anêmico | Camponês, casas |
| **Relógio de Bolso** | Médio | Sangrento | Mercador |
| **Joias da Nobreza** | Alto | Sanguinário | Nobre, castelo |
| **Relíquia da Catedral** | Muito alto | Carniceiro | Igreja (baú do altar) |

### 3.4 Chaves / Especiais
| Nome | Efeito | Raridade |
|---|---|---|
| **Chave Enferrujada** | Abre porões e armazéns trancados | Anêmico |
| **Chave do Cemitério** | Abre a cripta (loot alto, encontro especial ❓) | Sangrento |
| **Sino de Mão** | Toca ao ser usado: atrai humanos da vizinhança ao ponto (isca/armadilha reversa) | Sangrento |
| **Chave Mestra do Castelo** | Abre todas as portas do castelo | Sanguinário |

**Nota:** drops de vítimas seguem a rolagem de 2 estágios do §5.3 do GDD; loot tables por classe completas ficam para o documento de balanceamento.

---

## 4. Vampiros Jogáveis (conceitos)

### 4.1 Varkosh, a Fera
> *"Por que sussurrar, quando se pode rugir?"*
- **Fantasia:** brutamontes de combate aberto; ignora furtividade, resolve na força
- **Passiva única — Fome de Titã:** mordidas em humanos comuns (camponês, animais) **sempre sucedem** e drenam +50%; alvos abatidos pelo ataque básico explodem em poça de sangue
- **Perfil:** +Força, +Vida Máxima | −Furtividade, −Vel. de Movimento
- **Habilidade inicial:** Garras Rasgantes (A1)

### 4.2 Lamia, a Sombra
> *"Você nunca a viu. Ninguém vê."*
- **Fantasia:** assassina paciente; o jogo vira stealth puro
- **Passiva única — Silêncio Sepulcral:** parada por 1,5s fica **invisível**; mordidas por trás têm sucesso garantido e não geram grito
- **Perfil:** +Furtividade, +Vel. de Movimento | −Vida Máxima, −Força
- **Habilidade inicial:** Salto Sombrio (A2)

### 4.3 Mordecai, Senhor dos Morcegos
> *"Ele não caça sozinho."*
- **Fantasia:** controle de área e sangramento em massa; sinergia com canalização
- **Passiva única — Mãe dos Enxames:** sangue de animais vale +100%; +30% Velocidade de Canalização; enxames próprios (A9) duram +2s
- **Perfil:** +Poder Vampírico, +Vel. de Canalização | −Força, −Resistência
- **Habilidade inicial:** Enxame Famélico (A9, versão anêmica enfraquecida ❓)

### 4.4 Anciã Valdora
> *"Mil anos ensinam o que o sangue esconde."*
- **Fantasia:** RPG puro; frágil no corpo, imensa em poder e progressão
- **Passiva única — Sabedoria do Sangue:** escolhas de upgrade oferecem **4 opções** (em vez de 3) e +10% de chance de raridade maior; −Fome penalizada: drena 25% mais rápido
- **Perfil:** +Poder Vampírico, +Sorte | −Vida Máxima, +Fome
- **Habilidade inicial:** Espinho de Sangue (A3)

**Notas:**
- Desbloqueio: Varkosh inicial; Lamia (sobreviva 5 noites), Mordecai (10 noites), Valdora (15 noites) ❓ proposta
- As passivas únicas são o "fruto proibido" de cada build: definem o estilo antes do primeiro upgrade

---

## 5. Sinergias previstas (mapa de builds)

| Build | Peças-chave | Fantasia |
|---|---|---|
| **Sangramento** | A1 + A9 + P5 + P1 | Espalha sangue, canaliza depois |
| **Furtiva** | Lamia + A5 + A7 + Manto do Corvo | Nunca é vista |
| **Força bruta** | Varkosh + Força + P10 | Mordidas garantidas, briga com guardas |
| **Magia de sangue** | Valdora + A3 + PV + Rosário Profanado | HP como munição |
| **Economia** | P9 + P4 + itens de Sorte | Progressão e loot acelerados, vive no limite |
