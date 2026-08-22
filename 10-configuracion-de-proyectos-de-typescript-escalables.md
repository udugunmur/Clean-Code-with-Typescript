# Parte 3: TypeScript Avanzado y Aplicaciones en el Mundo Real

## Capítulo 10: Configuración de Proyectos de TypeScript Escalables

### Sección: Introducción

Has aprendido la sintaxis y los patrones avanzados de TypeScript; ahora es el momento de aplicar ese conocimiento a proyectos del mundo real. Este capítulo cierra la brecha entre la teoría y la práctica guiándote a través de la configuración de un entorno TypeScript listo para producción que sea escalable, mantenible y optimizado para la colaboración en equipo.

Irás más allá de escribir código para tomar decisiones arquitectónicas a las que los equipos reales se enfrentan a diario: elegir entre estrategias de *monorepo* y *polyrepo*, seleccionar las herramientas adecuadas para la gestión de paquetes y aplicar estándares de calidad de código de forma automática. Estas son las prácticas fundamentales que permiten a los equipos profesionales construir y escalar aplicaciones *full stack* con total confianza.

En este capítulo, crearás un espacio de trabajo de desarrollo completo utilizando Nx, lo configurarás con pnpm para una gestión eficiente de dependencias y automatizarás la aplicación de estándares de calidad mediante Git hooks. También aprenderás a definir estándares de equipo, gestionar configuraciones de entorno y optimizar tu flujo de trabajo de desarrollo para lograr productividad a largo plazo.

Al final de este capítulo, dispondrás de una base de proyecto de nivel de producción construida sobre valores predeterminados inteligentes y herramientas probadas en batalla. Más importante aún, comprenderás el razonamiento detrás de cada elección, habilidades críticas para liderar decisiones técnicas en cualquier equipo o proyecto.

En este capítulo, cubriremos los siguientes temas principales:

- De la especificación del producto a la arquitectura técnica
- Elección de una estrategia de repositorio para tu proyecto
- Nx: la elección profesional
- Comandos y flujos de trabajo esenciales de Nx
- Automatización de la calidad del código con Git hooks

---

### Sección: Requisitos técnicos

En este capítulo, necesitarás las siguientes herramientas y tecnologías instaladas en tu sistema:

- Node.js (v18 o posterior)
- pnpm (v8 o posterior)
- Nx CLI (`npx create-nx-workspace@latest`)
- Git
- VS Code (recomendado)
- Conocimientos básicos de TypeScript y comandos de terminal

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro. Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: De la especificación del producto a la arquitectura técnica

Es tentador pensar que crear software se reduce a escribir código. Sin embargo, existe una brecha crucial entre qué estamos construyendo, por qué lo construimos y cómo lo construimos. Las decisiones tomadas en este espacio a menudo determinan si un proyecto tiene éxito o fracasa.

El desarrollo de software es similar a la fabricación de cualquier otro producto, como un automóvil o un teléfono. Conocer los detalles técnicos es importante, pero ¿para quién lo estamos construyendo? ¿Qué problemas debe resolver? ¿Cuáles son los plazos previstos? ¿Es este un modelo insignia o una actualización de uno existente? Estas preguntas dan forma a cada decisión técnica que tomes.

Esta sección te guía a través del proceso de toma de decisiones colaborativo que transforma los requisitos del producto en arquitectura técnica. Exploraremos la fase de planificación estratégica: qué tipo de proyecto estamos construyendo, cómo crecerá, quién trabajará en él y con qué frecuencia se publicará.

#### Comprensión de las especificaciones del producto

Una especificación de producto (*product spec*) es un documento conciso que describe qué se está construyendo y por qué. No es código; es una fuente compartida de verdad entre las partes interesadas técnicas y no técnicas.

Una buena especificación de producto define:
- Las características principales del producto.
- Los usuarios a los que sirve.
- Los problemas que resuelve (tanto para los usuarios como para el negocio).
- Las restricciones relevantes (plazos, tecnologías, presupuesto).

Sin una especificación clara, las bases de código suelen evolucionar sin dirección, lo que genera retrabajo y deuda técnica.

##### Ejemplo práctico: Especificación de la plataforma DevJobs

> **Especificación de la plataforma DevJobs**: Construir una bolsa de trabajo moderna específicamente para desarrolladores. Los usuarios deben poder explorar y filtrar ofertas de trabajo por tecnología, nivel de experiencia y ubicación. Las empresas pueden publicar ofertas con requisitos técnicos claros y transparencia salarial. Los desarrolladores pueden crear perfiles básicos que muestren sus habilidades y su nombre de usuario de GitHub.
>
> La plataforma debe admitir la autenticación de usuarios y permitir que las empresas gestionen sus propias publicaciones de trabajo. El alcance inicial se centra únicamente en la web (*desktop-first*), sin necesidad inmediata de actualizaciones en tiempo real ni integraciones con APIs externas. El lanzamiento objetivo está previsto en 3 meses con un equipo de 3 a 5 ingenieros.

A partir de esta especificación, surgen decisiones técnicas críticas:
- ¿Necesitamos múltiples aplicaciones o una estructura monolítica?
- ¿Cómo debemos organizar el código para compartir tipos y validaciones?
- ¿Qué herramientas ayudarán a nuestro equipo a colaborar eficazmente?
- ¿Cómo estructuramos la base de código para el crecimiento futuro?

#### De la especificación del producto a la arquitectura técnica

Antes de escribir una sola línea de código, necesitamos un plan técnico: una arquitectura de alto nivel lo suficientemente concreta para guiar las decisiones, pero no tan detallada como para quedar atrapados en la teoría.

El desglose se realiza en tres pasos:

##### Paso 1: Revisar y desglosar la especificación

Requisitos funcionales (DevJobs):
- Creación de cuentas para desarrolladores y empresas.
- Creación de perfiles de desarrollador con habilidades y usuario de GitHub.
- Publicación y gestión de ofertas de trabajo por parte de las empresas.
- Exploración y filtrado de puestos de trabajo por parte de los candidatos.
- Autenticación y autorización basada en roles.

Requisitos no funcionales (DevJobs):
- Resultados de búsqueda rápidos (filtrado y consultas eficientes).
- Escalabilidad suficiente para manejar grandes volúmenes de ofertas y picos de tráfico.
- Autenticación segura y control de acceso basado en roles.
- Experiencia de usuario ágil y responsiva.

##### Paso 2: Añadir restricciones contextuales

- **Restricciones técnicas**: Volumen esperado de usuarios activos diarios, concurrencia máxima, tamaño y tipo de datos almacenados, y frecuencia de solicitudes entrantes (estimaciones mediante cálculos aproximados o *back-of-the-envelope calculations*).
- **Restricciones no técnicas**: Tamaño del equipo (3-5 desarrolladores), plazo de entrega (3 meses), y las habilidades y experiencia del equipo.

##### Paso 3: Registrar decisiones antes de codificar con ADRs

Los registros de decisiones de arquitectura (*Architecture Decision Records* o ADRs) garantizan que todos conozcan por qué se tomó una decisión y qué compensaciones (*trade-offs*) se consideraron.

Un ADR captura:
- **Contexto**: Por qué era necesaria la decisión.
- **Decisión**: Qué opción se eligió.
- **Consecuencias**: Compensaciones o acciones de seguimiento.

Ejemplo de ADR en `/docs/adr/0001-database-choice.md`:

```markdown
# ADR 0001: Database Choice ## Context The DevJobs platform requires storing job postings, company accounts, and developer profiles. The data is relational and includes filters such as technology, experience, and location. We also need support for queries that must remain performant as the dataset grows. ## Decision We will use PostgreSQL as the primary database. It supports relational data, indexing for fast queries, and has strong community and hosting support. ## Consequences Pros: Strong querying support, well-known by most developers, easy to host on cloud providers. Cons: Less flexible than NoSQL databases for unstructured data. We will review this decision if traffic or data patterns change significantly (e.g., very large-scale filtering, analytics-heavy use cases).
```

---

### Sección 2: Elección de una estrategia de repositorio para tu proyecto

La forma en que organizas tu base de código influye directamente en la colaboración, las integraciones CI/CD y la escalabilidad.

- **Monorepo**: Un único repositorio que contiene todo el código de múltiples proyectos o servicios (aplicaciones frontend, servicios backend, bibliotecas compartidas y utilidades).
- **Polyrepo**: Mantiene cada proyecto, servicio o biblioteca en su propio repositorio independiente.

#### Comparación de enfoques

Beneficios del Monorepo:
- **Herramientas compartidas**: Un único conjunto de scripts, herramientas de compilación y configuraciones.
- **Cambios atómicos**: Refactorización a través de múltiples proyectos en un solo commit.
- **Gestión simplificada de dependencias**: Facilidad para compartir y actualizar bibliotecas internas sin publicar paquetes intermedios.

Beneficios del Polyrepo:
- **Despliegues independientes**: Los equipos pueden desplegar servicios sin coordinar cambios en otros lugares.
- **Autonomía del equipo**: Cada equipo es propietario de su repositorio y puede adoptar flujos de trabajo específicos.
- **Diversidad tecnológica**: Mayor facilidad para combinar diferentes stacks, frameworks o lenguajes.

#### Decisión para DevJobs

Para DevJobs, adoptamos el enfoque de **monorepo**. Esto proporciona un lugar centralizado para gestionar el frontend, el backend y las librerías compartidas, manteniendo nuestros flujos de trabajo simples y consistentes para un equipo de 3 a 5 ingenieros.

---

### Sección 3: Nx: la elección profesional

Nx es un sistema de compilación y gestor de monorepos diseñado específicamente para proyectos TypeScript. Proporciona herramientas avanzadas, configuraciones inteligentes y soporte empresarial.

#### Por qué Nx destaca para TypeScript

- **Herramientas avanzadas**: Generadores integrados para crear aplicaciones, bibliotecas y configuraciones de forma consistente.
- **Compilaciones inteligentes**: Analiza el grafo de dependencias para compilar o probar únicamente los proyectos afectados por un cambio (*affected commands*).
- **Extensibilidad**: Ecosistema rico de plugins oficiales para React, Next.js, Express, NestJS y más.

#### Comparación con alternativas

- **Turborepo**: Muy enfocado en almacenamiento en caché y ejecución paralela. Nx ofrece capacidades de caché similares pero añade generadores de código integrados, visualización del grafo de proyectos y una integración más profunda con TypeScript.
- **Lerna**: Enfocado históricamente en el versionado y publicación de paquetes. Carece de compilaciones conscientes de dependencias o soporte nativo avanzado para TypeScript.
- **Rush**: Diseñado para organizaciones masivas con cientos de paquetes, pero con una sobrecarga de configuración mayor para equipos medianos o pequeños.
- **Yarn Workspaces**: Gestiona dependencias entre paquetes pero carece de orquestación avanzada de compilación y optimización de CI/CD.

#### Creación del espacio de trabajo Nx

Ejecuta el siguiente comando para inicializar el workspace de DevJobs con pnpm:

```bash
npx create-nx-workspace@latest devjobs --preset=ts --package-manager=pnpm
```

Opciones recomendadas en los prompts interactivos:
- **Prettier for code formatting**: `Yes`
- **CI provider**: `Skip`
- **Remote caching**: `Skip`

#### Estructura del espacio de trabajo

- `.vscode/`: Configuraciones de VS Code compartidas en el workspace.
- `node_modules/`: Paquetes instalados mediante enlaces simbólicos (*symlinks*) por pnpm.
- `packages/` (o `apps/` y `libs/`): Directorios de proyectos y bibliotecas.
- `nx.json`: Configuración principal de Nx y comportamientos predeterminados.
- `package.json`, `pnpm-lock.yaml`, `pnpm-workspace.yaml`: Definición del workspace y dependencias compartidas.

#### Establecimiento de la convención `apps/` y `libs/`

Para seguir la convención profesional estándar de Nx, eliminamos la carpeta `packages/` por defecto y creamos `apps/` y `libs/`:

```bash
rm -rf packages
```

```bash
mkdir apps libs
```

#### Instalación de plugins de Nx

Instalamos los plugins oficiales para Next.js y Express:

```bash
pnpm nx add @nx/next
pnpm nx add @nx/express
```

#### Generación de aplicaciones frontend y backend

##### Frontend con Next.js

```bash
pnpm nx g @nx/next:app apps/devjobs-frontend --style=css --tailwind
```

Selecciones recomendadas:
- Linter: `ESLint`
- Unit test runner: `Jest`
- E2E test runner: `Playwright`
- App Router: `Yes`
- `src/` directory: `Yes`

Iniciar el frontend:

```bash
pnpm nx serve devjobs-frontend
```

##### Backend con Express

```bash
pnpm nx g @nx/express:app apps/devjobs-backend
```

Selecciones recomendadas:
- Linter: `ESLint`
- Unit test runner: `Jest`

Iniciar el backend:

```bash
pnpm nx serve devjobs-backend
```

---

### Sección 4: Comandos y flujos de trabajo esenciales de Nx

En Nx, una tarea asignada a un proyecto se denomina **target** (por ejemplo: `dev`, `serve`, `build`, `test`). El patrón de ejecución es:

```bash
nx <target> <project-name>
```

Por ejemplo:

```bash
nx dev devjobs-frontend
```

Comandos principales utilizados en el proyecto DevJobs:

```bash
pnpm nx serve devjobs-backend
pnpm nx dev devjobs-frontend
pnpm nx build devjobs-frontend
pnpm nx test devjobs-backend
```

---

### Sección 5: Automatización de la calidad del código con Git hooks

Los Git hooks son scripts automatizados que se ejecutan en puntos clave del ciclo de vida de Git (como antes de hacer un commit o antes de hacer un push), garantizando que el código cumpla con los estándares del equipo.

- **Pre-commit**: Ideal para comprobaciones rápidas y ligeras sobre archivos preparados en el área de ensayo (*staged files*), como linting y formateo.
- **Pre-push**: Adecuado para comprobaciones más pesadas, como la ejecución de suites completas de pruebas unitarias o de integración.

#### Configuración de Husky y lint-staged

Guarda el estado actual de tu repositorio:

```bash
git add .
git commit -m "chore: setup Nx workspace with frontend and backend apps"
```

##### Paso 1: Instalar dependencias

```bash
pnpm add -D husky lint-staged -w
```

##### Paso 2: Inicializar Git (si no está inicializado)

```bash
git init
git add .
git commit -m "chore: initial commit"
```

##### Paso 3: Inicializar Husky

```bash
pnpm exec husky init
```

##### Paso 4: Configurar el hook `pre-commit`

```bash
echo "pnpm exec lint-staged" > .husky/pre-commit
chmod +x .husky/pre-commit
```

El archivo `.husky/pre-commit` resultante:

```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"
pnpm exec lint-staged
```

##### Paso 5: Configurar `.lintstagedrc.json`

Crea el archivo `.lintstagedrc.json` en la raíz del repositorio:

```json
{
  "*.{js,ts,tsx}": [
    "eslint --fix",
    "prettier --write"
  ],
  "*.json": "prettier --write",
  "*.md": "prettier --write"
}
```

#### Flujo de ejecución del commit

```bash
git add .
git commit -m "chore: update code"
```

1. Husky dispara el hook `pre-commit`.
2. `lint-staged` ejecuta ESLint y Prettier únicamente en los archivos que se encuentran en el área de preparación (*staged*).
3. Si los linters corrigen problemas de estilo automáticamente, los archivos preparados se actualizan antes de registrar el commit.
4. Si persisten errores no corregibles automáticamente, el commit se bloquea hasta su resolución.

#### Prueba del Git hook con un archivo de ejemplo

##### Paso 1: Crear un archivo de prueba

```bash
touch apps/devjobs-frontend/src/somefile.ts
```

Añadir código sin formatear:

```typescript
export function greet(name:string){console.log("Hello,"+name+"!")}
```

##### Paso 2: Preparar el archivo

```bash
git add apps/devjobs-frontend/src/somefile.ts
```

##### Paso 3: Ejecutar el commit

```bash
git commit -m "chore(frontend): add somefile to test Git hooks"
```

##### Paso 4: Verificar los cambios

El archivo se formatea automáticamente antes de guardarse en el historial de Git:

```typescript
export function greet(name: string) { console.log(`Hello, ${name}!`); }
```

---

### Sección: Resumen

En este capítulo, sentamos las bases para construir proyectos escalables de TypeScript de nivel profesional. Aprendiste a traducir una especificación funcional de producto en una arquitectura técnica sólida documentada con registros de decisiones de arquitectura (ADRs), evaluaste las ventajas estratégicas entre monorepos y polyrepos, y configuraste un espacio de trabajo modular con Nx y pnpm.

Además, implementamos la automatización de la calidad del código mediante Git hooks con Husky y `lint-staged`, asegurando que cada commit cumpla estrictamente con las reglas de linting y formateo del equipo.

En el próximo capítulo, nos centraremos en **TypeScript en Acción: Construcción de Aplicaciones Full Stack**, donde desarrollaremos componentes React con seguridad de tipos para el frontend y servicios Express para el backend, compartiendo contratos y tipos comunes a través de la librería compartida.
