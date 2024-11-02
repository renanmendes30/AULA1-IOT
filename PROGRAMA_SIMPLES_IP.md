```
#include <ESP8266WiFi.h> // biblioteca para ESP8266
const char* ssid = "LAB252"; //Substitua com o nome da sua rede WiFi
const char* senha = "alunos2022"; //substitua com a senha da sua rede WiFi 

void setup() {
Serial.begin(115200);//Velocidade da serial
pinMode(D1,OUTPUT); // configura pino 1 como saída
/* Aparece na serial a msg com nome da rede*/

Serial.print("Conectando com a rede ");
Serial.println(ssid);
WiFi.begin(ssid, senha); // Conexão na rede com senha

/* Enquanto a conexão não for realizada, faz*/
while(WiFi.status() != WL_CONNECTED){
delay(500); //aguarda 500ms
digitalWrite(D1, LOW);// apaga D1
Serial.print("."); // escreve pontos na tela
}

/*Quando a conexão foi realizada, faz*/
Serial.println("Parabéns, você está conectado!");
digitalWrite(D1, HIGH); // acende D1
Serial.println("Endereco IP: ");
Serial.println(WiFi.localIP()); // mostra o IP da placa
}


void loop() {
// Sem comandos
}
```
