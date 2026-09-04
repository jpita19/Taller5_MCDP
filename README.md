# Taller Semana 5 — El pipeline que nunca se escribió 🤖

**Empieza leyendo [`ENUNCIADO_TALLER.md`](ENUNCIADO_TALLER.md).**

## Qué hay aquí
```
pipeline-ci-ml/
├── ENUNCIADO_TALLER.md   <- LÉEME PRIMERO
├── src/inventario.py     <- el código (funciona)
├── tests/test_inventario.py  <- 5 pruebas (pasan)
├── requirements.txt
└── .gitignore
```

Falta lo más importante, y es tu trabajo: **`.github/workflows/ci.yml`**.

## Arranque
```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
ruff check .                     # All checks passed!
pytest                           # 5 passed
```

## Tu trabajo (resumen — el detalle está en el ENUNCIADO)
1. Corre el proyecto local y confirma que pasa.
2. Escribe `.github/workflows/ci.yml` (push/PR, ubuntu, checkout, python, install, ruff, pytest).
3. Súbelo a GitHub y velo ponerse verde en la pestaña Actions.
4. Rómpelo a propósito (un test y un lint) para ver el rojo, y arréglalo.
5. Documenta con capturas y trabaja con ≥5 commits.

## Pipeline CI — Taller5_MCDP

*Taller realizado por:*

 Angie Valentina Coba, Daniel Alfonso Gutiérrez Novoa y Jorge Luis Pitalúa 

Este repositorio automatiza la verificación de un módulo de reposición de inventario: cada cambio se revisa automáticamente con linter y pruebas antes de llegar a producción.

### ¿Qué hace el workflow?

El archivo se configuró para que el proceso se activara al hacer push y pull request. Como se solicitó, la máquina que se usó fue ubuntu-latest.

Los pasos que ejecuta, en orden, son:
1. Checkout código: trae el código del repositorio hacia el runner de GitHub, para que los demás pasos tengan el proyecto disponible.
2. Instalar Python: Se instala Python versión 3.12
3. Instalar dependencias: por medio de un pip install requirements.txt se instalan todas las dependencias necesarias para el proyecto.
4. Correr linter: ejecuta `ruff check .`, que revisa que el código no tenga errores de estilo ni imports sin usar.
5. Correr pruebas: ejecuta `pytest`, que corre las 5 pruebas automatizadas y confirma que la lógica de reposición funciona correctamente.

### Evidencia

Puedes ver el historial completo de corridas en [Actions](https://github.com/jpita19/Taller5_MCDP/actions).

#### Historial de corridas (verde → rojo → verde)
![historial](screenshots/verde-historial.png)

#### Rojo por test
![rojo-test](screenshots/rojo-test.png)

#### Rojo por lint
![rojo-lint](screenshots/rojo-lint.png)

### La defensa: PR con un test que falla

Si un compañero sube un Pull Request con un test que falla, el workflow se dispara
automáticamente gracias a `on: pull_request` — nadie tiene que acordarse de correr
las pruebas a mano, que fue justamente la falla que originó este taller. GitHub
ejecuta el pipeline completo y muestra el resultado directamente en la página del
PR, con un check en rojo visible para todo el equipo antes de que alguien pueda
aprobar el merge.

Esto protege el proyecto porque convierte un error de código en algo imposible de
ignorar: en vez de descubrirlo después por un reclamo (como las tres tiendas que
se quedaron sin stock en la historia del enunciado), el pipeline lo atrapa en el
momento exacto en que se propone el cambio, antes de que llegue a producción.