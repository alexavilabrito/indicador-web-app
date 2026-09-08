# Constitución de la Plataforma Web de Indicadores Financieros de Chile

<!--
Archivo objetivo para GitHub Spec Kit: .specify/memory/constitution.md
La constitución establece reglas duraderas. Los detalles funcionales se desarrollan
posteriormente mediante /speckit.specify y los detalles técnicos mediante /speckit.plan.
-->

## Preámbulo

Esta constitución gobierna el diseño, especificación, construcción, prueba y evolución de una aplicación web responsiva que consulta `mindicador.cl`, presenta indicadores económicos chilenos, permite explorar y comparar series históricas, y convierte valores entre unidades compatibles.

El producto es informativo y analítico. No es una plataforma de trading, no procesa pagos ni ejecuta operaciones financieras. Toda funcionalidad, decisión de diseño e implementación DEBE respetar los principios que siguen.

## Principios fundamentales

### I. Fidelidad, procedencia y transparencia del dato

- Todo valor visible DEBE conservar el indicador, valor, unidad, fecha efectiva y fuente entregados por la API.
- La interfaz DEBE mostrar la fecha y hora de última actualización y distinguir claramente entre fecha de consulta y fecha efectiva del dato.
- Los datos obtenidos directamente, invertidos, cruzados, normalizados o derivados DEBEN etiquetarse de forma inequívoca.
- La aplicación NO DEBE presentar datos estimados, interpolados o faltantes como si hubieran sido informados por la fuente.
- Los valores históricos NO DEBEN rellenarse silenciosamente en fines de semana, feriados ni períodos sin observación.
- Cada vista de detalle DEBE identificar `mindicador.cl` como fuente y advertir que la información es referencial, no una recomendación financiera ni una cotización transable.

**Razón:** en un producto financiero, la confianza depende de que el usuario pueda entender de dónde proviene cada cifra y qué representa.

### II. Cobertura completa y explícita de la API

La solución DEBE soportar los doce indicadores publicados por la API:

| Código | Indicador | Unidad informada |
| --- | --- | --- |
| `uf` | Unidad de Fomento | Pesos |
| `ivp` | Índice de Valor Promedio | Pesos |
| `dolar` | Dólar observado | Pesos |
| `dolar_intercambio` | Dólar acuerdo | Pesos |
| `euro` | Euro | Pesos |
| `ipc` | Índice de Precios al Consumidor | Porcentaje |
| `utm` | Unidad Tributaria Mensual | Pesos |
| `imacec` | IMACEC | Porcentaje |
| `tpm` | Tasa de Política Monetaria | Porcentaje |
| `libra_cobre` | Libra de cobre | Dólar |
| `tasa_desempleo` | Tasa de desempleo | Porcentaje |
| `bitcoin` | Bitcoin | Dólar |

La integración DEBE cubrir los cuatro patrones públicos de consulta:

1. último valor de todos los indicadores: `/api`;
2. serie reciente por indicador: `/api/{indicador}`;
3. valor por indicador y fecha: `/api/{indicador}/{dd-mm-yyyy}`;
4. serie por indicador y año: `/api/{indicador}/{yyyy}`.

Los contratos, ejemplos de payload, validaciones y manejo de errores DEBEN documentarse por feature en `specs/<feature>/contracts/`; no se duplican en esta constitución.

### III. Semántica financiera correcta

- Los cálculos monetarios DEBEN utilizar aritmética decimal, nunca punto flotante binario como fuente de verdad.
- El redondeo DEBE ocurrir únicamente para presentación o cuando una regla de negocio documentada lo exija.
- La precisión interna, la precisión visible y el modo de redondeo DEBEN quedar definidos y probados.
- Los porcentajes (`ipc`, `imacec`, `tpm`, `tasa_desempleo`) NO DEBEN tratarse como monedas ni habilitarse en el conversor.
- La UF, UTM e IVP DEBEN presentarse como unidades de cuenta/reajustabilidad, no como monedas de libre transacción.
- Libra de cobre y Bitcoin DEBEN identificarse como commodity y criptoactivo respectivamente; su unidad base informada por la API es USD.
- Las conversiones DEBEN incluir fórmula, tasa utilizada, fecha efectiva y sentido de la cotización.

### IV. Conversión verificable y limitada por la fuente

El conversor DEBE soportar, como mínimo:

- CLP ↔ USD observado;
- CLP ↔ EUR;
- CLP ↔ UF, UTM e IVP;
- USD ↔ EUR mediante tasa cruzada con CLP;
- USD ↔ libra de cobre y USD ↔ BTC cuando la unidad y naturaleza del indicador permitan una conversión informativa;
- inversión de origen y destino sin perder precisión;
- cálculo con valor actual o con una fecha histórica disponible.

Toda tasa cruzada DEBE mostrar la ruta de cálculo. Por ejemplo: `EUR/USD = (CLP por EUR) / (CLP por USD)`. Si las observaciones no comparten una fecha compatible, la conversión DEBE bloquearse o advertir explícitamente la diferencia; jamás debe mezclar fechas de forma silenciosa.

La aplicación NO DEBE denominar estas tasas como “precio de compra”, “precio de venta”, “spot”, “tiempo real” o “mid-market”, porque la API no entrega bid/ask ni un feed transable en tiempo real.

### V. Análisis de series reproducible y honesto

Cada indicador DEBE contar con una vista histórica que permita seleccionar período, consultar valores exactos y visualizar como mínimo:

- último valor y variación absoluta y porcentual respecto de la observación anterior;
- mínimo, máximo, media y mediana del período;
- primera y última observación, cambio acumulado y cantidad de observaciones;
- gráfico de línea con tooltip de fecha, valor y unidad;
- tabla accesible equivalente al gráfico;
- exportación de los datos visibles en CSV con fuente, rango y fecha de generación.

Las comparaciones entre series con unidades o escalas distintas DEBEN usar una vista normalizada, base 100 desde la primera observación común, o escalas separadas claramente rotuladas. La interfaz DEBE explicar el método aplicado.

Los indicadores técnicos opcionales —por ejemplo media móvil, volatilidad o variación móvil— DEBEN estar definidos matemáticamente, disponer de pruebas con resultados conocidos y diferenciarse de los datos originales. No se emitirán señales automáticas de compra o venta.

### VI. Experiencia progresiva, responsiva y accesible

- La aplicación DEBE ser mobile-first y funcionar en anchos de 320 px en adelante, además de tablet y escritorio.
- El dashboard DEBE priorizar resumen, búsqueda y acceso rápido; el detalle DEBE concentrar gráfico, estadísticas y tabla sin sobrecargar la portada.
- El conversor DEBE permitir ingresar monto, buscar unidades, intercambiar origen/destino y entender el resultado con una sola mirada.
- Los rangos temporales frecuentes DEBEN estar disponibles como accesos rápidos, junto con selección personalizada cuando la fuente lo permita.
- Color, iconos y texto DEBEN comunicar conjuntamente alzas, bajas, estados y errores; el color nunca será el único canal.
- Toda funcionalidad esencial DEBE operar con teclado, foco visible, lectores de pantalla y zoom al 200 %, conforme a WCAG 2.2 nivel AA.
- Los gráficos DEBEN ofrecer resumen textual y tabla de datos; sus tooltips también DEBEN ser accesibles mediante teclado o alternativa equivalente.
- Se DEBEN respetar las preferencias de movimiento reducido y los formatos locales `es-CL` para números y fechas, sin destruir la precisión subyacente.

### VII. Resiliencia y degradación controlada

- El acceso a `mindicador.cl` DEBE estar encapsulado detrás de un adaptador propio para evitar acoplar la experiencia al payload externo.
- La aplicación DEBE definir timeout, reintentos limitados con backoff, circuit breaker o protección equivalente, caché y política de datos obsoletos.
- Ante una caída de la fuente, la interfaz DEBE distinguir entre dato almacenado y dato actualizado, mostrar su antigüedad y evitar una pantalla vacía si existe un valor válido previo.
- Respuestas incompletas, fechas inválidas, valores no numéricos y cambios de contrato DEBEN fallar de manera observable y segura.
- La ausencia de datos en una fecha DEBE ser un estado de dominio esperado, no un error genérico.
- Ninguna falla externa DEBE generar resultados parciales presentados como completos.

### VIII. Seguridad y privacidad proporcionales

- Todo tráfico DEBE utilizar HTTPS y las entradas DEBEN validarse por lista permitida de indicadores, fechas, rangos y magnitudes.
- La solución DEBE protegerse contra XSS, inyección, abuso de recursos, manipulación de parámetros y consumo automatizado excesivo.
- No se DEBEN exponer secretos ni configuración sensible en el cliente, logs o repositorio.
- Cabeceras de seguridad, política de dependencias, escaneo de vulnerabilidades y actualización controlada DEBEN formar parte del pipeline.
- El producto DEBE minimizar telemetría y datos personales. Analítica, cookies o identificadores persistentes requieren finalidad, consentimiento y documentación explícitos.
- Si se agregan favoritos locales, DEBEN funcionar sin cuenta. Cualquier futura autenticación o alerta remota requiere un spec independiente con privacidad y retención definidas.

### IX. Rendimiento medible y presupuesto de experiencia

- Las especificaciones DEBEN incluir objetivos verificables de Core Web Vitals y tamaño de recursos críticos.
- Como objetivo de producción en el percentil 75: LCP ≤ 2,5 s, INP ≤ 200 ms y CLS ≤ 0,1 en móvil y escritorio.
- La ruta inicial NO DEBE descargar todas las series históricas; los datos de detalle y librerías pesadas DEBEN cargarse bajo demanda.
- La interacción del conversor y el cambio de rango DEBEN producir respuesta visual inmediata; el procesamiento local común no debe bloquear el hilo principal.
- Caché, compresión, paginación/ventanas y reducción de puntos para visualización DEBEN preservar los valores originales y estar justificadas en el plan técnico.

### X. Calidad comprobable y contratos estables

- Cada requisito funcional DEBE tener criterios de aceptación observables y trazabilidad hacia pruebas.
- La lógica de conversión, tasas cruzadas, inversión, redondeo, variaciones, normalización base 100 y estadísticas DEBE cubrirse con pruebas unitarias deterministas.
- El adaptador de API DEBE tener pruebas de contrato con fixtures versionados y pruebas para payloads inválidos o parciales.
- Los flujos críticos —dashboard, detalle histórico, cambio de rango, conversor y estados de error— DEBEN tener pruebas de integración y end-to-end responsivas.
- Accesibilidad, lint, tipos, pruebas, build y análisis de dependencias DEBEN ser puertas obligatorias de CI.
- Corregir una falla de cálculo o parsing exige primero una prueba que la reproduzca.
- Los cambios incompatibles de contratos internos o públicos requieren versionado y estrategia de migración.

### XI. Observabilidad sin ambigüedad

- La solución DEBE registrar salud de la fuente, latencia, errores, uso de caché, antigüedad de datos y fallas de transformación sin registrar información sensible.
- Cada solicitud interna DEBE ser correlacionable y los errores DEBEN usar categorías estables.
- Se DEBEN definir objetivos de nivel de servicio para disponibilidad propia y frescura del dato, reconociendo que la actualización depende de un tercero.
- Las alertas operacionales DEBEN distinguir caída de la aplicación, indisponibilidad de la API y retraso propio de la periodicidad del indicador.

### XII. Simplicidad evolutiva y separación de responsabilidades

- La arquitectura DEBE separar adquisición, normalización, dominio, análisis/conversión y presentación.
- El modelo canónico interno DEBE impedir que cambios menores del proveedor se propaguen por toda la solución.
- Toda dependencia relevante DEBE resolver una necesidad demostrable; la complejidad añadida requiere justificación en `plan.md`.
- El producto DEBE comenzar sin autenticación, pagos, trading, recomendaciones, noticias, alertas o fuentes adicionales, salvo que un spec aprobado amplíe expresamente el alcance.
- Las reglas duraderas pertenecen a esta constitución; `spec.md` define qué y por qué; `plan.md` define cómo; `tasks.md` descompone el trabajo ejecutable.

## Alcance funcional gobernado

La siguiente matriz fija capacidades mínimas, sin sustituir los specs detallados:

| Área | Capacidades obligatorias |
| --- | --- |
| Dashboard | listado de los 12 indicadores, último valor, unidad, fecha efectiva, búsqueda/filtro, estado de actualización y navegación al detalle |
| Detalle | metadatos del indicador, gráfico histórico, rango/año, estadísticas, tabla y estados sin datos/error |
| Consulta histórica | último mes, fecha exacta y año mediante las rutas disponibles; navegación consistente entre ellas |
| Comparador | selección de 2 o más series, período común, modo base 100 para unidades distintas, leyenda y tabla |
| Conversor | monto, origen, destino, inversión, tasa/fórmula/fecha, conversiones directas e indirectas válidas |
| Exportación | CSV del conjunto visible y metadatos de procedencia |
| Preferencias | tema y favoritos locales opcionales, sin requerir una cuenta |
| Soporte UX | skeletons, vacío, error, sin conexión, dato en caché y dato desactualizado claramente diferenciados |

## Reglas de diseño visual

- La estética DEBE ser sobria, financiera y de alta legibilidad, con jerarquía basada en tipografía, espacio y contraste antes que decoración.
- Las tarjetas de mercado DEBEN mostrar nombre, código, valor, unidad, fecha y variación sin densidad innecesaria.
- El escritorio PUEDE usar navegación lateral o superior y paneles simultáneos; móvil DEBE priorizar una columna, controles táctiles y detalles progresivos.
- El comparador DEBE permitir ocultar series, identificar cada línea sin depender solo del color y alternar entre valores absolutos y normalizados cuando sea válido.
- El conversor DEBE inspirarse en el patrón probado de monto + origen + destino + intercambio + resultado, pero sin copiar identidad visual ni afirmar tasas que la fuente no entrega.
- Debe existir modo claro y oscuro si se incluye en el alcance del spec; ambos deben superar los mismos controles de contraste y visualización de gráficos.

## Puertas constitucionales para Spec Kit

Antes de aprobar un `spec.md`, se DEBE verificar:

1. valor y problema del usuario definidos sin decisiones técnicas;
2. fuente, unidad, frecuencia y fecha efectiva de cada dato identificadas;
3. criterios de aceptación para carga, éxito, vacío, obsoleto y error;
4. fórmulas y reglas de conversión/análisis expresadas sin ambigüedad;
5. comportamiento responsive y accesible definido;
6. exclusiones y dependencias externas declaradas;
7. métricas de éxito medibles y tecnológicamente neutrales.

Antes de aprobar un `plan.md`, se DEBE verificar:

1. cumplimiento de los doce principios;
2. adaptador y modelo canónico para la API externa;
3. política de caché, resiliencia y frescura;
4. precisión decimal y estrategia de fechas/zona horaria;
5. presupuesto de rendimiento y estrategia de gráficos;
6. pruebas unitarias, contrato, integración, E2E y accesibilidad;
7. seguridad, observabilidad y pipeline de calidad;
8. justificación de cualquier complejidad o excepción.

Una violación constitucional bloquea implementación, salvo excepción documentada y aprobada según la sección de Gobierno.

## Secuencia inicial recomendada de especificaciones

Estas unidades sirven como backlog inicial para ejecutar `/speckit.specify` por separado:

1. **Dashboard de indicadores actuales**: resumen, búsqueda, filtros, actualización y navegación.
2. **Detalle y consulta histórica**: último mes, fecha específica, año, tabla y estados de datos.
3. **Visualización y análisis estadístico**: gráfico interactivo, rangos, métricas y exportación CSV.
4. **Comparación normalizada de series**: selección múltiple, intersección temporal y base 100.
5. **Conversor de unidades y monedas**: tasas directas, inversas y cruzadas con trazabilidad.
6. **Experiencia responsive, accesibilidad y temas**: navegación, teclado, lector de pantalla y visualización adaptable.
7. **Resiliencia, caché y observabilidad**: degradación, frescura, métricas y operación.

## Gobierno

- Esta constitución prevalece sobre convenciones, planes, tareas y decisiones locales que la contradigan.
- Toda enmienda DEBE incluir motivo, impacto, migración de specs/planes afectados y aprobación explícita del responsable del producto y del responsable técnico.
- El versionado sigue SemVer: **MAJOR** para eliminar o redefinir principios; **MINOR** para agregar principios o ampliar obligaciones; **PATCH** para aclaraciones sin cambio normativo.
- Cada `/speckit.analyze` DEBE validar coherencia entre constitución, spec, plan y tareas antes de implementar.
- Cada revisión de entrega DEBE aportar evidencia de las puertas de calidad; una declaración sin resultado verificable no constituye cumplimiento.
- Las excepciones DEBEN ser temporales, tener propietario, justificación, riesgo, fecha de expiración y tarea de remediación.
- La revisión ordinaria de esta constitución será trimestral o ante cambios relevantes de la API, del alcance financiero o de obligaciones legales.

**Versión**: 1.0.0  
**Ratificada**: 2026-09-07  
**Última modificación**: 2026-09-07

## Referencias de investigación

- API y cobertura oficial: <https://mindicador.cl/>
- Payload vigente de indicadores: <https://mindicador.cl/api>
- Fuente institucional relacionada: <https://si3.bcentral.cl/indicadoressiete/secure/indicadoresdiarios.aspx>
- Patrón de conversión, tasa, fecha y rangos históricos: <https://www.xe.com/en-us/currencyconverter/> y <https://wise.com/us/currency-converter/>
- Comparación de instrumentos y normalización base 100: <https://www.tradingview.com/support/solutions/43000543053-how-to-use-the-compare-tool/> y <https://www.tradingview.com/support/solutions/43000477709-when-comparing-symbols-i-only-see-detached-lines-on-the-chart/>
- Flujo oficial de GitHub Spec Kit: <https://github.com/github/spec-kit>

