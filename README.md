## Alumno.
- Mateo Ciotti

# Proyecto
<img src="https://github.com/0Mateciotti/Parcial-Spd/blob/main/Parcial/Fotos/arduino.PNG?raw=true" width="800"/>
<img src="https://github.com/0Mateciotti/Parcial-Spd/blob/main/Parcial/Fotos/esqueletico.PNG?raw=true" width="800"/>

# Funcionamiento del programa
### El programa cuenta con 3 botones (uno para subir de piso, otro para bajar de piso y otro para frenar el montacargas), un led rojo que indica que el montacargas no se esta moviendo , uno verde que indica que si se esta moviendo y un display de 7 segmentos que muestra en cada momento el piso en el que se encuentra.



# Defines y declaraciones.
~~~ C
#define SEGMENTOA 10
#define SEGMENTOB 9
#define SEGMENTOC 6
#define SEGMENTOD 7
#define SEGMENTOE 8
#define SEGMENTOF 11
#define SEGMENTOG 12
#define BOTON_SUBIR A0
#define BOTON_BAJAR A1
#define BOTON_PAUSA A2
#define LEDROJO 5
#define LEDVERDE 4
#define PAUSA 3000
int piso = 0;
int subir;
int bajar;
int pausa;
int flag =0;
~~~
# Set up y loop.
~~~ C
void setup()
{
  
  for (int i = 4; i<=12;i++){
    pinMode(i, OUTPUT);
  }
  
  pinMode(BOTON_SUBIR, OUTPUT);
  pinMode(BOTON_BAJAR, OUTPUT);
  pinMode(BOTON_PAUSA, OUTPUT);
  
  Serial.begin(9600);
}

void loop()
{
  //Se ejecuta la funcion secuencia y se muestra el piso en el que estamos
  piso = secuencia();
  displayNumero(piso); 
    
  Serial.print("Estas en el piso: ");
  Serial.println(piso);
    
  delay(PAUSA);
}
~~~

# Funcion displayNumero.
#### Esta funcion recibe un numero del 0-9 y muestra ese numero en un display de 7 segmentos.
~~~ C
void displayNumero(int piso){
	/*Recibe un entero(piso) y en los casos del 0-9 mostrara ese
    numero en un display de 7 segmentos, no retorna nada*/
  switch (piso){
  case 0:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, HIGH);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, LOW);
    
    break;
  case 1:
    
    digitalWrite(SEGMENTOF, LOW);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, LOW);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, LOW);
    digitalWrite(SEGMENTOG, LOW);
    
    break;
  case 2:
    
    digitalWrite(SEGMENTOF, LOW);
    digitalWrite(SEGMENTOE, HIGH);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, LOW);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
	
    break;
  case 3:
    
    digitalWrite(SEGMENTOF, LOW);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
  case 4:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, LOW);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, LOW);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
  case 5:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, LOW);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
  case 6:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, HIGH);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, LOW);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
   case 7:
    
    digitalWrite(SEGMENTOF, LOW);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, LOW);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, LOW);
    
    break;
   case 8:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, HIGH);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
   case 9:
    
    digitalWrite(SEGMENTOF, HIGH);
    digitalWrite(SEGMENTOE, LOW);
    digitalWrite(SEGMENTOD, HIGH);
    digitalWrite(SEGMENTOC, HIGH);
    digitalWrite(SEGMENTOB, HIGH);
    digitalWrite(SEGMENTOA, HIGH);
    digitalWrite(SEGMENTOG, HIGH);
    
    break;
  }
}
~~~
# Funcion detectarBoton.
#### Retorna 0 si no se detecta una pulsacion de boton en el pin ingresado o 1 si detecta una pulsacion en ese pin.
~~~ C 
int detectarBoton(int pin){
	int retorno = 0;
      
    if(digitalRead(pin) == 1){
     retorno = 1;
   
    }
	return retorno;
}
~~~
# Funcion ledMovimiento.
#### Recibe un parametro de tipo entero (0/1) y en cada caso prende el led rojo y apaga el verde o viceversa.
~~~C void led_movimiento(int tipo){

  if (tipo == 0){
    
  	digitalWrite(LEDROJO,LOW);
    digitalWrite(LEDVERDE,HIGH); 
  }else{
  
  	digitalWrite(LEDROJO,HIGH);
    digitalWrite(LEDVERDE,LOW);
  }
}
~~~



# Funcion secuencia.
#### Esta es la funcion principal del programa y se encarga de detectar que boton es pulsado subir, bajar de piso o mantenerse en el que ya esta.
~~~ C 
int secuencia(){
  	//En la primera iteracion se muetra el piso 0 y se prende el led que indica no movimiento
  if (flag == 0){
    
  	displayNumero(piso);
    led_movimiento(1);
    
    
    flag = 1; 
  }else{
    //secuencia pausa
   if (detectarBoton(BOTON_PAUSA) == 1){

      bajar = 0;
      subir = 0;
      pausa = 1;
      led_movimiento(1);
    }
    //Secuencia subir piso
    if (detectarBoton(BOTON_SUBIR) == 1 || subir == 1){

      subir = 1;
      bajar = 0;
      pausa = 0;

      if (piso < 9){
        piso++;
        led_movimiento(0);

      }else{
        led_movimiento(1);;
      }    

    }
    //Secuencia bajar piso
    if(detectarBoton(BOTON_BAJAR) == 1 || bajar == 1){

      bajar = 1;
      subir = 0;
      pausa = 0;

       if (piso > 0){
        piso--;
        led_movimiento(0);

      }else{
        led_movimiento(1);

      }
    }
  }
  return piso;
}
~~~
## Link
### -[ThinkerCad](https://www.tinkercad.com/things/0LaWNvsofWZ-parcial-1/editel?sharecode=V8c4sTnYOSaBqiiUxvruvRA-3f1B6Zt3BWbGSOSjx8M)
