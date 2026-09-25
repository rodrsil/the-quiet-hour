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
- [x] Trocar `namespace`/`applicationId` para `br.com.rodsil.quiethour`.
- [x] Subir `minSdk` de 24 para 26.
- [x] Ativar o core library desugaring (`java.time`).
- [x] Adicionar Hilt, Room e DataStore ao `libs.versions.toml`. WorkManager entra no M4, junto com as notificações.
- [x] Apagar os placeholders do template (`DataRepository`, `MainScreen`, `MainScreenViewModel` e os testes deles).
- [x] Criar o `git init` e o primeiro commit.
- [x] Criar o loader do content pack a partir de `assets/` com kotlinx.serialization. O engine não pode saber nada de estoicismo. O pack é gerado por `content/build-pack.py` a partir dos lotes aprovados.
- [x] Gerar o `installId` na primeira execução e guardar no DataStore.
- [x] Seleção determinística: `hash(installId + localDate) % count`, com janela de exclusão de 180 dias (reduzida para o tamanho do pack menos 1 enquanto o pack tiver menos de 181 passagens).
- [x] Persistir a passagem entregue em cada data (guarda o id, não o índice, para sobreviver a mudanças no pack).
- [x] Testes da seleção: mesma data dá a mesma passagem, sem repetição em 180 dias.
- [x] Schema do Room: `delivered_passages`. `completed_days` e `reflections` entraram no M2; `favorites` entra no M3. `streak_history` não é necessária, porque a streak é calculada a partir de `completed_days`.
- [x] Tema próprio: fonte serif e as variantes light/dark. O sepia entra no M3, junto com a configuração de tema.
- [x] Fluxo do ritual no back stack do Navigation 3 (o Nav3 não tem grafos aninhados; cada passo é uma chave).
- [x] Passo 1: tela da passagem, tela cheia, sem chrome, só com "Continue".
- [x] Passo 2: tela da pergunta.
- [ ] Acesso ao dia de ontem. O repositório já entrega a passagem de qualquer data; falta a tela, que vai para o journal (M3), porque o PRD proíbe outros botões na tela da passagem.

## M2 — Pausa, reflexão, streak
- [x] Timer da pausa com coroutine e relógio monotônico, que sobrevive a mudança de configuração. A duração fica fixa em 60s até a tela de settings (M3).
- [x] Ligar `FLAG_KEEP_SCREEN_ON` durante a pausa e soltar logo depois.
- [x] Indicador de respiração: ciclo de 4s inspirando e 6s expirando, com anel de progresso sutil e sem contador.
- [ ] Chime opcional no fim da pausa, desligado por padrão. Adiado para o M3, junto com a opção que liga o chime.
- [x] Botão de pular a pausa, registrando o skip.
- [x] Voltar durante a pausa pede confirmação.
- [x] Passo 4: campo de reflexão com o aviso de privacidade explícito ("salvo só no aparelho").
- [x] Passo 5: fechamento com incremento da streak e confirmação.
- [x] Máquina de estados da streak: `Active`, `AtRisk`, `Broken`, calculada a partir dos dias concluídos. `Restored` entra no M5, junto com o anúncio recompensado.
- [x] A streak só conta com o passo 3 concluído ou pulado.
- [x] A streak quebra depois de 1 dia perdido.
- [x] Testes da streak: fuso, DST e mudança do relógio do aparelho. Reinstalação depende do backup automático do Android restaurar o banco; falta testar num aparelho.

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
