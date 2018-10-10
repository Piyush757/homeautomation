# homeautomation
Controlling light and fans using NodeMCU ESP8266 through web browser


code
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

ESP8266WebServer server;
uint8_t pin_led = 16;
char* ssid = "Homeo";  //give the username of the wifi with which the nodemcu will be connected
char* password = "Amarakash";  //Password of the internet access point

void setup(void) {
  // put your setup code here, to run once:
  pinMode(pin_led, OUTPUT);
  WiFi.begin(ssid,password);
  Serial.begin(115200);   //Baud width
  while(WiFi.status() != WL_CONNECTED)
  {
    Serial.print(".");
    delay(500);
  }
  Serial.println("");
  Serial.print("Connected to ");
  Serial.println(ssid);
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  server.on("/",[](){server.send(200,"text/plain","Hello");});
  server.on("/toogle",toggleLED);
  server.begin();
  Serial.println("Web server started!");
}

void loop() {
  // put your main code here, to run repeatedly:
  server.handleClient();
}


void toggleLED()
{
  digitalWrite(pin_led,!digitalRead(pin_led));
  server.send(204,"");
  
}

