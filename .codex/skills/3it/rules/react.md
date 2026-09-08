<!-- BEGIN 3it-arch-kit managed block -->
framework: react
target: codex
<!-- END 3it-arch-kit managed block -->

# React Architecture Rules

## ReactJS project structure

# reactjs-project-structure

## Objetivo
Organizar aplicaciones ReactJS por features de negocio y capas claras.

## Reglas obligatorias
- Usar `src/app` para router, providers, bootstrap y configuracion transversal.
- Usar `src/shared` para componentes, hooks, tipos, utilidades y estilos reutilizables.
- Usar `src/features/{feature}` para capacidades de negocio independientes.
- Dentro de cada feature separar `presentation`, `application`, `domain` e `infrastructure`.
- Evitar carpetas globales gigantes por tipo tecnico cuando el codigo pertenece a una feature.
- Mantener dependencias desde UI hacia casos de uso y puertos, no al reves.
- No importar infraestructura desde componentes salvo adaptadores ya encapsulados.

## Criterios de validacion
- Validar que nuevas pantallas entren en la feature correcta.
- Detectar dependencias cruzadas innecesarias entre features.
- Diferenciar estructura real del proyecto de una estructura ideal no implementada.

## ReactJS clean architecture

# reactjs-clean-architecture

## Objetivo
Separar presentacion, aplicacion, dominio e infraestructura sin acoplar React a reglas de negocio.

## Reglas obligatorias
- Mantener componentes enfocados en render, eventos de UI y composicion.
- Ubicar casos de uso en `application` cuando exista orquestacion de negocio.
- Mantener modelos y puertos de dominio sin dependencias de React, HTTP, router o librerias UI.
- Implementar clientes API, DTOs, mappers y storage en infraestructura.
- No mezclar validaciones de negocio complejas dentro de JSX.
- Proteger boundaries con imports claros y revisables.

## Criterios de validacion
- Revisar imports entre capas.
- Detectar llamadas HTTP directas desde componentes.
- Validar que la logica reusable sea testeable sin montar la UI completa.

## ReactJS SOLID

# reactjs-solid

## Objetivo
Aplicar SOLID en componentes, hooks, servicios y adaptadores ReactJS.

## Reglas obligatorias
- Dar una responsabilidad clara a cada componente, hook, servicio y mapper.
- Preferir composicion sobre herencia o componentes con demasiadas props.
- Depender de contratos estables y tipos explicitos para casos de uso y servicios.
- Extraer variantes repetidas a componentes base o configuraciones tipadas.
- Evitar componentes que controlen UI, data fetching, autorizacion y mapeo al mismo tiempo.

## Criterios de validacion
- Revisar complejidad, longitud, duplicacion y cambios por multiples motivos.
- Detectar props booleanas acumuladas que indiquen variantes mal modeladas.

## ReactJS DDD lite

# reactjs-ddd-lite

## Objetivo
Modelar el frontend alrededor del lenguaje de negocio sin sobreingenieria.

## Reglas obligatorias
- Nombrar features, modelos, casos de uso y eventos con lenguaje ubicuo del dominio.
- Evitar que nombres de endpoints definan la estructura interna del frontend.
- Separar modelos de dominio de DTOs externos.
- Mantener reglas de validacion de negocio en dominio o aplicacion cuando sean reutilizables.
- Modelar estados de negocio explicitamente: draft, pending, approved, rejected, cancelled, etc.

## Criterios de validacion
- Revisar consistencia de nombres entre UI, casos de uso y contratos.
- Detectar DTOs usados como modelos internos sin mapeo.

## ReactJS components

# reactjs-components

## Objetivo
Construir componentes mantenibles, typed, accesibles y faciles de probar.

## Reglas obligatorias
- Separar componentes de pagina, componentes de feature y componentes compartidos.
- Mantener props tipadas, pequenas y expresivas.
- Evitar efectos secundarios dentro del render.
- Exponer callbacks con nombres de intencion de negocio cuando aplique.
- No duplicar componentes compartidos sin revisar el design system existente.
- Manejar estados disabled, loading, invalid, empty y error cuando correspondan.

## Criterios de validacion
- Revisar componentes largos, props ambiguas y render condicional excesivo.
- Validar que los componentes compartidos no dependan de una feature concreta.

## Testabilidad funcional E2E

- Todo elemento interactivo critico debe tener locator accesible estable: role, label, text semantico o `data-testid`.
- Usar `data-testid` obligatorio cuando el control no tenga nombre accesible confiable o el texto cambie por i18n.
- Definir `data-testid` en botones principales, inputs, selects, filtros, tablas, paginacion, menus de acciones, modales, drawers, toasts y mensajes de error/exito.
- Mantener nombres estables por dominio y accion, por ejemplo `customer-create-button`, `customer-filter-input`, `customer-row-actions-menu`.
- No generar UI que solo pueda probarse con clases CSS, indices, XPath fragil o estructura interna del DOM.

## ReactJS hooks

# reactjs-hooks

## Objetivo
Usar hooks para encapsular comportamiento reusable sin esconder reglas criticas.

## Reglas obligatorias
- Cumplir Rules of Hooks y dependencias completas en `useEffect`, `useMemo` y `useCallback`.
- Separar hooks de UI, hooks de estado y hooks de integracion.
- Evitar hooks con demasiadas responsabilidades o retornos ambiguos.
- Cancelar o controlar efectos asincronos para evitar race conditions.
- No usar memoizacion como parche sin evidencia de rendimiento.

## Criterios de validacion
- Revisar dependencias faltantes en efectos.
- Detectar hooks que mezclen routing, HTTP, storage, notificaciones y negocio.

## ReactJS state management

# reactjs-state

## Objetivo
Diferenciar estado local, remoto, global y derivado con herramientas adecuadas.

## Reglas obligatorias
- Mantener estado local cerca del componente que lo usa.
- Usar TanStack Query, SWR o herramienta equivalente para server state si existe en el stack.
- Usar Redux, Zustand, Context u otro store solo para estado compartido real.
- Evitar duplicar server state en stores globales sin necesidad.
- Modelar loading, error, empty, stale, optimistic y retry de forma explicita.
- No persistir datos sensibles en storage inseguro.

## Criterios de validacion
- Revisar stores globales sobredimensionados.
- Detectar estados derivados guardados innecesariamente.

## ReactJS router

# reactjs-router

## Objetivo
Gestionar navegacion, layouts, guards y rutas sin acoplarlos a negocio interno.

## Reglas obligatorias
- Definir rutas por feature o modulo funcional.
- Proteger rutas privadas con guards o loaders tipados.
- Mantener layouts y providers de ruta separados de paginas de negocio.
- Usar lazy loading por rutas cuando el tamano o dominio lo justifique.
- Manejar rutas no encontradas, errores de carga y permisos insuficientes.
- No exponer informacion sensible en query params o path.

## Criterios de validacion
- Revisar rutas duplicadas, guards incompletos y redirects fragiles.
- Validar comportamiento al refrescar y entrar directo por URL.

## ReactJS API integration

# reactjs-api

## Objetivo
Integrar APIs mediante clientes controlados, tipados y observables.

## Reglas obligatorias
- Centralizar clientes HTTP, base URLs, timeouts, headers y manejo de errores.
- Usar RequestDTO y ResponseDTO para contratos externos.
- Mapear DTOs a modelos internos antes de llegar a dominio o UI.
- Incluir correlation ID o trace context si el ecosistema lo soporta.
- Manejar errores 400, 401, 403, 404, 409, 422, 429 y 5xx de forma consistente.
- No hardcodear endpoints, tokens ni secretos en componentes.

## Criterios de validacion
- Revisar clientes API, interceptores, retry y normalizacion de errores.
- Detectar contratos externos filtrados directamente a componentes.

## ReactJS DTO contracts

# reactjs-dto

## Objetivo
Aislar contratos externos de los modelos usados por la aplicacion.

## Reglas obligatorias
- Definir RequestDTO y ResponseDTO para entradas y salidas de APIs.
- Usar mappers para transformar DTOs hacia modelos de dominio o view models.
- No modificar DTOs para resolver necesidades visuales internas.
- Validar nullability, fechas, enums, dinero, paginacion y errores de contrato.
- Versionar o aislar cambios incompatibles de APIs.

## Criterios de validacion
- Revisar uso directo de ResponseDTO en JSX.
- Detectar casts inseguros o `any` en bordes de integracion.

## ReactJS forms

# reactjs-forms

## Objetivo
Crear formularios seguros, accesibles, robustos y orientados a tareas.

## Reglas obligatorias
- Validar input en cliente con schemas tipados cuando el stack lo permita.
- Mantener validacion servidor como fuente definitiva.
- Asociar labels, errores y ayudas con cada campo.
- Prevenir doble envio y preservar datos ante fallas.
- Mostrar errores inline y resumen general cuando el formulario sea complejo.
- Manejar estados dirty, touched, submitting, success y server error.
- No ocultar errores tecnicos que bloqueen al usuario sin orientacion accionable.

## Criterios de validacion
- Revisar accesibilidad, validacion, conversion de tipos y control de submit.
- Validar casos de error 400, 409, 422 y timeout.

## Testabilidad funcional E2E

- Todo elemento interactivo critico debe tener locator accesible estable: role, label, text semantico o `data-testid`.
- Usar `data-testid` obligatorio cuando el control no tenga nombre accesible confiable o el texto cambie por i18n.
- Definir `data-testid` en botones principales, inputs, selects, filtros, tablas, paginacion, menus de acciones, modales, drawers, toasts y mensajes de error/exito.
- Mantener nombres estables por dominio y accion, por ejemplo `customer-create-button`, `customer-filter-input`, `customer-row-actions-menu`.
- No generar UI que solo pueda probarse con clases CSS, indices, XPath fragil o estructura interna del DOM.

## ReactJS auth

# reactjs-auth

## Objetivo
Gestionar autenticacion y autorizacion en frontend sin exponer secretos.

## Reglas obligatorias
- Usar flujos OAuth2/OIDC establecidos por la organizacion cuando existan.
- No almacenar access tokens en localStorage salvo decision explicita y mitigada.
- Separar autenticacion, autorizacion y personalizacion visual.
- Validar permisos en rutas, acciones y componentes sensibles.
- Manejar expiracion, refresh, logout, session timeout y estados anonimos.
- No confiar en controles frontend como unica barrera de seguridad.

## Criterios de validacion
- Revisar storage de tokens, guards, permisos y fuga de informacion.
- Detectar secretos, scopes amplios o datos sensibles en cliente.

## ReactJS security

# reactjs-security

## Objetivo
Reducir riesgos de seguridad propios del frontend empresarial.

## Reglas obligatorias
- Evitar `dangerouslySetInnerHTML`; si es inevitable, sanitizar contenido.
- Validar entradas antes de usarlas en URLs, HTML, markdown o comandos de navegador.
- Configurar CSP, HSTS, X-Frame-Options/frame-ancestors y headers de seguridad desde el delivery cuando aplique.
- No exponer secretos, tokens, claves API privadas ni endpoints internos sensibles.
- Controlar dependencias vulnerables y licencias no permitidas.
- Evitar logs de PII, tokens o payloads sensibles.

## Criterios de validacion
- Revisar XSS, inyeccion en URL, leakage por logs y dependencias vulnerables.
- Validar headers esperados en despliegue cuando exista evidencia.

## ReactJS OWASP

# reactjs-owasp

## Objetivo
Aplicar OWASP Top 10, OWASP API Security y controles de SPA.

## Reglas obligatorias
- Revisar broken access control en rutas y acciones.
- Revisar cryptographic failures por almacenamiento inseguro de datos sensibles.
- Revisar injection en HTML, URL, query params, markdown y plantillas.
- Revisar insecure design en flujos de negocio expuestos al cliente.
- Revisar security misconfiguration en build, assets y headers.
- Revisar SSRF indirecto por URLs controladas por usuario si el frontend llama proxies.

## Criterios de validacion
- Clasificar hallazgos con severidad y evidencia.
- No declarar cumplimiento OWASP sin revisar controles concretos.

## ReactJS API security

# reactjs-api-security

## Objetivo
Consumir APIs de forma segura desde ReactJS.

## Reglas obligatorias
- Enviar credenciales solo a origenes permitidos.
- Controlar CORS desde backend, no asumir que el frontend lo resuelve.
- Manejar 401 y 403 sin revelar informacion sensible.
- Evitar exponer IDs, scopes o roles que faciliten enumeracion.
- Aplicar rate limit UX para acciones repetitivas cuando el backend lo indique.
- No reintentar automaticamente operaciones no idempotentes sin control.

## Criterios de validacion
- Revisar interceptores, manejo de errores y politica de origenes.
- Detectar exposicion de datos en mensajes de error.

## ReactJS accessibility

# reactjs-accessibility

## Objetivo
Cumplir WCAG AA en interfaces ReactJS y flujos interactivos.

## Reglas obligatorias
- Usar semantica HTML correcta para navegacion, formularios, tablas y acciones.
- Todo icon button debe tener `aria-label` o texto visible equivalente.
- Mantener foco visible, orden de tabulacion logico y manejo de foco en modales.
- Asociar labels, descriptions y errores con inputs mediante `id`, `htmlFor` y `aria-describedby`.
- No usar ARIA para compensar HTML incorrecto salvo casos necesarios.
- Validar contraste, tamanos tactiles y contenido que no dependa solo del color.
- Anunciar actualizaciones asincronas relevantes con `aria-live` cuando aplique.
- Evitar traps de teclado y componentes no operables por teclado.

## Criterios de validacion
- Revisar componentes interactivos, routing, formularios y overlays.
- Detectar divs clickeables sin rol, tabIndex y handlers de teclado.
- Validar que errores y loading sean perceptibles para lectores de pantalla.

## Testabilidad funcional E2E

- Todo elemento interactivo critico debe tener locator accesible estable: role, label, text semantico o `data-testid`.
- Usar `data-testid` obligatorio cuando el control no tenga nombre accesible confiable o el texto cambie por i18n.
- Definir `data-testid` en botones principales, inputs, selects, filtros, tablas, paginacion, menus de acciones, modales, drawers, toasts y mensajes de error/exito.
- Mantener nombres estables por dominio y accion, por ejemplo `customer-create-button`, `customer-filter-input`, `customer-row-actions-menu`.
- No generar UI que solo pueda probarse con clases CSS, indices, XPath fragil o estructura interna del DOM.

## ReactJS UI UX

# reactjs-ui-ux

## Objetivo
Generar interfaces ReactJS orientadas a producto, con flujos claros y estados completos.

## Reglas obligatorias
- Definir loading, empty, error, success, disabled, optimistic y skeleton cuando el flujo lo requiera.
- Disenar responsive con layouts fluidos y breakpoints explicitos.
- Mantener componentes de presentacion separados de hooks, casos de uso y acceso a APIs.
- Usar accion primaria clara y limitar acciones competidoras en la misma zona visual.
- Evitar saltos de layout en carga, paginacion, filtros o validacion.
- Validar formularios con feedback inline y prevencion de doble envio.
- Preservar datos del usuario ante errores de validacion o fallas transitorias.
- Mantener copy de interfaz breve, consistente y orientado a tareas.

## Criterios de validacion
- Verificar estados de datos, errores y permisos en cada pantalla.
- Revisar que los componentes no mezclen logica de negocio con render.
- Validar responsive y overflow en tablas, cards y formularios.

## Testabilidad funcional E2E

- Todo elemento interactivo critico debe tener locator accesible estable: role, label, text semantico o `data-testid`.
- Usar `data-testid` obligatorio cuando el control no tenga nombre accesible confiable o el texto cambie por i18n.
- Definir `data-testid` en botones principales, inputs, selects, filtros, tablas, paginacion, menus de acciones, modales, drawers, toasts y mensajes de error/exito.
- Mantener nombres estables por dominio y accion, por ejemplo `customer-create-button`, `customer-filter-input`, `customer-row-actions-menu`.
- No generar UI que solo pueda probarse con clases CSS, indices, XPath fragil o estructura interna del DOM.

## ReactJS design system

# reactjs-design-system

## Objetivo
Alinear componentes ReactJS con el design system y los tokens existentes.

## Reglas obligatorias
- Usar componentes base existentes antes de crear nuevos.
- Consumir tokens de color, spacing, tipografia, radios y elevacion desde el tema.
- Definir variantes de componentes para intencion, tamano, densidad y estado.
- No duplicar estilos entre componentes si existe patron reutilizable.
- Asegurar compatibilidad con theme provider, dark mode o CSS variables si existen.
- Mantener APIs de componentes pequenas, typed y expresivas.
- Evitar estilos inline salvo valores dinamicos justificados.

## Criterios de validacion
- Revisar consistencia de botones, inputs, selects, modales, tablas y navegacion.
- Detectar magic numbers, colores hardcodeados y variantes incompletas.
- Validar estados hover, focus, active, disabled, invalid y loading.

## ReactJS styling

# reactjs-css

## Objetivo
Mantener estilos escalables, consistentes y compatibles con el stack real.

## Reglas obligatorias
- Usar la solucion existente: CSS Modules, Tailwind, styled-components, Emotion, vanilla-extract u otra.
- Evitar mezclar multiples estrategias de estilos sin convencion clara.
- No hardcodear colores o spacing cuando existan tokens.
- Mantener estilos responsive cerca del componente o capa definida por el proyecto.
- Evitar selectores globales fragiles y especificidad excesiva.
- Considerar dark mode, density y forced colors si el producto lo requiere.

## Criterios de validacion
- Revisar duplicacion, estilos globales, tokens y consistencia visual.
- Detectar clases dinamicas no soportadas por el build.

## ReactJS TypeScript

# reactjs-typescript

## Objetivo
Usar TypeScript como contrato de mantenibilidad y seguridad de cambio.

## Reglas obligatorias
- Evitar `any`; si es inevitable, encapsularlo en bordes de integracion y justificarlo.
- Usar tipos explicitos para DTOs, modelos, props, eventos y casos de uso.
- Activar modo estricto cuando el proyecto lo soporte.
- No usar casts para ocultar errores de contrato.
- Preferir discriminated unions para estados complejos.
- Tipar errores conocidos, resultados asincronos y estados de carga.

## Criterios de validacion
- Revisar `any`, `unknown`, casts, nullability y tipos duplicados.
- Detectar tipos generados o externos usados sin adaptacion.

## ReactJS performance

# reactjs-performance

## Objetivo
Optimizar experiencia y costo de render sin complejidad innecesaria.

## Reglas obligatorias
- Aplicar lazy loading y code splitting en rutas o modulos pesados.
- Medir antes de introducir memoizacion compleja.
- Evitar renders innecesarios por props inestables, stores amplios o contextos sobredimensionados.
- Optimizar listas largas con paginacion, virtualizacion o carga incremental.
- Controlar tamanos de bundle, assets, imagenes y fuentes.
- Mantener Core Web Vitals dentro de umbrales definidos por el producto.

## Criterios de validacion
- Revisar bundle, render profiling, listas, imagenes y waterfalls de red.
- Detectar memoizacion sin beneficio verificable.

## ReactJS code splitting

# reactjs-code-splitting

## Objetivo
Reducir carga inicial y aislar features grandes.

## Reglas obligatorias
- Usar dynamic import o lazy loading en rutas, modulos administrativos y pantallas pesadas.
- Mantener fallback de carga accesible y visualmente estable.
- Evitar cargar librerias grandes en el bundle inicial si solo se usan en una pantalla.
- Revisar chunks duplicados o dependencias compartidas mal ubicadas.
- No dividir codigo en exceso si empeora caching o mantenibilidad.

## Criterios de validacion
- Revisar configuracion de Vite/Webpack/Next segun stack real.
- Validar experiencia en conexiones lentas.

## ReactJS bundling

# reactjs-bundling

## Objetivo
Mantener builds reproducibles, seguros y optimizados.

## Reglas obligatorias
- Usar configuracion versionada para Vite, Webpack, Next.js o herramienta real.
- Diferenciar variables publicas de secretos privados.
- Validar source maps segun ambiente y politica de seguridad.
- Controlar polyfills, transpilation targets y browserslist.
- Ejecutar typecheck, lint, tests y build antes de merge.
- No incluir datos de ambiente productivo en assets generados.

## Criterios de validacion
- Revisar scripts, configuracion de build y variables expuestas.
- Detectar bundles con dependencias innecesarias.

## ReactJS SSR

# reactjs-ssr

## Objetivo
Usar SSR, SSG o hydration correctamente cuando el stack sea Next.js, Remix o equivalente.

## Reglas obligatorias
- Separar codigo server-only y client-only.
- No acceder a `window`, `document` o storage durante render server.
- Proteger secretos en server components, loaders o actions.
- Manejar cache, revalidation y headers por ruta.
- Evitar hydration mismatch por fechas, random, locale o datos no deterministas.
- Validar errores y loading en boundaries del framework.

## Criterios de validacion
- Revisar rutas SSR/SSG, data fetching, cache y exposicion de secretos.
- Omitir recomendaciones SSR si el proyecto es SPA puro.

## ReactJS SEO

# reactjs-seo

## Objetivo
Asegurar descubrimiento, previews y metadata cuando el producto lo requiere.

## Reglas obligatorias
- Configurar title, description, canonical y Open Graph por pagina publica.
- Mantener contenido indexable en paginas que dependan de SEO.
- Usar sitemap y robots cuando aplique.
- Validar status codes en SSR/BFF para 404, redirects y errores.
- No indexar pantallas privadas o datos sensibles.

## Criterios de validacion
- Revisar metadata, canonical, robots y contenido renderizado.
- Diferenciar aplicaciones internas donde SEO no aplica.

## ReactJS PWA

# reactjs-pwa

## Objetivo
Implementar capacidades PWA solo cuando aporten valor real al producto.

## Reglas obligatorias
- Definir manifest, service worker y estrategia de cache si existe modo offline.
- No cachear informacion sensible sin cifrado o politica explicita.
- Manejar actualizaciones de version y limpieza de caches.
- Validar fallback offline y recuperacion de conexion.
- No activar service worker accidentalmente en ambientes no preparados.

## Criterios de validacion
- Revisar cache, versionado, seguridad y comportamiento offline.
- Omitir PWA si el proyecto no declara ese alcance.

## ReactJS i18n

# reactjs-i18n

## Objetivo
Preparar interfaces para idioma, region, moneda, fechas y accesibilidad cultural.

## Reglas obligatorias
- Externalizar textos visibles cuando el producto sea multiidioma.
- Formatear fechas, numeros, monedas y pluralizacion con APIs o librerias adecuadas.
- Evitar concatenar strings traducibles en componentes.
- Soportar cambios de locale sin romper layout.
- Validar direccionalidad RTL si el alcance lo requiere.

## Criterios de validacion
- Revisar textos hardcodeados, formatos locales y overflow por traducciones.
- Diferenciar apps monolingues de productos multi-region.

## ReactJS observability

# reactjs-observability

## Objetivo
Dar visibilidad a errores, experiencia de usuario y trazabilidad frontend-backend.

## Reglas obligatorias
- Centralizar captura de errores de UI, promesas rechazadas y fallas de red.
- Incluir correlation ID o trace context en llamadas cuando el backend lo soporte.
- Registrar eventos de negocio sin PII ni secretos.
- Medir Web Vitals, tiempos de navegacion y fallas criticas si aplica.
- Configurar error boundaries en zonas de riesgo.
- No usar `console.log` como mecanismo operativo.

## Criterios de validacion
- Revisar logging, tracing, error reporting y sanitizacion de datos.
- Validar que errores visibles tengan seguimiento tecnico posible.

## ReactJS storage

# reactjs-storage

## Objetivo
Usar almacenamiento local del navegador con criterio de seguridad y ciclo de vida.

## Reglas obligatorias
- No guardar secretos, tokens sensibles o PII en localStorage/sessionStorage sin aprobacion explicita.
- Definir expiracion, versionado y migracion para datos persistidos.
- Encapsular acceso a storage en adaptadores.
- Manejar ausencia de storage, modo privado y errores de cuota.
- Limpiar datos al logout o cambio de usuario cuando corresponda.

## Criterios de validacion
- Revisar claves almacenadas, datos sensibles, expiracion y limpieza.
- Detectar acoplamiento directo de componentes a storage.

## ReactJS dependencies

# reactjs-dependencies

## Objetivo
Controlar dependencias para seguridad, peso de bundle y mantenibilidad.

## Reglas obligatorias
- Preferir librerias ya aprobadas por el proyecto u organizacion.
- Evaluar mantenimiento, licencias, vulnerabilidades y tamano antes de agregar dependencias.
- No agregar librerias para problemas simples resueltos por el stack existente.
- Mantener lockfile versionado y builds reproducibles.
- Revisar dependencias transitivas criticas y advisories.

## Criterios de validacion
- Revisar `package.json`, lockfile, bundle impact y vulnerabilidades.
- Detectar duplicidad de librerias con funciones equivalentes.

## ReactJS testing

# reactjs-testing

## Objetivo
Cubrir comportamiento relevante con pruebas unitarias, componentes, integracion y E2E segun riesgo.

## Reglas obligatorias
- Probar casos de uso, mappers, servicios, hooks y componentes criticos.
- Usar React Testing Library para comportamiento visible cuando exista en el stack.
- Mockear bordes externos, no la logica que se quiere validar.
- Cubrir loading, error, empty, success, permisos y validaciones.
- Agregar E2E para flujos criticos de negocio cuando el producto lo requiera.
- Mantener pruebas deterministas y sin depender de orden global.

## Criterios de validacion
- Revisar gaps por riesgo, flakiness, mocks excesivos y pruebas acopladas a implementacion.
- Validar que scripts de test corran en CI.

## Testabilidad funcional E2E

- Todo elemento interactivo critico debe tener locator accesible estable: role, label, text semantico o `data-testid`.
- Usar `data-testid` obligatorio cuando el control no tenga nombre accesible confiable o el texto cambie por i18n.
- Definir `data-testid` en botones principales, inputs, selects, filtros, tablas, paginacion, menus de acciones, modales, drawers, toasts y mensajes de error/exito.
- Mantener nombres estables por dominio y accion, por ejemplo `customer-create-button`, `customer-filter-input`, `customer-row-actions-menu`.
- No generar UI que solo pueda probarse con clases CSS, indices, XPath fragil o estructura interna del DOM.

## ReactJS coverage sonar

# reactjs-coverage-sonar

## Objetivo
Garantizar pruebas automatizadas suficientes, coverage empresarial >= 90% y Quality Gate SonarQube/SonarCloud bloqueante.

## Reglas obligatorias
- Mantener coverage >= 90% global.
- Exigir statements, branches, functions y lines >= 90% cuando la herramienta lo permita.
- Generar `lcov.info` sin depender de pasos manuales.
- Configurar `sonar.javascript.lcov.reportPaths` apuntando al reporte real.
- Bloquear merge/deploy ante fallas de Quality Gate, bugs, vulnerabilidades, security hotspots criticos o code smells severos.
- Cubrir componentes, hooks, stores, clientes API, DTOs, mappers, routing, providers y paginas por feature.
- No bajar umbrales de coverage para aprobar una entrega.

## Criterios de validacion
- Identificar gestor real: npm, yarn o pnpm.
- Identificar stack real de pruebas: Vitest/Jest + React Testing Library cuando exista.
- Revisar `package.json`, configs de test, `sonar-project.properties` y pipelines.
- Diferenciar defectos comprobados de recomendaciones.
- No inventar dependencias, comandos, archivos ni metricas no presentes.

## ReactJS CI CD

# reactjs-ci-cd

## Objetivo
Asegurar calidad continua antes de merge y despliegue.

## Reglas obligatorias
- Ejecutar install reproducible, lint, typecheck, tests, coverage, build y Sonar en CI.
- Usar lockfile y cache de dependencias controlada.
- Separar ambientes dev, qa, staging y prod mediante configuracion segura.
- Bloquear despliegue si falla Quality Gate o pruebas criticas.
- Publicar artefactos estaticos versionados y trazables.
- No desplegar con variables o source maps inseguros sin politica definida.

## Criterios de validacion
- Revisar workflows, scripts, artifacts, Quality Gate y protecciones de rama.
- Detectar pasos manuales no documentados.

## ReactJS code review

# reactjs-code-review

## Objetivo
Guiar revisiones tecnicas enterprise para cambios ReactJS.

## Reglas obligatorias
- Priorizar bugs, riesgos de seguridad, regresiones, accesibilidad y falta de tests.
- Validar arquitectura, limites de capas, dependencias, UX, rendimiento y mantenibilidad.
- Exigir evidencia: archivo, comportamiento, riesgo y recomendacion concreta.
- Diferenciar hallazgos bloqueantes de mejoras.
- No aprobar cambios con coverage insuficiente o Quality Gate fallido.

## Criterios de validacion
- Entregar hallazgos ordenados por severidad.
- Evitar observaciones cosmeticas si hay riesgos funcionales o de seguridad.

## ReactJS governance

# reactjs-governance

## Objetivo
Mantener gobierno arquitectonico, consistencia y evolucion controlada.

## Reglas obligatorias
- Documentar decisiones relevantes en ADR o README tecnico cuando aplique.
- Mantener convenciones de carpetas, nombres, testing, UI y seguridad.
- No introducir frameworks, librerias UI o stores nuevos sin justificacion.
- Revisar deuda tecnica, duplicacion, obsolescencia y compatibilidad de versiones.
- Mantener alineacion con API Gateway/BFF y contratos backend.

## Criterios de validacion
- Revisar cambios que alteren arquitectura, tooling o dependencias base.
- Detectar desviaciones sin decision documentada.

## ReactJS Playwright E2E

# reactjs-playwright-e2e

## Objetivo

Estandarizar pruebas funcionales E2E con Playwright para aplicaciones ReactJS, generando evidencia confiable para CI/CD y asegurando que la UI sea testeable desde su diseno.

## Reglas obligatorias

- Usar Playwright para pruebas funcionales E2E web en proyectos ReactJS.
- Mantener tests en `e2e/` o `tests/e2e/`, organizados por feature y flujo de negocio.
- Configurar `playwright.config.ts` con `baseURL`, retries controlados, browsers requeridos, reporter HTML/JUnit y `trace: 'on-first-retry'` en CI.
- Ejecutar Chromium como minimo; Firefox y WebKit son obligatorios cuando el producto declare soporte cross-browser.
- Probar flujos criticos: login, navegacion, listado, filtros, paginacion, crear, editar, eliminar, permisos, errores de API, estados vacios y validaciones de formulario.
- Preferir locators accesibles: `getByRole`, `getByLabel`, `getByText` estable y visible para usuario.
- Usar `data-testid` estable cuando no exista locator accesible confiable o cuando i18n haga variable el texto.
- No usar selectores fragiles: clases CSS decorativas, indices, XPath innecesario o estructura interna del DOM.
- Preparar datos de prueba mediante API, fixtures o seed reproducible; no depender de datos manuales del ambiente.
- Los tests deben ser independientes, idempotentes y ejecutables en cualquier orden.
- No guardar usuarios, passwords, tokens ni secretos reales en tests, fixtures, traces, videos o screenshots.
- Publicar `playwright-report/` y `test-results/` como evidencia ante fallos.
- En CI usar instalacion reproducible de dependencias y browsers, por ejemplo `npx playwright install --with-deps`.

## Estructura recomendada

```text
e2e/
  fixtures/
  pages/
  specs/
    auth.spec.ts
    customer-crud.spec.ts
  support/
playwright.config.ts
```

## Data-testid enterprise

- Convencion: `<feature>-<element>-<action|state>` o `<feature>-<workflow>-<element>`.
- Ejemplos: `customer-create-button`, `customer-name-input`, `customer-filter-input`, `customer-page-size-select`, `customer-row-actions-menu`, `customer-delete-confirm-button`.
- Aplicar en `*.tsx`, `*.jsx`, `*.ts`, `*.js` cuando el elemento sea parte de un flujo E2E critico.
- No usar IDs generados aleatoriamente, textos temporales ni clases visuales como contrato de prueba.

## Comandos esperados

```json
{
  "test:e2e": "playwright test",
  "test:e2e:ui": "playwright test --ui",
  "test:e2e:report": "playwright show-report"
}
```

## Criterios de validacion

- Existe configuracion Playwright coherente con el stack real: React Testing Library, MSW, Vite/Next.js segun stack real.
- Los flujos criticos tienen specs ejecutables y legibles.
- Los locators son accesibles o usan `data-testid` estable.
- Hay estrategia clara de datos, setup y limpieza.
- El pipeline CI/CD puede instalar browsers, ejecutar E2E y publicar evidencia.
- No inventar pantallas, rutas, credenciales, datos ni dependencias no presentes sin confirmacion.

## Jira Sync Atlassian Rovo MCP

# Jira Sync Atlassian Rovo MCP

## Decisiones de arquitectura

- Mantener SDD Kit como fuente funcional y tecnica de verdad: `spec.md`, `plan.md` y `tasks.md`.
- Usar Jira como control operacional del proyecto: responsables, estados, sprint, fechas, bloqueos y metricas.
- Usar Git como evidencia de implementacion: branch, commits, PR y CI/CD.
- Usar exclusivamente Atlassian Rovo MCP para leer, buscar, crear, actualizar, comentar o transicionar issues Jira.
- No implementar cliente REST Jira propio dentro del framework salvo que el usuario lo solicite explicitamente.
- Configurar Jira con `/jira-config` antes de ejecutar `/jira-sync` por primera vez.
- No pedir ni guardar credenciales Jira en el repositorio; la autenticacion vive en Atlassian Rovo MCP.
- No intentar operaciones Jira si `jira.enabled` es `false`.
- Tratar `assignment` como configuracion opcional para implementacion; si no existe, el default efectivo al iniciar una tarea es `assignment.mode: current_user`.
- No asignar tareas al crear backlog con `/jira-sync-tasks`; asignar solo cuando el usuario comienza la implementacion con `/jira-start-task` o `/speckit-implement`.

## Mapping SDD a Jira

- `Spec` -> `Epic`.
- `User Story` -> `Historia`.
- `Task` -> `Tarea`.
- `Technical Task` -> `Sub-task` solo si el proyecto Jira lo soporta y el usuario lo solicita.
- `spec.md` gobierna la epica.
- `plan.md` gobierna las historias.
- `tasks.md` gobierna las tareas.

## Configuracion esperada

Proyecto:

```yaml
# .3it-arch-kit/jira.yaml
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

sdd:
  incremental: true
  syncOnClarify: true
  syncOnSpec: true
  syncOnPlan: true
  syncOnTasks: true
  syncOnImplement: true
  stageMapping:
    clarify: epic-context
    spec: Epic
    plan: Historia
    tasks: Tarea
    implement: workflow-transition

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

Estado idempotente por spec:

```yaml
# specs/001-feature/.jira-sync.yaml
version: 1

spec:
  sddId: SPEC-001
  jiraKey: VEX-100

stories:
  US-001:
    jiraKey: VEX-101

tasks:
  T001:
    jiraKey: VEX-102
```

## Reglas obligatorias

- Antes de crear issues, leer `spec.md`, `plan.md`, `tasks.md`, `.3it-arch-kit/jira.yaml` y `.jira-sync.yaml` si existe.
- Si `.3it-arch-kit/jira.yaml` no existe o esta incompleto, ejecutar primero `/jira-config`.
- Si `jira.enabled` es `false`, detener la operacion Jira con estado SKIPPED y continuar solo el trabajo local permitido.
- Sincronizar incrementalmente cada etapa SDD cuando `sdd.incremental` este activo.
- Despues de clarify o analisis, actualizar el contexto de la epica desde el requerimiento refinado.
- Despues de crear o ajustar `spec.md`, crear o actualizar la epica.
- Despues de crear o ajustar `plan.md`, crear o actualizar historias bajo la epica.
- Despues de crear o ajustar `tasks.md`, crear o actualizar tareas bajo la historia correspondiente.
- Consultar Jira con Atlassian Rovo MCP para verificar proyecto, tipos de issue, campos requeridos y existencia de keys.
- Crear solo elementos faltantes; actualizar elementos existentes cuando cambie el contenido SDD.
- Durante `/jira-sync`, `/jira-sync-spec`, `/jira-sync-plan` y `/jira-sync-tasks`, no enviar `assignee` por defecto.
- Solo asignar durante sincronizacion si existe `assignment.assignOnSync: true` y el usuario lo solicito explicitamente.
- Si `assignment` no existe, asumir `assignment.assignOnSync: false` y `assignment.assignOnStart: true`.
- Si `assignment.mode` es `current_user`, resolver el usuario conectado con Atlassian Rovo MCP al iniciar implementacion.
- Si `assignment.mode` es `explicit_account_id`, usar `assignment.accountId` como `assignee` al iniciar implementacion.
- Si `assignment.mode` es `unassigned`, no enviar `assignee`.
- Nunca duplicar un issue que ya tenga `jiraKey` en `.jira-sync.yaml`.
- Si existe `jiraKey`, validar que el issue existe antes de actualizar.
- Si un issue fue eliminado o no es accesible, reportar la brecha y pedir confirmacion antes de recrearlo.
- Guardar toda key creada o encontrada en `.jira-sync.yaml`.
- No borrar issues Jira porque desaparezcan del SDD; marcar como brecha o cambio pendiente salvo solicitud explicita.
- Mantener trazabilidad en branch, commits y PR usando la key Jira correspondiente cuando se implemente una tarea.
- Cuando `/speckit-implement` inicie una tarea con `jiraKey`, ejecutar automaticamente el flujo de `/jira-start-task` y transicionar el issue desde `To Do` a `In Progress`.
- Cuando `/speckit-implement` termine con evidencia valida, ejecutar automaticamente el flujo de `/jira-complete-task` y transicionar el issue desde `In Progress` a `Done`.
- No mover a `Done` si pruebas, build, coverage, OWASP o validaciones requeridas fallan o no se pudieron ejecutar.
- Si el tablero usa nombres distintos, leer `workflow` en `.3it-arch-kit/jira.yaml` antes de transicionar.

## Criterios de validacion

- El mapeo usa los tipos reales del proyecto Jira, por ejemplo `Epic`, `Historia` y `Tarea`.
- El flujo es idempotente: ejecutar dos veces no crea duplicados.
- La asignacion es trazable y ocurre al tomar trabajo: sin `assignment`, `/jira-start-task` equivale a `current_user`.
- SDD conserva el contenido funcional; Jira no reemplaza `spec.md`, `plan.md` ni `tasks.md`.
- Las operaciones Jira se hacen por Atlassian Rovo MCP y respetan permisos OAuth del usuario.
- El resultado informa creados, actualizados, omitidos, errores y brechas de trazabilidad.
