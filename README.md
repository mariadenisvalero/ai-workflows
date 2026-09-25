# RL Email Pipeline — Routine setup

Este repo es la única pieza que le falta a Claude Code para poder construir emails de Ralph Lauren en Figma sin que nadie esté mirando el chat: las 4 skills ya probadas (`rl-brand-copy`, `rl-campaign-scaffolding`, `rl-email-assembly`, `rl-email-pipeline`) en `.claude/skills/`, listas para que una **Routine** de Claude Code las cargue automáticamente.

No hace falta ningún servidor propio (Express, Agent SDK standalone, bridge de Figma custom). El servidor MCP oficial de Figma solo acepta OAuth desde clientes en su lista blanca — y Claude Code está en esa lista — así que el motor de producción **es** una sesión de Claude Code en la nube, disparada por API.

## Qué es una Routine

Una Routine = un prompt guardado + este repo + los connectors (Figma, Google Drive) + un trigger. Se dispara por API, corre como sesión en la nube (con su propio link para verla en vivo), y usa los mismos connectors que ya están autenticados a nivel de tu organización — los mismos que acabamos de probar en vivo (lectura y escritura reales contra el archivo de trabajo).

## Pasos para crear la Routine

1. Sube este repo a GitHub (o pídeme el zip y lo subes tú a tu repo ya existente).
2. Ve a **claude.ai/code/routines** → **New routine**.
3. **Repository**: conecta este repo.
4. **Connectors**: activa **Figma** y **Google Drive** para esta routine (aunque ya estén autenticados a nivel de organización, hay que habilitarlos explícitamente por routine).
5. **Prompt**: pega el contenido de [`routine-prompt.md`](./routine-prompt.md) tal cual.
6. **Trigger**: click en **Add another trigger** → **API** → **Generate token**. Guarda el token (`sk-ant-oat01-...`) — solo se muestra una vez. Este token va en el backend de Lovable, **nunca** en el frontend.

## Cómo se dispara desde Lovable

```bash
curl -X POST https://api.anthropic.com/v1/claude_code/routines/$ROUTINE_ID/fire \
  -H "Authorization: Bearer $ROUTINE_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{"text": "<brief de marketing pegado por el usuario + respuestas de las preguntas aclaratorias>"}'
```

Respuesta:
```json
{
  "type": "routine_fire",
  "claude_code_session_id": "session_...",
  "claude_code_session_url": "https://claude.ai/code/session_..."
}
```

El backend de Lovable devuelve `claude_code_session_url` al chat para que el usuario la vea en vivo (razonamiento + cambios en Figma en tiempo real).

## Aviso de "listo" (sin polling nativo)

El endpoint `fire` responde inmediatamente al crear la sesión — no espera a que termine ni hace streaming. Para avisar cuando el email está listo, la opción limpia es que **el propio prompt le pida al agente que, como último paso, haga un POST a un webhook de Lovable** con el resultado (la sesión en la nube tiene salida de red). Para eso, el `text` que dispara la routine debe incluir un `job_id` y la URL del webhook, algo así:

```json
{"text": "job_id: abc123. Cuando termines, haz POST a https://tu-app.lovable.app/api/campaign-webhook/abc123 con {status, figma_url, summary}. Brief: <...>"}
```

Esto todavía no está en `routine-prompt.md` porque necesita la URL real del webhook de Lovable — se añade en una línea en cuanto exista.

## Límites a tener en cuenta

- 30 disparos/hora por routine, 100/hora por cuenta — de sobra para uso normal, pero hay que manejar el error 429 en el backend de Lovable.
- Cada disparo crea una sesión nueva — no hay idempotencia, así que el backend de Lovable no debe reintentar automáticamente sin comprobar antes si ya se creó una sesión para ese job.

## Contenido de este repo

```
.claude/skills/
  rl-brand-copy/          — plan de la campaña + copy on-brand (no toca Figma)
  rl-campaign-scaffolding/ — duplica la sección master, mete SL/PH/timeline/brief
  rl-email-assembly/      — construye los módulos reales en Figma
  rl-email-pipeline/      — el orquestador: en qué orden se llaman las 3 anteriores
```
