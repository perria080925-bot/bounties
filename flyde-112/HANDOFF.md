# HANDOFF — Bounty flyde #112 (estado y siguiente paso)

**Fecha:** 14-sep-2026 · Cuenta GitHub: `perria080925-bot` · Wallet Base (pago Algora): `0x5FEEeeF08B94e1D4CBf22EB536c1C0d4578B4cDd`

## Estado del trabajo

| Paso | Estado |
|---|---|
| Elegir bounty viva (la de highlight $400 ya estaba otorgada; las pequeñas tienen enjambres de 10-18 agentes) | ✅ |
| Causa raíz del bug #112 localizada y confirmada | ✅ |
| Fix implementado (`recoverMovedFileSource.ts` + parche en `findReferencedNodeServer.ts`) | ✅ |
| Tests: 5/5 pass (validación standalone real; spec mocha incluida para su CI) | ✅ |
| Commit en rama `fix/issue-112-import-recovery` + patch `.patch` generado | ✅ |
| Publicar comentario `/attempt` y abrir el PR | ⛔ bloqueado por permisos del token |
| Merge (maintainer) → pago Algora en USDC a tu wallet | ⏳ tras el PR |

## El bloqueante y cómo se resuelve (2 minutos de tu parte)

Tu PAT actual solo tiene acceso a `perria080925-bot/bounties` — GitHub rechaza con
`Resource not accessible by personal access token` el fork de `flydelabs/flyde` y el
comentario `/attempt` en su issue.

**Opción A (recomendada):** regenera el token con más permisos:
1. GitHub → Settings → Developer settings → Fine-grained tokens → **Generate new token**
2. Repository access: **All repositories**
3. Permissions: **Contents: Read and write**, **Pull requests: Read and write**, **Issues: Read and write**
4. Pásamelo por el chat → yo hago fork + push de la rama + PR + comentario `/attempt` en minutos.

**Opción B (sin nuevo token):** tú haces 3 clics:
1. Abre https://github.com/flydelabs/flyde/fork → Create fork
2. En tu fork: sube el patch (o me das el token y lo hago yo — el fork de un repo tuyo sí queda en el scope si eliges All repositories)
3. Yo preparo todo listo para que solo pegues el título y el cuerpo del PR (están en `PR_BODY.md`).

## Cuerpo del PR

Listo en `PR_BODY.md` (inglés, con referencia `Fixes #112`, causa raíz, estrategia y pruebas).

## Veredicto honesto sobre el dinero

- El bounty se cobra **después del merge** (Algora paga en USDC a la wallet Base registrada en console.algora.io — entra con tu GitHub y registra `0x5FEE...B4cDd`).
- Merge realista: días-semanas según el maintainer (y V1ki/David Dionisio son los reviewers habituales).
- Mientras tanto: cada venta Gumroad (~$9-16 netos) acerca los $100 sin esperar al maintainer — los 6 artículos publicados ya están indexando.
