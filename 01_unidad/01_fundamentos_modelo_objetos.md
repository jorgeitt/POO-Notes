---
title: "Fundamentos del Modelo de Objetos"
description: "Abstracción, clases vs. objetos y modularidad"
---

# Fundamentos del Modelo de Objetos

## 1.1 Introducción: Del Pensamiento Estructurado al Paradigma Orientado a Objetos

El desarrollo de sistemas de software experimentó una transformación fundamental con la transición de la programación estructurada o procedimental hacia el paradigma orientado a objetos.

En el modelo estructurado, fundamentado en lenguajes imperativos tradicionales como C o Pascal, la arquitectura del código se basa en una separación tajante entre las estructuras de datos pasivas y los algoritmos o funciones activas que operan sobre ellas. Este enfoque organiza la solución a través de una descomposición funcional jerárquica (*top-down*), donde un programa principal invoca subrutinas que manipulan variables globales o locales pasadas por referencia.

Aunque el pensamiento estructurado resulta adecuado para problemas computacionales lineales o puramente matemáticos, presenta limitaciones cuando se aplica a sistemas empresariales complejos y extensos. Entre sus principales debilidades se encuentran:

- El alto acoplamiento derivado del acceso a estados globales.
- La fragilidad del código ante cambios estructurales en los datos.
- La necesidad de modificar múltiples funciones cuando cambia una estructura de datos.
- Una marcada brecha semántica entre los conceptos del dominio del mundo real y su representación computacional.

El paradigma de **Programación Orientada a Objetos (POO)** resuelve estas deficiencias al unificar los datos y los comportamientos asociados dentro de una misma entidad computacional autónoma: el **objeto**.

En lugar de concebir un software como una secuencia lineal de instrucciones ejecutadas sobre un bloque de memoria, la POO concibe la aplicación como un ecosistema de objetos dinámicos que colaboran entre sí mediante el intercambio de mensajes. Esta unificación mitiga el acoplamiento directo, promueve la encapsulación y permite estructurar el software utilizando abstracciones cercanas a las entidades del mundo real.

---

## 1.2 Conceptos Clave del Modelo de Objetos

### Abstracción

La **abstracción** constituye el proceso mental mediante el cual se identifican las características y comportamientos esenciales de un elemento del dominio del problema, ignorando los detalles accidentales o irrelevantes para el contexto del sistema.

En el modelado orientado a objetos, una clase conceptual se define mediante tres dimensiones interrelacionadas:

- **Símbolo:** representa la denominación lingüística o término formal del concepto en el dominio del negocio, por ejemplo `Estudiante` o `CuentaBancaria`.
- **Intensión:** corresponde a la definición semántica y al conjunto de reglas, propiedades y responsabilidades que caracterizan a todos los miembros comprendidos por el concepto.
- **Extensión:** representa el conjunto tangible de todas las instancias u objetos concretos a los que aplica la definición conceptual dentro del entorno de ejecución.

Gracias a la abstracción, el desarrollador crea modelos simplificados pero precisos que encapsulan la complejidad operativa bajo interfaces limpias.

### Clases vs. Objetos e Instanciación

La distinción entre clase y objeto es formal y existencial en el diseño de software:

#### Clase: plantilla o especificación

Una **clase** es un tipo de dato por referencia definido por el usuario que actúa como plano arquitectónico o molde formal. Especifica:

- Los atributos que compondrán el estado interno de los objetos.
- Las firmas de los métodos que definirán su comportamiento.

La clase no representa todavía un objeto concreto con valores propios de instancia.

#### Objeto: instancia dinámica

Un **objeto** es una entidad concreta creada dinámicamente en tiempo de ejecución a partir de la estructura de una clase mediante el proceso de **instanciación**.

En Java, los objetos se crean habitualmente con el operador `new` y residen en la memoria dinámica administrada por la JVM.

Todo objeto posee tres propiedades fundamentales:

1. **Estado:** conjunto de valores almacenados en sus variables de instancia en un momento dado.
2. **Comportamiento:** conjunto de métodos que el objeto puede ejecutar en respuesta a mensajes.
3. **Identidad:** propiedad que distingue de forma única a un objeto de cualquier otro, incluso cuando dos objetos comparten estados idénticos.

### Modularidad

La **modularidad** es la propiedad arquitectónica que permite descomponer una aplicación en un conjunto de módulos o componentes desacoplados y altamente cohesivos.

En POO, la clase representa una unidad fundamental de modularidad y posibilita el desarrollo, compilación, prueba y mantenimiento independiente de cada bloque de software.

---

## 1.3 Analogía Didáctica

Para comprender la relación entre una clase y un objeto, considere la construcción y operación de un vehículo automotor.

El **plano arquitectónico** diseñado en la planta de ingeniería representa la **clase**. Este documento técnico especifica que todo vehículo fabricado bajo esa línea dispondrá de atributos como:

- color de carrocería;
- nivel de combustible;
- velocidad actual;

y de comportamientos como:

- encender el motor;
- acelerar;
- aplicar los frenos.

Sin embargo, el plano técnico en sí mismo no es un automóvil: no puede ocupar un carril, consumir gasolina ni transportar pasajeros. Es una especificación abstracta y estática.

El **objeto** se materializa cuando la línea de ensamblado utiliza el plano para fabricar una unidad concreta, equivalente en Java a ejecutar:

```java
new Automovil();
```

El vehículo producido ocupa un espacio físico concreto, posee un estado particular y responde a acciones. Si la fábrica produce diez vehículos basados en el mismo plano, cada uno existirá independientemente y administrará su propio estado.

---

## 1.4 Definición de Clase en Java con Constructores Básicos

El siguiente ejemplo ilustra la declaración de una clase orientada al dominio de control escolar, incorporando atributos privados, constructores sobrecargados y métodos de comportamiento.

### Clase `Estudiante`

```java
package com.tecnm.poo.unidad1;

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
        this.numeroControl = Objects.requireNonNull(
                numeroControl,
                "El número de control no puede ser nulo."
        );

        this.nombreCompleto = Objects.requireNonNull(
                nombreCompleto,
                "El nombre no puede ser nulo."
        );

        this.carrera = Objects.requireNonNull(
                carrera,
                "La carrera no puede ser nula."
        );

        this.promedioAcumulado = 0.0;
    }

    /**
     * Constructor sobrecargado que permite inicializar
     * un estudiante con un promedio previo.
     */
    public Estudiante(
            String numeroControl,
            String nombreCompleto,
            String carrera,
            double promedioInicial) {

        this(numeroControl, nombreCompleto, carrera);

        if (promedioInicial < 0.0 || promedioInicial > 100.0) {
            throw new IllegalArgumentException(
                    "El promedio debe estar en el rango de 0.0 a 100.0"
            );
        }

        this.promedioAcumulado = promedioInicial;
    }

    // Métodos de comportamiento
    public void registrarCalificacion(double calificacion) {
        if (calificacion < 0.0 || calificacion > 100.0) {
            throw new IllegalArgumentException(
                    "Calificación fuera del rango permitido (0-100)."
            );
        }

        this.promedioAcumulado = (this.promedioAcumulado == 0.0)
                ? calificacion
                : (this.promedioAcumulado + calificacion) / 2.0;
    }

    public boolean esElegibleParaBeca() {
        return this.promedioAcumulado >= 90.0;
    }

    // Métodos de acceso
    public String getNumeroControl() {
        return numeroControl;
    }

    public String getNombreCompleto() {
        return nombreCompleto;
    }

    public void setNombreCompleto(String nombreCompleto) {
        this.nombreCompleto = Objects.requireNonNull(
                nombreCompleto,
                "El nombre no puede ser nulo."
        );
    }

    public String getCarrera() {
        return carrera;
    }

    public double getPromedioAcumulado() {
        return promedioAcumulado;
    }
}
```

### Instanciación y uso en la clase principal

```java
package com.tecnm.poo.unidad1;

public class DemostracionFundamentos {

    public static void main(String[] args) {

        // Instanciación de dos objetos independientes
        Estudiante alumnoUno = new Estudiante(
                "24590001",
                "Ana María López",
                "Ingeniería en Sistemas"
        );

        Estudiante alumnoDos = new Estudiante(
                "24590002",
                "Carlos Ruiz",
                "Animación Digital",
                92.5
        );

        // Envío de mensajes a los objetos
        alumnoUno.registrarCalificacion(95.0);
        alumnoUno.registrarCalificacion(98.0);

        System.out.println(
                "Estado de Alumno 1: " + alumnoUno.getNombreCompleto()
                + " | Promedio: " + alumnoUno.getPromedioAcumulado()
                + " | ¿Elegible para Beca?: " + alumnoUno.esElegibleParaBeca()
        );

        System.out.println(
                "Estado de Alumno 2: " + alumnoDos.getNombreCompleto()
                + " | Promedio: " + alumnoDos.getPromedioAcumulado()
                + " | ¿Elegible para Beca?: " + alumnoDos.esElegibleParaBeca()
        );
    }
}
```

---

## 1.5 Tip de Industria

> **Buenas prácticas de industria:** considerar el uso de métodos estáticos de fábrica en lugar de constructores públicos directos, de acuerdo con la recomendación presentada en *Effective Java*.

En el diseño de bibliotecas de clases, los **métodos estáticos de fábrica** (*Static Factory Methods*) proporcionan varias ventajas:

1. **Claridad semántica:** pueden tener nombres descriptivos que expresen con precisión la intención del objeto creado.
2. **Control de instanciación:** no obligan necesariamente a crear una nueva instancia cada vez que son invocados.
3. **Retorno de subtipos:** pueden retornar objetos de subtipos compatibles con el tipo declarado.

Ejemplo:

```java
public static Estudiante crearNuevoIngreso(
        String numeroControl,
        String nombreCompleto,
        String carrera) {

    return new Estudiante(
            numeroControl,
            nombreCompleto,
            carrera,
            0.0
    );
}

public static Estudiante crearConRevalidacion(
        String numeroControl,
        String nombreCompleto,
        String carrera,
        double promedioRevalidado) {

    return new Estudiante(
            numeroControl,
            nombreCompleto,
            carrera,
            promedioRevalidado
    );
}
```

---

## 1.6 Reto / Ejercicio Rápido para el Alumno

1. Diseñe mentalmente la abstracción para la clase `Cancion` en el contexto de una plataforma de reproducción en línea.
2. Identifique cuatro atributos esenciales que definan el estado del objeto y dos métodos que representen su comportamiento.
3. Declare en Java la clase `Cancion`, incluyendo un constructor primario y un método `reproducir()` que imprima los datos del tema en consola.
4. **Pregunta de autoevaluación:** si se ejecutan tres sentencias `new Cancion(...)` asignadas a tres variables distintas, ¿cuántos objetos han sido creados en el Heap de la JVM y cuántas plantillas de clase existen?

---

## Referencias

1. `AED-1286_Programacion_Orientada_a_Objetos (1).pdf`
2. *Java How to Program Early Objects*, 11e (2021).
3. Craig Larman, *UML y Patrones*, 2.ª ed.
4. Joshua Bloch, *Effective Java*, 3rd Edition.
5. *Head First Java*, 2nd Edition.
