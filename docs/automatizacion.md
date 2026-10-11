# Automatización del perfil

## Qué hace

Una GitHub Action ejecuta el script `scripts/actualizar_perfil.py`, que consulta la API de GitHub y regenera automáticamente algunas secciones del README del perfil de SIS-INF.

**Bloques que actualiza:**

| Bloque | Contenido |
|---|---|
| `cifras` | Línea de insignias con la cantidad de proyectos, estrellas, forks, colaboradores y aportes. |
| `proyectos` | Listado de los proyectos de la organización. |
| `lenguajes` | Lenguajes de programación usados en los proyectos. |
| `colaboradores` | Personas que contribuyen en los proyectos. |
| `participar` | Cómo contribuir a los proyectos de la comunidad. |

**Archivos que modifica:** `README.md` (español) y `README.en.md` (inglés).

**Cada cuánto se ejecuta:** una vez al día, a las 06:00 UTC (02:00 en Bolivia), mediante el workflow `.github/workflows/actualizar-perfil.yml`. También puede ejecutarse manualmente desde la pestaña **Actions** con el botón **Run workflow**. Si el contenido generado no cambió, no se crea ningún commit.

**Qué repositorios considera:** solo los repositorios **públicos** de la organización. Los repositorios privados nunca aparecen en el perfil.

**No se editan a mano:** cada bloque automático está delimitado por un par de marcadores:

```markdown
<!-- auto:proyectos -->
(contenido generado)
<!-- /auto:proyectos -->
```

Todo lo que está entre `<!-- auto:nombre -->` y `<!-- /auto:nombre -->` se reemplaza en cada ejecución, por lo que cualquier cambio manual en esa zona se pierde. Para cambiar ese contenido hay que modificar el script, no el README. El texto que está fuera de los marcadores sí se edita a mano con normalidad.

## Cómo excluir un repositorio

Por defecto, todos los repositorios públicos de la organización aparecen en el perfil. Hay dos formas de excluir uno.

### Opción 1: agregar el topic `no-perfil` en GitHub

1. Abre el repositorio en GitHub.
2. En la columna derecha, junto a **About**, pulsa el ícono de engranaje.
3. En el campo **Topics**, escribe `no-perfil` y pulsa Enter.
4. Pulsa **Save changes**.

Es la opción recomendada cuando tienes permisos de administración sobre ese repositorio, porque no requiere cambios en este repositorio.

### Opción 2: agregarlo a `repos_excluidos` en `config/perfil.json`

Agrega el nombre del repositorio, sin el nombre de la organización, a la lista `repos_excluidos`:

```json
{
  "repos_excluidos": [
    "repositorio-de-prueba",
    "otro-repositorio"
  ]
}
```

Este cambio se propone con un Pull Request a este repositorio. Úsala cuando no tengas permisos de administración sobre el repositorio que quieres excluir.

En ambos casos, el repositorio deja de aparecer en el perfil a partir de la siguiente ejecución de la Action. Para que vuelva a aparecer, quita el topic o elimina su nombre de la lista.
