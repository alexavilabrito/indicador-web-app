# Feature Specification: Indicator Detail

**Feature Branch**: `001-indicator-detail`

**Created**: 2026-09-07

**Status**: Draft

**Input**: User description: "Crear la vista de detalle de un indicador economico. El usuario debe poder seleccionar un indicador desde el dashboard y consultar su informacion completa y la serie reciente disponible. Debe visualizar nombre y codigo del indicador, ultimo valor, unidad de medida, fecha efectiva, fuente de informacion, variacion absoluta y porcentual respecto de la observacion anterior, serie reciente ordenada cronologicamente, y tabla accesible con fecha y valor. Debe contemplar carga, exito, ausencia de serie, indicador invalido, datos desactualizados y servicio no disponible. No incluir todavia graficos, estadisticas avanzadas, seleccion por ano, conversion ni comparacion."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consultar detalle del indicador (Priority: P1)

Como persona que revisa indicadores economicos chilenos, quiero abrir un indicador desde el
dashboard y ver su informacion principal en una vista de detalle para entender rapidamente que
valor esta publicado, a que fecha corresponde y de donde proviene.

**Why this priority**: Es el flujo principal de la feature y entrega valor aun sin serie reciente,
porque permite verificar el ultimo dato disponible con contexto de fuente y fecha.

**Independent Test**: Puede probarse seleccionando cualquier indicador valido desde el dashboard y
verificando que la vista de detalle muestre nombre, codigo, ultimo valor, unidad, fecha efectiva,
fuente, fecha de actualizacion visible y advertencia de informacion referencial.

**Acceptance Scenarios**:

1. **Given** el dashboard muestra indicadores disponibles, **When** el usuario selecciona un
   indicador valido, **Then** se abre una vista de detalle para ese indicador con su nombre,
   codigo, ultimo valor, unidad, fecha efectiva y fuente.
2. **Given** la vista de detalle carga datos del indicador seleccionado, **When** la informacion
   se presenta al usuario, **Then** la vista distingue fecha efectiva del dato y fecha de consulta
   o actualizacion de la pantalla.
3. **Given** el indicador seleccionado existe pero su ultimo dato no esta disponible, **When** la
   vista termina de cargar, **Then** se muestra un estado comprensible sin presentar valores
   estimados como publicados.

---

### User Story 2 - Revisar variacion reciente (Priority: P2)

Como persona que compara cambios recientes, quiero ver la variacion absoluta y porcentual respecto
de la observacion anterior para comprender si el ultimo valor subio, bajo o se mantuvo.

**Why this priority**: La variacion agrega interpretacion inmediata al ultimo valor sin introducir
graficos ni estadisticas avanzadas.

**Independent Test**: Puede probarse con un indicador que tenga al menos dos observaciones
recientes y validando que la variacion use la observacion anterior disponible, muestre unidad,
porcentaje y direccion, y no dependa solo del color.

**Acceptance Scenarios**:

1. **Given** un indicador valido tiene al menos dos observaciones recientes, **When** se muestra el
   detalle, **Then** la vista presenta la variacion absoluta y porcentual respecto de la
   observacion anterior.
2. **Given** un indicador valido tiene menos de dos observaciones recientes, **When** se muestra el
   detalle, **Then** la vista informa que la variacion no puede calcularse con los datos
   disponibles.
3. **Given** la variacion indica alza, baja o sin cambio, **When** se comunica el estado, **Then**
   la direccion se expresa mediante texto o iconografia ademas de cualquier color.

---

### User Story 3 - Explorar serie reciente accesible (Priority: P3)

Como persona que necesita revisar valores historicos inmediatos, quiero ver la serie reciente
ordenada cronologicamente en una tabla accesible para consultar fechas y valores exactos sin usar
un grafico.

**Why this priority**: La tabla permite inspeccion precisa y accesible de la serie reciente dentro
del alcance inicial, sin adelantar visualizaciones futuras.

**Independent Test**: Puede probarse abriendo el detalle de un indicador con serie reciente y
validando que la tabla incluya fecha y valor, use orden cronologico consistente, sea navegable con
teclado y conserve unidad y procedencia.

**Acceptance Scenarios**:

1. **Given** la fuente entrega una serie reciente para el indicador, **When** el detalle se muestra
   correctamente, **Then** la tabla lista cada observacion con fecha y valor en orden cronologico.
2. **Given** la serie reciente esta vacia o ausente, **When** el usuario abre el detalle, **Then**
   la vista mantiene la informacion principal disponible y muestra un estado especifico de ausencia
   de serie.
3. **Given** el usuario navega la tabla con teclado o lector de pantalla, **When** revisa sus
   filas y encabezados, **Then** puede asociar cada fecha con su valor y unidad sin depender de una
   representacion visual adicional.

---

### User Story 4 - Entender estados de carga y falla (Priority: P4)

Como persona usuaria, quiero que la vista explique claramente si esta cargando, si el indicador es
invalido, si los datos estan desactualizados o si el servicio no esta disponible para saber si
puedo confiar en lo que veo o intentar mas tarde.

**Why this priority**: Los estados de error y frescura protegen la confianza del producto y evitan
que datos parciales se interpreten como completos.

**Independent Test**: Puede probarse forzando cada estado esperado y verificando que la pantalla
muestre mensajes diferenciados para carga, exito, ausencia de serie, indicador invalido, datos
desactualizados y servicio no disponible.

**Acceptance Scenarios**:

1. **Given** la vista aun no tiene datos suficientes para presentar el detalle, **When** el usuario
   abre un indicador, **Then** se muestra un estado de carga que conserva orientacion sobre el
   indicador solicitado.
2. **Given** el identificador del indicador no corresponde a un indicador soportado, **When** el
   usuario intenta abrir el detalle, **Then** se muestra un estado de indicador invalido y una ruta
   clara para volver al dashboard.
3. **Given** el servicio de informacion no esta disponible, **When** el usuario abre el detalle,
   **Then** se muestra un estado de servicio no disponible sin presentar resultados parciales como
   completos.
4. **Given** existe un dato previo valido pero no actualizado, **When** se muestra en la vista,
   **Then** la pantalla lo etiqueta como desactualizado e informa su antiguedad.

### Edge Cases

- El indicador seleccionado pertenece a la lista soportada, pero la fuente no entrega serie
  reciente para ese indicador.
- La serie reciente contiene solo una observacion, por lo que no existe observacion anterior para
  calcular variacion.
- La fuente entrega observaciones en orden no cronologico; la vista debe presentarlas en orden
  cronologico sin modificar fechas ni valores.
- La fecha efectiva del ultimo valor no coincide con la fecha de consulta o actualizacion visible.
- Un valor recibido no es numerico o no tiene unidad reconocible; la vista debe tratarlo como dato
  no confiable y no calcular variaciones con ese valor.
- El usuario abre directamente una URL o enlace profundo con un codigo de indicador inexistente.
- El servicio externo no responde o responde incompleto; la vista debe distinguir indisponibilidad
  de ausencia legitima de datos.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: El usuario DEBE poder abrir la vista de detalle seleccionando un indicador desde el
  dashboard.
- **FR-002**: La vista DEBE aceptar solo los doce indicadores gobernados por la constitucion y
  mostrar estado de indicador invalido para cualquier codigo fuera de esa lista.
- **FR-003**: La vista DEBE mostrar nombre, codigo, ultimo valor, unidad de medida, fecha efectiva
  y fuente de informacion del indicador seleccionado.
- **FR-004**: La vista DEBE mostrar la fecha y hora de consulta o actualizacion de la informacion
  y distinguirla de la fecha efectiva del dato.
- **FR-005**: La vista DEBE incluir una advertencia visible de que la informacion es referencial y
  no constituye recomendacion financiera ni cotizacion transable.
- **FR-006**: La vista DEBE presentar la variacion absoluta y porcentual respecto de la observacion
  anterior disponible cuando existan al menos dos observaciones validas.
- **FR-007**: La vista DEBE informar claramente cuando la variacion no puede calcularse por falta
  de observacion anterior o por datos no confiables.
- **FR-008**: La serie reciente DEBE presentarse en orden cronologico consistente y conservar fecha,
  valor y unidad de cada observacion.
- **FR-009**: La tabla de serie reciente DEBE ser accesible, con encabezados de fecha y valor,
  navegacion por teclado y una asociacion clara entre cada fila, su unidad y su indicador.
- **FR-010**: La vista DEBE diferenciar los estados de carga, exito, ausencia de serie, indicador
  invalido, datos desactualizados y servicio no disponible.
- **FR-011**: Cuando solo existan datos almacenados o desactualizados, la vista DEBE etiquetarlos
  como tales, mostrar su antiguedad y evitar presentarlos como actuales.
- **FR-012**: La vista NO DEBE rellenar, estimar, interpolar ni reemplazar valores historicos
  faltantes.
- **FR-013**: La vista NO DEBE incluir graficos, estadisticas avanzadas, seleccion por ano,
  conversiones ni comparaciones entre indicadores en esta feature.
- **FR-014**: La vista DEBE ofrecer una accion clara para volver al dashboard desde estados de
  exito, indicador invalido, ausencia de serie y servicio no disponible.
- **FR-015**: La vista DEBE usar formatos locales `es-CL` para fechas y numeros visibles, sin
  alterar la precision subyacente del dato.

### Key Entities

- **Indicador Economico**: Representa un indicador soportado. Incluye codigo, nombre, unidad
  informada, categoria de unidad, fuente y estado de soporte.
- **Observacion de Indicador**: Representa un valor publicado para un indicador en una fecha
  efectiva. Incluye fecha efectiva, valor, unidad, fuente y marca de confiabilidad.
- **Detalle de Indicador**: Representa la informacion visible consolidada para la pantalla:
  indicador, ultimo valor, observacion anterior, variacion, serie reciente, fecha de consulta y
  estado de frescura.
- **Estado de Detalle**: Representa la condicion de la vista: carga, exito, ausencia de serie,
  indicador invalido, datos desactualizados o servicio no disponible.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: El 95 % de usuarios de prueba puede abrir el detalle de un indicador desde el
  dashboard y encontrar ultimo valor, unidad, fecha efectiva y fuente en menos de 20 segundos.
- **SC-002**: Para indicadores con al menos dos observaciones validas, el 100 % de las vistas de
  detalle muestra variacion absoluta y porcentual calculada contra la observacion anterior
  disponible.
- **SC-003**: El 100 % de las filas de la serie reciente visible presenta fecha y valor en orden
  cronologico, sin valores estimados ni filas generadas para fechas sin observacion.
- **SC-004**: El 100 % de los estados requeridos, carga, exito, ausencia de serie, indicador
  invalido, datos desactualizados y servicio no disponible, puede verificarse mediante escenarios
  de aceptacion independientes.
- **SC-005**: En pruebas de accesibilidad, una persona puede recorrer la tabla de serie reciente
  solo con teclado y comprender encabezados, fechas, valores y unidad sin depender de color.
- **SC-006**: En el percentil 75 de sesiones evaluadas, la vista muestra contenido util o un estado
  explicativo en menos de 2,5 segundos despues de seleccionar un indicador.

## Assumptions

- La feature se limita a la vista de detalle de un indicador y parte desde un dashboard existente
  o especificado por otra feature.
- La serie reciente corresponde al conjunto reciente disponible para el indicador en la fuente
  oficial, sin seleccion manual de ano ni rangos personalizados.
- La observacion anterior para la variacion es la observacion valida inmediatamente anterior dentro
  de la serie reciente disponible.
- Los doce indicadores soportados y sus unidades son los definidos por la constitucion vigente.
- Los estados de dato desactualizado pueden basarse en la fecha de ultima consulta, la fecha
  efectiva y la politica de frescura definida durante la planificacion.
- La feature no incorpora autenticacion, favoritos, alertas, exportacion, graficos, comparador ni
  conversor.
