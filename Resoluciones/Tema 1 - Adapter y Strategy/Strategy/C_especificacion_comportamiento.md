# Patrón Strategy – Especificación del comportamiento

**Tema 1 – Punto 2.** Rediseño del CU *Registrar pago de cuota de un préstamo*. El escenario es el pago de una cuota vencida de un préstamo en mora con **recargo porcentual**.

## Problema de diseño

En el análisis, el gestor calculaba el recargo con métodos propios: `calcularDiasAtraso()` y `calcularRecargoAplicable()`. Para eso le pedía al préstamo `obtenerParametrosMora()` y a la definición de mora `getPorcentajeRecargo()` y `getMontoFijoRecargo()`, y elegía la fórmula con un `if`. Según la RN *Definición de Mora*, el recargo puede ser:

- **porcentual**: `recargo = montoBase * porcentajeRecargo * diasAtraso`, por ejemplo 2% diario;
- **fijo**: `recargo = montoFijoRecargo * diasAtraso`, por ejemplo $500 por día.

Con el Strategy, cada forma de cálculo queda encapsulada en una estrategia concreta. El gestor (contexto) delega el cálculo a través de la interfaz `IEstrategiaCalculoMora`. Una nueva forma de cálculo, por ejemplo un recargo escalonado, se agrega con una estrategia nueva, sin modificar el gestor.

## Roles del patrón

| Rol | Clase |
|---|---|
| Contexto | `GestorRegistrarPagoCuota` |
| Estrategia abstracta | `IEstrategiaCalculoMora` |
| Estrategias concretas | `EstrategiaCalculoPorcentual`, `EstrategiaCalculoFijo` |

## Especificación por clase

### GestorRegistrarPagoCuota (contexto)

- **calcularMontoAAbonar()**: método del análisis rediseñado.
  1. Obtiene la fecha actual con `getFechaActual()`.
  2. Obtiene de la cuota seleccionada `getMonto()` y `getFechaVencimiento()`.
  3. Pide la definición de mora al préstamo seleccionado con `obtenerDefinicionMora()`, que a su vez se la pide a su `TipoPrestamo`.
  4. Pregunta a la definición `esPorcentual()` y llama a `crearEstrategiaCalculoMora()`.
  5. Delega el cálculo: `estrategiaCalculoMora.calcularRecargo(...)`.
  6. Calcula `montoAAbonar = montoBase + recargo`.

  Después sigue igual que en el análisis: `mostrarTotalAPagar()` y `pedirConfirmacion()`.
- **crearEstrategiaCalculoMora(definicionMora): IEstrategiaCalculoMora**: **método de enganche**. Si la definición es porcentual crea `EstrategiaCalculoPorcentual`; si no, crea `EstrategiaCalculoFijo`. Guarda la estrategia en el atributo `estrategiaCalculoMora`, tipado con la interfaz.
- **Métodos que se eliminan del gestor:** `calcularDiasAtraso()` y `calcularRecargoAplicable()` pasan a las estrategias. También se eliminan los atributos `diasRetraso`, `porcentajeRecargo` y `montoFijoRecargo`.

### IEstrategiaCalculoMora (estrategia abstracta)

- **calcularRecargo(montoBase, fechaVencimiento, fechaActual, definicionMora): Double**: **método polimórfico**. Recibe por parámetro todo lo que necesita cualquier estrategia. La definición de mora va por parámetro para que la estrategia le pida el valor que le corresponde.
- **calcularDiasAtraso(fechaVencimiento, fechaActual): int**: calcula la diferencia en días entre la fecha actual y la fecha de vencimiento. Por ejemplo, si vence el 1/6 y se paga el 5/6, son 4 días.

### EstrategiaCalculoPorcentual

- **new()**: crea la estrategia.
- **calcularRecargo(...)**: calcula los días de atraso, obtiene `getPorcentajeRecargo()` de la definición de mora y devuelve `montoBase * porcentajeRecargo * diasAtraso`.

### EstrategiaCalculoFijo

- **new()**: crea la estrategia.
- **calcularRecargo(...)**: calcula los días de atraso, obtiene `getMontoFijoRecargo()` de la definición de mora y devuelve `montoFijoRecargo * diasAtraso`. No usa `montoBase`, pero lo recibe para respetar la signatura polimórfica.

### Clases de dominio

- **Prestamo.obtenerDefinicionMora(): DefinicionMora** y **TipoPrestamo.obtenerDefinicionMora(): DefinicionMora**: reemplazan a `obtenerParametrosMora()`. En lugar de devolver los valores sueltos, devuelven el objeto `DefinicionMora`.
- **DefinicionMora.esPorcentual(): Boolean**: método nuevo de diseño. Devuelve `true` si la definición tiene cargado un `porcentajeRecargo` y `false` si tiene un `montoFijoRecargo`.

## Pseudocódigo

```java
// ===== Contexto =====
public void calcularMontoAAbonar() {
    fechaActual           = getFechaActual();
    montoCuota            = cuotaSeleccionada.getMonto();
    fechaVencimientoCuota = cuotaSeleccionada.getFechaVencimiento();
    definicionMora        = prestamoSeleccionado.obtenerDefinicionMora();

    estrategiaCalculoMora = crearEstrategiaCalculoMora(definicionMora);
    recargo = estrategiaCalculoMora.calcularRecargo(
                  montoCuota, fechaVencimientoCuota, fechaActual, definicionMora);

    montoAAbonar = montoCuota + recargo;
    pantalla.mostrarTotalAPagar(montoAAbonar);
    pantalla.pedirConfirmacion();
}

// método de enganche
public IEstrategiaCalculoMora crearEstrategiaCalculoMora(DefinicionMora dm) {
    if (dm.esPorcentual()) return new EstrategiaCalculoPorcentual();
    else                   return new EstrategiaCalculoFijo();
}

// ===== Estrategia abstracta =====
interface IEstrategiaCalculoMora {
    Double calcularRecargo(Double montoBase, Date fechaVencimiento,
                           Date fechaActual, DefinicionMora definicionMora);
    int    calcularDiasAtraso(Date fechaVencimiento, Date fechaActual);
}

// ===== Estrategias concretas =====
class EstrategiaCalculoPorcentual implements IEstrategiaCalculoMora {
    public Double calcularRecargo(Double montoBase, Date fv, Date fa, DefinicionMora dm) {
        int diasAtraso = calcularDiasAtraso(fv, fa);
        Double porcentaje = dm.getPorcentajeRecargo();      // ej: 0.02
        return montoBase * porcentaje * diasAtraso;
    }
    public int calcularDiasAtraso(Date fv, Date fa) {
        return diferenciaEnDias(fa, fv);                     // fa - fv
    }
}

class EstrategiaCalculoFijo implements IEstrategiaCalculoMora {
    public Double calcularRecargo(Double montoBase, Date fv, Date fa, DefinicionMora dm) {
        int diasAtraso = calcularDiasAtraso(fv, fa);
        Double montoFijo = dm.getMontoFijoRecargo();        // ej: 500
        return montoFijo * diasAtraso;
    }
    public int calcularDiasAtraso(Date fv, Date fa) {
        return diferenciaEnDias(fa, fv);
    }
}

// ===== Dominio =====
// Prestamo
public DefinicionMora obtenerDefinicionMora() { return tipoPrestamo.obtenerDefinicionMora(); }
// TipoPrestamo
public DefinicionMora obtenerDefinicionMora() { return definicionMora; }
// DefinicionMora
public Boolean esPorcentual() { return porcentajeRecargo != null && porcentajeRecargo > 0; }
```

## Notas

- La dinámica muestra el escenario del parcial (recargo porcentual). Con recargo fijo la secuencia es la misma, pero se crea `:EstrategiaCalculoFijo` y la estrategia le pide a la definición `getMontoFijoRecargo()`.
- **Ejemplo:** una cuota de $10.000 con mora del 2% diario y 4 días de atraso da un recargo de 10.000 × 0,02 × 4 = **$800**, y un monto a abonar de **$10.800**.
