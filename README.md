# Documentación

![Tinkercad](./img/ArduinoTinkercad.jpg)

## Integrantes

- Mauro Sosa

## Proyecto: Parcial 1.

![Tinkercad](./img/Esquema.png)

## Descripción

El proyecto costa de dos display 7 segmentos, los cuales a través de un interruptor deslizante (switch) y dos botones (conexión pullup) logran indicar:

- Números primos
- Contador 0 a 99

Además, la función general del proyecto es controlar un motor de aficionado, el cual será puesto en marcha una vez que el usuario ingrese la opción indicada por terminal. Estará vinculado con un sensor de temperatura, que mantendra el motor prendido entre las temperaturas 0°C a 30°C. El giro del motor dependerá del estado del interruptor deslizante (switch).

## Función principal

- imprimirDigito()

```C (lenguaje en el que esta escrito)
void imprimirDigito (int digito)
{
  digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  switch (digito)
  {
    case 1:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 2:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 3:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 4:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 5:
    {
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 6:
    {
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 7:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 8:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 9:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 0:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      break;
    }
  }
}
```

- imprimirDisplay()

```C (lenguaje en el que esta escrito)

```

- prendeDisplay()

```C (lenguaje en el que esta escrito)

```

- esPrimo()

```C (lenguaje en el que esta escrito)

```

- numeroPrimoDisplay()

```C (lenguaje en el que esta escrito)

```

## :robot: Link al proyecto

- [proyecto](https://www.tinkercad.com/things/2qQMSe9IU7U-mauro-sosa-1d-parcial-parte-2-/editel?sharecode=W0wKO39AYSWAxuNdpO5i2tp-pWip2XSOkXhDSDocGd0)

---

<!-- ### Fuentes

- [Consejos para documentar](https://www.sohamkamani.com/how-to-write-good-documentation/#architecture-documentation).

- [Lenguaje Markdown](https://markdown.es/sintaxis-markdown/#linkauto).

- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

- [Tutorial](https://www.youtube.com/watch?v=oxaH9CFpeEE).

- [Emojis](https://gist.github.com/rxaviers/7360908).

--- -->
