@startuml
class Cliente{
    + nombre : str
    + apellido : str
    + documento str
    + email : str
    + telefono : str
}

class Prestamo{
    + montoProporcionado : float
    + plazoMaximoDeDevolucion : date
}

class EstadoPrestamo{
    + nombre : str
    + descripcion : str
}

class TipoPrestamo{
    + nombre : str
    + descripcion : str
}

class Cuota{
    + montoAAbonar : float
    + fechaLimite : date
    - calcularDiasExcedidos()
}

class EstadoCuota{
    + nombre : str
    + descripcion : str
}

class Pago{
    + monto : float
    + fechaDePago : date
}

class MedioDePago{
    + nombre : str
}

class PlanDePago{
    + fechaLimiteCondonamiento : date
    + porcentajeReduccion : float
    + montoDePlan : float
}

class DefinicionMora{
    reglaCalculoMora : str
}

class Factura{
    
}

Cliente --> Prestamo : prestamo 1..*
Prestamo -> TipoPrestamo : tipoPrestamo 1
TipoPrestamo --> DefinicionMora : definicionMora 1
Prestamo -> EstadoPrestamo : estadoPrestamo 1
Prestamo --> Cuota : cuota 1..*
Cuota -> EstadoCuota : estadoCuota 1
Cuota --> Pago : pago 0...1
Pago -> MedioDePago : medioDePago 1
PlanDePago -> Prestamo : prestamo 1
PlanDePago -> Cuota : cuota 1...*
Pago -> Factura : factura 1
@enduml