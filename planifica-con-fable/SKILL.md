---
name: planifica-con-fable
description: Planifica un proyecto con Fable 5.2 y lo reparte en agentes de Claude Code, uno por tarea, cada uno con Opus o con Sonnet según lo que pida la tarea. Pregunta antes, enseña el plan en una tabla y el archivo de cada agente, y no crea nada hasta que el usuario escribe «sí» o algo equivalente, como «adelante». Para empezar un proyecto, en una sesión abierta con Fable 5.2.
disable-model-invocation: true
argument-hint: "[tu proyecto en unas líneas]"
license: MIT
---

# Planifica con Fable y reparte el trabajo en agentes

Tú haces el plan; el trabajo lo harán los agentes, cada uno con Opus o con Sonnet.

El proyecto, según el usuario: $ARGUMENTS

Si arriba no hay ninguna descripción del proyecto, pídesela en una frase (qué quiere conseguir, qué hay ya hecho y qué no se puede tocar) y espera.

Si sabes que en esta sesión no estás funcionando con Fable 5.2 o posterior, dilo en una línea antes de seguir y pregunta si sigues.

Sigue estos pasos, en este orden.

Hasta el «sí» del paso 4, no crees ni cambies ningún archivo: todo lo enseñas en el chat.

## 1. Pregunta antes

Lee del proyecto lo que necesites. Antes de planificar, haz en un solo mensaje las preguntas que te falten, cinco como mucho, y espera a que el usuario conteste.

## 2. El plan, en una tabla

Escribe el plan en el chat: una tabla con cada tarea, qué entrega, cómo se sabe que está hecha y de qué tarea depende. Ocho tareas como mucho; si salen más, agrúpalas.

## 3. Un agente por tarea

Propón un agente para cada tarea y enseña en el chat su archivo entero, tal como quedaría en `.claude/agents/`, con este formato:

```markdown
---
name: nombre-de-la-tarea
description: Cuándo hay que encargarle trabajo, en una frase
tools: Read, Glob, Grep
model: sonnet
---

Qué hace, qué no puede tocar, qué entrega y cómo sabe que ha terminado.
```

Reglas para los agentes:

- `model: opus` si la tarea pide pensar: decidir cómo se hace, diseñar, resolver algo difícil o revisar el trabajo de otro agente. `model: sonnet` si es repetitiva o de volumen: muchos archivos parecidos, pasar datos de un formato a otro, documentación. Solo `opus` o `sonnet`: ni `fable`, ni `haiku`, ni `inherit`.
- Debajo de cada archivo, una línea con el porqué de su modelo.
- `tools`: solo las que necesite. Si solo lee, `Read, Glob, Grep`.
- `name`: en minúsculas y con guiones.
- Al acabar, cada agente dice qué ha cambiado, para que lo revise una persona.

## 4. Para y espera

No crees ni cambies ningún archivo hasta que el usuario escriba «sí» o algo equivalente, como «adelante». Si pide cambios, enseña lo que cambie y vuelve a esperar.

## 5. Con el «sí»

Guarda el plan en `PLAN.md`, en la raíz del proyecto, y crea los agentes en `.claude/agents/`, un archivo por agente. Al principio de `PLAN.md`, estas reglas para quien coordine:

- Cada tarea la hace su agente.
- Al encargarla, no le cambies el modelo.
- Una tarea cada vez; al acabar cada una, para y enséñasela al usuario.

No empieces ninguna tarea.

## Datos de clientes y de personas

Ni en el plan, ni en los agentes, ni en los ejemplos, datos de clientes ni de personas: nombres, correos, teléfonos, direcciones, DNI o importes. Si una tarea los necesita, escribe `[DATO REAL: lo pone una persona]`. Si encuentras archivos con esos datos, no copies nada de ellos y avisa al usuario.

## Al terminar

Di qué archivos has creado y recuerda al usuario dos cosas:

1. Que cierre Claude Code y lo abra otra vez para el trabajo, con Opus (`claude --model opus`): así quien coordina ya no es Fable.
2. Que, si la carpeta `.claude/agents/` no existía antes, Claude Code no verá los agentes hasta que lo vuelva a abrir.
