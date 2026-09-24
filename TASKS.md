# The Quiet Hour — Tasks do MVP

Baseado no `PRD-the-quiet-hour.md`, organizado pelos milestones M0–M5.

## M0 — Conteúdo (roda em paralelo com M1–M4, é o gargalo)
- [ ] Escolher e verificar as edições de tradução em domínio público: Long (Marco Aurélio), Higginson/Carter (Epicteto), Gummere (Sêneca), Musonius Rufus, Zenão.
- [ ] Definir o schema JSON do content pack: `id`, `text`, `author`, `work`, `reference`, `question`, `context`.
- [ ] Selecionar 365+ passagens de 2 a 5 frases cada (a meta confortável é 400–450).
- [ ] Gerar uma pergunta de reflexão por passagem (Claude) e aprovar (Rodrigo). Passagem sem boa pergunta fica de fora.
- [ ] Escrever 2–3 frases de contexto por passagem.
- [ ] Revisar a atribuição e a referência (capítulo ou carta) de cada entrada.

## M1 — Setup, conteúdo, seleção, passos 1–2
- [ ] Trocar `namespace`/`applicationId` para `br.com.rodsil.quiethour` (hoje está `com.example.thequiethour`).
- [ ] Subir `minSdk` de 24 para 26.
- [ ] Ativar o core library desugaring (`java.time`).
- [ ] Adicionar Hilt, Room, DataStore e WorkManager ao `libs.versions.toml`.
- [ ] Apagar os placeholders do template (`DataRepository`, `MainScreen`, `MainScreenViewModel` e os testes deles).
- [ ] Criar o `git init` e o primeiro commit.
- [ ] Criar o loader do content pack a partir de `assets/` com kotlinx.serialization. O engine não pode saber nada de estoicismo.
- [ ] Gerar o `installId` na primeira execução e guardar no DataStore.
- [ ] Seleção determinística: `hash(installId + localDate) % count`, com janela de exclusão de 180 dias.
- [ ] Persistir o índice entregue em cada data.
- [ ] Testes da seleção: mesma data dá a mesma passagem, sem repetição em 180 dias.
- [ ] Schema do Room: `completed_days`, `reflections`, `favorites`, `streak_history`.
- [ ] Tema próprio: fonte serif e as variantes light/dark/sepia.
- [ ] Grafo aninhado do ritual.
- [ ] Passo 1: tela da passagem, tela cheia, sem chrome, só com "Continue".
- [ ] Passo 2: tela da pergunta.
- [ ] Acesso ao dia de ontem.

## M2 — Pausa, reflexão, streak
- [ ] Timer da pausa com coroutine e relógio monotônico, que sobrevive a mudança de configuração.
- [ ] Ligar `FLAG_KEEP_SCREEN_ON` durante a pausa e soltar logo depois.
- [ ] Indicador de respiração: ciclo de 4s inspirando e 6s expirando, com anel de progresso sutil e sem contador.
- [ ] Chime opcional no fim da pausa, desligado por padrão.
- [ ] Botão de pular a pausa, registrando o skip.
- [ ] Voltar durante a pausa pede confirmação.
- [ ] Passo 4: campo de reflexão com o aviso de privacidade explícito ("salvo só no aparelho").
- [ ] Passo 5: fechamento com incremento da streak e confirmação.
- [ ] Máquina de estados da streak: `Active`, `AtRisk`, `Broken`, `Restored`.
- [ ] A streak só conta com o passo 3 concluído ou pulado.
- [ ] A streak quebra depois de 1 dia perdido.
- [ ] Testes da streak: fuso, DST, mudança do relógio do aparelho, reinstalação.

## M3 — Journal e settings
- [ ] Lista cronológica de dias concluídos (passagem, pergunta, reflexão).
- [ ] Busca nas reflexões.
- [ ] Favoritos.
- [ ] Exportar para `.md`/`.txt` pelo share sheet.
- [ ] Settings no DataStore: duração da pausa (30/60/120/180), chime, horário da notificação, tema, tamanho da fonte.

## M4 — Notificações, share, milestones
- [ ] Notificação diária no horário escolhido (padrão 07:00) com AlarmManager `setExactAndAllowWhileIdle` e WorkManager.
- [ ] Notificação de "streak at risk" às 20:00 se o ritual não foi feito.
- [ ] Pedir `POST_NOTIFICATIONS` (API 33+) e tratar a permissão de alarme exato.
- [ ] Reagendar os alarmes no boot.
- [ ] Tela de ajuda "not getting reminders?" com deep link para a otimização de bateria.
- [ ] Share card em imagem: passagem + pergunta, 3 temas, marca d'água, nunca inclui a reflexão.
- [ ] Momentos de milestone em 7/30/100/365 com share card.

## M5 — Monetização e lançamento
- [ ] AdMob: native no journal a cada 6 entradas.
- [ ] AdMob: rewarded para restaurar a streak (1 vez a cada 7 dias).
- [ ] AdMob: rewarded para os 3 temas extras de share.
- [ ] AdMob: interstitial pós-ritual, no máximo 1 por dia e nunca nos 3 primeiros dias.
- [ ] Garantir zero ads nos passos 1–4.
- [ ] Play Billing: IAP "remove ads" de USD 4.99, que também libera os temas e as pausas longas.
- [ ] Firebase Analytics com os 15 eventos da seção 10. A reflexão só vai como bucket de tamanho.
- [ ] Minify/R8 no release e signing config.
- [ ] Política de privacidade e formulário de Data Safety.
- [ ] Listing em EN: título, descrição curta com o posicionamento e screenshots na ordem da seção 11.
- [ ] Verificar a colisão do nome (Play Store, INPI/USPTO, domínio, Instagram).
- [ ] Closed test com 12 testers por 14 dias.
- [ ] Release.

## Pontos em aberto
- **Fora do MVP:** localização PT-BR/ES é segunda onda, e SQLCipher fica para a v1.1.
