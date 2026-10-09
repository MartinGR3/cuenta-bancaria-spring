# Cajero automático — Spring Core e inyección de dependencias

**Autor:** Martin Gonzalez Rico

## Cómo correrlo

    ./correr.sh App final-app
    ./probar.sh final

## Las piezas

| Bean | Clase | Cómo lo declara Spring (`@Component` o `@Bean`) | Singleton o prototype |
|---|---|---|---|
| cajeroAutomatico | CajeroAutomatic | @Component | Singleton |
| antifraudeEstricto |  AntifraudeEstricto  |  @Component  | Singleton |
| antifraudePorMonto |  AntifraudePorMonto  |  @Component  +  @Primary  | Singleton |
| notificadorConsola |  NotificadorConsola  |  @Component  | Singleton |
| repositorioEnMemoria |  RepositorioEnMemoria  |  @Component  | Singleton |
| reloj |  Clock  |  @Bean  | Singleton |
| sesionCajero |  SesionCajero  |  @Component  +  @Scope("prototype")  | Prototype |
| configuracionBanco |  ConfiguracionBanco  |  @Configuration  | Singleton |

## Boleto de salida

1. ¿Qué es la inyección de dependencias? Explícalo con el cajero, en tus palabras.

La inyección de dependencias consiste en que una clase recibe los objetos que necesita, en lugar de crearlos por sí misma. En el caso del cajero, recibe el repositorio, el antifraude, el notificador y el reloj. Spring se encarga de crear esos objetos y utilizarlos.

2. En la MP-1, ¿quién decidía qué antifraude usaba el cajero? ¿Y desde la MP-2?

En la MP-1, lo decidía el método main, porque creaba el antifraude con new AntifraudePorMonto() y se lo pasaba al cajero. Desde la MP-2, se declara la implementación en ConfiguracionBanco mediante @Bean, y Spring se encarga de inyectarla en el cajero.

3. ¿Cuándo usarías `@Bean` en vez de `@Component`? Da el ejemplo de hoy.

Usaría @Bean cuando necesito definir manualmente cómo se crea un objeto, especialmente si pertenece a una clase que no puedo modificar. El ejemplo de hoy es Clock, que se configura con Clock.system(ZoneId.of("America/Mexico_City")) en el método reloj() de ConfiguracionBanco.

4. ¿Qué gana: `@Primary` o `@Qualifier`? ¿Por qué tiene sentido?

Gana @Qualifier, porque seleccionar un bean por su nombre y tiene prioridad sobre @Primary. Tiene sentido porque`@Primary establece el antifraude predeterminado, mientras que @Qualifier("antifraudeEstricto") permite indicar que el cajero debe utilizar específicamente el antifraude estricto.

5. En tu proyecto de Empleados de la Semana 3 nunca escribiste `@ComponentScan`. ¿Quién lo hace?

Lo hace @SpringBootApplication, porque incluye la anotación @ComponentScan, además de @SpringBootConfiguration y @EnableAutoConfiguration. Por eso Spring Boot detecta automáticamente clases como @Service y @RestController dentro del paquete correspondiente y sus subpaquetes.