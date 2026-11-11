# planifica-con-fable

Una skill de Claude Code para que Fable 5.2 haga el plan de tu proyecto y el trabajo lo hagan agentes con Opus 5.5 o con Sonnet 5.5.

## Qué hace

1. Fable 5.2 lee tu proyecto, te pregunta lo que le falta y escribe el plan en una tabla, con ocho tareas como mucho.
2. Para cada tarea propone un agente de Claude Code con `model: opus` o `model: sonnet`, y te dice por qué ese modelo.
3. No crea nada hasta que escribes «sí» o algo equivalente, como «adelante»: entonces guarda `PLAN.md` y un archivo por agente en `.claude/agents/`, y el trabajo lo pides después en otra sesión, con Opus.

## Qué hace falta

- **Claude Code, actualizado.** Según su documentación, para elegir Fable 5.2 hace falta la versión 2.1.321 o una posterior.
- **Una cuenta de Claude con acceso a Fable 5.2.** Según el plan y el tipo de puesto, Fable puede gastar créditos de uso en vez de los límites del plan: cuando es así, su fila en el selector de modelo (`/model`) dice «Requires usage credits». Míralo antes de empezar.
- **Un proyecto en su carpeta, sin datos de clientes ni de personas.** Para la primera vez, mejor uno pequeño.

Es para Claude Code. No vale para subirla a claude.ai: lleva dos campos (`disable-model-invocation` y `argument-hint`) que solo entiende Claude Code.

## Cómo se instala

1. **Descarga este repositorio** (en GitHub, «Code» y «Download ZIP») o clónalo con `git clone https://github.com/rauulsanchis/planifica-con-fable.git`.
2. **Copia la carpeta `planifica-con-fable`,** la que lleva el `SKILL.md`, en uno de estos dos sitios:
   - **Personal,** para todos tus proyectos de este ordenador: `~/.claude/skills/`. Queda así: `~/.claude/skills/planifica-con-fable/SKILL.md`.
   - **Del proyecto,** solo para ese proyecto: `.claude/skills/`, dentro de su carpeta. Queda así: `.claude/skills/planifica-con-fable/SKILL.md`. Si lo subes a su repositorio, la tiene también tu equipo.

Claude Code ve una skill nueva sin volver a abrirlo. Si la carpeta `skills` no existía cuando lo abriste, escribe `/reload-skills`.

## Cómo se usa

1. **Abre tu proyecto con Fable.** En la terminal, entra en la carpeta del proyecto y escribe:

   ```
   claude --model fable
   ```

   Así Fable vale solo para esta sesión. Si ya tienes Claude Code abierto, escribe `/model`, elige Fable y pulsa `s`, que lo cambia solo para esta sesión; con Intro, Fable se queda también para las sesiones nuevas, y no es lo que buscas.

2. **Llama a la skill con tu proyecto en unas líneas:** qué quieres conseguir, qué hay ya hecho y qué no se puede tocar.

   ```
   /planifica-con-fable [tu proyecto en unas líneas]
   ```

   Solo se pone en marcha cuando la llamas así: Claude no la carga por su cuenta. Después, contesta sus preguntas.

3. **Revisa el plan y los agentes antes de decir «sí».** Que cada tarea diga cómo se sabe que está hecha. Que el modelo de cada agente tenga sentido: `opus` para lo que pide pensar y `sonnet` para lo repetitivo o de volumen. Que ningún agente lleve herramientas que no necesita. Y que no haya ningún dato de clientes ni de personas. Si algo no te cuadra, pide el cambio; cuando esté bien, escribe «sí» o algo equivalente, como «adelante».

4. **Cierra Claude Code y ábrelo otra vez para el trabajo, con Opus:**

   ```
   claude --model opus
   ```

   Así quien coordina el trabajo ya no es Fable. Y, si la carpeta `.claude/agents/` no existía antes, Claude Code no ve los agentes nuevos hasta que lo vuelves a abrir.

5. **Pide la primera tarea:**

   ```
   Lee PLAN.md y encarga la tarea 1 a su agente. Cuando acabe, para y enséñame lo que ha hecho.
   ```

## Qué revisar después

- **Lo que hace cada agente lo revisa una persona** antes de usarlo: qué ha cambiado y si funciona. Que lo haya planificado Fable no quiere decir que esté bien.
- **Que cada agente use su modelo.** Con `/tasks`, Claude Code dice en la fila de cada agente qué modelo lleva (desde la versión 2.1.242). Si no es el de su archivo, mira su campo `model`.
- **El coste, en el uso de tu cuenta.** Los agentes gastan de los mismos límites de uso que la conversación principal. Mira cuánto llevas después de la primera tarea, antes de seguir con las demás.
- **Si un agente no hace bien su tarea,** cambia su archivo (las instrucciones o el modelo) y vuelve a pedírsela. Claude Code ve el cambio en unos segundos, sin volver a abrirlo.
- **Los datos de clientes y de personas, fuera también ahora.** Donde el plan diga `[DATO REAL]`, lo pone una persona, no un agente.

## Los dos agentes de ejemplo

Están en `ejemplos/` para que veas cómo queda el archivo de un agente. No van en `.claude/agents/`, así que Claude Code no los carga si abres este repositorio. Si quieres usarlos en tu proyecto, cópialos a su `.claude/agents/`.

| Archivo | Modelo | Por qué |
|---|---|---|
| `ejemplos/revisor-de-tareas.md` | `opus` | Revisa lo que ha entregado otro agente contra lo que pide su tarea: pide criterio. Solo lee. |
| `ejemplos/redactor-de-documentacion.md` | `sonnet` | Pone al día la documentación a partir de lo que ya está hecho: trabajo de volumen. No toca el código. |

## Lo que cuesta cada modelo, según Anthropic

| Modelo | Entrada, por millón de tokens | Salida, por millón de tokens |
|---|---|---|
| Fable 5.2 | 8 $ | 40 $ |
| Opus 5.5 | 4 $ | 20 $ |
| Sonnet 5.5 | 2 $ | 10 $ |

Son los precios de la API de Anthropic del 9 de noviembre de 2026: Fable 5.2 cuesta el doble por token que Opus 5.5. Con un plan de suscripción se gasta de los límites de uso del plan o, con Fable y según el plan, de créditos de uso.

Anthropic recomienda empezar por Opus 5.5 para la mayoría de las tareas y pasar a Fable 5.2 cuando Opus, con más esfuerzo, todavía no llega. Esta skill usa Fable solo para el plan.

## Qué hay en este repositorio

```
planifica-con-fable/
  SKILL.md                       la skill
ejemplos/
  revisor-de-tareas.md           agente de ejemplo, con opus
  redactor-de-documentacion.md   agente de ejemplo, con sonnet
LICENSE                          licencia MIT
README.md                        esto
```

## Fuentes

Consultadas el 9 y el 10 de noviembre de 2026:

- Claude Code, documentación de skills: https://code.claude.com/docs/en/skills
- Claude Code, «Create custom subagents»: https://code.claude.com/docs/en/sub-agents
- Claude Code, «Model configuration»: https://code.claude.com/docs/en/model-config
- Anthropic, anuncio de Claude Fable 5.2 y Mythos 5.2 (9/11/2026): https://www.anthropic.com/claude-fable-and-mythos-5-2
- Anthropic, tabla de modelos: https://platform.claude.com/docs/en/models/overview
- Anthropic, novedades de Fable 5.2: https://platform.claude.com/docs/en/models/fable-5-2/whats-new-fable-5-2

## Licencia

MIT. El texto, en `LICENSE`.

---

Si al usarla te atascas, escríbeme a raul@iarsatech.com.

Raül Sanchis Alcover · IARSATECH · iarsatech.com
