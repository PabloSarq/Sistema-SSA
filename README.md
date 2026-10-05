# Sistema SSA · Time Report

Registro de horas profesionales del estudio: cada miembro carga a diario sus horas por proyecto y etapa, el sistema separa horas ordinarias y extra según su horario contratado, controla asistencia y presentismo, y genera la liquidación mensual y los indicadores de producción por proyecto.

## Estado

**Etapa 0 · Definición.** El diseño funcional está en [PLAN.md](PLAN.md) y hay un prototipo navegable con datos de ejemplo. Todavía no hay código de la aplicación.

## Contenido

| Carpeta / archivo | Qué hay |
|---|---|
| [PLAN.md](PLAN.md) | Plan completo: reglas de carga, horas extra, asistencia, presentismo, liquidación, modelo de datos y etapas de desarrollo |
| [prototipo/](prototipo/) | `time-report.html`: prototipo navegable (descargalo y abrilo con doble clic en el navegador) |
| [plantillas/](plantillas/) | Planillas CSV para la carga inicial de datos, solo con filas `EJEMPLO`. Ver [LEEME](plantillas/LEEME.md) |
| [referencias-diseno/](referencias-diseno/) | Logos de SSA en SVG y PNG para el front |

## Datos privados

Este repositorio es público. Los datos reales del estudio (nombres del equipo, emails, valores hora, honorarios) **no se suben**: se completan en las planillas fuera del repo o en una carpeta `datos-privados/`, que git ignora.

## Próximos pasos

1. Cerrar las definiciones pendientes y validar el prototipo con el equipo.
2. Etapa 1: aplicación web con Next.js + Supabase (login con Google y lista blanca, carga diaria por rangos, asistencia, presentismo y liquidación).

Ver la sección "Etapas de desarrollo" de [PLAN.md](PLAN.md).
