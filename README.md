# Documentación

![Tinkercad](./img/ArduinoTinkercad.jpg)

## Integrantes

- Mauro Sosa

## Proyecto: Parcial 1.

![Tinkercad](./img/Esquema.png)

## Descripción

El proyecto consta de la implementación de un motor de aficionado, el cual será puesto en marcha una vez que el usuario ingrese la opción indicada por terminal. Estará vinculado con un sensor de temperatura, que mantendrá el motor prendido entre las temperaturas 0°C a 30°C. Además, se dispondrá de un sensor Fotodiodo que controlará la iluminación (entre el siguiente rango: 0 -> oscuridad, 5 -> iluminado), una vez que el nivel de iluminación sea muy bajo se encenderá un led. El giro del motor dependerá del estado del interruptor deslizante (switch).

Por otra parte, se contará con dos display 7 segmentos, los cuales a través del interruptor deslizante (switch) y dos botones (conexión pullup) logran indicar:

- Números primos
- Contador 0 a 99

## Funciones

- Función principal
  En la función principal primeramente se encuentra el control de encendido del motor que a través de la lectura de la entrada por terminal comienza o termina el funcionamiento general.
  Luego una vez que se cambie el estado de la bandera "banderaPrendido", se establece el control del interruptor deslizante (switch). Si este registra voltaje (1 -> HIGH) se podrá controlar la subida o bajada de los números primos por los display, el giro del motor será positivo y se habilitarán los sensores de temperatura y fotodiodo. De lo contrario, no se registra voltaje (0 -> LOW) se podrá controlar la subida o bajada del contador por los display, el giro del motor será negativo y se habilitarán los sensores de temperatura y fotodiodo.

```C (lenguaje en el que esta escrito)
void loop()
{
  // Lecturas de los sensores.
  int lecturaSensorTemperatura = analogRead(SENSOR_TEMPERATURA);
  int temperatura = map(lecturaSensorTemperatura,20,358,-40,125);
  int lecturaFotodiodo = analogRead(FOTODIODO);

  if (Serial.available()>0){// Control de encendido del motor por terminal.
    int opcion = Serial.read();
    if (opcion == '1'){
      Serial.println("Motor prendido (Temp. Funcionamiento -> 0 C a 30 C)");
      banderaPrendido = true;
    }
    else if (opcion == '2'){
      digitalWrite(ENTRADA_UNO_MOTOR, LOW);
      digitalWrite(ENTRADA_DOS_MOTOR, LOW);

      digitalWrite(UNIDAD, HIGH);
      digitalWrite(DECENA, HIGH);

      digitalWrite(LED_BLANCO, LOW);

      Serial.println("Motor apagado");
      banderaPrendido = false;
    }
    else {
      Serial.println("INVALIDO, Ingrese opcion corecta");
    }
  }

  if (banderaPrendido){
    // Lecturas de los botones "sube" / "baja" y interruptor deslizante.
    int sube = digitalRead(SUBE);
    int baja = digitalRead(BAJA);
    int modo = digitalRead(MODO);

    switch (modo){// Control del Modo de funcionamiento.
      case 1: // Numeros primos y Giro (+).
        controlTemperatura(temperatura,0,30,1,0);// Control Temperatura.
        controlFotodiodo (lecturaFotodiodo,2);// Control Fotodiodo.

        banderaSwitch = true;

        if (sube == 1){// Control del boton Sube.
          noPresionoSube = 1;
        }
        if (sube == 0 && noPresionoSube == 1){
          noPresionoSube = 0;
          contadorPrimos += 1;
          while (esPrimo(contadorPrimos) == false){
            contadorPrimos += 1;
          }
        }
        if (baja == 1){// Control del boton Baja.
          noPresionoBaja = 1;
        }
        if (baja == 0 && noPresionoBaja == 1){
          noPresionoBaja = 0;
          contadorPrimos -= 1;
          while (esPrimo(contadorPrimos) == false){
            contadorPrimos -= 1;
          }
        }
        if(contadorPrimos > 97){// Se controla el rango numeros primos.
          contadorPrimos = 2;
        }
        else if(contadorPrimos < 0){
          contadorPrimos = 97;
        }
        numeroPrimoDisplay(contadorPrimos);
        imprimirDisplay(contadorDecena, contadorUnidad);
        break;

      case 0: // Contador y Giro (-).
        controlTemperatura(temperatura,0,30,0,1);// Control Temperatura.
        controlFotodiodo (lecturaFotodiodo,2);// Control Fotodiodo.

        if (banderaSwitch){
          contadorDecena = 0;
          contadorUnidad = 0;
          banderaSwitch = false;
        }

        if (sube == 1){// Control del boton Sube.
          noPresionoSube = 1;
        }
        if (sube == 0 && noPresionoSube == 1){
          noPresionoSube = 0;
          contadorDecena += 1;
          contadorUnidad += 10;
        }
        if (baja == 1){// Control del boton Baja.
          noPresionoBaja = 1;
        }
        if (baja == 0 && noPresionoBaja == 1){
          noPresionoBaja = 0;
          contadorDecena -= 1;
          contadorUnidad -= 10;
        }
        if(contadorDecena == 100){// Se controla el rango -> Decena.
          contadorDecena = 0;
        }
        else if(contadorDecena == -1){
          contadorDecena = 99;
        }
        if(contadorUnidad == 100){// Se controla el rango -> Unidad.
          contadorUnidad = 0;
        }
        else if(contadorUnidad == -10){
          contadorUnidad = 90;
        }
        imprimirDisplay(contadorDecena/10, contadorUnidad/10);
        break;
    }
  }
}
```

- controlTemperatura()
  Esta función a través del parámetro temperatura, que será el que registre el sensor, y los valores mínimo y máximo encenderá o apagará el motor. Los parámetros entrada uno y dos variarían entre 1 o 0 (HIGH o LOW), dependiendo el giro del motor configurado.

```C (lenguaje en el que esta escrito)
void controlTemperatura (int temperatura, int temperaturaMinima, int temperaturaMaxima, int entradaUnoMotor, int entradaDosMotor){
  if (temperatura >= temperaturaMinima && temperatura <= temperaturaMaxima){
    digitalWrite(ENTRADA_UNO_MOTOR, entradaUnoMotor);
    digitalWrite(ENTRADA_DOS_MOTOR, entradaDosMotor);
  }
  else{
    digitalWrite(ENTRADA_UNO_MOTOR, LOW);
    digitalWrite(ENTRADA_DOS_MOTOR, LOW);
  }
}
```

- controlFotodiodo()
  La función controlara el encendido o apagado del led que se encuentra al lado del motor, dependiendo del valor de lectura mínima del fotodiodo pasada por parámetro.

```C (lenguaje en el que esta escrito)
void controlFotodiodo (int lecturaFotodiodo, int estadoLuzMinima){
  if (lecturaFotodiodo < estadoLuzMinima){
    digitalWrite(LED_BLANCO, HIGH);
  }
  else{
    digitalWrite(LED_BLANCO, LOW);
  }
}
```

- imprimirDisplay()
  La función realiza la multiplexación de los display 7 segmentos.

```C (lenguaje en el que esta escrito)
void imprimirDisplay (int contadorDecena, int contadorUnidad){
  prendeDisplay(APAGADOS);
  imprimirDigito(contadorDecena);
  prendeDisplay(DECENA);

  prendeDisplay(APAGADOS);
  imprimirDigito(contadorUnidad);
  prendeDisplay(UNIDAD);
}
```

- prendeDisplay()
  La función dependiendo del parámetro pasado:
  #define APAGADOS 0
  #define DECENA A5
  #define UNIDAD A4
  Controla el estado, encendido o apagado (HIGH o LOW) de los dos disply.

```C (lenguaje en el que esta escrito)
void prendeDisplay (int display){
  if (display == APAGADOS){
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
  else if (display == DECENA){
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
    delay(TIEMPO_DISPAY_ON);
  }
  else if (display == UNIDAD){
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
    delay(TIEMPO_DISPAY_ON);
  }
}
```

- imprimirDigito()
  La función se encarga de encender los distintos leds de los display dependiendo el digito pasado por parámetro (0 a 9).

```C (lenguaje en el que esta escrito)
void imprimirDigito (int digito){
  digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  switch (digito){
    case 1:{
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 2:{
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 3:{
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 4:{
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 5:{
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 6:{
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 7:{
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 8:{
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 9:{
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 0:{
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

- esPrimo()
  La función controla si el número pasado por parámetro es o no un numero primo, retorna True si lo es o False si no.

```C (lenguaje en el que esta escrito)
bool esPrimo(int numero) {
  // Casos especiales
  if (numero == 0 || numero == 1 || numero == 4) return false;
  for (int x = 2; x < numero ; x++) {
    if (numero % x == 0) return false;
  }
  // Si no se pudo dividir por ninguno de los danteriores, sí es primo
  return true;
}
```

- numeroPrimoDisplay()
  La función dependiendo del número primo pasado por parámetro cambia el estado de los contadores (decena y unidad), para poder ser pasados a la función imprimirDisplay.

```C (lenguaje en el que esta escrito)
void numeroPrimoDisplay(int numeroPrimo){
  if (numeroPrimo < 10){
      contadorDecena = 0;
      contadorUnidad = numeroPrimo;
  }
  if (numeroPrimo > 10 && numeroPrimo < 20){
      contadorDecena = 1;
      contadorUnidad = numeroPrimo -10;
  }
  if (numeroPrimo > 20 && numeroPrimo < 30){
      contadorDecena = 2;
      contadorUnidad = numeroPrimo -20;
  }
  if (numeroPrimo > 30 && numeroPrimo < 40){
      contadorDecena = 3;
      contadorUnidad = numeroPrimo -30;
  }
  if (numeroPrimo > 40 && numeroPrimo < 50){
      contadorDecena = 4;
      contadorUnidad = numeroPrimo -40;
  }
  if (numeroPrimo > 50 && numeroPrimo < 60){
      contadorDecena = 5;
      contadorUnidad = numeroPrimo -50;
  }
  if (numeroPrimo > 60 && numeroPrimo < 70){
      contadorDecena = 6;
      contadorUnidad = numeroPrimo -60;
  }
  if (numeroPrimo > 70 && numeroPrimo < 80){
      contadorDecena = 7;
      contadorUnidad = numeroPrimo -70;
  }
  if (numeroPrimo > 80 && numeroPrimo < 90){
      contadorDecena = 8;
      contadorUnidad = numeroPrimo -80;
  }
  if (numeroPrimo > 90 && numeroPrimo < 100){
      contadorDecena = 9;
      contadorUnidad = numeroPrimo -90;
  }
}
```

## :robot: Link al proyecto

- [proyecto](https://www.tinkercad.com/things/kNfmk8niHjr-mauro-sosa-1d-parcial-1-spd-parte-3/editel?sharecode=hpATLfDFD5F7J23Ke82y5dbDq937qWGlckK6yOetmPY)
