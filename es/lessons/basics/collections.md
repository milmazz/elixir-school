---
layout: page
title: Colecciones
category: basics
order: 2
lang: es
---

Listas, tuplas, listas de palabras clave, mapas, diccionarios y combinadores funcionales

## Tabla de Contenidos

- [Listas](#listas)
	- [Concatenación de listas](#concatenación-de-listas)
	- [Sustracción de listas](#sustraccion-de-listas)
	- [Cabeza/Cola](#cabeza/cola)
- [Tuplas](#tuplas)
- [Listas de palabas claves](#listas-de-palabras-clave)
- [Mapas](#mapas)
- [Diccionarios](#diccionarios)

## Listas

Las listas son simples colleciones de valores, estas pueden incluir múltiples tipos; las listas puede incluir valores no únicos

```elixir
iex> [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
```

Elixir implementa las listas como listas enlazadas. Esto significa que acceder a la longitud de la lista es una operación `O(n)`. Por esta razón, es normalmente mas rápido agregar un elemento al inicio que al final.

```elixir
iex> list = [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
iex> ["π"] ++ list
["π", 3.41, :pie, "Apple"]
iex> list ++ ["Cherry"]
[3.41, :pie, "Apple", "Cherry"]
```


### Concatenación de listas

La concatenation de listas usa el operador `++/2`.

```elixir
iex> [1, 2] ++ [3, 4, 1]
[1, 2, 3, 4, 1]
```

### Sustracción de listas

El soporte para sustracción es provisto por el operador `--/2`; es seguro sustraer un valor que no existente:

```elixir
iex> ["foo", :bar, 42] -- [42, "bar"]
["foo", :bar]
```

### Cabeza/Cola

Cuando usas listas es común trabajar con la cabeza y la cola de la lista. La cabeza es el primer elemento de la lista y la cola son los elementos restantes. Elixir provee dos funciones útiles, `hd` y `tl`, para trabajar con estas partes:

```elixir
iex> hd [3.41, :pie, "Apple"]
3.41
iex> tl [3.41, :pie, "Apple"]
[:pie, "Apple"]
```

Ademas de la funciones citadas, puedes usar el operador tuberia `|`; veremos este patrón en futuras lecciones:

```elixir
iex> [h|t] = [3.41, :pie, "Apple"]
[3.41, :pie, "Apple"]
iex> h
3.41
iex> t
[:pie, "Apple"]
```

## Tuplas

Las tuplas son similares a las listas pero son guardadas de manera contigua en memoria. Esto permite acceder a su longitud de forma rápida pero hace su modificación costosa; la nueva tupla debe ser copiada entera en memoria. Las tuplas son definidas con llaves.

```elixir
iex> {3.41, :pie, "Apple"}
{3.41, :pie, "Apple"}
```

Es común para las tuplas ser usadas como un mecanismo que retorna información adicional de funciones; la utilidad de esto será mas aparante cuando entremos en la coincidencia de patrones:

```elixir
iex> File.read("path/to/existing/file")
{:ok, "... contents ..."}
iex> File.read("path/to/unknown/file")
{:error, :enoent}
```

## Listas de palabras clave

Las listas de palabras clave y los mapas son colecciones asociativas de Elixir; ambas implementan el módulo `Dict`. En Elixir, una lista de palabras clave es una lista especial de tuplas cuyo primer elemento es un átomo; ellos comparten el rendimiento de las listas:

```elixir
iex> [foo: "bar", hello: "world"]
[foo: "bar", hello: "world"]
iex> [{:foo, "bar"}, {:hello, "world"}]
[foo: "bar", hello: "world"]
```

Las tres características resaltantes de las listas de palabras clave son:

+ Las llaves son átomos.
+ Las llaves estan ordenadas.
+ Las llaves no son únicas.

Por estas razones las listas de palabras clave son comunmente usadas para pasar opciones a funciones.

## Mapas

A diferencia de las listas de palabras clave estos permiten llaves de cualquier tipo y no siguen un órden. Tu puedes definir un mapa con la sintaxis `%{}`:

```elixir
iex> map = %{:foo => "bar", "hello" => :world}
%{:foo => "bar", "hello" => :world}
iex> map[:foo]
"bar"
iex> map["hello"]
:world
```

Si un elemento duplicado es agregado al mapa, este reemplazará el valor anterior:

```elixir
iex> %{:foo => "bar", :foo => "hello world"}
%{foo: "hello world"}
```

Como podemos ver de la salida anterior, hay una sintaxis especial para los mapas que solo contienen átomos como llaves:

```elixir
iex> %{foo: "bar", hello: "world"}
%{foo: "bar", hello: "world"}

iex> %{foo: "bar", hello: "world"} == %{:foo => "bar", :hello => "world"}
true
```

## Diccionarios

En Elixir, ambos, listas de palabras claves y mapas implementan el módulo `Dict`; como tales se conocen colectivamente como diccionarios. Si necesitas construir tu propio almacenamiento clave-valor, implementar el módulo `Dict` es un buen lugar para empezar.

El [módulo `Dict`](http://elixir-lang.org/docs/stable/elixir/#!Dict.html) provee un número de funciones útiles para interactuar y manipular esos diccionarios:

```elixir
# keyword lists
iex> Dict.put([foo: "bar"], :hello, "world")
[hello: "world", foo: "bar"]

# maps
iex> Dict.put(%{:foo => "bar"}, "hello", "world")
%{:foo => "bar", "hello" => "world"}

iex> Dict.has_key?(%{:foo => "bar"}, :foo)
true
```
