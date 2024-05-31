
# Laboratorio de optimizacion de codigo

## Escenario 1(javascript):

```
// Inefficient loop handling and excessive DOM manipulation
function updateList(items) {
    let list = document.getElementById("itemList");
    list.innerHTML = "";
    for (let i = 0; i < items.length; i++) {
        let listItem = document.createElement("li"); // creacion ineficiente de elemento mediante el dom
        listItem.innerHTML = items[i];
        list.appendChild(listItem); // se modifica el dom por cada elemento agregado
    }
}
```
### Analisis y Solución:
Revisando el codigo anterior se observa que dentro de cada iteracion en el ciclo for se hace un 
llamado para crear un elemento li hacia el DOM en la linea `let listItem = document.createElement("li");`, lo que puede ser ineficiente debido a que dicha operacion es costosa en terminos de procesamiento,
por lo que se sugiere sacarla del ciclo y en su lugar optar por otro metodo que pemita anexar todos los elementos de la lista una vez que se han procesado

A continuacion se muestra una alternativa mas eficiente:

```
function updateList(items) {
    let list = document.getElementById("itemList");
    let innerHTML = "";
    for (let i = 0; i < items.length; i++) {
        innerHTML += "<li>" + items[i] + "</li>"; // se concatenan los nuevos elementos del li en una variable temporal
    }
    list.innerHTML = innerHTML; // finalmente se inserta la informacion de la lista al dom mediante la asignacion al innerHTML
    list.appendChild(listItem); // solo se hace una unica modificacion hacia el dom mediante el append lo que es mas eficiente
}
```

## Escenario 2(java):

```
// Redundant database queries
public class ProductLoader {
    public List<Product> loadProducts() {
    List<Product> products = new ArrayList<>();
    for (int id = 1; id <= 100; id++) {
        products.add(database.getProductById(id)); // se consulta un elemento por cada iteracion de la bd 
    }
    return products;
    }
}
```

### Analisis y Solución:
En el codigo anterior se observa que dentro de un ciclo se realiza la consulta  hacia la base de datos para  obtener un producto por id,
lo que implica apertura y cierre de la conexion de base de datis por cada iteracion, lo que impacta el performance , por lo que se recomienda hacer una llamada única en lugar de X cantidad
y asi evitar el consumo inecesario de conexiones a BD

A continuacion se muestra una posible solución:

```
public class ProductLoader {
    public List<Product> loadProducts() {
        return database.getProductsRangeById(1, 100); // se modifica el servicio de consulta individual por uno en batch que soporte rangos y con esto se reduce la carga al servidor de base de datos
        // intermanete en la consulta a base de datos cambiaria de select * from  products where id =? a select * from  products where id >= 1 AND id<=100         
    }
}
```

## Escenario 3 (c#):

```
// Unnecessary computations in data processing
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) { // se puede cambiar la manera de procesar los elementos del listado de secuencial hacia paralelo para un mejor tiempo
        if (d % 2 == 0) {
        result.Add(d * 2);
        } else {
        result.Add(d * 3);
        }
    }
    return result;
}
```
### Analisis y Solución:

Para este escenario no me parece  que contenga computo inecesario en el procesamiento de los datos
lo que se pudiera optimizar es el tiempo de operacion y  aplicar el uso de streams para procesar en paralelo el calculo de los registros al no depender unos de otros


```
public List<int> ProcessData(List<int> data) {
    return data.AsParallel()  // se convierte el listado de elementos a un stream paralelo
            .Select(d => d % 2 == 0 ? d * 2 : d * 3) // se aplica la operacion dependiendo del caso par * 2  o impar * 3
            .ToList(); // se convierte nuevamente a una lista            
}
```
