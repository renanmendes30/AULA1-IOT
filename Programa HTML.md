```
#include <ESP8266WiFi.h>
 
 
//-- Define o pino D1 como pino do Led--
#define ledPin D4


 
//--- Definições de rede 
const char* ssid = "LAB252";
const char* password = "alunos2022"; 
WiFiServer server(80);// --- Escutar porta 80
 
 
 
// --- Setup
void setup() { 
  Serial.begin(115200);
delay(10);
 

pinMode(ledPin, OUTPUT);
digitalWrite(ledPin, LOW); //Apaga Led
 
Serial.println();
Serial.println();
Serial.print("Conectando a rede: ");
Serial.println(ssid);
WiFi.begin(ssid, password);
 
 
 
//Verifica e aguarda conexão Printa ip atribuido ao Nodemcu
while (WiFi.status() != WL_CONNECTED) {
delay(500); 
Serial.print(".");
Serial.println("");
Serial.println("WiFi conectado"); server.begin();
Serial.println("Web server started");
Serial.print("Utilize a seguinte url "); Serial.print("http://");
Serial.print(WiFi.localIP()); Serial.println("/");
}
}
 
 
void loop() { 
  Serial.println(WiFi.localIP()); // Verifica conexão com servidor. 
  WiFiClient client = server.available();
if (!client) return;
 
 
 
// Aguardar envio de dados 
Serial.println("Novo cliente...");
while (!client.available()) delay(1);
 
 
// Realiza leitura da primeira linha da request 
String request = client.readStringUntil('\r'); 
Serial.println(request); 
client.flush();
int value = LOW;
 
 
 
//Verifica request e atualiza valor led
if (request.indexOf("/LED=LIGADO") != -1) {
digitalWrite(ledPin, LOW);
value = LOW;
}
if (request.indexOf("/LED=DESLIGADO") != -1) { 
  digitalWrite(ledPin, HIGH);
  value = HIGH;
}
 
 
 
 
//Retorna resposta 
client.println("HTTP/1.1 200 OK");
client.println("Content-Type: text/html");
client.println("");
client.println("<!DOCTYPE HTML>"); 
client.println("<html>");
client.println("<h1> Webserver com NodeMCU </h1>"); 
client.print("O led esta: ");
if (value)   client.print(F("LED LIGADO"));
  else   client.print(F("LED DESLIGADO")); 
client.println("<br><br>");
client.println("<a href=\"/LED=LIGADO\"\"><button> LIGAR </button></a>"); 
client.println("<a href=\"/LED=DESLIGADO\"\"><button> DESLIGAR </button></a><br />");
 
delay(1); 
Serial.println("Cliente desconectado");
Serial.println("");
}
```
