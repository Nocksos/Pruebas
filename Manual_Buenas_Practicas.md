# Buenas Prácticas para Programación en C\#

## **1. Estructura General del Código**

### Organización del Código

1. **Usa espacios de nombres (namespaces):** Agrupa clases relacionadas bajo un mismo espacio de nombres.
2. **Divide el código en capas:** Utiliza una arquitectura en capas como MVC o Clean Architecture para separar responsabilidades (ej.: lógica de negocio, acceso a datos, presentación).
3. **Archivos separados:** Cada clase debe estar en su propio archivo con un nombre que coincida con el nombre de la clase.

### Ejemplo:

```csharp
namespace MiAplicacion.Servicios
{
    public class CalculadoraService
    {
        // Métodos relacionados con cálculos matemáticos
    }
}
```

---

## **2. Nomenclatura**

### Convenciones de Nombres

1. **Clases:** Usa PascalCase (ej.: `CalculadoraService`).
2. **Métodos:** Usa PascalCase (ej.: `CalcularArea`).
3. **Propiedades:** Usa PascalCase (ej.: `NombreCliente`).
4. **Variables locales y parámetros:** Usa camelCase (ej.: `numero`).
5. **Constantes:** Usa UPPER_CASE con guiones bajos (ej.: `PI_VALOR`).

### Ejemplo:

```csharp
public class Cliente
{
    public string NombreCliente { get; set; } // Propiedad

    public void MostrarInformacion() // Método
    {
        string mensaje = "Información del cliente"; // Variable local
        Console.WriteLine(mensaje);
    }
}
```

---

## **3. Documentación XML**

### ¿Por qué usar documentación XML?

- Facilita el entendimiento del propósito del código.
- Proporciona información en IntelliSense.
- Ayuda a otros desarrolladores a utilizar tus métodos correctamente.


### Ejemplo Completo:

```csharp
/// &lt;summary&gt;
/// Clase que representa un cliente.
/// &lt;/summary&gt;
public class Cliente
{
    /// &lt;summary&gt;
    /// Nombre del cliente.
    /// &lt;/summary&gt;
    public string NombreCliente { get; set; }

    /// &lt;summary&gt;
    /// Obtiene la información completa del cliente.
    /// &lt;/summary&gt;
    /// &lt;returns&gt;Una cadena con la información del cliente.&lt;/returns&gt;
    public string ObtenerInformacion()
    {
        return $"Cliente: {NombreCliente}";
    }
}
```

---

## **4. Manejo de Excepciones**

### Buenas Prácticas:

1. Usa excepciones específicas (`ArgumentException`, `InvalidOperationException`, etc.) en lugar de capturar `Exception`.
2. No abuses de las excepciones: solo úsalas para errores inesperados.
3. Registra los errores para facilitar el diagnóstico (puedes usar herramientas como Serilog o log4net).

### Ejemplo:

```csharp
public void ProcesarArchivo(string rutaArchivo)
{
    try
    {
        if (string.IsNullOrEmpty(rutaArchivo))
        {
            throw new ArgumentException("La ruta del archivo no puede estar vacía.");
        }

        using (var lector = new StreamReader(rutaArchivo))
        {
            string contenido = lector.ReadToEnd();
            Console.WriteLine(contenido);
        }
    }
    catch (FileNotFoundException ex)
    {
        Console.WriteLine($"Error: El archivo no existe. Detalles: {ex.Message}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error inesperado: {ex.Message}");
        throw; // Relanzar excepción para manejo superior
    }
}
```

---

## **5. Principios SOLID**

Adherirse a los principios SOLID mejora la calidad y escalabilidad del diseño.

### Resumen de SOLID:

1. **Responsabilidad Única:** Cada clase debe tener una única responsabilidad.
2. **Abierto/Cerrado:** Las clases deben ser abiertas para extensión pero cerradas para modificación.
3. **Sustitución de Liskov:** Las clases derivadas deben poder usarse como clases base sin problemas.
4. **Segregación de Interfaces:** Divide interfaces grandes en más pequeñas y específicas.
5. **Inversión de Dependencias:** Las clases deben depender de abstracciones, no implementaciones concretas.

### Ejemplo:

```csharp
// Interface segregada
public interface IRepositorioCliente
{
    Cliente ObtenerPorId(int id);
}

// Implementación concreta
public class RepositorioCliente : IRepositorioCliente
{
    public Cliente ObtenerPorId(int id)
    {
        // Lógica para obtener un cliente por ID
        return new Cliente { NombreCliente = "Juan Pérez" };
    }
}
```

---

## **6. Métodos Limpios y Reutilizables**

### Recomendaciones:

1. Evita métodos largos: divide las tareas complejas en métodos más pequeños.
2. Utiliza parámetros opcionales cuando sea apropiado.
3. Devuelve valores claros y manejables.

### Ejemplo:

```csharp
/// <summary>
/// Calcula el área de un rectángulo.
/// </summary>
/// <param name="ancho">Ancho del rectángulo.</param>
/// <param name="alto">Alto del rectángulo.</param>
/// <returns>El área calculada.</returns>
/// <exception cref="ArgumentException">
/// Se lanza si el ancho o el alto son menores o iguales a cero.
/// </exception>
public double CalcularArea(double ancho, double alto)
{
    if (ancho &lt;= 0 || alto &lt;= 0)
    {
        throw new ArgumentException("El ancho y el alto deben ser mayores a cero.");
    }

    return ancho * alto;
}
```

---

## **7. Comentarios y Espaciado**

### Reglas:

1. Usa comentarios solo cuando sea necesario explicar algo complejo o poco evidente.
2. Deja líneas en blanco entre bloques lógicos para mejorar la legibilidad.

### Ejemplo:

```csharp
// Inicializar variables principales
int resultado = 0;

// Realizar cálculo principal
resultado = CalcularArea(5, 10);

// Mostrar resultado al usuario
Console.WriteLine($"El área es: {resultado}");
```

---

## **8. Ejemplo Completo**

Un ejemplo que combina todas las buenas prácticas mencionadas:

```csharp
using System;
using System.IO;

namespace MiAplicacion.Servicios
{
    /// &lt;summary&gt;
    /// Servicio para realizar cálculos matemáticos.
    /// &lt;/summary&gt;
    public class CalculadoraService
    {
        /// &lt;summary&gt;
        /// Calcula el área de un rectángulo.
        /// &lt;/summary&gt;
        /// &lt;param name="ancho"&gt;Ancho del rectángulo.&lt;/param&gt;
        /// &lt;param name="alto"&gt;Alto del rectángulo.&lt;/param&gt;
        /// &lt;returns&gt;El área calculada.&lt;/returns&gt;
        /// &lt;exception cref="ArgumentException"&gt;
        /// Se lanza si el ancho o el alto son menores o iguales a cero.
        /// &lt;/exception&gt;
        public double CalcularArea(double ancho, double alto)
        {
            if (ancho &lt;= 0 || alto &lt;= 0)
            {
                throw new ArgumentException("El ancho y el alto deben ser mayores a cero.");
            }

            return ancho * alto;
        }

        /// &lt;summary&gt;
        /// Procesa un archivo y muestra su contenido.
        /// &lt;/summary&gt;
        /// &lt;param name="rutaArchivo"&gt;Ruta completa del archivo.&lt;/param&gt;
        public void ProcesarArchivo(string rutaArchivo)
        {
            try
            {
                if (string.IsNullOrEmpty(rutaArchivo))
                {
                    throw new ArgumentException("La ruta del archivo no puede estar vacía.");
                }

                using (var lector = new StreamReader(rutaArchivo))
                {
                    string contenido = lector.ReadToEnd();
                    Console.WriteLine(contenido);
                }
            }
            catch (FileNotFoundException ex)
            {
                Console.WriteLine($"Error: El archivo no existe. Detalles: {ex.Message}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error inesperado: {ex.Message}");
                throw;
            }
        }
    }
}
```
