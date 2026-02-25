# Mapa de Incidentes de Seguridad - Medellín

Desarrollar una interfaz grafica que pueda visualizar en la ciudad de medellin eventos de riña,hurto ,homicidios y violencia intrafamiliar con etiquetas de colores y que cada evento se puede leer a traves de una ventana o pop up la hora ,fecha y resumen del evento, usa interfaces tipo ESRI o de geolocalizacion, puedes usar la imagen de referencia anexada.

https://chat.z.ai/space/x1brb1m6zjw0-art

Aplicación web interactiva para visualizar incidentes de seguridad ciudadana en la ciudad de Medellín, Colombia.

## Características

- **Visualización de incidentes**: Riñas, hurtos, homicidios y violencia intrafamiliar
- **Mapa interactivo**: Ubicación precisa de cada incidente
- **Heatmap de clasificación**: Visualización de densidad por gravedad
- **Dark/Light Mode**: Cambio dinámico de tema
- **Popups informativos**: Descripción, dirección, fecha y gravedad
- **Panel de estadísticas**: Conteo en tiempo real por tipo de incidente
- **Filtros interactivos**: Mostrar/ocultar tipos específicos

## Tecnologías

- HTML5
- JavaScript (ES6+)
- Mapbox GL JS v2.15.0
- Bootstrap 5.3
- Font Awesome 6.4

## Estructura del Proyecto

```
mapbox1/
├── index.html              # Aplicación principal
├── skills/                 # Skills para AI agents
│   ├── vercel-best-practices/
│   │   ├── SKILL.md
│   │   ├── AGENTS.md
│   │   ├── metadata.json
│   │   ├── vercel-config.md
│   │   └── nextjs-patterns.md
│   └── design-interfaces/
│       ├── SKILL.md
│       ├── AGENTS.md
│       ├── metadata.json
│       ├── ux-principles.md
│       ├── components.md
│       ├── responsive.md
│       ├── accessibility.md
│       └── design-tokens.md
└── README.md
```

## Uso

1. Abrir `index.html` en un navegador web
2. Navegar por el mapa de Medellín
3. Hacer clic en los marcadores para ver detalles
4. Usar el panel lateral para filtrar por tipo de incidente
5. Activar/desactivar el heatmap desde el toggle
6. Cambiar entre modo oscuro y claro

## Datos de Incidentes

El mapa incluye 25 incidentes de ejemplo distribuidos en diferentes zonas de Medellín, con datos simulados para demostrar la funcionalidad.

### Tipos de Incidentes

| Tipo | Color | Icono |
|------|-------|-------|
| Riña | Rojo | Fist raised |
| Hurto | Naranja | User secret |
| Homicidio | Gris oscuro | Skull |
| Violencia Intrafamiliar | Morado | House user |

## Personalización

### Agregar más incidentes

Editar el array `incidentes` en `index.html`:

```javascript
const incidentes = [
  { 
    id: 26, 
    tipo: 'riña', 
    lat: 6.2xxx, 
    lng: -75.5xxx, 
    descripcion: 'Descripción del incidente',
    direccion: 'Dirección',
    fecha: '2024-01-20',
    gravedad: 3
  },
  // ...
];
```

### Cambiar el token de Mapbox

Reemplazar el token en la línea:

```javascript
mapboxgl.accessToken = 'TU_TOKEN_AQUI';
```

## Skills Disponibles

### Vercel Best Practices

Skill con guías completas para desarrollo en Vercel y Next.js.

**Instalación**: Copiar la carpeta `skills/vercel-best-practices/`

**Contenido**:
- Configuración de proyectos
- Patrones de Next.js
- Deployment y serverless
- Optimización de rendimiento

### Design Interfaces

Skill con guías de mejores prácticas para diseño de UI/UX.

**Instalación**: Copiar la carpeta `skills/design-interfaces/`

**Contenido**:
- Principios de UX
- Componentes UI
- Diseño responsivo
- Accesibilidad (WCAG)
- Design tokens

## Licencia

MIT
