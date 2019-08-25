# Mini_Arcondicionado
Desenvolvimento de um ar condicionado, utilizando programação C (Arduino)
#include <LiquidCrystal.h> 

#include <Limits.h> 
 

// Pino analógico em que o sensor de temperatura está conectado 

const int sensorTemp = 0; 

int AR = 13; // Declara o pino digital 13 para acionar o cooler e pastilha peltier 

// Declara o pino analógico 0 para ler o valor do sensor de temperatura 

int tempPin = 0; 

int valorSensorTemp = 0; // Variável usada para ler o valor de temperatura 

// Variável usada para armazenar o menor valor de temperatura 

int valorTemp = INT_MAX; 


//Define os pinos para lcd operando em 4 Bits 

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() { 

 

  pinMode (AR, OUTPUT); // Define o pino 13 como saída 

  lcd.begin(16, 2); // Define o display com 16 colunas e 2 linhas 

} 


void loop() { 

  /*Para evitar as grandes variações de leitura do componente 

  LM35 são feitas 6 leitura é o menor valor lido prevalece*/ 


  // Inicializando a variável com o maior valor int possível 

  valorTemp  = INT_MAX; 

  for (int i = 1; i <= 6; i++) { // Lendo o valor do sensor de temperatura 

 
    valorSensorTemp = analogRead(sensorTemp); 


    // Transforma o valor lido do sensor de temperatura em graus... 

    //Celsius aproximados 

    valorSensorTemp *= 0.54 ; 


    // Mantendo sempre a menor temperatura lida 

    if (valorSensorTemp < valorTemp) { 

      valorTemp = valorSensorTemp; 

    } 

    delay(100); // Aguarda um milesimo de segundo 

  } 

  if (valorTemp > 25) // Indica condição para acionamento do cooler e pastilha de peltier 

  { 

    // Define a coluna 0 e linha 1 do LCD onde será impresso a string 

    lcd.setCursor(0, 1); 

    lcd.write("AR LIGADO !!!"); // Imprime no LCD 

    digitalWrite(AR, HIGH); // Quando condição verdadeira, liga o cooler e pastilha de peltier 

  } 

  else 

    if (valorTemp <= 22) { 

      lcd.setCursor(0, 1); 

      lcd.write("AR DESLIGADO !!!"); 

      // Desliga cooler e pastilha de peltier quando este estiver a baixo da valorTemp, indicando... 

      //no LCD que esta desligado 

      digitalWrite(AR, LOW); 

    } 

  delay(500); // Aguarda meio segundo 


  // Exibindo valor da leitura do sensor de temperatura no display LCD 

  lcd.clear();  // Limpa o display do LCD 

  lcd.print("Temperatura:");  // Imprime a string no display do LCD 

  lcd.print(valorTemp); 

  lcd.write(B11011111); // Símbolo de graus Celsius 

  lcd.print("C"); 
  

  delay(1000); // Aguarda um segundo 

} 
