# Time Report — Estudio de Arquitectura

Plan de trabajo v0.4 · 2026-10-01 · Estudio en **San Miguel de Tucumán, Argentina** · ~20 usuarios

---

## 1. Objetivo

Medir las horas profesionales dedicadas a cada **proyecto y etapa**, separando **horas ordinarias** y **horas extra**, valorizarlas con el valor hora de cada miembro y producir:

- indicadores de producción por proyecto, etapa y miembro (semanales y mensuales);
- el volumen de horas extra que demanda el cronograma de cada proyecto;
- la liquidación mensual de cada miembro (incluido presentismo);
- el control de asistencia.

El control de horas es **interno**: al cliente se le cotizan honorarios por etapa.

---

## 2. Perfiles de usuario

| Perfil | Qué puede hacer |
|---|---|
| **Profesional** | Ingresar con su Gmail. Cargar los **rangos horarios del día de hoy** en los proyectos asignados. Ver su calendario, su historial y **su propia liquidación mensual**. |
| **Manager** | Todo lo anterior, más: gestionar miembros y condiciones de contratación, proyectos, etapas, honorarios, asignaciones, valores hora, feriados y ausencias. Habilitar cargas tardías. Ver todos los informes y recibir alertas. |

Valores hora de otros miembros y honorarios: **solo el manager**.

---

## 3. Carga diaria por rangos horarios

### Pantalla "Hoy"

```
┌────────────────────────────────────────────────────────────────────────┐
│  Hoy: lunes 5 de octubre de 2026                      Día hábil          │
│  Horario contratado hoy: 8 h       Mes: 72,0 / 168,0 h  █████░░░░  43%  │
│  ⚠ Si no cargás tus horas hoy, el día queda registrado como            │
│    inasistencia no justificada.                                        │
│                                                                        │
│  08   09   10   11   12   13   14   15   16   17   18   19   20   21    │
│       ███████████████████       ███████████████████ ▒▒▒▒▒▒▒▒▒▒▒         │
│       Casa Ruiz · Antep.        Ed. Belgrano · P.Ejec.  extra           │
│                                                                        │
│  09:00 – 13:00   Casa Ruiz · Anteproyecto             4,0 h  Ordinaria  │
│  14:00 – 18:00   Edificio Belgrano · P. Ejecutivo     4,0 h  Ordinaria  │
│  18:00 – 20:30   Edificio Belgrano · P. Ejecutivo     2,5 h  EXTRA      │
│                                                                        │
│  + Nuevo rango  [desde 20:30 ▼] [hasta 21:00 ▼] [Proyecto ▼] [Etapa ▼]  │
│                                                                        │
│  Ordinarias: 8,0 / 8,0 h         Extra: 2,5 h            [Guardar]      │
└────────────────────────────────────────────────────────────────────────┘
```

- La carga se hace en **rangos horarios** ("paquetes de horas"): desde – hasta + proyecto + etapa. Un día entero en un mismo proyecto es un solo rango (ej. 09:00 – 17:00).
- Se selecciona en una **línea de tiempo** del día (arrastrando sobre la barra) o con los desplegables desde/hasta.
- Fracciones de **media hora** (09:00, 09:30 …). Los rangos **no pueden superponerse**.
- Proyectos: solo los activos asignados a la persona, más una categoría interna ("Estudio / Administración / Formación").
- **Etapa**: si el proyecto tiene una sola etapa en curso se asigna sola; si hay varias, el profesional elige.
- Nota opcional por rango.
- **Calendario mensual** personal: verde = completo, amarillo = parcial, rojo = inasistencia, naranja = carga tardía, azul = ausencia justificada / feriado, gris = no laborable.

### Ordinarias y extra (automático, por franja horaria)

- Cada miembro tiene definida en su perfil una **franja horaria ordinaria por día** (ver sección 5).
- **Rango dentro de la franja → ordinaria. Rango fuera de la franja → extra.** Si un rango cruza el borde de la franja, el sistema lo divide solo (ej. franja 9–18, rango 15:00–20:00 → 15:00–18:00 ordinaria + 18:00–20:00 extra).
- **Almuerzo**: las franjas largas incluyen **1 h de almuerzo** (ej. 9–18 = 8 h ordinarias). Las horas ordinarias del día tienen como máximo las contratadas (franja − almuerzo). Si alguien trabaja durante el almuerzo, las horas dentro de la franja que superan las contratadas del día **cuentan como extra** (ej. 9–18 sin cortar = 8 h ordinarias + 1 h extra).
- **Sin tope de horas extra.**
- Ordinarias solo de **lunes a viernes**. **Sábado** (el estudio está cerrado), **domingo, feriado**, o día sin franja contratada: todo es extra.
- Durante el **período de prueba** las horas extra **se cargan** (cuentan en las horas de los proyectos) pero **no se liquidan**.
- Como cada rango tiene proyecto y etapa, queda registrado **qué proyecto generó cada hora extra** (volumen de extras que demanda el cronograma).

### Regla "solo hoy"

- Validada en el **servidor**, en hora argentina (UTC-3). No se aceptan fechas pasadas ni futuras.
- Propuesta: tampoco se aceptan rangos que **empiecen después de la hora actual**. Ej.: a las 10:00 se puede cargar 09:00–10:00, pero no 14:00–18:00, porque esas horas todavía no se trabajaron. Así nadie deja cargada la tarde a la mañana y después no la cumple.
- Se puede editar lo cargado hasta las **23:59**, cuando el día se cierra.
- **Trabajo pasada la medianoche**: se carga al día siguiente (ej. 00:00 – 01:30), como extra.
- **Carga tardía**: solo si el manager habilita ese día (persona + fecha + motivo). Queda registrada como **"carga tardía"**.

---

## 4. Asistencia y presentismo

- **Día laborable contratado sin carga al cierre = inasistencia no justificada** (generada automáticamente a las 23:59).
- **Avisos al profesional**: cartel permanente en "Hoy" + recordatorio por email por la tarde y antes del cierre si no cargó.
- **Manager**, cada mañana: inasistencias del día anterior → dejar como no justificada, justificar, o habilitar carga tardía.
- **Ausencias justificadas** (las registra el manager): vacaciones, enfermedad, licencias. No generan inasistencia, **se pagan** y **cuentan como horas cumplidas** (no hacen perder el presentismo).
- **Día con menos horas que las contratadas**: se acepta; se liquidan las horas efectivamente trabajadas.

### Presentismo

- **Monto fijo, igual para todos**, adicional en la liquidación mensual (se configura en un solo lugar).
- Solo para perfiles con la opción **"Computa presentismo"** activada (los freelance no lo computan) y que **no estén en período de prueba**.
- Se gana si en el mes el miembro **completa todas sus horas ordinarias contratadas** (las ausencias justificadas cuentan como cumplidas), sin inasistencias no justificadas ni permisos especiales.
- Se pierde con **3 cargas tardías en el mes**.
- En "Hoy" y en su liquidación, el profesional ve el estado: "Presentismo: en curso / perdido (motivo)".

---

## 5. Perfil del miembro (lo carga el manager)

Con **fecha de vigencia** (un cambio no altera los meses anteriores):

| Campo | Valores |
|---|---|
| Tipo de relación | Relación de dependencia · Monotributo · **Freelance** |
| Fecha de ingreso | Fecha |
| Período de prueba | Hasta (por defecto: ingreso + 3 meses, editable). Durante la prueba: **sin presentismo**; las horas extra se cargan pero **no se liquidan** |
| Horario ordinario | **Franja horaria por día hábil** (lunes a viernes), con o sin almuerzo — ver abajo |
| Jornada | **Completa** (40 h semanales) o **reducida** (menos de 40 h) — se calcula sola según el horario, editable |
| Valor hora | $ en pesos, con fecha de vigencia |
| Multiplicador de horas extra | **×1,5** jornada completa · **×1,25** jornada reducida (por defecto según la jornada, editable). El mismo multiplicador aplica en sábados, domingos y feriados |
| Computa presentismo | Sí / No (por defecto No para freelance) |

### Definición del horario ordinario

```
┌──────────────────────────────────────────────────────────────┐
│  Horario ordinario                 Vigente desde: 01/10/2026  │
│                                                              │
│  [✓] Todos los días igual                                     │
│      Desde [09:00 ▼] Hasta [18:00 ▼]  [✓] 1 h almuerzo  = 8 h │
│                                                              │
│  Lunes      [✓]  08:00 – 18:00   [✓] almuerzo   = 9 h        │
│  Martes     [✓]  08:00 – 18:00   [✓] almuerzo   = 9 h        │
│  Miércoles  [✓]  09:00 – 17:00   [✓] almuerzo   = 7 h        │
│  Jueves     [✓]  09:00 – 17:00   [✓] almuerzo   = 7 h        │
│  Viernes    [✓]  09:00 – 18:00   [✓] almuerzo   = 8 h        │
│                       Total semanal: 40 h → Jornada completa  │
│                                              [Guardar]        │
└──────────────────────────────────────────────────────────────┘
```

- **Horario regular del estudio: 09:00 a 18:00 con 1 h de almuerzo = 8 h.** Es el valor por defecto con la casilla "Todos los días igual".
- Desmarcando la casilla se define **cada día por separado**: qué días viene, en qué franja (inicio desde las 08:00 si corresponde) y si incluye almuerzo. Esto cubre jornadas de 9 h algunos días y 7 h otros.
- El sistema muestra las horas de cada día y el **total semanal**, y propone la jornada (completa / reducida).
- El horario se puede **modificar en cualquier momento**; el cambio rige desde la fecha que se indique (los meses anteriores no cambian).

| Miembro | Lun | Mar | Mié | Jue | Vie | Semanal | Jornada | Extra |
|---|---|---|---|---|---|---|---|---|
| Ana | 9–18 (8 h) | 9–18 (8 h) | 9–18 (8 h) | 9–18 (8 h) | 9–18 (8 h) | 40 h | Completa | ×1,5 |
| Bruno | 8–18 (9 h) | 8–18 (9 h) | 9–17 (7 h) | 9–17 (7 h) | 9–18 (8 h) | 40 h | Completa | ×1,5 |
| Carla | 9–13 (4 h) | 14–18 (4 h) | 9–13 (4 h) | — | — | 12 h | Reducida | ×1,25 |
| Diego | 14–18 (4 h) | — | 14–18 (4 h) | — | 14–18 (4 h) | 12 h | Reducida | ×1,25 |

- **Horas contratadas del mes** = suma automática de las franjas de cada día hábil del mes, descontando feriados. El manager puede ajustar un mes puntual con motivo.
- **Feriados**: si el feriado cae en un día con franja contratada, se paga (todos los tipos de relación, incluido freelance).

> Nota a verificar con contador/a: para personal en **relación de dependencia**, la LCT (art. 201) fija recargos mínimos de 50% (días comunes) y 100% (sábados después de las 13 h, domingos y feriados). Un ×1,25 o un ×1,5 en fin de semana podría quedar por debajo del mínimo legal. El sistema puede mostrar una advertencia en esos casos.

---

## 6. Liquidación mensual

| Concepto | Cálculo |
|---|---|
| Horas ordinarias trabajadas | horas × valor hora |
| Feriados pagos | horas de la franja contratada en esos días × valor hora |
| Ausencias justificadas | horas de la franja contratada en esos días × valor hora |
| Horas extra | horas × valor hora × multiplicador (1,5 / 1,25) — en período de prueba se informan pero no se liquidan |
| Presentismo | monto fijo igual para todos, si corresponde — no aplica en período de prueba |
| **Total** | |

- Cada profesional ve la suya; el manager ve todas y las exporta a Excel.
- Para dependencia es un **insumo para el contador/a** (el sistema no calcula aportes ni recibo de sueldo); para monotributo/freelance, el importe a facturar.
- **Costo por proyecto** = horas imputadas × valor hora (× multiplicador en extras). Feriados, ausencias pagas y presentismo son **costo de estructura**, no se imputan a proyectos. Sin factor de cargas sociales.

---

## 7. Proyectos y etapas

- **Etapas estándar** (lista editable por el manager), en orden:
  1. Factibilidad *(solo algunos proyectos)*
  2. Masterplan *(solo algunos proyectos)*
  3. Anteproyecto
  4. Proyecto Municipal
  5. Proyecto Ejecutivo
- Al crear un proyecto, el manager elige qué etapas incluye y carga los **honorarios cotizados de cada etapa, en pesos**, con fecha de cotización.
- Estado de cada etapa: pendiente / en curso / terminada.
- **Ajustes de honorarios** con fecha (por inflación o ampliaciones de encargo).
- Sin horas objetivo por etapa por ahora.
- **Dirección de obra**: se incorporará más adelante en un **módulo DT** separado.

---

## 8. Informes, indicadores y alertas

**Por proyecto y etapa**
- Horas ordinarias y extra por semana, mes y acumuladas.
- **Horas extra por proyecto/etapa**: volumen que demanda el cronograma y % sobre el total.
- Costo acumulado; honorarios vs. costo → margen y % consumido.
- Desglose por miembro.
- Base para cotizar: horas y costo reales por etapa de proyectos terminados.

**Por miembro**
- Horas trabajadas vs. contratadas; horas extra y su costo.
- Distribución por proyecto y etapa.
- Asistencia: inasistencias, ausencias justificadas, cargas tardías, estado del presentismo.
- Liquidación mensual.

**Del estudio**
- Ranking de proyectos más demandantes (horas, horas extra, costo).
- Horas imputables a proyectos vs. internas.
- Evolución semanal y mensual.
- Total de horas extra, costo extra y presentismos del mes.
- Exportación a Excel / PDF.

**Alertas al manager** (en el sistema + email)
- Miembro que **supera 20 horas extra en el mes**.
- Inasistencias del día anterior.
- Miembro que llega a 2 cargas tardías en el mes (a una de perder presentismo).
- Etapas que superan el 70% / 100% de sus honorarios.

---

## 9. Arquitectura técnica

- **Aplicación web** responsive, usable desde el celular (instalable como app).
- **Next.js** (frontend + backend) + **Supabase** (PostgreSQL + autenticación).
- **Ingreso con Google** con las cuentas de **Gmail personales** y **lista blanca** (solo los emails dados de alta por el manager). Al desactivar un miembro pierde el acceso; sus datos quedan. Alternativa: enlace por email.
- **Tareas programadas**: cierre del día a las 23:59 (inasistencias), recordatorios, alertas, cálculo de presentismo a fin de mes.
- **Costos estimados**: USD 0–45/mes (Supabase Pro ~USD 25 para backups automáticos; hosting gratuito en Netlify / Cloudflare Pages o Vercel Pro ~USD 20).
- Código en una carpeta local **fuera de Google Drive** (ej. `C:\dev\time-report`) con git; documentos de diseño en esta carpeta.

### Feriados (Tucumán)

- Calendario **nacional** + **provinciales de Tucumán** (ej. 24 de septiembre, a confirmar con el calendario oficial), precargados cada año.
- **Días no laborables** optativos (ej. Jueves Santo): el manager marca si el estudio trabaja.
- Cierres propios del estudio.

### Modelo de datos

```
usuarios              id, nombre, email, perfil (profesional|manager), activo
condiciones_contrato  usuario_id, vigente_desde, vigente_hasta,
                      tipo_relacion (dependencia|monotributo|freelance),
                      fecha_ingreso, fin_periodo_prueba,
                      jornada (completa|reducida), multiplicador_extra (1.5|1.25|otro),
                      computa_presentismo
franjas_horario       condicion_id, dia_semana (lun…vie), hora_inicio, hora_fin,
                      almuerzo_horas (0|1), horas_ordinarias (calculadas)
valores_hora          usuario_id, valor_ars, vigente_desde
ajustes_mes           usuario_id, mes, horas_contratadas_override, motivo
proyectos             id, codigo, nombre, cliente, tipologia?, superficie_m2?,
                      estado (activo|pausado|cerrado), fecha_inicio, fecha_fin
etapas_catalogo       id, nombre, orden, opcional
etapas_proyecto       proyecto_id, etapa_id, honorarios_ars, fecha_cotizacion,
                      estado (pendiente|en_curso|terminada)
ajustes_honorarios    proyecto_id, etapa_id, fecha, monto_ars, motivo
asignaciones          usuario_id, proyecto_id, desde, hasta
feriados              fecha, descripcion, ambito (nacional|tucuman|estudio),
                      tipo (inamovible|trasladable|puente|no_laborable), el_estudio_trabaja
registros_horas       id, usuario_id, proyecto_id, etapa_id, fecha,
                      hora_inicio, hora_fin (múltiplos de 30 min), horas (calculadas),
                      categoria (ordinaria|extra) (calculada), nota,
                      creado_en, creado_por, carga_tardia
desbloqueos           usuario_id, fecha, motivo, otorgado_por, usado
ausencias             usuario_id, fecha, tipo (no_justificada|vacaciones|enfermedad|
                      licencia|permiso_especial|otra), paga, nota, registrado_por
presentismo_mes       usuario_id, mes, estado (ganado|perdido), motivo, monto
configuracion         monto_presentismo, max_cargas_tardias (3),
                      umbral_horas_extra_mes (20), multiplicadores por defecto,
                      horario_regular (09:00–18:00), meses_periodo_prueba (3)
auditoria             quién, qué, cuándo, valor anterior / nuevo
```

---

## 10. Pendientes de confirmar

Resueltos: mismo multiplicador en fines de semana y feriados · sábado completo es extra (estudio cerrado) · ordinarias por franja horaria del perfil, con 1 h de almuerzo en las franjas largas · jornadas distintas por día (9 h / 7 h) · presentismo con monto único; las ausencias justificadas no lo hacen perder · período de prueba de 3 meses: extras se cargan pero no se liquidan · freelance cobran feriados en días contratados.

1. ~~Trabajar en el almuerzo~~ → **cuenta como extra**.
2. ~~Jornada completa / reducida~~ → **40 h semanales = completa (×1,5); menos = reducida (×1,25)**.
3. **Rangos futuros**: ¿se impide cargar a la mañana horas de la tarde que todavía no se trabajaron? (ver sección 3)

---

## 11. Etapas de desarrollo

| Etapa | Contenido |
|---|---|
| **0. Definición** | Cerrar pendientes · **Prototipo visual** navegable (hecho: `prototipo/time-report.html`) (pantalla "Hoy" con rangos, calendario, liquidación, tablero del manager) · Reunir lista de miembros, condiciones, valores hora y proyectos. |
| **1. Versión inicial** | Ingreso con Google + lista blanca · Gestión de miembros, condiciones, valores hora, proyectos, etapas, asignaciones y feriados · Carga diaria por rangos "solo hoy" · Clasificación ordinaria/extra · Cierre diario con inasistencias · Ausencias y cargas tardías · Presentismo · Calendario personal · Liquidación mensual + Excel. |
| **2. Indicadores** | Tableros por proyecto/etapa, miembro y estudio · Honorarios vs. costo y margen · Informes semanales · Recordatorios por email · Alertas. |
| **3. Mejoras** | Auditoría completa · Importación masiva desde Excel · Histórico para cotizar · Ajustes de honorarios. |
| **4. Módulo DT** | Dirección de obra. |
