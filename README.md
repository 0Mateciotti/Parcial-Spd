## Alumno 
- Mateo Ciotti

## Proyecto
<img src="https://github.com/0Mateciotti/Parcial-Spd/blob/main/Parcial/Fotos/arduino.PNG?raw=true" width="800"/>
<img src="https://github.com/0Mateciotti/Parcial-Spd/blob/main/Parcial/Fotos/esqueletico.PNG?raw=true" width="800"/>

# Defines y declaraciones
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
# Set up y loop
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
  /*Empieza el programa con la flag = 0 asi muestra el 0 al inicio*/
  if (flag == 0){
  
  displayNumero(piso);
  digitalWrite(LEDROJO,HIGH);
  digitalWrite(LEDVERDE,LOW);
    
  Serial.print("Estas en el piso: ");
  Serial.println(piso);
    
  flag = 1;  
  delay(PAUSA);
    
  }else{
    
  //Apartir de la segunda iteracion empieza esta parte que ya opera con los botones
  piso = secuencia();
 
  displayNumero(piso); 
    
  Serial.print("Estas en el piso: ");
  Serial.println(piso);
    
  delay(PAUSA);
  }
}
~~~

# Funcion displayNumero
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
# Funcion detectarBoton
~~~ C 
int detectarBoton(int pin){
	int retorno = 0;
      
    if(digitalRead(pin) == 1){
     retorno = 1;
   
    }
	return retorno;
}
~~~
# Funcion secuencia
~~~ C 
int secuencia(){
  //secuencia pausa
 if (detectarBoton(BOTON_PAUSA) == 1){
   
    bajar = 0;
    subir = 0;
    pausa = 1;
  }
  //Secuencia subir piso
  if (detectarBoton(BOTON_SUBIR) == 1 || subir == 1){
    
  	subir = 1;
    bajar = 0;
    pausa = 0;
    
    if (piso < 9){
      piso++;
      digitalWrite(LEDROJO,LOW);
      digitalWrite(LEDVERDE,HIGH);
      
    }else{
      digitalWrite(LEDROJO,HIGH);
      digitalWrite(LEDVERDE,LOW);
    }    
    
  }
  //Secuencia bajar piso
  if(detectarBoton(BOTON_BAJAR) == 1 || bajar == 1){
  
  	bajar = 1;
    subir = 0;
    pausa = 0;
    
     if (piso > 0){
      piso--;
      digitalWrite(LEDROJO,LOW);
      digitalWrite(LEDVERDE,HIGH);
       
    }else{
      digitalWrite(LEDROJO,HIGH);
      digitalWrite(LEDVERDE,LOW);
       
    }
  }
  
  return piso;
}
~~~
## Link
-[ThinkerCad](https://www.tinkercad.com/things/0LaWNvsofWZ-parcial-1/editel?sharecode=V8c4sTnYOSaBqiiUxvruvRA-3f1B6Zt3BWbGSOSjx8M)
