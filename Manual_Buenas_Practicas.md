# Manual de Buenas Prácticas en C#

## Índice
1. Comentarios y Documentación
2. Nombres de Variables y Métodos
3. Estructura de Clases
4. Optimización de Código
5. Manejo de Excepciones
6. Buenas Prácticas de Programación

## Comentarios y Documentación

### Summary
Utiliza `///` para generar comentarios XML que describan métodos, clases y propiedades.

```csharp
/// <summary>
/// Este método calcula la suma de dos números.
/// </summary>
/// <param name="a">Primer número</param>
/// <param name="b">Segundo número</param>
/// <returns>La suma de los dos números</returns>
public int Sumar(int a, int b)
{
    return a + b;
}
