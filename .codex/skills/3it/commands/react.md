<!-- BEGIN 3it-arch-kit managed block -->
framework: react
target: codex
<!-- END 3it-arch-kit managed block -->

# React Commands

## /valida-react

Auditoria empresarial ReactJS

```text
Usa el skill `3itkit-reactjs-enterprise-review` y aplica todas las reglas de `.cursor/rules`.

Revisa arquitectura por features, Clean Architecture, SOLID, DDD lite, componentes, hooks,
estado, router, APIs, DTOs, auth, seguridad, OWASP, accesibilidad, UX/UI, design system,
TypeScript, rendimiento, bundling, SSR/PWA cuando aplique, observabilidad, storage,
dependencias, testing, coverage, Sonar y CI/CD.

No inventes hallazgos; utiliza unicamente evidencia encontrada en el proyecto.
```

## /valida-coverage-react

Valida coverage y Sonar para ReactJS

```text
Usa el skill `3itkit-reactjs-coverage-review` y aplica la regla `reactjs-coverage-sonar`
junto con todas las reglas de `.cursor/rules`.

Objetivo: validar o preparar el proyecto ReactJS para coverage >= 90% y Quality Gate SonarQube/SonarCloud.

Revisa scripts, configuracion de tests, `lcov.info`, Sonar, pipeline CI/CD, bugs,
vulnerabilidades, security hotspots, code smells y cobertura de componentes, logica,
servicios, hooks, stores y estados de UI.

No inventes hallazgos; utiliza unicamente evidencia encontrada en el proyecto.
Si falta informacion, declaralo como brecha y propon el cambio minimo compatible con el stack real.
```

## /valida-owasp-react

Valida OWASP para react

```text
Usa el skill `3itkit-reactjs-owasp-review` y aplica la regla `reactjs-owasp` junto con las reglas de seguridad relacionadas en `.cursor/rules`.

Objetivo: validar exclusivamente seguridad OWASP para `react`. Cubre OWASP Top 10 Web, OWASP API Security, OWASP ASVS, XSS, CSP, auth frontend, storage, CORS, dependencias y exposicion de datos en cliente.

Revisa autenticacion, autorizacion, validacion de entradas, injection, exposicion de datos, manejo de errores, headers, CORS, secrets, logs, dependencias vulnerables, abuso de APIs y configuracion insegura segun aplique al framework.

Entrega resumen ejecutivo, matriz OWASP cubierta, hallazgos priorizados, evidencia por archivo/configuracion y plan de remediacion.

No inventes hallazgos; utiliza unicamente evidencia encontrada en el proyecto.
```

## /genera-tests-react

Genera tests para coverage >= 90% en react

```text
Usa el skill `3itkit-reactjs-test-generation` y aplica la regla `reactjs-coverage-sonar` junto con las reglas de testing, arquitectura y seguridad de `.cursor/rules`.

Objetivo: generar o ajustar tests para que `react` alcance coverage >= 90% y mantenga Quality Gate SonarQube/SonarCloud.

Alcance principal: components, hooks, stores, API clients, DTOs, mappers, routing, providers, forms and UI states.

Instrucciones:
- Analiza el stack real del proyecto antes de modificar archivos.
- Revisa scripts, configuracion de tests, coverage, Sonar y CI/CD.
- Prioriza tests sobre reglas de negocio, mappers, servicios/use cases, seguridad, errores y flujos criticos.
- Para frontend/mobile cubre loading, error, empty, success, disabled, permisos y accesibilidad basica cuando aplique.
- Para backend cubre validacion, autorizacion, transacciones, repositorios, eventos, idempotencia y excepciones cuando aplique.
- No bajes thresholds ni excluyas codigo para simular coverage.
- Ejecuta tests y coverage si el entorno lo permite; si no, deja comandos exactos y brechas.

Entrega resumen ejecutivo, tests generados, archivos modificados, metricas de coverage, brechas restantes y siguiente accion recomendada.
```

## /valida-e2e-playwright

Valida E2E Playwright en ReactJS

```text
Usa el skill `3itkit-reactjs-playwright-e2e-review` y aplica la regla `reactjs-playwright-e2e` junto con las reglas frontend de `.cursor/rules`.

Objetivo: validar que el proyecto ReactJS tenga E2E Playwright enterprise listo para CI/CD.

Revisa configuracion, scripts, specs, locators accesibles, `data-testid`, fixtures, datos idempotentes, reportes, traces y pipeline.

Entrega PASS/WARN/FAIL, matriz de flujos cubiertos, brechas y acciones concretas. No inventes ejecuciones.
```

## /genera-e2e-playwright

Genera E2E Playwright en ReactJS

```text
Usa el skill `3itkit-reactjs-playwright-e2e-generation` y aplica la regla `reactjs-playwright-e2e` junto con las reglas frontend de `.cursor/rules`.

Objetivo: generar o completar E2E Playwright para flujos funcionales criticos del proyecto ReactJS.

Crea o ajusta configuracion, scripts, fixtures y specs usando locators accesibles y `data-testid` estable. Cubre login, navegacion, CRUD, filtros, paginacion, validaciones, permisos, errores y estados vacios segun evidencia real.

No inventes pantallas ni dependencias; respeta el stack existente.
```

## /ejecuta-e2e-playwright

Ejecuta E2E Playwright en ReactJS

```text
Usa el skill `3itkit-reactjs-playwright-e2e-execution` y aplica la regla `reactjs-playwright-e2e` junto con las reglas frontend de `.cursor/rules`.

Objetivo: ejecutar las pruebas E2E Playwright del proyecto ReactJS y entregar evidencia.

Detecta gestor y scripts, instala browsers solo con confirmacion si faltan, ejecuta `test:e2e` o `playwright test`, y resume resultados, fallos, traces, reportes y siguientes acciones.

No inventes resultados ni ocultes fallos.
```

## /jira-config

Configura Jira para SDD Kit creando o validando .3it-arch-kit/jira.yaml

```text
Usa el skill `3itkit-jira-config` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: configurar el proyecto para sincronizar SDD Kit con Jira mediante Atlassian Rovo MCP.

Pide o confirma:
- sitio Jira, por ejemplo `tresit.atlassian.net`
- project key, por ejemplo `VEX`
- activar Jira, default `false`
- tipo para Spec, default `Epic`
- tipo para User Story, default `Historia`
- tipo para Task, default `Tarea`
- tipo para Technical Task, default `Sub-task`
- `sync.updateExisting`, default `true`
- `sync.deleteRemoved`, default `false`
- asignacion de issues, opcional, default efectivo `current_user`

Procedimiento:
1. Revisa si existe `.3it-arch-kit/jira.yaml`.
2. Crea o actualiza `.3it-arch-kit/jira.yaml` con los valores confirmados.
3. Si `jira.enabled` es `false`, no intentes conectar con Atlassian Rovo MCP.
4. Si `assignment` no existe, usa default efectivo `current_user`.
5. Si `assignment.mode` es `current_user` y Atlassian Rovo MCP esta disponible, detecta el usuario conectado y guarda `jira.user.accountId` solo si el usuario acepta dejar trazabilidad local.
6. Si `assignment.mode` es `explicit_account_id`, valida que `assignment.accountId` este informado.
7. Si `assignment.mode` es `unassigned`, no guardes assignee.
8. Si `jira.enabled` es `true` y Atlassian Rovo MCP esta disponible, valida sitio, proyecto, usuario, permisos e issue types.
9. Si `jira.enabled` es `true` y Atlassian Rovo MCP no esta disponible, deja la configuracion local y reporta validacion remota pendiente.

No pidas ni guardes credenciales. No inventes accountId. No crees issues Jira. No modifiques archivos SDD.
```

## /jira-sync

Sincroniza spec.md, plan.md y tasks.md con Jira mediante Atlassian Rovo MCP

```text
Usa el skill `3itkit-jira-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: sincronizar el spec SDD activo con Jira mediante Atlassian Rovo MCP.

Mapping obligatorio:
- spec.md / Spec -> Epic
- plan.md / User Story -> Historia
- tasks.md / Task -> Tarea

Procedimiento:
1. Lee `spec.md`, `plan.md`, `tasks.md`, `.3it-arch-kit/jira.yaml` y `.jira-sync.yaml`.
2. Si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
3. Valida conexion Jira, proyecto, issue types y campos requeridos usando Atlassian Rovo MCP.
4. Detecta etapa disponible o solicitada: spec, plan, tasks o full.
5. Calcula el delta SDD/Jira de esa etapa.
6. No asigna issues durante sincronizacion salvo `assignment.assignOnSync: true` solicitado explicitamente.
7. Crea solo issues faltantes de esa etapa sin assignee por defecto.
8. Actualiza issues existentes cuando corresponda.
9. Guarda las keys, source, parent y lastSyncedStage en `.jira-sync.yaml`.
10. Entrega resumen de etapa, creados, actualizados, omitidos, errores y brechas.

No dupliques issues. No uses REST Jira directo. No inventes resultados de Jira.
```

## /jira-sync-spec

Sincroniza spec.md con la epica Jira de forma incremental

```text
Usa el skill `3itkit-jira-sdd-incremental-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: sincronizar solo la etapa `spec.md` con Jira.

Procedimiento:
1. Lee `.3it-arch-kit/jira.yaml`; si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
2. Lee `spec.md` y `.jira-sync.yaml` si existe.
3. Crea o actualiza la epica Jira segun `mapping.spec`, default `Epic`.
4. No asigna la epica durante sincronizacion salvo solicitud explicita con `assignment.assignOnSync: true`.
5. Guarda `spec.source: spec.md`, `spec.jiraKey` y `spec.lastSyncedStage: spec` en `.jira-sync.yaml`.
6. No crees historias ni tareas en esta etapa.

No dupliques issues. No inventes contenido faltante.
```

## /jira-sync-plan

Sincroniza plan.md con historias Jira bajo la epica

```text
Usa el skill `3itkit-jira-sdd-incremental-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: sincronizar solo la etapa `plan.md` con Jira.

Procedimiento:
1. Lee `.3it-arch-kit/jira.yaml`; si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
2. Lee `spec.md`, `plan.md` y `.jira-sync.yaml`.
3. Verifica que la epica tenga `jiraKey`; si no existe, ejecuta primero el flujo de `/jira-sync-spec`.
4. Crea o actualiza historias segun `mapping.userStory`, default `Historia`.
5. Vincula cada historia bajo la epica cuando Jira lo permita.
6. No asigna historias durante sincronizacion salvo solicitud explicita con `assignment.assignOnSync: true`.
7. Guarda `source: plan.md`, `jiraKey`, `parent` y `lastSyncedStage: plan` en `.jira-sync.yaml`.
8. No crees tareas tecnicas en esta etapa.

No dupliques issues. No inventes historias que no esten en `plan.md`.
```

## /jira-sync-tasks

Sincroniza tasks.md con tareas Jira bajo sus historias

```text
Usa el skill `3itkit-jira-sdd-incremental-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: sincronizar solo la etapa `tasks.md` con Jira.

Procedimiento:
1. Lee `.3it-arch-kit/jira.yaml`; si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
2. Lee `spec.md`, `plan.md`, `tasks.md` y `.jira-sync.yaml`.
3. Verifica que existan epica e historias con `jiraKey`; si faltan, ejecuta antes los flujos de `/jira-sync-spec` y `/jira-sync-plan`.
4. Crea o actualiza tareas segun `mapping.task`, default `Tarea`.
5. Asocia cada tarea con su historia cuando Jira lo permita.
6. No asigna tareas durante sincronizacion salvo solicitud explicita con `assignment.assignOnSync: true`.
7. Guarda `source: tasks.md`, `jiraKey`, `parent`, `status` y `lastSyncedStage: tasks` en `.jira-sync.yaml`.

No dupliques issues. No cierres tareas removidas sin confirmacion explicita.
```

## /jira-start-task

Mueve una tarea SDD/Jira a In Progress al iniciar /speckit-implement

```text
Usa el skill `3itkit-jira-implement-trace` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: reflejar en Jira el inicio real de una tarea que sera implementada con `/speckit-implement`.

Procedimiento:
1. Detecta la tarea SDD indicada por el usuario, por ejemplo `T004`.
2. Lee `.3it-arch-kit/jira.yaml` y `specs/<spec>/.jira-sync.yaml`.
3. Si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
4. Resuelve `T004 -> jiraKey`.
5. Valida que Atlassian Rovo MCP este disponible.
6. Resuelve assignee efectivo; si `assignment` no existe, usa usuario conectado via MCP.
7. Asigna el issue al assignee efectivo cuando corresponda.
8. Transiciona el issue a `workflow.inProgress`, default `In Progress`.
9. Si `implementation.commentOnTransition` es `true`, comenta que la implementacion inicio.

No crees issues. No inventes keys. Si no existe `jiraKey`, pide ejecutar `/jira-sync`.
```

## /jira-complete-task

Mueve una tarea SDD/Jira a Done cuando /speckit-implement termina con evidencia valida

```text
Usa el skill `3itkit-jira-implement-trace` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: reflejar en Jira el cierre real de una tarea implementada con `/speckit-implement`.

Procedimiento:
1. Detecta la tarea SDD indicada por el usuario, por ejemplo `T004`.
2. Lee `.3it-arch-kit/jira.yaml` y `specs/<spec>/.jira-sync.yaml`.
3. Si `jira.enabled` es `false`, entrega SKIPPED y no intentes Atlassian Rovo MCP.
4. Resuelve `T004 -> jiraKey`.
5. Recopila evidencia de implementacion: archivos modificados, pruebas, build, coverage, OWASP y validaciones 3it ejecutadas.
6. Si `implementation.requireValidationBeforeDone` es `true`, exige evidencia PASS antes de cerrar.
7. Transiciona el issue a `workflow.done`, default `Done`, solo si la evidencia es suficiente.
8. Comenta en Jira el resumen de evidencia y resultado.

Si faltan validaciones o alguna falla, deja el issue en `In Progress` y reporta el bloqueo. No uses REST Jira directo.
```

## /jira-status

Revisa trazabilidad entre SDD, Jira, Git y estado local de sincronizacion

```text
Usa el skill `3itkit-jira-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: reportar el estado de trazabilidad SDD/Jira sin crear ni actualizar issues.

Revisa:
- `.3it-arch-kit/jira.yaml`
- `spec.md`
- `plan.md`
- `tasks.md`
- `.jira-sync.yaml`
- si `jira.enabled` es `false`, reportar solo trazabilidad local con SKIPPED
- existencia de Jira keys mediante Atlassian Rovo MCP cuando este disponible

Entrega:
- Spec -> Epic
- User Story -> Historia
- Task -> Tarea
- issues sin key
- keys que no existen o no son accesibles
- brechas de idempotencia
- recomendaciones de sincronizacion

No hagas cambios en Jira ni en archivos locales.
```

## /jira-validate

Valida configuracion Jira, mapping SDD y precondiciones de sincronizacion

```text
Usa el skill `3itkit-jira-sync` y aplica la regla `jira-sync-atlassian-rovo`.

Objetivo: validar que el proyecto puede sincronizar SDD Kit con Jira de forma idempotente.

Valida:
- Atlassian Rovo MCP disponible.
- Jira activo con `jira.enabled: true`; si esta en `false`, reporta SKIPPED.
- Sitio Jira configurado, por ejemplo `tresit.atlassian.net`.
- Project key configurado, por ejemplo `VEX`.
- Issue types disponibles: `Epic`, `Historia`, `Tarea`.
- Usuario conectado detectable al iniciar implementacion cuando `assignment` no exista o `assignment.mode` sea `current_user`.
- `assignment.accountId` valido cuando `assignment.mode` sea `explicit_account_id`.
- `.3it-arch-kit/jira.yaml` valido.
- `.jira-sync.yaml` valido si existe.
- No hay IDs SDD duplicados.
- El mapping Spec/User Story/Task esta completo.

No crees issues. No actualices Jira. Entrega PASS/FAIL y acciones de correccion.
```
