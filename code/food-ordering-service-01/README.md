# 01 Creando Order Service. Módulos, Submódulos y dependencias entre ellos.

### Crear módulo principal
- Crear sin código.
- pom packaging, Crea un contenedor para los submodulos, empaquetados como JAR
```
<packaging>pom</packaging>
```
- Spring boot para crear los microservicios.
- Ruta relativa vacia. Busca en el sistema de archivos el proyecto maven.
  ```
      <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.7</version>
        <relativePath/>
    </parent>
  ```
- Setear el proyecto como Java 17
```
   <!-- NO REQUERIDAS -->
   <maven.compiler.source>17</maven.compiler.source>
   <maven.compiler.target>17</maven.compiler.target>
 
   <!-- UTILIZAR ESTAS -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.9.0</version>
                <configuration>
                    <release>17</release>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

### Crear Submodulos para order-service
- New -> Module -> JDK 17, Maven  -> order-service
- order-service, será un proyecto base, se elimina src.
- De acuerdo con la arquitectura hexagonal, también tendrá submódulos:
- food-ordering-service-01
  - order-service
    - order-domain (Business Layer)
      - order-application-service (Puertos adaptadores)
      - order-domain-core (Lógica central, incluye entidades, objetos de valor, servicios de dominio)
    - order-application (API)
    - order-dataaccess (Persistence Layer)
    - order-messaging (Kafka)
    - order-container (Empaqueta el Jar del microservicio, Docker image)

### dependencyManagment
- Unifica las versiones de las dependencias en los módulos hijos.
- Agregas las dependencias a las dependencias de los hijos.
- NO se agrega el order-container, porque no es utilizado por los otros módulos

### Graphviz
- Software grafico que ayuda a visualizar las dependencias
-  Visualize dependencies:
   https://github.com/ferstl/depgraph-maven-plugin
- Instalar y ejecutar(Desde el power Shell) :
  - mvn clean install
  - mvn com.github.ferstl:depgraph-maven-plugin:aggregate -DcreateImage=true -DreduceEdges=false -Dscope=compile "-Dincludes=com.food.ordering.system*:*"

- Todas las flechas deberían de apuntar al módulo lógico del dominio (order-domain-core)
- Núcleo del dominio es el componente mas importante en la imagen
- DIP(Dependency Inversion Principle)
  - Permite revertir cualquier dependencia en tiempo de ejecución. Independencia del dominio.

