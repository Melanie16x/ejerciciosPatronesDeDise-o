# Ejercicios de Patrones De Diseño

## Patrón Singleton

### Ejercicio 1: Gestionar Configuración Global del Sistema

Objetivo: Implementar un patrón Singleton para gestionar la configuración global de la 
aplicación de inventario.
* Crear una clase Configuracion que siga el patrón Singleton.
* Esta clase debe almacenar propiedades como idioma, rutaBaseDatos y nivelRegistro.
* Agregar métodos para obtener y actualizar cada una de estas propiedades.
* Asegurar que solo exista una instancia de Configuracion en toda la aplicación.

```
class configGlobal {
    private static instancia: configGlobal;
    private idioma: string;
    private rutaBaseDeDatos: string;
    private nivelRegistro: string;

    private constructor() {
        this.idioma = "Español";
        this.rutaBaseDeDatos = "localhost";
        this.nivelRegistro = "info"
    }

    public static obtenerInstancia(): configGlobal {
        if (!configGlobal.instancia) {
            configGlobal.instancia = new configGlobal()
        }
        return configGlobal.instancia;
    }

    public obtenerIdioma(): string {
        return this.idioma;
    }

    public obtenerRutaBaseDeDatos(): string {
        return this.rutaBaseDeDatos;
    }

    public obtenerNivelRegistro(): string {
        return this.nivelRegistro;
    }

    public actualizarIdioma(nuevoIdioma: string): void {
        this.idioma = nuevoIdioma;
    }

    public actualizarRutaBaseDeDatos(nuevaRuta: string): void {
        this.rutaBaseDeDatos = nuevaRuta;
    }

    public actualizarNivelRegistro(nuevoNivel: string): void {
        this.nivelRegistro = nuevoNivel;
    }

}

const configuracion = configGlobal.obtenerInstancia();
console.log(configuracion.obtenerIdioma());
configuracion.actualizarIdioma("Ingles");
console.log(configuracion.obtenerIdioma());

console.log(configuracion.obtenerRutaBaseDeDatos());
configuracion.actualizarRutaBaseDeDatos("192.168.1.1");
console.log(configuracion.obtenerRutaBaseDeDatos());

console.log(configuracion.obtenerNivelRegistro());
configuracion.actualizarNivelRegistro("debug");
console.log(configuracion.obtenerNivelRegistro());
```


### Ejercicio 2: Control de Conexiones a la Base de Datos

Objetivo: Utilizar el patrón Singleton para manejar la conexión a la base de datos de inventario.
* Crear una clase ConexionDB que siga el patrón Singleton.
* Esta clase debe manejar la conexión a una base de datos ficticia, con propiedades como host, puerto y usuario.
* Implementar métodos para conectar y desconectar la base de datos.
* Garantizar que todas las partes de la aplicación utilicen la misma instancia de ConexionDB.

```
class ConexionDB {
    private static instancia: ConexionDB;
    private host: string;
    private puerto: number;
    private usuario: string;
    private conectada: boolean;

    private constructor() {
        this.host = "localhost";
        this.puerto = 3000;
        this.usuario = "admin";
        this.conectada = false;
    }

    public static obtenerInstancia(): ConexionDB {
        if (!ConexionDB.instancia) {
            ConexionDB.instancia = new ConexionDB()
        }
        return ConexionDB.instancia;
    }

    public conectar(): void {
        if (!this.conectada) {
            this.conectada = true;
            console.log(`Conectado a la base de datos en ${this.host}:${this.puerto} como ${this.usuario}`)
        } else {
            console.log("Ya hay una conexion activa")
        }
    }

    public desconectar(): void {
        if (this.conectada) {
            this.conectada = false;
            console.log("Desconectado de la base de datos.");
        } else {
            console.log("No hay conexion activa para desconectar")
        }
    }

}

const conexion = ConexionDB.obtenerInstancia();
conexion.conectar();
conexion.desconectar();
```

## Patrón Factory Method

### Ejercicio 1: Crear Dispositivos de Entrada

Objetivo: Utilizar el patrón Factory Method para crear diferentes tipos de dispositivos de entrada.
* Crear una clase DispositivoEntradaFactory con un método crearDispositivo que, basado en el tipo de dispositivo ("Teclado", "Ratón", "Scanner"), devuelva una instancia de la clase adecuada.
* Crear clases específicas para cada tipo de dispositivo (Teclado, Ratón, Scanner), cada una con sus propias propiedades (Ej.: tipoConexion, marca).
* Integrar la creación de estos dispositivos en el sistema de inventario.

```
interface DispositivoEntrada {
    tipo: string;
    tipoConexion: string;
    marca: string;
    detalles(): string;
}

class Teclado implements DispositivoEntrada {
    tipo: string;
    tipoConexion: string;
    marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipo = "Teclado";
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Conexion: ${this.tipoConexion}`;
    }
}

class Raton implements DispositivoEntrada {
    tipo: string;
    tipoConexion: string;
    marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipo = "Raton";
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Conexion: ${this.tipoConexion}`;
    }
}


class Scanner implements DispositivoEntrada {
    tipo: string;
    tipoConexion: string;
    marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipo = "Scanner";
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Conexion: ${this.tipoConexion}`;
    }
}

class DispositivoEntradaFactory {
    crearDispositivo(tipo: string, tipoConexion: string, marca: string): DispositivoEntrada {
        if (tipo === "Teclado") {
            return new Teclado(tipoConexion, marca);
        } else if (tipo === "Raton") {
            return new Raton(tipoConexion, marca);
        } else if (tipo === "Scanner") {
            return new Scanner(tipoConexion, marca);
        } else {
            throw new Error("Tipo de dispositivo no encontrado");
        }
    }
}

const factory = new DispositivoEntradaFactory();

const teclado = factory.crearDispositivo("Teclado", "USB", "Logitech");
console.log(teclado.detalles());

const raton = factory.crearDispositivo("Raton", "Inalambrico", "HP");
console.log(raton.detalles());

const scanner = factory.crearDispositivo("Scanner", "USB", "Epson");
console.log(scanner.detalles());
```

### Ejercicio 2: Fabricar Periféricos de Salida

Objetivo: Implementar el patrón Factory Method para crear diferentes periféricos de salida.
* Crear una clase PerifericoSalidaFactory con un método crearPeriferico que, basado en el tipo ("Monitor", "Impresora", "Proyector"), devuelva una instancia de la clase correspondiente.
* Crear clases específicas para cada tipo de periférico (Monitor, Impresora, Proyector), con propiedades particulares (Ej.: resolución para Monitor, tipoImpresión para Impresora).
* Asegurar que el factory maneje correctamente la creación de cada periférico según el tipo especificado.

```
interface PerifericoSalida {
    tipo: string;
    marca: string;
    detalles(): string;
}

class Monitor {
    tipo: string;
    marca: string;
    resolucion: string;

    constructor(marca: string, resolucion: string) {
        this.tipo = "Monitor";
        this.marca = marca;
        this.resolucion = resolucion;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Resolucion: ${this.resolucion}`;
    }
}

class Impresora {
    tipo: string;
    marca: string;
    tipoImpresion: string;

    constructor(marca: string, tipoImpresion: string) {
        this.tipo = "Impresora";
        this.marca = marca;
        this.tipoImpresion = tipoImpresion;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Tipo de Impresion: ${this.tipoImpresion}`;
    }
}

class Proyector {
    tipo: string;
    marca: string;
    resolucion: string;

    constructor(marca: string, resolucion: string) {
        this.tipo = "Proyector";
        this.marca = marca;
        this.resolucion = resolucion;
    }

    detalles(): string {
        return `Tipo: ${this.tipo}, Marca: ${this.marca}, Resolucion: ${this.resolucion}`;
    }
}

class PerifericoSalidaFactory {
    crearPerferico(tipo: string, marca: string, propiedad: string): PerifericoSalida {
        if (tipo === "Monitor") {
            return new Monitor(marca, propiedad);
        } else if (tipo === "Impresora") {
            return new Impresora(marca, propiedad);
        } else if (tipo === "Proyector") {
            return new Proyector(marca, propiedad);
        } else {
            throw new Error("Tipo de periferico no encontrado")
        }
    }
}


const factory = new PerifericoSalidaFactory();

const monitor = factory.crearPerferico("Monitor", "Dell", "1920x1080");
console.log(monitor.detalles());

const impresora = factory.crearPerferico("Impresora", "HP", "Laser");
console.log(impresora.detalles());

const proyector = factory.crearPerferico("Proyector", "Epson", "4k");
console.log(proyector.detalles());
```

## Patrón Observer

### Ejercicio 1: Notificación de Mantenimiento Preventivo

Objetivo: Utilizar el patrón Observer para notificar al departamento de mantenimiento cuando un equipo alcanza un cierto tiempo de uso.
* Crear una clase DepartamentoMantenimiento que actúe como observador y reciba notificaciones cuando un equipo necesite mantenimiento preventivo.
* Implementar la clase Equipo que permita agregar observadores y notifique a los observadores cuando su tiempo de uso alcance un umbral definido.
* Definir propiedades como nombre, tipo, estado y tiempoUso en la clase Equipo.

```
interface Observador {
    notificar(mensaje: string): void;
}

class DepartamentoMantenimiento implements Observador {
    notificar(mensaje: string): void {
        console.log(`Departamento de Mantenimiento notificado: ${mensaje}`)
    }
}

class Equipo {
    private nombre: string;
    private tipo: string;
    private estado: string;
    private tiempoUso: number;
    private umbralMantenimiento: number;
    private observadores: Observador[] = [];

    constructor(nombre: string, tipo: string, estado: string, umbralMantenimiento: number) {
        this.nombre = nombre;
        this.estado = estado;
        this.tipo = tipo;
        this.tiempoUso = 0;
        this.umbralMantenimiento = umbralMantenimiento;
    }

    agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    incrementarTiempoUso(horas: number): void {
        this.tiempoUso += horas;
        console.log(`${this.nombre} ha sido usado por ${horas} horas. Tiempo total de uso: ${this.tiempoUso} horas`)
        this.verificarMantenimiento();
    }

    private verificarMantenimiento(): void {
        if (this.tiempoUso >= this.umbralMantenimiento) {
            this.notificarObservadores();
        }
    }

    private notificarObservadores(): void {
        const mensaje = `${this.nombre} ha alcansado el tipo de uso limite (${this.tiempoUso} horas). Se requiere mantenimiento preventivo`
        this.observadores.forEach(observador => observador.notificar(mensaje));
    }
}

const departamentoMantenimiento = new DepartamentoMantenimiento();
const equipo = new Equipo("Servidor Dell", "Servidor", "En uso", 100)

equipo.agregarObservador(departamentoMantenimiento);

equipo.incrementarTiempoUso(50);
equipo.incrementarTiempoUso(60);
```

### Ejercicio 2: Actualización de Inventario en Tiempo Real

Objetivo: Implementar el patrón Observer para actualizar en tiempo real la interfaz de usuario cuando se realicen cambios en el inventario.
* Crear una clase InterfazUsuario que actúe como observador y actualice la visualización del inventario cuando se agreguen, eliminen o modifiquen equipos.
* Modificar la clase Inventario para que permita agregar observadores y notifique a los observadores correspondientes cuando ocurra un cambio en la lista de equipos.
* Asegurar que múltiples instancias de InterfazUsuario puedan recibir y manejar las notificaciones adecuadamente.

```
interface Observador {
    actualizar(mensaje: string): void;
}

class InterfazUsuario implements Observador {
    private id: number;

    constructor(id: number) {
        this.id = id;
    }

    actualizar(mensaje: string): void {
        console.log(`Interfaz de Usuario ${this.id} actualizada: ${mensaje}`);
    }
}

class Inventario {
    private equipos: { nombre: string, tipo: string, estado: string }[] = [];
    private observadores: Observador[] = [];

    agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    private notificarObservadores(mensaje: string): void {
        this.observadores.forEach(observador => observador.actualizar(mensaje));
    }

    agregarEquipo(nombre: string, tipo: string, estado: string): void {
        this.equipos.push({ nombre, tipo, estado });
        this.notificarObservadores(`Equipo agregado: ${nombre} (${tipo}) - Estado: ${estado}`);
    }

    eliminarEquipo(nombre: string): void {
        this.equipos = this.equipos.filter(equipo => equipo.nombre !== nombre);
        this.notificarObservadores(`Equipo eliminado: ${nombre}`);
    }

    modificarEquipo(nombre: string, nuevoEstado: string): void {
        const equipo = this.equipos.find(equipo => equipo.nombre === nombre);
        if (equipo) {
            equipo.estado = nuevoEstado;
            this.notificarObservadores(`Equipo modificado: ${nombre} - Nuevo estado: ${nuevoEstado}`);
        }
    }

    listarEquipos(): { nombre: string, tipo: string, estado: string }[] {
        return this.equipos;
    }
}

const interfaz1 = new InterfazUsuario(1);
const interfaz2 = new InterfazUsuario(2);

const inventario = new Inventario();

inventario.agregarObservador(interfaz1);
inventario.agregarObservador(interfaz2);

inventario.agregarEquipo("Laptop HP", "Portátil", "Disponible");

inventario.eliminarEquipo("Laptop HP");

inventario.agregarEquipo("Monitor Dell", "Monitor", "En uso");
inventario.modificarEquipo("Monitor Dell", "Disponible");
```

## Patrón Adaptador

### Ejercicio 1: Integrar Sistema de Facturación Antiguo

Objetivo: Implementar el patrón Adaptador para integrar un sistema antiguo de facturación con el nuevo sistema de inventario.
* Crear una clase FacturacionVieja que tenga métodos como crearFactura y obtenerFactura.
* Implementar una clase AdaptadorFacturacion que permita utilizar FacturacionVieja con la nueva interfaz IFacturacion, la cual requiere métodos como generarFactura y consultarFactura.
* Asegurar que las facturas generadas a través del adaptador sean compatibles con el nuevo sistema de inventario.

```
class FacturacionVieja {
    private facturas: { id: number; detalle: string; monto: number }[] = [];

    crearFactura(id: number, detalle: string, monto: number): void {
        this.facturas.push({ id, detalle, monto });
        console.log(`Factura vieja creada: ID=${id}, Detalle=${detalle}, Monto=${monto}`);
    }

    obtenerFactura(id: number): { id: number; detalle: string; monto: number } | undefined {
        return this.facturas.find(factura => factura.id === id);
    }
}

interface IFacturacion {
    generarFactura(id: number, descripcion: string, total: number): void;
    consultarFactura(id: number): { id: number; descripcion: string; total: number } | undefined;
}

class AdaptadorFacturacion implements IFacturacion {
    private facturacionVieja: FacturacionVieja;

    constructor(facturacionVieja: FacturacionVieja) {
        this.facturacionVieja = facturacionVieja;
    }

    generarFactura(id: number, descripcion: string, total: number): void {
        this.facturacionVieja.crearFactura(id, descripcion, total);
    }

    consultarFactura(id: number): { id: number; descripcion: string; total: number } | undefined {
        const factura = this.facturacionVieja.obtenerFactura(id);
        if (factura) {
            return {
                id: factura.id,
                descripcion: factura.detalle,
                total: factura.monto,
            };
        }
        return undefined;
    }
}

const sistemaAntiguo = new FacturacionVieja();
const adaptadorFacturacion = new AdaptadorFacturacion(sistemaAntiguo);

adaptadorFacturacion.generarFactura(1, "Compra de Servidor", 5000);
const factura = adaptadorFacturacion.consultarFactura(1);
if (factura) {
    console.log(`Factura consultada: ID=${factura.id}, Descripción=${factura.descripcion}, Total=${factura.total}`);
} else {
    console.log("Factura no encontrada.");
}
```

### Ejercicio 2: Compatibilidad con APIs Externas

Objetivo: Utilizar el patrón Adaptador para integrar una API externa de proveedores con el sistema de inventario existente.
* Crear una clase ProveedorExternoAPI que ofrezca métodos como fetchProductos y updateStock.
* Implementar una clase AdaptadorProveedor que permita utilizar ProveedorExternoAPI con la interfaz IProveedor, que requiere métodos como obtenerProductos y actualizarInventario.
* Asegurar que los datos obtenidos de la API externa se adapten correctamente al formato requerido por el sistema de inventario.

```
class ProveedorExternoAPI {
    fetchProductos(): { id: number; nombre: string; stock: number }[] {
        return [
            { id: 1, nombre: "Teclado", stock: 100 },
            { id: 2, nombre: "Ratón", stock: 150 }
        ];
    }

    updateStock(id: number, nuevoStock: number): void {
        console.log(`API externa: Producto con ID=${id} actualizado a stock=${nuevoStock}`);
    }
}

interface IProveedor {
    obtenerProductos(): { id: number; nombre: string; cantidadDisponible: number }[];
    actualizarInventario(id: number, cantidad: number): void;
}

class AdaptadorProveedor implements IProveedor {
    private proveedorExterno: ProveedorExternoAPI;

    constructor(proveedorExterno: ProveedorExternoAPI) {
        this.proveedorExterno = proveedorExterno;
    }

    obtenerProductos(): { id: number; nombre: string; cantidadDisponible: number }[] {
        const productosExterno = this.proveedorExterno.fetchProductos();
        return productosExterno.map(producto => ({
            id: producto.id,
            nombre: producto.nombre,
            cantidadDisponible: producto.stock
        }));
    }

    actualizarInventario(id: number, cantidad: number): void {
        this.proveedorExterno.updateStock(id, cantidad);
    }
}


const apiExterna = new ProveedorExternoAPI();
const adaptadorProveedor = new AdaptadorProveedor(apiExterna);

const productos = adaptadorProveedor.obtenerProductos();
console.log("Productos obtenidos del proveedor externo:", productos);

adaptadorProveedor.actualizarInventario(1, 120);
```