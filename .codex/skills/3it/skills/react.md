<!-- BEGIN 3it-arch-kit managed block -->
framework: react
target: codex
<!-- END 3it-arch-kit managed block -->

# React Skills

## 3itkit-reactjs-enterprise-review

Auditoria empresarial para ReactJS

Command: /valida-react

## Obligatorio: estructura de carpetas (PASS/FAIL)

Comparar el arbol real contra la arquitectura React por features.

Estructura canonica esperada:

```
src/
  app/          # router, providers, store
  shared/
  features/{feature}/
    presentation/
    application/
    domain/
    infrastructure/
```

Reglas de veredicto:
1. Si hay logica de negocio o HTTP directo en components de presentation → **FAIL de arquitectura** (severidad ALTA).
2. Hexagonal incompleto en features triviales no es FAIL; si hay dominio real sin boundaries → FAIL.
3. Reportar siempre una tabla PASS/FAIL de distribucion de carpetas **antes** de hallazgos OWASP.
4. Priorizar: estructura → seguridad/OWASP → calidad/coverage → CI/CD.

Revisa arquitectura por features, Clean Architecture, SOLID, DDD lite, componentes,
hooks, estado, router, APIs, DTOs, auth, seguridad, OWASP, accesibilidad, UX/UI,
design system, TypeScript, rendimiento, bundling, SSR/PWA cuando aplique, observabilidad,
storage, dependencias, testing, coverage, Sonar y CI/CD.

Entrega resumen ejecutivo, puntuacion por categoria, hallazgos priorizados y plan de mejora.

No inventes hallazgos; utiliza unicamente evidencia encontrada en el proyecto.

## 3itkit-reactjs-coverage-review

Valida coverage >= 90% y Sonar para proyectos ReactJS

Command: /valida-coverage-react

Valida y, cuando el usuario lo pida, genera pruebas y configuracion para que un proyecto
ReactJS cumpla coverage >= 90% y Quality Gate SonarQube/SonarCloud.

Evalua:
- Stack real de pruebas y gestor de paquetes.
- Scripts de test, coverage y CI/CD.
- Reporte `lcov.info` y umbrales statements/branches/functions/lines >= 90%.
- Configuracion Sonar y `sonar.javascript.lcov.reportPaths`.
- Bugs, vulnerabilidades, security hotspots, code smells, duplicacion y deuda tecnica.
- Cobertura de componentes React, hooks, stores, clientes API, DTOs, mappers, routing, providers y paginas por feature.
- Estados loading, error, empty, success y accesibilidad basica en interfaces.

Cuando generes cambios:
- Usa el framework de pruebas ya presente en el proyecto.
- No inventes dependencias ni migres herramientas sin evidencia y justificacion.
- Agrega tests de mayor valor primero: logica de negocio, mappers, servicios, hooks, stores y componentes criticos.
- Mantener el objetivo de coverage >= 90% sin bajar umbrales.

Entrega resumen ejecutivo, metricas encontradas, brechas contra 90%, hallazgos Sonar priorizados y plan de correccion.

## 3itkit-reactjs-owasp-review

Auditoria OWASP dedicada para ReactJS

Command: /valida-owasp-react

Valida exclusivamente seguridad OWASP para `react`. Cubre OWASP Top 10 Web, OWASP API Security, OWASP ASVS, XSS, CSP, auth frontend, storage, CORS, dependencias y exposicion de datos en cliente.

Entrega hallazgos priorizados por severidad, evidencia concreta, riesgo, impacto, recomendacion y estado bloqueante/no bloqueante.
No inventes vulnerabilidades; si falta evidencia, declaralo como brecha de validacion.

## 3itkit-reactjs-test-generation

Genera tests para alcanzar coverage >= 90% en react

Command: /genera-tests-react

Genera o ajusta tests para `react` con objetivo minimo de coverage >= 90% sin bajar umbrales ni ocultar codigo sin probar.

Alcance principal: components, hooks, stores, API clients, DTOs, mappers, routing, providers, forms and UI states.

Herramientas esperadas: Vitest, Jest, React Testing Library, MSW, Playwright/Cypress or the real test stack already configured.

Proceso obligatorio:
- Detecta primero el stack real de pruebas, gestor de paquetes, scripts y configuracion de coverage.
- Identifica brechas por riesgo antes de crear archivos.
- Genera tests pequenos, mantenibles y deterministas para comportamiento observable y reglas de negocio.
- Mockea solo bordes externos; no mockees la logica que se quiere validar.
- Cubre success, error, validaciones, permisos, estados vacios, concurrencia/idempotencia o UI states segun aplique.
- Ejecuta tests y coverage si el entorno lo permite.
- Mantiene Quality Gate SonarQube/SonarCloud y coverage >= 90%.

No inventes dependencias ni migres herramientas sin evidencia. Si falta informacion o no puedes ejecutar comandos, declaralo como brecha.

## 3itkit-reactjs-playwright-e2e-review

Valida E2E Playwright enterprise en ReactJS

Command: /valida-e2e-playwright

Valida pruebas funcionales E2E Playwright para proyectos ReactJS.

Objetivo: asegurar que el proyecto tenga pruebas E2E confiables, mantenibles y listas para CI/CD.

Revisa:

- `package.json`, scripts `test:e2e`, `test:e2e:ui`, `test:e2e:report`.
- `playwright.config.ts` o configuracion equivalente.
- Carpeta `e2e/` o `tests/e2e/`.
- Tests por feature y flujos criticos de negocio.
- Locators accesibles y `data-testid` estable.
- Uso de fixtures, API setup, seed, teardown y datos idempotentes.
- Evidencia: `playwright-report/`, `test-results/`, traces, screenshots o videos en fallos.
- CI/CD con instalacion de browsers y ejecucion reproducible.

Criterios PASS/FAIL:

- FAIL si no existe Playwright ni alternativa E2E aprobada.
- FAIL si los tests dependen de selectores fragiles, sleeps fijos o datos manuales.
- FAIL si hay credenciales reales o secretos en tests/evidencias.
- WARN si faltan flujos criticos o solo hay smoke tests.
- PASS si los flujos criticos son independientes, legibles, trazables y ejecutables.

Entrega resumen ejecutivo, matriz de flujos cubiertos, hallazgos por severidad, evidencia por archivo y plan de remediacion.
No inventes resultados de ejecucion; si no ejecutaste tests, declaralo como brecha.

## 3itkit-reactjs-playwright-e2e-generation

Genera E2E Playwright para flujos funcionales en ReactJS

Command: /genera-e2e-playwright

Genera o completa pruebas funcionales E2E Playwright para proyectos ReactJS.

Flujo obligatorio:

1. Detectar framework, gestor de paquetes y stack real antes de modificar archivos.
2. Revisar rutas, pantallas, formularios, API clients, mocks, auth y estado existente.
3. Si Playwright no existe, proponer configuracion minima compatible con el proyecto.
4. Crear o ajustar `playwright.config.ts` sin romper configuracion existente.
5. Crear tests por feature en `e2e/` o `tests/e2e/`.
6. Agregar Page Objects o fixtures solo cuando reduzcan duplicacion real.
7. Usar locators accesibles; agregar `data-testid` estable en UI critica cuando haga falta.
8. Cubrir login, navegacion, CRUD, filtros, paginacion, validaciones, permisos, errores y estados vacios segun aplique.
9. Agregar scripts `test:e2e`, `test:e2e:ui` y `test:e2e:report` si no existen.
10. Ejecutar o dejar comandos exactos si el entorno no permite correr browsers.

No inventes pantallas ni flujos inexistentes. No agregues secretos reales. No reemplaces el stack sin evidencia.

## 3itkit-reactjs-playwright-e2e-execution

Ejecuta E2E Playwright y resume evidencia en ReactJS

Command: /ejecuta-e2e-playwright

Ejecuta pruebas funcionales E2E Playwright para proyectos ReactJS y entrega evidencia.

Flujo obligatorio:

1. Detectar gestor de paquetes: npm, pnpm o yarn.
2. Verificar scripts E2E existentes antes de usar comandos directos.
3. Verificar que browsers Playwright esten instalados; si falta instalacion, pedir confirmacion antes de descargar dependencias.
4. Ejecutar el comando existente, preferentemente `test:e2e`; si no existe, usar `playwright test` compatible con el gestor.
5. En CI recomendar reporter `line,junit` y preservar `playwright-report/` y `test-results/`.
6. Si falla, resumir specs fallidos, mensajes, traces y rutas de evidencia.
7. Si pasa, reportar total de tests, browsers, duracion y evidencia generada.

No ocultes fallos ni reintentes indefinidamente. No inventes resultados. Si no puedes ejecutar, entrega comandos exactos y causa.

## 3itkit-jira-config

Configura .3it-arch-kit/jira.yaml para sincronizar SDD Kit con Jira

Command: /jira-config

Configura el proyecto para usar Jira con SDD Kit y 3it-arch-kit mediante Atlassian Rovo MCP.

Flujo obligatorio:

1. Revisar si existe `.3it-arch-kit/jira.yaml`.
2. Si no existe, pedir los datos minimos al usuario.
3. Si existe, validar estructura, valores y mapping.
4. Preguntar o confirmar el sitio Jira, por ejemplo `tresit.atlassian.net`.
5. Preguntar o confirmar el project key, por ejemplo `VEX`.
6. Preguntar si Jira queda activo, default `false`.
7. Preguntar o confirmar mapping:
   - `spec`: default `Epic`
   - `userStory`: default `Historia`
   - `task`: default `Tarea`
   - `technicalTask`: default `Sub-task`
8. Preguntar si se actualizan existentes, default `true`.
9. Preguntar si se borran o cierran removidos, default `false`.
10. Tratar asignacion como opcional; si el usuario no configura nada, usar `current_user` solo al iniciar implementacion.
11. Si el modo efectivo es `current_user` y Atlassian Rovo MCP esta disponible, detectar el usuario conectado y guardar `jira.user.accountId` y `jira.user.displayName` si el usuario acepta dejar trazabilidad local.
12. Si el modo es `explicit_account_id`, pedir o confirmar `assignment.accountId`.
13. Crear o actualizar `.3it-arch-kit/jira.yaml`.
14. Validar, si `jira.enabled` es `true` y Atlassian Rovo MCP esta disponible, que el sitio, proyecto, usuario, permisos e issue types existan.

Reglas:

- No pedir usuario, password, API token ni credenciales.
- No guardar secretos en el repositorio.
- No crear issues Jira.
- No modificar `spec.md`, `plan.md`, `tasks.md` ni `.jira-sync.yaml`.
- Si el usuario deja `jira.enabled: false`, no intentar conectar con Atlassian Rovo MCP.
- Si Atlassian Rovo MCP no esta disponible, crear la configuracion local y reportar que la validacion remota queda pendiente.
- No inventar `accountId`. Si no se puede resolver el usuario conectado, dejar el issue sin asignar o pedir confirmacion y reportar WARN.

Configuracion esperada:

```yaml
jira:
  enabled: false
  provider: atlassian-rovo-mcp
  site: tresit.atlassian.net
  projectKey: VEX
  user:
    accountId: ""
    displayName: ""

mapping:
  spec: Epic
  userStory: Historia
  task: Tarea
  technicalTask: Sub-task

sync:
  updateExisting: true
  deleteRemoved: false

# Opcional. Si se omite, al iniciar implementacion se asume mode: current_user.
assignment:
  mode: current_user
  accountId: ""
  assignOnSync: false
  assignOnStart: true
```

Entrega siempre resumen PASS/WARN/FAIL y ruta del archivo creado o actualizado.

## 3itkit-jira-sync

Sincroniza SDD Kit con Jira mediante Atlassian Rovo MCP sin duplicar issues

Command: /jira-sync

Sincroniza un spec SDD con Jira usando Atlassian Rovo MCP como unica capa de integracion.

Sincronizacion incremental:

- `spec.md` crea o actualiza la epica.
- `plan.md` crea o actualiza historias bajo la epica.
- `tasks.md` crea o actualiza tareas bajo cada historia.
- `/speckit-implement` asigna automaticamente la tarea Jira al usuario conectado, la mueve a `In Progress` al iniciar y a `Done` al terminar con evidencia valida.

Flujo obligatorio:

1. Detectar el spec activo o usar el directorio indicado por el usuario.
2. Leer `spec.md`, `plan.md`, `tasks.md`, `.3it-arch-kit/jira.yaml` y `specs/<spec>/.jira-sync.yaml` si existe.
3. Si `jira.enabled` es `false`, detener con SKIPPED sin intentar Atlassian Rovo MCP.
4. Validar conexion Jira mediante Atlassian Rovo MCP.
5. Validar proyecto Jira, issue types y campos requeridos.
6. Mapear `Spec` a `Epic`, `User Story` a `Historia` y `Task` a `Tarea`.
7. Detectar etapa disponible o solicitada: spec, plan, tasks o full.
8. No asignar issues durante sincronizacion salvo solicitud explicita con `assignment.assignOnSync: true`.
9. Calcular delta entre SDD y Jira para esa etapa.
10. Crear solo issues faltantes de la etapa.
11. Actualizar issues existentes cuando cambie titulo, descripcion, parent o labels.
12. Guardar cada `jiraKey`, `source`, `parent` y `lastSyncedStage` en `.jira-sync.yaml`.
13. Entregar resumen con etapa, creados, actualizados, omitidos, errores y brechas.

Idempotencia:

- Si existe `jiraKey`, no crear otro issue.
- Si no existe `jiraKey`, buscar coincidencias por metadata SDD, titulo normalizado y labels antes de crear.
- Etiquetar issues con `sdd`, `3it-arch-kit` y el identificador SDD cuando Jira lo permita.
- No borrar ni cerrar issues automaticamente por ausencia en SDD sin autorizacion explicita.

Mapping default para 3IT/Coface:

```yaml
jira:
  enabled: false
  site: tresit.atlassian.net
  projectKey: VEX
  user:
    accountId: ""
    displayName: ""

mapping:
  spec: Epic
  userStory: Historia
  task: Tarea

# Opcional. Si se omite, al iniciar implementacion se asume mode: current_user.
assignment:
  mode: current_user
  accountId: ""
  assignOnSync: false
  assignOnStart: true
```

No inventes resultados de Jira. Si Atlassian Rovo MCP no esta disponible, informa el bloqueo y deja los pasos exactos para reintentar.

## 3itkit-jira-sdd-incremental-sync

Sincroniza Jira incrementalmente durante clarify, spec, plan, tasks e implement

Command: /jira-sync-spec, /jira-sync-plan, /jira-sync-tasks

Sincroniza Jira a medida que avanza el proceso SDD, sin esperar al final.

Mapping incremental:

- Clarify / analisis -> actualiza contexto de la epica.
- `spec.md` -> crea o actualiza la epica.
- `plan.md` -> crea o actualiza historias bajo la epica.
- `tasks.md` -> crea o actualiza tareas bajo cada historia.
- `/speckit-implement` -> asigna la tarea al usuario conectado y la mueve con `/jira-start-task` y `/jira-complete-task`.

Flujo obligatorio:

1. Leer `.3it-arch-kit/jira.yaml`.
2. Si `jira.enabled` es `false`, responder SKIPPED y no intentar Atlassian Rovo MCP.
3. Detectar la etapa SDD ejecutada: clarify, spec, plan, tasks o implement.
4. Leer solo los archivos disponibles; no inventar historias ni tareas antes de que existan.
5. Usar `.jira-sync.yaml` para preservar idempotencia por etapa.
6. No asignar backlog durante sincronizacion, salvo `assignment.assignOnSync: true` solicitado explicitamente.
7. En etapa spec, crear o actualizar solo la epica.
8. En etapa plan, crear o actualizar historias bajo la epica.
9. En etapa tasks, crear o actualizar tareas bajo cada historia sin assignee por defecto.
10. En etapa implement, resolver la tarea SDD a `jiraKey` y moverla automaticamente `To Do -> In Progress -> Done` segun evidencia.
11. Guardar o actualizar `.jira-sync.yaml` con `source`, `jiraKey`, `parent`, `status` y `lastSyncedStage`; guardar `assignee` solo cuando una tarea sea tomada para implementacion.

Estado recomendado:

```yaml
spec:
  source: spec.md
  jiraKey: VEX-100
  lastSyncedStage: spec

stories:
  US-001:
    source: plan.md
    jiraKey: VEX-101
    parent: VEX-100
    lastSyncedStage: plan

tasks:
  T001:
    source: tasks.md
    jiraKey: VEX-102
    parent: VEX-101
    status: To Do
    lastSyncedStage: tasks
```

Reglas:

- La sincronizacion debe ser incremental e idempotente.
- No duplicar issues si `.jira-sync.yaml` ya tiene `jiraKey`.
- No borrar ni cerrar issues por cambios en SDD sin confirmacion explicita.
- No mover tareas a `Done` sin evidencia valida de implementacion.
- No convertir Jira en fuente de verdad; SDD sigue gobernando requerimiento, plan y tareas.
- No inventar assignee: usar solo usuario conectado via MCP o `assignment.accountId` confirmado al iniciar implementacion.

## 3itkit-jira-implement-trace

Refleja en Jira el avance real de /speckit-implement usando To Do, In Progress y Done

Command: /jira-start-task, /jira-complete-task

Refleja en Jira el trabajo que realiza `/speckit-implement`.

Automatizacion:

- Si `jira.enabled` es `true`, `/speckit-implement` debe aplicar este skill automaticamente.
- Al comenzar una tarea SDD sincronizada, debe ejecutar el flujo de `/jira-start-task`.
- Al comenzar, debe asignar la tarea al usuario conectado via MCP cuando `assignment` no exista o `assignment.mode` sea `current_user`.
- Al terminar con evidencia valida, debe ejecutar el flujo de `/jira-complete-task`.
- Si falla una validacion, debe dejar la tarea en `In Progress` y comentar el bloqueo.

Flujo obligatorio:

1. Detectar la tarea SDD activa, por ejemplo `T004`, desde el comando del usuario o el contexto de implementacion.
2. Leer `.3it-arch-kit/jira.yaml` para obtener `jira.projectKey`, `workflow` e `implementation`.
3. Si `jira.enabled` es `false`, no intentar Jira y reportar trazabilidad SKIPPED.
4. Leer `specs/<spec>/.jira-sync.yaml` para resolver `T004 -> jiraKey`.
5. Si no existe `jiraKey`, pedir ejecutar `/jira-sync` antes de transicionar Jira.
6. Resolver assignee efectivo; si `assignment` no existe, usar usuario conectado via MCP.
7. Al iniciar implementacion, asignar el issue si corresponde y usar Atlassian Rovo MCP para moverlo a `workflow.inProgress`, default `In Progress`.
8. Agregar comentario de inicio si `implementation.commentOnTransition` es `true`.
9. Ejecutar o acompanar `/speckit-implement` sin cambiar la fuente de verdad SDD.
10. Al finalizar, recopilar evidencia: archivos modificados, tests, build, coverage, OWASP, validaciones 3it y errores.
11. Si la evidencia requerida pasa, mover el issue a `workflow.done`, default `Done`, y comentar el resumen.
12. Si alguna validacion falla o queda pendiente, mantener el issue en `In Progress` y comentar el bloqueo.

Reglas:

- No mover issues sin `jiraKey` trazable en `.jira-sync.yaml`.
- No intentar Jira cuando `jira.enabled` sea `false`.
- No inventar transiciones, estados, keys ni resultados.
- No mover a `Done` sin evidencia de validacion cuando `requireValidationBeforeDone` es `true`.
- No usar REST Jira directo; toda operacion remota debe ir por Atlassian Rovo MCP.
- Si Atlassian Rovo MCP no esta disponible, continuar la implementacion local si el usuario lo permite y reportar Jira como pendiente.
- Usar los nombres reales del tablero configurados en `.3it-arch-kit/jira.yaml.workflow`.

Configuracion recomendada:

```yaml
workflow:
  todo: To Do
  inProgress: In Progress
  done: Done

implementation:
  speckitCommand: /speckit-implement
  transitionOnStart: In Progress
  transitionOnComplete: Done
  requireValidationBeforeDone: true
  commentOnTransition: true
```

Salida esperada:

```text
Tarea SDD: T004
Jira: VEX-123
Estado inicial: To Do
Transicion inicio: In Progress
Implementacion: completada
Validaciones: PASS
Transicion cierre: Done
```
