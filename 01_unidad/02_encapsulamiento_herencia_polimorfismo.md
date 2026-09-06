---
title: "Protección y Mecanismos Reutilizables"
description: "Encapsulamiento, herencia y polimorfismo"
---

# Protección y Mecanismos Reutilizables

## 2.1 Encapsulamiento y Ocultamiento de Información

El **encapsulamiento** es el principio de diseño mediante el cual los detalles internos de implementación de un componente se mantienen ocultos tras una interfaz pública de acceso, protegiendo la integridad del estado interno del objeto.

El **ocultamiento de información** (*information hiding*) evita que las clases cliente accedan o modifiquen directamente las variables de instancia de un objeto.

Cuando los campos se exponen como `public`, cualquier fragmento del código externo puede alterar sus valores sin pasar por filtros de validación, violando los invariantes de la clase y provocando estados inconsistentes o errores difíciles de rastrear.

Al restringir la visibilidad mediante modificadores `private` y proporcionar métodos de consulta (*getters*) y modificación (*setters*), la clase asume el control de sus propios datos. Los métodos modificadores pueden actuar como barreras de validación antes de alterar el estado interno.

---

## 2.2 Herencia y Polimorfismo

### Herencia y la relación "Es-Un" (IS-A)

La **herencia** es un mecanismo de reutilización que permite crear nuevas clases, denominadas subclases o clases derivadas, a partir de clases existentes, denominadas superclases o clases base.

La subclase hereda los atributos y métodos accesibles de la superclase y puede añadir nuevos miembros o especializar comportamientos existentes.

La herencia se rige bajo la regla semántica de la relación **"Es-Un" (IS-A)**:

- Un `Automovil` **es un** `Vehiculo`.
- Una `CuentaAhorro` **es una** `CuentaBancaria`.

Si la relación conceptual no satisface esta premisa, la herencia no debe emplearse.

### Polimorfismo y despacho dinámico

El **polimorfismo** representa la capacidad de tratar objetos de diferentes subclases de manera uniforme a través de una referencia común de la superclase, permitiendo que un mismo mensaje provoque comportamientos distintos según la instancia real que lo reciba.

El polimorfismo se fundamenta en la **sobrescritura de métodos** (*method overriding*), donde una subclase proporciona su propia implementación para un método previamente definido en la superclase.

En Java, el polimorfismo opera mediante **enlace tardío** o **despacho dinámico de métodos** (*dynamic dispatch*): la JVM determina en tiempo de ejecución el tipo concreto del objeto y ejecuta la versión del método correspondiente a esa instancia.

---

## 2.3 Matriz de Modificadores de Acceso y Visibilidad

Java proporciona cuatro niveles de accesibilidad para regular la visibilidad de atributos, métodos y constructores:

| Modificador | Misma clase | Mismo paquete | Subclase (mismo paquete) | Subclase (diferente paquete) | Exterior |
|---|---:|---:|---:|---:|---:|
| `private` | Sí | No | No | No | No |
| *(default / package)* | Sí | Sí | Sí | No | No |
| `protected` | Sí | Sí | Sí | Sí | No |
| `public` | Sí | Sí | Sí | Sí | Sí |

---

## 2.4 Código Java Integrador: Encapsulamiento, Herencia y Sobrescritura

### Superclase base: `CuentaBancaria.java`

```java
package com.tecnm.poo.unidad2;

import java.util.Objects;

/**
 * Superclase que representa una Cuenta Bancaria general.
 * Aplica encapsulamiento sobre sus variables de estado.
 */
public class CuentaBancaria {

    private final String numeroCuenta;
    private final String titular;
    private double saldo;

    public CuentaBancaria(
            String numeroCuenta,
            String titular,
            double saldoInicial) {

        if (saldoInicial < 0.0) {
            throw new IllegalArgumentException(
                    "El saldo inicial no puede ser negativo."
            );
        }

        this.numeroCuenta = Objects.requireNonNull(
                numeroCuenta,
                "El número de cuenta es obligatorio."
        );

        this.titular = Objects.requireNonNull(
                titular,
                "El titular es obligatorio."
        );

        this.saldo = saldoInicial;
    }

    public void depositar(double monto) {
        if (monto <= 0.0) {
            throw new IllegalArgumentException(
                    "El monto a depositar debe ser mayor a cero."
            );
        }

        this.saldo += monto;
    }

    /**
     * Método diseñado para ser sobrescrito
     * polimórficamente por las subclases.
     */
    public boolean retirar(double monto) {
        if (monto > 0.0 && this.saldo >= monto) {
            this.saldo -= monto;
            return true;
        }

        return false;
    }

    public String getNumeroCuenta() {
        return numeroCuenta;
    }

    public String getTitular() {
        return titular;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

### Subclase derivada: `CuentaAhorro.java`

```java
package com.tecnm.poo.unidad2;

/**
 * Subclase especializada que extiende
 * la funcionalidad de CuentaBancaria.
 */
public class CuentaAhorro extends CuentaBancaria {

    private final double tasaInteresAnual;

    public CuentaAhorro(
            String numeroCuenta,
            String titular,
            double saldoInicial,
            double tasaInteresAnual) {

        super(numeroCuenta, titular, saldoInicial);
        this.tasaInteresAnual = tasaInteresAnual;
    }

    /**
     * Sobrescritura polimórfica del método de retiro.
     * Aplica una comisión operativa por cada retiro.
     */
    @Override
    public boolean retirar(double monto) {
        double comisionOperativa = 12.50;
        double montoTotalARetirar = monto + comisionOperativa;

        return super.retirar(montoTotalARetirar);
    }

    public void aplicarInteresMensual() {
        double interesGenerado =
                getSaldo() * (this.tasaInteresAnual / 12.0 / 100.0);

        depositar(interesGenerado);
    }
}
```

### Ejecución polimórfica: `DemostracionPolimorfismo.java`

```java
package com.tecnm.poo.unidad2;

import java.util.ArrayList;
import java.util.List;

public class DemostracionPolimorfismo {

    public static void main(String[] args) {

        // Colección heterogénea sustentada en el tipo de la superclase
        List<CuentaBancaria> cuentas = new ArrayList<>();

        cuentas.add(
                new CuentaBancaria(
                        "CTA-101",
                        "María Fernández",
                        3000.0
                )
        );

        cuentas.add(
                new CuentaAhorro(
                        "SAV-202",
                        "Jorge Rodríguez",
                        3000.0,
                        6.5
                )
        );

        System.out.println(
                "=== PROCESAMIENTO POLIMÓRFICO DE RETIROS ==="
        );

        for (CuentaBancaria cuenta : cuentas) {

            // El mismo mensaje produce un resultado distinto
            boolean resultado = cuenta.retirar(500.0);

            System.out.println(
                    "Cuenta: " + cuenta.getNumeroCuenta()
                    + " | Transacción Exitosa: " + resultado
                    + " | Saldo Resultante: $" + cuenta.getSaldo()
            );
        }
    }
}
```

---

## 2.5 Tip de Industria

> **Buenas prácticas de industria:** favorecer la composición sobre la herencia y utilizar la anotación `@Override` de manera sistemática.

La herencia puede debilitar la encapsulación si una subclase depende de detalles de implementación de la superclase. Si esos detalles cambian en versiones posteriores, la subclase podría presentar comportamientos no deseados.

Por ello, *Effective Java* recomienda **favorecer la composición sobre la herencia** cuando no existe una relación de especialización genuina y documentada.

Asimismo, el uso de `@Override` permite que el compilador verifique que la firma de un método coincide con la declaración que se desea sobrescribir y ayuda a evitar sobrecargas accidentales.

---

## 2.6 Reto / Ejercicio Rápido para el Alumno

1. Implemente una nueva subclase denominada `CuentaCheques` que herede de `CuentaBancaria`.
2. Agregue un atributo privado `limiteSobregiro`.
3. Sobrescriba el método `retirar(double monto)` para permitir cargos aun cuando el saldo sea insuficiente, siempre que el saldo negativo no supere dicho límite.
4. **Pregunta de análisis:** si se modifica la visibilidad del atributo `saldo` en `CuentaBancaria` de `private` a `protected`, ¿qué riesgos de encapsulamiento se introducen para las aplicaciones que consuman dicha jerarquía?

---

## Referencias

1. `AED-1286_Programacion_Orientada_a_Objetos (1).pdf`
2. *Java How to Program Early Objects*, 11e (2021).
3. Craig Larman, *UML y Patrones*, 2.ª ed.
4. Joshua Bloch, *Effective Java*, 3rd Edition.
