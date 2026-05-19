Bautista-post2-u10

Actividad Post-Contenido 2 – Unidad 10
Patrones de Diseño de Software – Métricas de Calidad y SonarQube

Productos Service — Mejora de Calidad con SonarQube
Objetivo

El objetivo de esta actividad fue configurar un Quality Gate personalizado en SonarQube, corregir Bugs y Code Smells detectados en el análisis inicial, ejecutar un segundo análisis para verificar la mejora de calidad e integrar el análisis automático mediante GitHub Actions.

Tecnologías Utilizadas
Java 21
Spring Boot 3
Maven
H2 Database
SonarQube Community
JaCoCo
GitHub Actions
JPA / Hibernate
Paso 1: Configuración del Quality Gate

Se creó un Quality Gate personalizado llamado:

Estándar Universidad
Condiciones configuradas
Condición	Regla
Bugs	Mayor que 0 → FAIL
Coverage	Menor a 60% → FAIL
Code Smells	Mayor que 5 → FAIL
Duplicated Lines	Mayor a 5% → FAIL

El proyecto productos-service fue asociado al Quality Gate personalizado desde:

Project Settings → Quality Gate
Estado Inicial del Análisis
Categoría	Cantidad	Rating
Bugs	1	C
Vulnerabilidades	0	A
Code Smells	2	A
Cobertura	2.8%	Failed
Duplicaciones	0%	Passed
Hallazgos Iniciales Detectados
Bug Detectado
Retorno null innecesario
Archivo: ProductoService.java
Problema: El método retornaba null, generando un posible NullPointerException.
Severidad: Medium
Code Smell 1 — Inyección por Campo
Antes
@Autowired
private ProductoRepository repo;
Problema

La inyección mediante @Autowired en atributos dificulta las pruebas unitarias y aumenta el acoplamiento.

Code Smell 2 — Uso de equals("")
Antes
if (nombre == null || nombre.equals(""))
Problema

No cubría cadenas con espacios vacíos y no seguía buenas prácticas modernas de Java.

Code Smell 3 — Complejidad Ciclomática Alta

El método procesarProducto() concentraba múltiples validaciones y lógica de negocio.

Paso 2: Corrección del Bug

Se eliminó el retorno null y se implementó el guardado correcto del producto utilizando:

return productoRepository.save(producto);

Con esto se evitó el posible NullPointerException.

Paso 3: Corrección de Code Smells
1. Inyección por Constructor
Después
private final ProductoRepository productoRepository;

public ProductoService(ProductoRepository productoRepository) {
    this.productoRepository = productoRepository;
}
2. Reemplazo de equals("") por isBlank()
Después
if (nombre == null || nombre.isBlank())

Ahora se validan correctamente cadenas vacías y espacios en blanco.

3. Extracción del Método validarDatos()
Método principal reducido
public Producto procesarProducto(String nombre, Double precio, Integer stock) {
    validarDatos(nombre, precio, stock);

    Producto producto = new Producto();
    producto.setNombre(nombre.strip());
    producto.setPrecio(precio);
    producto.setStock(stock);

    return productoRepository.save(producto);
}
Método extraído
private void validarDatos(String nombre, Double precio, Integer stock)

Esto redujo la complejidad ciclomática y mejoró la mantenibilidad del código.

Paso 4: Segundo Análisis SonarQube

Después de las correcciones se ejecutó nuevamente:

mvn clean verify sonar:sonar -Dsonar.token=TU_TOKEN
Resultados del Segundo Análisis
Categoría	Antes	Después
Bugs	1	0
Code Smells	2	0
Cobertura	2.8%	2.8%
Duplicaciones	0%	0%
Mejora Obtenida
Se eliminaron todos los Bugs detectados.
Se corrigieron los Code Smells principales.
Se redujo la complejidad del servicio.
Se aplicaron buenas prácticas recomendadas por SonarQube.
El código quedó más mantenible y desacoplado.
Paso 5: Integración con GitHub Actions

Se configuró un pipeline CI automático mediante GitHub Actions para ejecutar:

Compilación
Pruebas
JaCoCo
Análisis SonarQube

Archivo configurado:

.github/workflows/ci.yml
Workflow utilizado
name: CI con GitHub Actions

on:
  push:
    branches: [ main ]

  pull_request:
    branches: [ main ]

jobs:
  build-and-analyze:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          java-version: "21"
          distribution: "temurin"

      - name: Compilar y probar
        run: mvn -B clean verify --no-transfer-progress
Evidencias
Dashboard Inicial
src/main/resources/docs/
sonar-dashboard.png
sonar-issues.png
sonar-details.png
Dashboard Final
src/main/resources/docs/docs-parte 2/
sonar-dashboard-parte-2.png
Comandos Utilizados
Compilar proyecto
mvn compile
Ejecutar pruebas
mvn clean verify
Ejecutar análisis SonarQube
mvn sonar:sonar -Dsonar.token=TU_TOKEN
Ejecutar aplicación
mvn spring-boot:run
Historial de Commits
Paso 1
Paso 1 :Configuracion del Quality Gate en SonarQube
Paso 2
Paso 2: Correccion del Bug detectado por SonarQube
Paso 3
Paso 3: Refactorizacion y correccion de Code Smells
Paso 4
Paso 4: Segundo analisis SonarQube y verificacion de mejoras
Paso 5
Paso 5: Integracion de SonarQube y GitHub Actions
Conclusiones

La integración de SonarQube permitió identificar problemas de mantenibilidad y calidad en el proyecto.
Gracias a las refactorizaciones realizadas, se logró reducir Bugs y Code Smells, mejorando la calidad general del código.
