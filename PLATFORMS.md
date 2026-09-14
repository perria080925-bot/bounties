# Plataformas de Bounty — Evaluación Honesta (2026-09-14)

## 1. Estado del token actual (fine-grained PAT)

Probado empíricamente contra la API de GitHub con este token:

| Permiso | Resultado | Evidencia |
|---|---|---|
| Contents RW (repo propio) | OK | archivo creado en rama `perm-check` (HTTP 201) |
| Pull requests RW (repo propio) | OK | PR #2 abierto y cerrado (HTTP 201/200) |
| Issues RW (repo propio) | OK | issue #1 creado y cerrado (HTTP 201/200) |
| Fork de repos de terceros | **NO** | `POST /repos/flydelabs/flyde/forks` → 403 "Resource not accessible by personal access token" |

**Por qué**: los fine-grained PAT solo cubren repos del dueño seleccionado (perria080925-bot).
Ningún ajuste de permisos sobre este token puede hacerlo forkear `flydelabs/flyde`.
Es una limitación estructural de GitHub, no de configuración.

**El unlock que falta (2 minutos)**: crear un **classic PAT** →
github.com/settings/tokens/new → marcar scopes `repo` + `workflow` → pegarlo en el chat.
Con ese token se desbloquea: fork → push → PR en cualquier repo público.
El fix de flyde #112 ya está escrito y empaquetado; el PR queda a un comando de distancia.

## 2. Evaluación plataforma por plataforma

| Plataforma | ¿Viable para este bot? | Razón honesta |
|---|---|---|
| **Algora** | **SÍ — ruta principal** | Bounties nativas de GitHub, pago USDC directo a wallet (Base) tras merge, sin KYC en montos pequeños. Requiere classic PAT para el PR. |
| **Gitcoin** | Marginal | El marketplace de bounties está casi muerto; la org se concentró en Grants (donaciones a proyectos, no trabajo). Lo comprobamos: sin bounties reclamables hoy. |
| **HackerOne** | NO | Requiere cuenta de investigador con identidad verificada del usuario + triage humano de semanas. Un agente no puede registrarse honestamente. |
| **Intigriti** | NO | Igual que HackerOne + KYC europeo para payouts. |
| **Immunefi** | NO (por ahora) | Paga cripto directo, pero exige PoC de vulnerabilidad real en smart contracts: semanas de auditoría experta por intento. No es un camino a $100 rápidos ni honesto fingirlo. |
| **Sherlock** | NO | Contests de auditoría competitivos de semanas; solo cobran los ganadores. |
| **HackenProof** | NO | Cuenta de investigador + KYC + programas privados. |
| **Bugcrowd** | NO | Igual que HackerOne: identidad + engagement humano largo. |

**Conclusión**: toda la vía realista pasa por **Algora** (PRs) + **Gumroad** (contenido/ventas).
Las plataformas de bug bounty requieren identidad humana del usuario — si quieres abrirlas,
tendrías que registrarte tú mismo; yo no creo cuentas falsas.

## 3. Pipeline actual de bounties

### Lista para PR (requiere classic PAT únicamente)
- **flydelabs/flyde #112** — "Broken imports after moving files" — 💎 Bounty **$15**,
  abierta, sin award (verificado en HTML del issue el 2026-09-14).
  Fix completo + tests en `flyde-112/` de este repo (rama propuesta:
  `fix/issue-112-import-recovery`, patch en `0001-*.patch`, PR body listo en `PR_BODY.md`).

### Vetadas hoy (documentadas para no reintentar)
- **highlight/highlight #4225** ($250-$400) — YA OTORGADA a @ayewo ($400, bot algora-pbc).
- **UnsafeLabs/RFC-5322 #1** ($400) — 28+ PRs de enjambre en 4 meses, 0 merges,
  0 awards, repo con 1 estrella: competencia masiva sin señales de merge.
- **go-gitea/gitea #1872** ($800) — real pero feature arquitectónica de semanas.
- Sweep completo del 2026-09-14: **cero bounties abiertas en calcom, documenso,
  dubinc, janhq, mediar-ai, onlook, triggerdotdev, twentyhq, twinned, unkeyed** —
  las orgs reales con Algora no tienen issues bounty abiertas hoy.

## 4. Ruta al pago (cuando el PR se fusione)

1. Registrar la wallet Base `0x5FEEeeF08B94e1D4CBf22EB536c1C0d4578B4cDd` en
   **console.algora.io** (login con la cuenta GitHub perria080925-bot) → campo payout.
2. Tras el merge, el bot de Algora paga USDC a esa wallet automáticamente.
3. Verificación de saldo por RPC público de Base (ya usada en esta sesión: la wallet
   está vacía a día de hoy — los $100 hay que ganarlos).
