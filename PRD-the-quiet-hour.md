# PRD: The Quiet Hour

**Owner:** Rodrigo
**Platform:** Android native only (Kotlin + Jetpack Compose)
**Business model:** Ad supported, with an optional one time "remove ads" IAP
**Status:** Draft v3 (final name, meditative direction, Android only)
**Date:** September 2026

---

## 1. Summary

A daily contemplative ritual built around one Stoic passage. The user reads the passage, is given a single reflection question, sits with it for a short timed pause, and optionally writes one line in response. The app is not a quote feed. It is a two to three minute daily practice, and the quote is only the raw material for it.

## 2. Naming

Final name: **The Quiet Hour**. Play Store title: *The Quiet Hour: Daily Stoic Reflection*.

Names rejected during research, all already in use: "Stoa" (multiple apps, one with ~198k downloads), "Stillpoint" (three apps, including a daily devotional with an almost identical one-screen-per-day structure), "Daily Stoic" (registered brand of Daily Stoic LLC, Ryan Holiday), "Stoic Pause" (no direct collision, but "Pause" is crowded), "Meditari" (no collision, but indistinguishable from Medita, Meditara, Medito in search).

Still to verify before launch: exact-title search inside the Play Store and App Store, INPI and USPTO records, domain availability, and the Instagram handle. Application id under the existing company: `br.com.rodsil.quiethour`.

## 3. Problem and opportunity

Quote apps are abundant and disposable: the user reads, feels something for four seconds, and closes. Nothing is retained and nothing is built. Meditation apps solve the retention problem but demand 10 to 20 minutes and a subscription.

The gap is a practice that costs two minutes, asks for real attention, and is free. The Stoics themselves wrote for this use: Meditations is a private notebook of prompts, not a book of aphorisms. Restoring the reflective frame is both more faithful to the source and more defensible as a product.

Strategic consequence: sessions get longer and more deliberate, retention improves, and the reflection history becomes a personal artifact the user does not want to abandon. That artifact is what makes someone keep the app installed for a year.

### 3.1 Competitive reality (be honest about this)

The differentiator is narrower than it first appears. Research turned up:

- **stoic.** (Stoic app inc.), over 3 million users, morning preparation and evening reflection with journaling prompts, mood tracking, breathing exercises. The heavyweight incumbent.
- **Stoic Core** (March 2026), daily quote, home screen widget, evening reflection unlocking at 8pm, guided journaling, mood tracker.
- **Stoic Daily Habit**, daily Stoic reflection, distraction-free interface, fully offline, no ads.
- **Stoa** and **Daily Stoic Philosophy: Stoa**, guided Stoic meditations plus original texts, subscription based.
- **Stillpoint: Daily Devotional** (July 2026), the same one-calm-screen-per-day shape applied to Orthodox Christianity.

So "daily Stoic passage plus journaling" is a solved and occupied problem. What is not occupied is the **enforced pause between reading and writing**, plus a curated question written for each passage rather than generic prompts. That is the entire differentiator. It is real but thin, and the product must be built around it rather than around the quote library.

## 4. Target user

- Primary: English speaking, 25 to 45, already reading Stoicism, journaling, or meditating. Wants a practice, not content. High value ad markets (US, UK, CA, AU, DE).
- Secondary: PT-BR audience, second localization wave.
- Explicitly not the target: users who want an infinite scroll of quotes to screenshot. Do not build for them, they do not retain.

## 5. Goals and success metrics

| Metric | Target at 90 days |
|---|---|
| Installs | 10,000 |
| Pause completion rate (of users who open the passage) | 50% |
| Reflection written rate (of completed pauses) | 25% |
| D1 retention | 35% |
| D7 retention | 18% |
| D30 retention | 9% |
| Median session length | 2 min 30 s |
| Notification opt in | 60% |

D30 and pause completion are the two metrics that matter. Session count per day is deliberately capped at roughly one by design, so do not optimize for it.

## 6. The daily ritual (core product)

The whole app is one screen sequence. Build this before anything else.

**Step 1: The passage**
Full screen, generous whitespace, serif type, no chrome, no ads, no navigation bar. Passage text, author, source work. Nothing else is tappable except "Continue".

**Step 2: The question**
A single reflection question tied to that specific passage, drafted with AI assistance during content curation and approved by the owner before it ships. Specific to the passage, never generic.

Examples:
- Passage (Marcus Aurelius, on the obstacle becoming the way) → "What are you currently treating as an obstacle that could be the path itself?"
- Passage (Seneca, on borrowing suffering from the future) → "Name one thing you suffered today that has not happened yet."
- Passage (Epictetus, on what is within our control) → "What did you try to control today that was never yours to control?"

Curation rule: every passage ships with exactly one question. A passage without a good question does not go in the app.

**Step 3: The pause**
A timed silence. Default 60 seconds, configurable 30 / 60 / 120 / 180 seconds. The screen shows the question in small type plus a slow breathing indicator (a circle expanding and contracting on a 4 second in, 6 second out cycle). No counter ticking down in the user face, just a subtle progress ring. Optional soft chime at the end (off by default).

The user can skip the pause. Track skips, they are a quality signal for the question.

**Step 4: The reflection (optional)**
A plain text field, one line suggested but unlimited. Saved locally only. Never uploaded, never analyzed, never shown to anyone. Say this explicitly in the UI, the privacy promise is part of why people write.

**Step 5: The close**
Streak increments. Short confirmation. Quiet exit. This is the only point where an interstitial is permitted, and only under the rules in section 8.

## 7. Scope: MVP

### 7.1 Included

**Daily passage**
- One passage per calendar day, deterministic per device (seeded by date plus install id) so it cannot be rerolled. Same passage all day if the user reopens.
- Yesterday is accessible, older days are in the journal. No infinite browsing of future passages.

**Streak**
- Increments only when the ritual is completed through step 3 (pause finished or explicitly skipped). Reading alone does not count.
- Breaks after one missed calendar day, device local timezone.
- Restore a broken streak once every 7 days with a rewarded ad. Highest value placement in the app.
- Milestones at 7, 30, 100, 365 with a full screen moment and a share card.

**Journal**
- Chronological list of completed days: passage, question, and the user reflection if written.
- Search across reflections.
- Export all reflections to a plain text or markdown file via the system share sheet. This is a trust feature, ship it in v1.
- Favorites.

**Share**
- Render passage plus question to an image, 3 typographic themes. Watermarked for organic acquisition. Never shares the user reflection, and the UI must make that unmistakable.

**Notifications**
- One daily local notification at a user chosen time (default 07:00). Copy should invite, not nag: "Two minutes with Marcus Aurelius."
- One "streak at risk" notification at 20:00 if the ritual is not done.

**Settings**
- Pause duration, chime on/off, notification time, theme (light, dark, sepia), font size.

### 7.2 Content

- 365 passages minimum at launch, one per day of the year, each with an approved reflection question. 400 to 450 is the comfortable target.
- Public domain sources and public domain translations only: Marcus Aurelius (Long), Epictetus (Long; the Gutenberg Higginson text is a 1948 revised edition with unclear status, so avoid it), Seneca (Aubrey Stewart, minor dialogues), plus Musonius Rufus and Zeno fragments. Verify the translation edition, not just the original author, translations have their own copyright. Public domain must hold worldwide (life plus 70 years, the Brazil and EU term), not only in the US: Gummere's Seneca (translator died 1969) is protected until 2039 outside the US and is excluded.
- Passages should be 2 to 5 sentences. Single line aphorisms do not sustain a reflection question.
- Each entry carries: text, author, work, chapter or letter reference, the question, and 2 to 3 sentences of context shown on demand.
- Ships bundled as a JSON asset in the APK. No backend in v1.

### 7.3 Out of scope for v1

- Accounts, cloud sync, cloud backup of reflections
- iOS
- Audio narration
- AI generated questions at runtime (all questions are curated and bundled) or AI responses to reflections
- Social features of any kind
- Widgets (v1.1 candidate)
- Wear OS

## 8. Monetization

The tension in this app is sharper than in a normal utility: ads are hostile to contemplation. Resolve it by isolating ads completely from the ritual.

**Ad free zones, non negotiable:** the passage screen, the question screen, the pause screen, the reflection input. No banners, no interstitials, no preloading animations. If an ad appears during the pause, the product is dead.

| Placement | Format | Trigger | Notes |
|---|---|---|---|
| Journal screen | Native, every 6th entry | On scroll | The browsing surface, not the practice surface |
| Streak restore | Rewarded | User taps "Restore streak" | Opt in, highest eCPM |
| Share themes | Rewarded | Tap a locked theme | 3 extra themes |
| Ritual complete | Interstitial | After step 5, capped at 1 per day, and never on the user first 3 days | Only ad in the ritual path, and it comes after all value is delivered |

- Remove ads IAP: USD 4.99 one time. Also unlocks all share themes and longer pause options.
- Do not run the first day interstitial. Early retention is worth more than the impression.

**Honest tradeoff:** this ad plan yields perhaps a third of what an aggressive quote app extracts per user. The bet is that D30 retention and IAP conversion more than compensate, and that the app survives past the first month where aggressive competitors churn out. If after 90 days D30 is below 6%, the bet failed and the ad restraint was not the problem, the content was.

## 9. Technical design

**Stack (Android native, no KMP)**
- Kotlin, Jetpack Compose, Material 3 with a custom restrained theme.
- Single activity, Navigation 3 (`androidx.navigation3`, already configured by the project template). The ritual is a nested navigation graph with its own back handling: back during the pause should confirm, not silently discard.
- Hilt for DI.
- Room for persistence: `completed_days`, `reflections`, `favorites`, `streak_history`.
- DataStore Preferences for settings.
- kotlinx.serialization for the bundled content JSON.
- `java.time` with core library desugaring for all date logic.
- WorkManager plus AlarmManager for the daily notification, `setExactAndAllowWhileIdle` where permitted.
- Minimum SDK 26, target latest stable.

**Content engine is data, not code (architectural requirement)**
The ritual engine must know nothing about Stoicism. Passages, authors, sources, questions, and context are pure data loaded from a content pack; the app reads whichever pack it is built with. This costs almost nothing now and is what makes a second edition (section 13) a matter of curation plus branding rather than a second project. It is also the part of this codebase worth showing to someone.

**Passage selection**
Deterministic: `index = hash(installId + localDate) % passageCount`, with a rolling exclusion window so a passage is not repeated within 180 days. Persist the delivered index per date so reopening always returns the same passage even if the algorithm changes in a later version.

**Streak state machine**
States: `Active(days, lastCompletedDate)`, `AtRisk`, `Broken(previousDays, brokenAt)`, `Restored`. All transitions computed from local dates. Unit test timezone shifts, DST, device clock changes, and app reinstall. This is where the one star reviews come from.

**Pause timer**
Foreground only, driven by a coroutine on a monotonic clock so it survives configuration changes. Keep the screen on during the pause (`FLAG_KEEP_SCREEN_ON`), release it immediately after.

**Privacy**
Reflections stay on device. Consider Room encryption (SQLCipher) in v1.1 and say so in the store listing. No analytics event ever carries reflection text, only its length bucket.

**Known Android risk:** OEM battery managers on Xiaomi, Samsung, Oppo drop scheduled alarms. Add a "not getting reminders?" help screen with a deep link to battery optimization settings.

## 10. Analytics

Firebase Analytics: `passage_viewed`, `question_viewed`, `pause_started`, `pause_completed`, `pause_skipped` (with passage id), `reflection_written` (length bucket only), `ritual_completed`, `streak_broken`, `streak_restored`, `share_generated`, `journal_opened`, `export_used`, `rewarded_completed`, `interstitial_shown`, `iap_purchased`.

Weekly review: pause skip rate per passage id. A question skipped by most users is a bad question, rewrite it in the next content update.

## 11. ASO

- English listing first, PT-BR and ES second.
- Keywords: stoic, stoicism, marcus aurelius, meditations, seneca, epictetus, daily reflection, mindfulness journal, philosophy daily, daily practice. Avoid building the listing around "daily stoic" as a phrase, given the Daily Stoic LLC brand.
- Positioning line: a daily Stoic practice, not a quote feed. Say it in the short description.
- Screenshots in order: the passage screen, the reflection question, the pause, the journal, the streak. Lead with the question screen in the feature graphic, it is the differentiator.

## 12. Milestones

| Milestone | Content | Estimate |
|---|---|---|
| M0 | Content: 365 passages verified, sourced, plus one approved question each | 5 to 6 weekends |
| M1 | Project setup, content pack loading, passage selection, Room, ritual steps 1 and 2 | 2 weekends |
| M2 | Pause timer, reflection input, ritual completion, streak state machine with tests | 2 weekends |
| M3 | Journal, search, export, favorites, settings, themes | 2 weekends |
| M4 | Notifications, share cards, milestones | 1 weekend |
| M5 | Ads, IAP, store listing, closed test with 12 testers (Play requirement, 14 days), release | 2 weekends |

M0 is the project. Getting 365 good reflection questions is genuinely hard. Questions are drafted with AI assistance, but every one is reviewed and approved by the owner; a question that is not specific to its passage gets rewritten or the passage is cut. Start it before writing Kotlin, and work on it in parallel with M1 through M4.

## 13. Post launch backlog

Home screen widget with today's passage, audio narration, evening variant (a second shorter prompt for review at night, closer to how Seneca actually practiced), themed multi day series (on death, on anger, on control), encrypted local backup, iOS.

**Second edition (only if the first gets traction).** The same engine with a different content pack, published as a separate app with its own name, icon and listing. A Christian or Catholic edition is the obvious candidate: the shared moral ground is large (the cardinal virtues, memento mori, detachment, the nightly examination of conscience that runs from Seneca through the Ignatian Examen), and the ritual shape fits.

Two warnings if that happens. First, do not merge the two into one app: the secular Stoic audience and the devotional audience reject each other's content, and a hybrid listing ranks for neither. Second, devotional content is unforgiving. Approved translations, correct attribution, an explicit decision on whether to follow the liturgical calendar, and review by someone who actually knows the tradition. A misattributed saint earns a one star review that explains exactly why.

## 14. Risks

- **Content is the bottleneck and the moat.** Both at once. Underinvesting here produces a generic quote app with extra steps.
- **The differentiator is thin.** See 3.1. The pause and the per-passage question carry the entire product. If they are not excellent, there is no reason to choose this over three established competitors.
- **Ad restraint versus revenue.** Accepted deliberately, see section 8.
- **Translation copyright.** Verify every translation edition individually.
- **Ritual friction.** Four steps is already at the limit. Do not add a fifth. If completion rate is below 40%, cut, do not add.
- **Category saturation.** Paid acquisition is not viable at this revenue level. Growth depends entirely on ASO and share cards.
