<<<<<<< HEAD
# Bautista-post2-u10
Actividad Post-Contenido 2 / Unidad 10
=======
# Bautista-post1-u10
Actividad Post-Contenido 1 / Unidad 10

# Productos Service — Análisis SonarQube

## Estado inicial del análisis

| Categoría | Cantidad | Rating |
|-----------|----------|--------|
| Bugs | 1 | A |
| Vulnerabilidades | 0 | A |
| Code Smells | 2 | C |
| Cobertura | 2.8% | — |

---

## Hallazgos principales identificados

### Bug 1: Field Injection detectado

- Archivo: `ProductoService.java`, línea 13
- Descripción: Se detectó inyección de dependencias mediante `@Autowired` en un atributo. SonarQube recomienda usar inyección por constructor para mejorar mantenibilidad y pruebas.
- Severidad: Medium

---

### Code Smell 1: Uso de Field Injection

- Archivo: `ProductoService.java`, línea 13
- Descripción: La dependencia `ProductoRepository` fue inyectada directamente como atributo. Esto dificulta las pruebas unitarias y aumenta el acoplamiento.
- Severidad: Medium

---

### Code Smell 2: Parámetros no utilizados

- Archivo: `ProductoService.java`, línea 19
- Descripción: Los parámetros `cat`, `activo` y `proveedor` no son utilizados dentro del método `procesarProducto`, generando código innecesario y reduciendo la claridad.
- Severidad: Medium

---

## Capturas del dashboard

### Dashboard General

![Dashboard SonarQube](docs/sonar-dashboard.png)

---

### Hallazgos detectados

![Detalle Issues](docs/sonar-issues.png)

---

### Detalle de Bugs y Code Smells

![Detalle SonarQube](docs/sonar-details.png)

---

## Tecnologías utilizadas

- Java 21
- Spring Boot 3
- Maven
- H2 Database
- SonarQube Community
- JaCoCo
- JPA / Hibernate

---

## Ejecución del proyecto

### Compilar el proyecto

```bash
mvn compile
```

### Ejecutar pruebas y cobertura

```bash
mvn clean verify
```

### Ejecutar análisis SonarQube

```bash
mvn sonar:sonar -Dsonar.token=TU_TOKEN
```

### Ejecutar aplicación

```bash
mvn spring-boot:run
```
>>>>>>> 8ed92ae (Paso 1 :Configuracion del Quality Gate en SonarQube)
