PÁGINA 1: Fundamentos del Modelo de Objetos (Abstracción, Clases vs. Objetos, Modularidad)1.1 Introducción: Del Pensamiento Estructurado al Paradigma Orientado a ObjetosEl desarrollo de sistemas de software experimentó una transformación fundamental con la transición de la programación estructurada o procedimental hacia el paradigma orientado a objetos. En el modelo estructurado, fundamentado en lenguajes imperativos tradicionales como C o Pascal, la arquitectura del código se basa en una separación tajante entre las estructuras de datos pasivas y los algoritmos o funciones activas que operan sobre ellas. Este enfoque organiza la solución a través de una descomposición funcional jerárquica (top-down), donde un programa principal invoca subrutinas que manipulan variables globales o locales pasadas por referencia.Aunque el pensamiento estructurado resulta adecuado para problemas computacionales lineales o puramente matemáticos, presenta severas limitaciones cuando se aplica a sistemas empresariales complejos y extensos. Entre sus principales debilidades se encuentra el alto acoplamiento derivado del acceso a estados globales, la fragilidad del código ante cambios estructurales en los datos (donde una modificación en un tipo de dato exige reescribir múltiples funciones vinculadas) y una marcada brecha semántica entre los conceptos del dominio del mundo real y su representación computacional.El paradigma de Programación Orientada a Objetos (POO) resuelve estas deficiencias al unificar los datos y los comportamientos asociados dentro de una misma entidad computacional autónoma: el objeto. En lugar de concebir un software como una secuencia lineal de instrucciones ejecutadas sobre un bloque de memoria, la POO concibe la aplicación como un ecosistema de objetos dinámicos que colaboran entre sí mediante el intercambio de mensajes. Esta unificación mitiga el acoplamiento directo, promueve la encapsulación y permite estructurar el software utilizando abstracciones idénticas a las entidades del mundo real.1.2 Conceptos Clave del Modelo de ObjetosAbstracciónLa abstracción constituye el proceso mental mediante el cual se identifican las características y comportamientos esenciales de un elemento del dominio del problema, ignorando los detalles accidentales o irrelevantes para el contexto del sistema. En el modelado orientado a objetos, una clase conceptual se define rigurosamente mediante tres dimensiones interrelacionadas:Símbolo: Representa la denominación lingüística o término formal del concepto en el dominio del negocio (por ejemplo, Estudiante o CuentaBancaria).Intensión: Corresponde a la definición semántica y al conjunto de reglas, propiedades y responsabilidades que caracterizan a todos los miembros comprendidos por el concepto.Extensión: Representa el conjunto tangible de todas las instancias u objetos concretos a los que aplica la definición conceptual dentro del entorno de ejecución.Gracias a la abstracción, el desarrollador crea modelos simplificados pero precisos que encapsulan la complejidad operativa bajo interfaces limpias.Clases vs. Objetos e InstanciaciónLa distinción entre clase u objeto es formal y existencial en el diseño de software:Clase (Plantilla o Especificación): Es un tipo de dato por referencia definido por el usuario que actúa como el plano arquitectónico o molde formal. Una clase especifica la estructura de los atributos que compondrán el estado interno de los objetos y las firmas de los métodos que definirán su comportamiento, sin ocupar espacio de almacenamiento dinámico para datos de instancia en memoria antes de ejecutarse.Objeto (Instancia Dinámica): Es una entidad concreta creada dinámicamente en tiempo de ejecución a partir de la estructura de una clase mediante el proceso de instanciación. Todo objeto habita en la memoria dinámica del entorno de ejecución (el Heap en la Máquina Virtual de Java) y posee tres propiedades fundamentales:Estado: Representado por el conjunto de valores almacenados en sus variables de instancia en un momento dado.Comportamiento: Definido por el conjunto de métodos que el objeto puede ejecutar en respuesta a mensajes.Identidad: Una propiedad intrínseca que distingue de forma única a un objeto de cualquier otro dentro de la memoria del sistema, independientemente de si comparten estados idénticos.ModularidadLa modularidad es la propiedad arquitectónica que permite descomponer una aplicación en un conjunto de módulos o componentes desacoplados y altamente cohesivos. En POO, la clase representa la unidad fundamental de modularidad, posibilitando el desarrollo, compilación, prueba y mantenimiento independiente de cada bloque de software.1.3 Analogía Didáctica (Estilo Head First)Para comprender la relación entre una clase y un objeto, considere la construcción y operación de un vehículo automotor.El plano arquitectónico diseñado en la planta de ingeniería representa la Clase. Este documento técnico especifica con precisión que todo vehículo fabricado bajo esa línea dispondrá de atributos como un color de carrocería, un nivel de combustible y una velocidad actual, así como comportamientos tales como encender el motor, acelerar o aplicar los frenos. Sin embargo, el plano técnico en sí mismo no es un automóvil: no puede ocupar un carril en la carretera, no consume gasolina ni puede transportar pasajeros. Es una especificación abstracta y estática.El objeto se materializa cuando la línea de ensamblado utiliza el plano para fabricar una unidad física real mediante la orden de producción (new Automovil()). El vehículo producido ocupa un espacio físico concreto en el mundo real (equivalente a la memoria Heap de la JVM), posee una pintura roja específica (su estado) y responde físicamente cuando el conductor presiona el pedal del acelerador (su comportamiento). Si la fábrica produce diez vehículos basados en el mismo plano, cada uno existirá independientemente: si el primer vehículo sufre una ponchadura en un neumático, el estado de las restantes nueve unidades permanecerá inalterado, puesto que cada objeto administra su propio estado en su respectiva dirección de memoria.1.4 Definición de Clase en Java con Constructores BásicosEl siguiente ejemplo en Java ilustra la declaración formal de una clase orientada al dominio del programa oficial de la asignatura AED-1286 del TecNM, incorporando atributos de instancia privados, constructores sobrecargados y métodos de comportamiento.Javapackage com.tecnm.poo.unidad1;

import java.util.Objects;

/**
 * Representa la abstracción de un Estudiante en el sistema de control escolar.
 * Ilustra los conceptos formales de clase, instanciación, estado y comportamiento.
 */
public class Estudiante {

    // Atributos de instancia (Estado del objeto)
    private final String numeroControl;
    private String nombreCompleto;
    private String carrera;
    private double promedioAcumulado;

    /**
     * Constructor principal de la clase.
     * 
     * @param numeroControl Identificador único del alumno.
     * @param nombreCompleto Nombre y apellidos del alumno.
     * @param carrera Programa educativo en el que está inscrito.
     */
    public Estudiante(String numeroControl, String nombreCompleto, String carrera) {
        this.numeroControl = Objects.requireNonNull(numeroControl, "El número de control no puede ser nulo.");
        this.nombreCompleto = Objects.requireNonNull(nombreCompleto, "El nombre no puede ser nulo.");
        this.carrera = Objects.requireNonNull(carrera, "La carrera no puede ser nula.");
        this.promedioAcumulado = 0.0;
    }

    /**
     * Constructor sobrecargado que permite inicializar un estudiante con un promedio previo.
     */
    public Estudiante(String numeroControl, String nombreCompleto, String carrera, double promedioInicial) {
        this(numeroControl, nombreCompleto, carrera);
        if (promedioInicial < 0.0 || promedioInicial > 100.0) {
            throw new IllegalArgumentException("El promedio debe estar en el rango de 0.0 a 100.0");
        }
        this.promedioAcumulado = promedioInicial;
    }

    // Métodos de comportamiento (Operaciones que modifican o consultan el estado)
    public void registrarCalificacion(double calificacion) {
        if (calificacion < 0.0 || calificacion > 100.0) {
            throw new IllegalArgumentException("Calificación fuera del rango permitido (0-100).");
        }
        this.promedioAcumulado = (this.promedioAcumulado == 0.0) 
                ? calificacion 
                : (this.promedioAcumulado + calificacion) / 2.0;
    }

    public boolean esElegibleParaBeca() {
        return this.promedioAcumulado >= 90.0;
    }

    // Métodos de acceso (Getters y Setters)
    public String getNumeroControl() {
        return numeroControl;
    }

    public String getNombreCompleto() {
        return nombreCompleto;
    }

    public void setNombreCompleto(String nombreCompleto) {
        this.nombreCompleto = Objects.requireNonNull(nombreCompleto, "El nombre no puede ser nulo.");
    }

    public String getCarrera() {
        return carrera;
    }

    public double getPromedioAcumulado() {
        return promedioAcumulado;
    }
}
Instanciación y Uso en la Clase PrincipalJavapackage com.tecnm.poo.unidad1;

public class DemostracionFundamentos {

    public static void main(String[] args) {
        // Instanciación de dos objetos independientes en el Heap
        Estudiante alumnoUno = new Estudiante("24590001", "Ana María López", "Ingeniería en Sistemas");
        Estudiante alumnoDos = new Estudiante("24590002", "Carlos Ruiz", "Animación Digital", 92.5);

        // Envío de mensajes a los objetos
        alumnoUno.registrarCalificacion(95.0);
        alumnoUno.registrarCalificacion(98.0);

        System.out.println("Estado de Alumno 1: " + alumnoUno.getNombreCompleto() + 
                           " | Promedio: " + alumnoUno.getPromedioAcumulado() + 
                           " | ¿Elegible para Beca?: " + alumnoUno.esElegibleParaBeca());

        System.out.println("Estado de Alumno 2: " + alumnoDos.getNombreCompleto() + 
                           " | Promedio: " + alumnoDos.getPromedioAcumulado() + 
                           " | ¿Elegible para Beca?: " + alumnoDos.esElegibleParaBeca());
    }
}
1.5 Tip de Industria (Basado en Effective Java por Joshua Bloch)Buenas Prácticas de Industria: Considera el uso de métodos estáticos de fábrica en lugar de constructores públicos directos (Item 1).En el diseño de bibliotecas de clases de calidad profesional, el uso indiscriminado de constructores públicos presenta serias limitaciones. Joshua Bloch recomienda la implementación de métodos estáticos de fábrica (Static Factory Methods), los cuales proporcionan ventajas arquitectónicas sustanciales:Claridad Semántica: A diferencia de los constructores, cuyo nombre está restringido al nombre exacto de la clase, los métodos estáticos tienen nombres descriptivos que expresan con precisión la intención del objeto instanciado.Control de Instanciación: No obligan a crear una nueva instancia en memoria cada vez que son invocados, permitiendo el uso de objetos en caché o el control de tipos inmutables.Retorno de Subtipos: Tienen la capacidad de retornar cualquier objeto que sea un subtipo del tipo devuelto declarado, ofreciendo flexibilidad polimórfica.Java// Implementación de Métodos Estáticos de Fábrica en lugar de constructores ambiguos
public static Estudiante crearNuevoIngreso(String numeroControl, String nombreCompleto, String carrera) {
    return new Estudiante(numeroControl, nombreCompleto, carrera, 0.0);
}

public static Estudiante crearConRevalidacion(String numeroControl, String nombreCompleto, String carrera, double promedioRevalidado) {
    return new Estudiante(numeroControl, nombreCompleto, carrera, promedioRevalidado);
}
1.6 Reto / Ejercicio Rápido para el AlumnoDiseñe mentalmente la abstracción para la clase Cancion en el contexto de una plataforma de reproducción en línea.Identifique cuatro atributos esenciales que definan el estado del objeto y dos métodos que representen su comportamiento.Declare en lenguaje Java la clase Cancion incluyendo un constructor primario y un método reproducir() que imprima los datos del tema en consola.Pregunta de Autoevaluación: Si se ejecutan tres sentencias new Cancion(...) asignadas a tres variables distintas, ¿cuántos objetos han sido creados en el Heap de la Máquina Virtual de Java y cuántas plantillas de clase existen en memoria?
