# Saigon

#include<Servo.h>
#include <string.h>
Servo microServo;
int servoPin = 10;


//Функция считывающая имена
String commandName(String y)
{
  String letters = "";
  int spacePosition = -1;
  for (int count = 0; count < y.length(); count++)
  {
    if (y[count] == ' ')
    {
      spacePosition = count;
    }
  }

  if (spacePosition >= 0) {
    for (int l = 0; l < spacePosition; l++) {
      letters += y[l];
    }
  }

  return letters;
}

//Функция считывающая числа
String commandNumbers(String y)
{
  String numbers = "";
  int somePosition = -1;
  for (int q = 0; q < y.length(); q++)
  {
    if (y[q] == ' ')
    {
      somePosition = q;
    }
  }

  if (somePosition >= 0) {
    for (int l = somePosition ; l < y.length(); l++) {
      numbers += y[l];
    }
  }

  return numbers;
}


void setup() {
  Serial.begin(9600);
  microServo.attach(servoPin);
}
String lettersEnd = "";
String command = "";


void loop()
{

  if (Serial.available() > 0)
  {
    if (command.length() == 0) {
      char tempSimbol = Serial.read();
      if (tempSimbol == '/') {
        command += tempSimbol;
      };
    }
    else
    {
      char tempSimbol = Serial.read();
      if (tempSimbol != ';') {

        command += tempSimbol;
      }
      else {
        lettersEnd = command;
        command = "";
      }
    }
  }


  
  String lettersOrig = commandName(lettersEnd);
  Serial.println(lettersOrig);
  
  String numberOrig = commandNumbers(lettersEnd);
  Serial.println(numberOrig);


}
