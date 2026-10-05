# Plantillas de datos iniciales

Estas planillas sirven para cargar de una vez los datos con los que arranca el sistema.
Se abren con **Excel** (doble clic) o con **Google Sheets** (Archivo > Importar).

## Cómo completarlas

1. Abrí cada archivo, dejá la **primera fila (encabezados) sin tocar**.
2. Borrá las filas que empiezan con `EJEMPLO` y cargá los datos reales.
3. Guardá en el mismo formato (CSV). Si Excel pregunta, elegí "CSV (delimitado por punto y coma)". También sirve guardarlo como .xlsx o exportarlo desde Google Sheets.

## Archivos

| Archivo | Qué contiene | Formato de los campos |
|---|---|---|
| `miembros.csv` | Una fila por persona | `perfil`: profesional / manager · `tipo_relacion`: dependencia / monotributo / freelance · fechas `dd/mm/aaaa` · `fin_periodo_prueba` vacío si ya pasó · `valor_hora_ars` sin puntos ni signo $ · `computa_presentismo`: si / no · horarios `hh:mm`; días que no viene, vacíos · `almuerzo`: si / no |
| `proyectos.csv` | Una fila **por etapa** de cada proyecto | `estado`: activo / pausado / cerrado · `etapa`: Factibilidad, Masterplan, Anteproyecto, Proyecto Municipal, Proyecto Ejecutivo (solo las que incluye) · `honorarios_ars` sin puntos · `estado_etapa`: pendiente / en_curso / terminada |
| `asignaciones.csv` | Qué proyectos ve cada persona en su menú | Una fila por persona + proyecto |
| `configuracion.csv` | Parámetros generales | Completar **monto_presentismo**; revisar el resto |
| `cierres_estudio.csv` | Días que el estudio cierra además de los feriados | Los feriados nacionales y de Tucumán se cargan automáticamente |

La jornada (completa / reducida) y el multiplicador de horas extra los calcula el sistema a partir del horario.

Los valores hora y los honorarios son confidenciales: estos archivos solo deben compartirse con el manager.
