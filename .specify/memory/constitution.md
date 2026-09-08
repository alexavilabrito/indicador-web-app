<!--
Sync Impact Report
Version change: 0.0.0 (scaffold) -> 1.0.0
Modified principles:
- Scaffold principle 1 -> I. Fidelidad, procedencia y transparencia del dato
- Scaffold principle 2 -> II. Cobertura completa y explicita de la API
- Scaffold principle 3 -> III. Semantica financiera correcta
- Scaffold principle 4 -> IV. Conversion verificable y limitada por la fuente
- Scaffold principle 5 -> V-XII. Analisis, UX, resiliencia, seguridad, rendimiento,
  calidad, observabilidad y simplicidad evolutiva
Added sections:
- Alcance funcional y reglas de diseno
- Puertas constitucionales y secuencia inicial
- Referencias de investigacion
Removed sections:
- None; scaffold placeholders replaced
Follow-up TODOs:
- None
-->

# Constitucion de la Plataforma Web de Indicadores Financieros de Chile

## Core Principles

### I. Fidelidad, procedencia y transparencia del dato

- Todo valor visible DEBE conservar el indicador, valor, unidad, fecha efectiva y fuente
  entregados por la API.
- La interfaz DEBE mostrar la fecha y hora de ultima actualizacion y distinguir claramente entre
  fecha de consulta y fecha efectiva del dato.
- Los datos obtenidos directamente, invertidos, cruzados, normalizados o derivados DEBEN
  etiquetarse de forma inequivoca.
- La aplicacion NO DEBE presentar datos estimados, interpolados o faltantes como si hubieran sido
  informados por la fuente.
- Los valores historicos NO DEBEN rellenarse silenciosamente en fines de semana, feriados ni
  periodos sin observacion.
- Cada vista de detalle DEBE identificar `mindicador.cl` como fuente y advertir que la informacion
  es referencial, no una recomendacion financiera ni una cotizacion transable.

Razon: en un producto financiero, la confianza depende de que el usuario pueda entender de donde
proviene cada cifra y que representa.

### II. Cobertura completa y explicita de la API

La solucion DEBE soportar los doce indicadores publicados por la API:

| Codigo | Indicador | Unidad informada |
| --- | --- | --- |
| `uf` | Unidad de Fomento | Pesos |
| `ivp` | Indice de Valor Promedio | Pesos |
| `dolar` | Dolar observado | Pesos |
| `dolar_intercambio` | Dolar acuerdo | Pesos |
| `euro` | Euro | Pesos |
| `ipc` | Indice de Precios al Consumidor | Porcentaje |
| `utm` | Unidad Tributaria Mensual | Pesos |
| `imacec` | IMACEC | Porcentaje |
| `tpm` | Tasa de Politica Monetaria | Porcentaje |
| `libra_cobre` | Libra de cobre | Dolar |
| `tasa_desempleo` | Tasa de desempleo | Porcentaje |
| `bitcoin` | Bitcoin | Dolar |

La integracion DEBE cubrir los cuatro patrones publicos de consulta:

1. ultimo valor de todos los indicadores: `/api`;
2. serie reciente por indicador: `/api/{indicador}`;
3. valor por indicador y fecha: `/api/{indicador}/{dd-mm-yyyy}`;
4. serie por indicador y ano: `/api/{indicador}/{yyyy}`.

Los contratos, ejemplos de payload, validaciones y manejo de errores DEBEN documentarse por
feature en `specs/<feature>/contracts/`; no se duplican en esta constitucion.

Razon: la cobertura explicita evita que una feature dependa de supuestos incompletos sobre la
fuente externa.

### III. Semantica financiera correcta

- Los calculos monetarios DEBEN utilizar aritmetica decimal, nunca punto flotante binario como
  fuente de verdad.
- El redondeo DEBE ocurrir unicamente para presentacion o cuando una regla de negocio documentada
  lo exija.
- La precision interna, la precision visible y el modo de redondeo DEBEN quedar definidos y
  probados.
- Los porcentajes (`ipc`, `imacec`, `tpm`, `tasa_desempleo`) NO DEBEN tratarse como monedas ni
  habilitarse en el conversor.
- La UF, UTM e IVP DEBEN presentarse como unidades de cuenta/reajustabilidad, no como monedas de
  libre transaccion.
- Libra de cobre y Bitcoin DEBEN identificarse como commodity y criptoactivo respectivamente; su
  unidad base informada por la API es USD.
- Las conversiones DEBEN incluir formula, tasa utilizada, fecha efectiva y sentido de la
  cotizacion.

Razon: una aplicacion de indicadores financieros puede informar mal aun con datos correctos si
aplica unidades, precision o formulas incorrectas.

### IV. Conversion verificable y limitada por la fuente

El conversor DEBE soportar, como minimo:

- CLP <-> USD observado;
- CLP <-> EUR;
- CLP <-> UF, UTM e IVP;
- USD <-> EUR mediante tasa cruzada con CLP;
- USD <-> libra de cobre y USD <-> BTC cuando la unidad y naturaleza del indicador permitan una
  conversion informativa;
- inversion de origen y destino sin perder precision;
- calculo con valor actual o con una fecha historica disponible.

Toda tasa cruzada DEBE mostrar la ruta de calculo. Por ejemplo:
`EUR/USD = (CLP por EUR) / (CLP por USD)`. Si las observaciones no comparten una fecha compatible,
la conversion DEBE bloquearse o advertir explicitamente la diferencia; jamas debe mezclar fechas
de forma silenciosa.

La aplicacion NO DEBE denominar estas tasas como "precio de compra", "precio de venta", "spot",
"tiempo real" o "mid-market", porque la API no entrega bid/ask ni un feed transable en tiempo
real.

Razon: el conversor debe ser auditable por el usuario y no debe afirmar propiedades que la fuente
no entrega.

### V. Analisis de series reproducible y honesto

Cada indicador DEBE contar con una vista historica que permita seleccionar periodo, consultar
valores exactos y visualizar como minimo:

- ultimo valor y variacion absoluta y porcentual respecto de la observacion anterior;
- minimo, maximo, media y mediana del periodo;
- primera y ultima observacion, cambio acumulado y cantidad de observaciones;
- grafico de linea con tooltip de fecha, valor y unidad;
- tabla accesible equivalente al grafico;
- exportacion de los datos visibles en CSV con fuente, rango y fecha de generacion.

Las comparaciones entre series con unidades o escalas distintas DEBEN usar una vista normalizada,
base 100 desde la primera observacion comun, o escalas separadas claramente rotuladas. La interfaz
DEBE explicar el metodo aplicado.

Los indicadores tecnicos opcionales, por ejemplo media movil, volatilidad o variacion movil,
DEBEN estar definidos matematicamente, disponer de pruebas con resultados conocidos y
diferenciarse de los datos originales. No se emitiran senales automaticas de compra o venta.

Razon: el analisis solo es util si otra persona puede reproducirlo y distinguirlo de los datos
originales.

### VI. Experiencia progresiva, responsiva y accesible

- La aplicacion DEBE ser mobile-first y funcionar en anchos de 320 px en adelante, ademas de
  tablet y escritorio.
- El dashboard DEBE priorizar resumen, busqueda y acceso rapido; el detalle DEBE concentrar
  grafico, estadisticas y tabla sin sobrecargar la portada.
- El conversor DEBE permitir ingresar monto, buscar unidades, intercambiar origen/destino y
  entender el resultado con una sola mirada.
- Los rangos temporales frecuentes DEBEN estar disponibles como accesos rapidos, junto con
  seleccion personalizada cuando la fuente lo permita.
- Color, iconos y texto DEBEN comunicar conjuntamente alzas, bajas, estados y errores; el color
  nunca sera el unico canal.
- Toda funcionalidad esencial DEBE operar con teclado, foco visible, lectores de pantalla y zoom
  al 200 %, conforme a WCAG 2.2 nivel AA.
- Los graficos DEBEN ofrecer resumen textual y tabla de datos; sus tooltips tambien DEBEN ser
  accesibles mediante teclado o alternativa equivalente.
- Se DEBEN respetar las preferencias de movimiento reducido y los formatos locales `es-CL` para
  numeros y fechas, sin destruir la precision subyacente.

Razon: la experiencia debe servir tanto a consulta rapida como a exploracion detallada, sin
excluir usuarios por dispositivo o capacidad.

### VII. Resiliencia y degradacion controlada

- El acceso a `mindicador.cl` DEBE estar encapsulado detras de un adaptador propio para evitar
  acoplar la experiencia al payload externo.
- La aplicacion DEBE definir timeout, reintentos limitados con backoff, circuit breaker o
  proteccion equivalente, cache y politica de datos obsoletos.
- Ante una caida de la fuente, la interfaz DEBE distinguir entre dato almacenado y dato
  actualizado, mostrar su antiguedad y evitar una pantalla vacia si existe un valor valido
  previo.
- Respuestas incompletas, fechas invalidas, valores no numericos y cambios de contrato DEBEN
  fallar de manera observable y segura.
- La ausencia de datos en una fecha DEBE ser un estado de dominio esperado, no un error generico.
- Ninguna falla externa DEBE generar resultados parciales presentados como completos.

Razon: depender de una fuente externa exige degradar con honestidad, no ocultar fallas ni fabricar
completitud.

### VIII. Seguridad y privacidad proporcionales

- Todo trafico DEBE utilizar HTTPS y las entradas DEBEN validarse por lista permitida de
  indicadores, fechas, rangos y magnitudes.
- La solucion DEBE protegerse contra XSS, inyeccion, abuso de recursos, manipulacion de
  parametros y consumo automatizado excesivo.
- No se DEBEN exponer secretos ni configuracion sensible en el cliente, logs o repositorio.
- Cabeceras de seguridad, politica de dependencias, escaneo de vulnerabilidades y actualizacion
  controlada DEBEN formar parte del pipeline.
- El producto DEBE minimizar telemetria y datos personales. Analitica, cookies o identificadores
  persistentes requieren finalidad, consentimiento y documentacion explicitos.
- Si se agregan favoritos locales, DEBEN funcionar sin cuenta. Cualquier futura autenticacion o
  alerta remota requiere un spec independiente con privacidad y retencion definidas.

Razon: el alcance informativo reduce la necesidad de datos personales, pero no elimina riesgos de
seguridad web ni de abuso.

### IX. Rendimiento medible y presupuesto de experiencia

- Las especificaciones DEBEN incluir objetivos verificables de Core Web Vitals y tamano de
  recursos criticos.
- Como objetivo de produccion en el percentil 75: LCP <= 2,5 s, INP <= 200 ms y CLS <= 0,1 en
  movil y escritorio.
- La ruta inicial NO DEBE descargar todas las series historicas; los datos de detalle y librerias
  pesadas DEBEN cargarse bajo demanda.
- La interaccion del conversor y el cambio de rango DEBEN producir respuesta visual inmediata; el
  procesamiento local comun NO DEBE bloquear el hilo principal.
- Cache, compresion, paginacion/ventanas y reduccion de puntos para visualizacion DEBEN preservar
  los valores originales y estar justificadas en el plan tecnico.

Razon: los indicadores se consultan en sesiones breves y repetidas; la aplicacion debe responder
rapido sin sacrificar precision.

### X. Calidad comprobable y contratos estables

- Cada requisito funcional DEBE tener criterios de aceptacion observables y trazabilidad hacia
  pruebas.
- La logica de conversion, tasas cruzadas, inversion, redondeo, variaciones, normalizacion base
  100 y estadisticas DEBE cubrirse con pruebas unitarias deterministas.
- El adaptador de API DEBE tener pruebas de contrato con fixtures versionados y pruebas para
  payloads invalidos o parciales.
- Los flujos criticos, dashboard, detalle historico, cambio de rango, conversor y estados de
  error, DEBEN tener pruebas de integracion y end-to-end responsivas.
- Accesibilidad, lint, tipos, pruebas, build y analisis de dependencias DEBEN ser puertas
  obligatorias de CI.
- Corregir una falla de calculo o parsing exige primero una prueba que la reproduzca.
- Los cambios incompatibles de contratos internos o publicos requieren versionado y estrategia de
  migracion.

Razon: los errores de calculo, contrato o accesibilidad son regresiones de producto, no solo
fallas tecnicas.

### XI. Observabilidad sin ambiguedad

- La solucion DEBE registrar salud de la fuente, latencia, errores, uso de cache, antiguedad de
  datos y fallas de transformacion sin registrar informacion sensible.
- Cada solicitud interna DEBE ser correlacionable y los errores DEBEN usar categorias estables.
- Se DEBEN definir objetivos de nivel de servicio para disponibilidad propia y frescura del dato,
  reconociendo que la actualizacion depende de un tercero.
- Las alertas operacionales DEBEN distinguir caida de la aplicacion, indisponibilidad de la API y
  retraso propio de la periodicidad del indicador.

Razon: operar datos financieros referenciales requiere saber si fallo la app, la fuente o la
periodicidad esperada del dato.

### XII. Simplicidad evolutiva y separacion de responsabilidades

- La arquitectura DEBE separar adquisicion, normalizacion, dominio, analisis/conversion y
  presentacion.
- El modelo canonico interno DEBE impedir que cambios menores del proveedor se propaguen por toda
  la solucion.
- Toda dependencia relevante DEBE resolver una necesidad demostrable; la complejidad anadida
  requiere justificacion en `plan.md`.
- El producto DEBE comenzar sin autenticacion, pagos, trading, recomendaciones, noticias, alertas
  o fuentes adicionales, salvo que un spec aprobado amplie expresamente el alcance.
- Las reglas duraderas pertenecen a esta constitucion; `spec.md` define que y por que; `plan.md`
  define como; `tasks.md` descompone el trabajo ejecutable.

Razon: separar responsabilidades permite ampliar el producto sin mezclar proveedor, dominio,
analisis y presentacion.

## Alcance funcional y reglas de diseno

La siguiente matriz fija capacidades minimas, sin sustituir los specs detallados:

| Area | Capacidades obligatorias |
| --- | --- |
| Dashboard | listado de los 12 indicadores, ultimo valor, unidad, fecha efectiva, busqueda/filtro, estado de actualizacion y navegacion al detalle |
| Detalle | metadatos del indicador, grafico historico, rango/ano, estadisticas, tabla y estados sin datos/error |
| Consulta historica | ultimo mes, fecha exacta y ano mediante las rutas disponibles; navegacion consistente entre ellas |
| Comparador | seleccion de 2 o mas series, periodo comun, modo base 100 para unidades distintas, leyenda y tabla |
| Conversor | monto, origen, destino, inversion, tasa/formula/fecha, conversiones directas e indirectas validas |
| Exportacion | CSV del conjunto visible y metadatos de procedencia |
| Preferencias | tema y favoritos locales opcionales, sin requerir una cuenta |
| Soporte UX | skeletons, vacio, error, sin conexion, dato en cache y dato desactualizado claramente diferenciados |

- La estetica DEBE ser sobria, financiera y de alta legibilidad, con jerarquia basada en
  tipografia, espacio y contraste antes que decoracion.
- Las tarjetas de mercado DEBEN mostrar nombre, codigo, valor, unidad, fecha y variacion sin
  densidad innecesaria.
- El escritorio PUEDE usar navegacion lateral o superior y paneles simultaneos; movil DEBE
  priorizar una columna, controles tactiles y detalles progresivos.
- El comparador DEBE permitir ocultar series, identificar cada linea sin depender solo del color y
  alternar entre valores absolutos y normalizados cuando sea valido.
- El conversor DEBE inspirarse en el patron probado de monto, origen, destino, intercambio y
  resultado, pero sin copiar identidad visual ni afirmar tasas que la fuente no entrega.
- Debe existir modo claro y oscuro si se incluye en el alcance del spec; ambos deben superar los
  mismos controles de contraste y visualizacion de graficos.

## Puertas constitucionales y secuencia inicial

Antes de aprobar un `spec.md`, se DEBE verificar:

1. valor y problema del usuario definidos sin decisiones tecnicas;
2. fuente, unidad, frecuencia y fecha efectiva de cada dato identificadas;
3. criterios de aceptacion para carga, exito, vacio, obsoleto y error;
4. formulas y reglas de conversion/analisis expresadas sin ambiguedad;
5. comportamiento responsive y accesible definido;
6. exclusiones y dependencias externas declaradas;
7. metricas de exito medibles y tecnologicamente neutrales.

Antes de aprobar un `plan.md`, se DEBE verificar:

1. cumplimiento de los doce principios;
2. adaptador y modelo canonico para la API externa;
3. politica de cache, resiliencia y frescura;
4. precision decimal y estrategia de fechas/zona horaria;
5. presupuesto de rendimiento y estrategia de graficos;
6. pruebas unitarias, contrato, integracion, E2E y accesibilidad;
7. seguridad, observabilidad y pipeline de calidad;
8. justificacion de cualquier complejidad o excepcion.

Una violacion constitucional bloquea implementacion, salvo excepcion documentada y aprobada segun
la seccion de Governance.

Estas unidades sirven como backlog inicial para ejecutar `$speckit-specify` por separado:

1. Dashboard de indicadores actuales: resumen, busqueda, filtros, actualizacion y navegacion.
2. Detalle y consulta historica: ultimo mes, fecha especifica, ano, tabla y estados de datos.
3. Visualizacion y analisis estadistico: grafico interactivo, rangos, metricas y exportacion CSV.
4. Comparacion normalizada de series: seleccion multiple, interseccion temporal y base 100.
5. Conversor de unidades y monedas: tasas directas, inversas y cruzadas con trazabilidad.
6. Experiencia responsive, accesibilidad y temas: navegacion, teclado, lector de pantalla y
   visualizacion adaptable.
7. Resiliencia, cache y observabilidad: degradacion, frescura, metricas y operacion.

Referencias de investigacion:

- API y cobertura oficial: <https://mindicador.cl/>
- Payload vigente de indicadores: <https://mindicador.cl/api>
- Fuente institucional relacionada:
  <https://si3.bcentral.cl/indicadoressiete/secure/indicadoresdiarios.aspx>
- Patron de conversion, tasa, fecha y rangos historicos:
  <https://www.xe.com/en-us/currencyconverter/> y
  <https://wise.com/us/currency-converter/>
- Comparacion de instrumentos y normalizacion base 100:
  <https://www.tradingview.com/support/solutions/43000543053-how-to-use-the-compare-tool/>
  y
  <https://www.tradingview.com/support/solutions/43000477709-when-comparing-symbols-i-only-see-detached-lines-on-the-chart/>
- Flujo oficial de GitHub Spec Kit: <https://github.com/github/spec-kit>

## Governance

- Esta constitucion prevalece sobre convenciones, planes, tareas y decisiones locales que la
  contradigan.
- Toda enmienda DEBE incluir motivo, impacto, migracion de specs/planes afectados y aprobacion
  explicita del responsable del producto y del responsable tecnico.
- El versionado sigue SemVer: MAJOR para eliminar o redefinir principios; MINOR para agregar
  principios o ampliar obligaciones; PATCH para aclaraciones sin cambio normativo.
- Cada `$speckit-analyze` DEBE validar coherencia entre constitucion, spec, plan y tareas antes de
  implementar.
- Cada revision de entrega DEBE aportar evidencia de las puertas de calidad; una declaracion sin
  resultado verificable no constituye cumplimiento.
- Las excepciones DEBEN ser temporales, tener propietario, justificacion, riesgo, fecha de
  expiracion y tarea de remediacion.
- La revision ordinaria de esta constitucion sera trimestral o ante cambios relevantes de la API,
  del alcance financiero o de obligaciones legales.

**Version**: 1.0.0 | **Ratified**: 2026-09-07 | **Last Amended**: 2026-09-07
