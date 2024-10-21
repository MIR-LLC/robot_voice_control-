# Инструкция по использованию AI-модуля голосового управления роботом.
## Содержание
- Описание AI-модуля
	- Интерфейсы
	- Bluetooth
	- Подключение питания
	- Размеры платы
	- Список голосовых команд пользователя
	- Голосовые сообщения модуля
	- Значения цвета светодиода
- Пример сборки робота
	- Сборка корпуса робота
	- Сборка логической части робота
	- Написание скетча
	- Итоговый вид робота

 




<!---## Содержание
1. [Сборка корпуса робота](#1-Сборка-корпуса-робота)
   - [Подготовка инструментов и деталей](#11-Подготовка-инструментов-и-деталей)
   - [Установка моторчиков](#12-Установка-моторчиков)
   - [Сборка колёс](#13-Сборка-колёс)
   - [Установка верхней части](#14-Установка-верхней-части)
2. [Написание скетча](#2-Написание-скетча)
   - [Подготовка к написанию кода](#21-Подготовка-к-написанию-кода)
   - [Написаниие кода](#22-Написаниие-кода)
   - [Итоговый код](#23-Итоговый-код)
   - [Перенос кода на Arduino](#24-Перенос-кода-на-Arduino)
3. [Сборка логической части робота](#3-Сборка-логической-части-робота)
   - [Подготовка инструментов и деталей](#31-Подготовка-инструментов-и-деталей)
   - [Подключение моторчиков](#32-Подключение-моторчиков)
   - [Подключение источника питания к L298N](#33-Подключение-источника-питания-к-L298N)
   - [Подключение голсового модуля к Arduino](#34-Подключение-голсового-модуля-к-Arduino)
   - [Подключение логической части к L298N](#35-Подключение-логической-части-к-L298N)
   - [Итоговый вид](#36-Итоговый-вид)
-->
## Описание AI-модуля

**Краткое описание работы:** находясь в активном режиме, модуль слушает голосовые команды пользователя. 

Если модуль распознал команду из списка, он повторяет её через внешний динамик и отправляет сигнал для управления роботом.

Модуль представляет собой плату с микроконтроллером, который распознаёт голосовые команды пользователя с помощью нейросети и отправляет их роботу.

<img src="https://github.com/user-attachments/assets/51bf3f42-8d42-47d4-8e31-3881663815e5" width=40%>

## Интерфейсы (как подключить к роботу)

<img src="https://github.com/user-attachments/assets/a147909a-35ad-4ec9-9828-fa2e412806ff" width=600px>


Вы можете воспользоваться любым удобным для вас интерфейсом, из перечисленных, на свой выбор, исходя из любых предпочтений (например, из опыта работы с ними). 


Ниже также размещены схемы подключения и пример скетча для подключения с помощью I2C и Bluetooth, можете взять их за основу вашего собственного проекта. 

### UART

Отправка данных в лог через USB-разъём (Type-C), а также на контакты 9 и 10 (см. рис. выше) 

Передаётся длина в байтах и само слово русскими символами через пробел.


### SPI

Используются контакты (см. рис. выше)  

5 - MOSI,  

6 - MISO,  

7 - SCLK,  

8 - CS 

 

Передаётся длина в байтах и само слово русскими символами двумя транзакциями (длина - цифра, слово - массив). 

### I2C

Используются контаткы (см. рис. выше) 

 

1 - SCL 

2 - SDA 
 

Длина в байтах и само слово русскими символами двумя транзакциями (длина - цифра, слово - массив). 

![image](https://github.com/user-attachments/assets/7d541578-d1e5-442f-9ab7-7ddbb7f815c7)


## Bluetooth

В этом случае модуль не нужно устанавливать непосредственно на вашего робота. Используйте его как выносной пульт.

Пользователь говорит команды в модуль, который держит в руке, модуль распознаёт их и отправляет по Bluetooth на робота.
В этом случае подключите блютус-модуль (например HM-10) к вашей плате. 

![блютус](https://github.com/user-attachments/assets/40718588-0ccd-4638-8c2e-817548e858f1)

Для увеличения дальности связи приобретите и используйте антенну с IPX-разъемом.

## Подключение питания

USB Type-C, 5 В, рекомендуем от 500 мА.

Либо используйте контакты 3 - VCC, 5В; 4 - земля для подачи питания, например, от платы Ардуино.

Средняя потребляемая модулем мощность - 30 mA.

## Размеры платы

<img src="https://github.com/user-attachments/assets/1705a399-5096-4f3d-a426-3e1b505ec82e" width=400px>


## Список голосовых команд пользователя.

|     Голосовая команда        |       Код команды на выходе модуля      |       Значение команды                   |
|------------------------------|-----------------------------------------|------------------------------------------|
|     Робот                    |     «10 робот»                          |     Вывод модуля из спящего   режима.    |
|     Спать                    |     «10 спать»                          |     Перевод в спящий режим.              |
|     Старт                    |     «10 старт»                          |     Начать   движение.                   |
|     Стоп                     |     «8 стоп»                            |     Прекратить движение.                 |
|     Вправо                   |     «12 вправо»                         |     Повернуть   направо.                 |
|     Влево                    |     «10 влево»                          |     Повернуть налево.                    |
|     Вперёд                   |     «12 вперед»                         |     Двигаться   вперёд.                  |
|     Назад                    |     «10 назад»                          |     Двигаться назад.                     |
|     Домой                    |     «10 домой»                          |     Вернуться на   исходную позицию.     |
|     Медленно                 |     «16 медленно»                       |     Двигаться медленно.                  |
|     Быстро                   |     «12 быстро»                         |     Двигаться быстро.                    |

## Голосовые сообщения модуля.

|     Сообщение    |     ЗНачение сообщения        |
|------------------------------------------------------------------------------|---------------------------------------------|
|     Привет!   Это нейросеть голосового управления роботом. Скажи команду.    |     Приветствие   при включении.            |
|     Не понял.                                                                |     При неудачном распознавании команды.    |
|     Уснул.                                                                   |     При переходе в спящий режим.            |
|     Слушаю.                                                                  |     При выходе из спящего режима.           |


## Значения цвета светодиода.

| Цвет           | Значение                               |
|----------------|----------------------------------------|
|     Синий      |     Микрофон модуля   ждёт команды.    |
|     Жёлтый     |     Модуль выполняет распознавание.    |
|     Красный    |     Модуль не   распознал команду.     |
|     Голубой    |     Модуль в спящем режиме.            |



## Пример сборки робота на четырехколёсной платформе.

   ![robot](https://github.com/user-attachments/assets/131145bb-d22b-4f07-9d8d-638a319daf06)

### Сборка корпуса робота

Мы использовали четырехколесную платформу с обычными щеточными двигателями, управляемыми через драйвер двигателей L298N. 
Вы можете использовать AI-модуль для управления любыми устройствами на Ардуино - принцип работы и подключения не меняется. 

#### Подготовка инструментов и деталей
   Набор корпуса включает в себя следующий список деталей:
  
   - 2 платформы корпуса
   - 4 моторчика
   - 4 колеса
   - 4 диска-енкодера
   - 8 крепежей для моторчиков
   - 12 обычных винтов
   - 8 длинных винтов
   - 8 гаек
   - 6 стоек

   ![details](https://github.com/user-attachments/assets/50ad3057-91b9-44c3-9f6f-28900af01800)

   Из инструментов нам будет достаточно крестовой отвёртки и гаечного ключа соответствующих размеров.
  
#### Установка моторчиков
   Детали для даннного этапа:
  
   - 1 платформа
   - 4 моторчика
   - 8 крпежей для моторчика
   - 8 длинных винтов
   - 8 гаек

   В первую очередь нам нужно взять платформу и найти следующие отверстия и вырезы для крепежей моторчиков.
    
   ![platform](https://github.com/user-attachments/assets/85c5d3f9-1b28-4cf6-a5e2-b9a4ea9a06ee)

   Далее мы берём один из крепежей и вставляем широкой частью вниз в указанное на рисунке выше отверстие платформы.
   Подставляем к вставленному крепежу моторчик и берём второй крепёж, который сопоставляем с указанным выше вырезом.
   Сопоставляем отверстия на крепежах и моторе и затем вставляем в них длинные винты. Закрепляем винты гайками при помощи гаечного ключа.
  
   ![bolts](https://github.com/user-attachments/assets/c7244cb8-2405-4f5d-b270-91391fe1168d)

   Установка одного моторчика закончена, теперь нужно закрепить остальные три моторчика аналогичным способом.
  
#### Сборка колёс
   
   Теперь мы установим сами колёса. Сопоставим отверстия на самих колёсах и на моторчиках.

   ![wheels](https://github.com/user-attachments/assets/c496d7fe-ba5c-4770-8601-31a83c646256)

#### Установка верхней части
   Последним этапом сборки является установка верхней платформы. Для её установки нам понадобится:

   - 6 стоек
   - 12 обычных винтов
   - 1 платформа корпуса

   Стойки нужно закрепить винтами при помощи крестовой отвёртки в следующие отверстия на платформе с моторчиками:

   ![stands_platform](https://github.com/user-attachments/assets/be3a76db-a316-44e6-8214-151867520bb4)

   Стойки, прикрученные к платформе с моторчиками:
   
   ![stands](https://github.com/user-attachments/assets/15064e04-da6d-473e-96ca-eddd799e834c)

   Теперь остаётся взять верхнюю платформу, сопоставить её с отверстиями для стоек и закрепить винтами.

   ![top_platform](https://github.com/user-attachments/assets/bfb9040e-58df-443f-a081-fe93f7316685)

### Сборка логической части робота
#### Подготовка инструментов и деталей
   Логическая часть робота состоит из:

   - Плата Arduino UNO
   - Плата управления моторами L298N
   - AI-модуль голсового управления
   - Источник питания (9В батарейка или аккумулятор)
   - Модуль bluetooth управления HM-10 (если делаете управление через Bluetooth)
   - Провода

Из инструментов нам будет достаточно крестовой отвёртки соответствующего размера.

#### Подключение моторчиков
   В первую очередь подключаем моторчики к плате L298N следующим образом:
   - Подключаем моторчики с правой стороны к выходам L298N OUT1 и OUT2
   - Подключаем моторчики с левой стороны к выходам L298N OUT3 и OUT4

   ![Motors](https://github.com/user-attachments/assets/6e702409-39c7-4e78-8de5-9bc0e80573f2)

#### Подключение источника питания к L298N
   Теперь мы подключим источник питания к драйверу.
   Для этого на плате есть клемы 12V и GND. Подключаем плюс к 12V и минус к GND.
   Оставшуюся клему 5V мы оставим для подключения питания Arduino и голосового модуля.

   ![Power](https://github.com/user-attachments/assets/83059d02-cd3a-4de2-a9ce-6eb517b76672)

  > [!WARNING]
  > При подключении к драйверу источник питания должен быть выключен!

#### Подключение голсового модуля к Arduino (на примере I2C)
   Перед тем как подключить логическую часть к питанию, мы должны соединить голосовой модуль с Arduino для передачи команд.
   
   ![AI-Arduino](https://github.com/user-attachments/assets/32d397e0-d0d7-4eb1-a7b4-809e232b7213)


#### Подключение логической части к L298N
   Теперь можно подключить Arduino и голосовой модуль к драйверу.
   Помимо питания нам нужно будет подключить входы, которые позволят передавать команды на моторчики.
   На драйвере эти ходы обозначены как: IN1, IN2, IN3 и IN4.

   ![Arduino-L298N](https://github.com/user-attachments/assets/b3e9b5db-942b-4141-98c5-8d3e379b5a6b)

#### Подключение bluetooth-модуля HM-10 к Arduino

Если вы решили использовать модлуль распознавания не на работе, а в качестве выносного беспроводного пульта управления, вам нужно подключить bluetooth к плате Ардуино. В этом случае подключать модуль к роботу, как описано выше, не нужно. 
Сам модуль уже оснащён всем необходимым для беспроводной отправки команд.

![блютус](https://github.com/user-attachments/assets/b3cd8bcc-c1b1-46f0-87d0-1f154d15e38d)




   ### Написание скетча
#### Подготовка к написанию кода
   Для того чтобы начать написание кода для Arduino, нам нужно установить драйвер для платы, а также специальную среду разработки Arduino IDE с официального сайта производителя.
  
   [Ссылка на драйвер Arduino UNO](https://iarduino.ru/file/126.html)
  
   [Ссылка на Arduino IDE](https://www.arduino.cc/en/software)
  
#### Написаниие кода
   Теперь, когда мы установили всё нужное программное обеспечение для написания кода, мы открываем Arduino IDE и нажимаем на кнопку создания нового скетча.
   
   Сначала нам стоит определиться, каким образом Arduino будет принимать информацию с голосового модуля.
   Есть три способа:
  
   - UART
   - I2C
   - SPI

   Возьмем для примера подключение через I2C, так как передача данных по нему быстрее, чем на UART и проще, чем на SPI. 
   Чтобы Arduino смогла читать данные через I2C, нужно его подключить, а также прописать порты, через которые Arduino будет передавать сигналы моторчикам.
Пример скетча с комментариями для проводного подключения с использованием I2C здесь: 	
   
   Также возможно взаимодействие с Bluetooth-модулем, если вы захотите использовать этот вариант, то также нужно будет объявить переменные для выводов платы, к которым подключен драйвер управления двигателями, инциилизровать обмен данными с блютус-модулем. Для работы потребуется указать мак-адрес модуля. Посмотреть его можно с помощью смартфона, например, с использованием BLE-Scanner. Далее разберем скетч для подключения по Bluetooth.
   .

   ```
#include <SoftwareSerial.h>

const int in1 = 7;             // Пин движения вперёд для колёс с правой стороны
const int in2 = 6;             // Пин движения назад для колёс с правой стороны (+ШИМ)
const int in3 = 5;             // Пин движения вперёд для колёс с левой стороны (+ШИМ)
const int in4 = 4;             // Пин движения назад для колёс с левой стороны
const int ble_rx = 3;          // Bluetooth module RX pin
const int ble_tx = 2;          // Bluetooth module TD pin
const String mac_adress = "
const int motion_delay = 550;  // Engine running time

SoftwareSerial mySerial(ble_tx, ble_rx);

void setup() {
  // Настройка UART
  Serial.begin(9600);  // Настраиваем Serial для вывода логов
  mySerial.begin(9600);

  // Инициализируем пины для управления двигателями как выходы
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
}
   ```

  Теперь мы должны прописать процесс считывания информации и распознавания команд.

   ```
void loop() {
  if (mySerial.available()) {
    String command = "";
    command = mySerial.readString();
    Serial.println("DATA RECEIVED:");
    Serial.println(command);

    if (command.indexOf("вперед") != -1) {
      moveForward();
    } else if (command.indexOf("назад") != -1) {
      moveBack();
    } else if (command.indexOf("вправо") != -1) {
      moveRight();
    } else if (command.indexOf("влево") != -1) {
      moveLeft();
    }
  }
}
   ```

  Остаётся прописать сами команды, а именно какие моторы и в каком направлении нам нужно активировать при той или иной команде.
  В данном скетче мы пропишем самые основные команды: вперёд, назад, влево, вправо и стоп.
  
   ```
   // Движение вперед
void moveForward() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
  delay(motion_delay);
  stopMotors();
}

// Движение назад
void moveBack() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  delay(motion_delay);
  stopMotors();
}

// Поворот вправо
void moveRight() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  delay(motion_delay);
  stopMotors();
}

// Поворот влево
void moveLeft() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
  delay(motion_delay);
  stopMotors();
}

// Остановка двигателей
void stopMotors() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}
   ```

   >[!TIP]
   > Помимо стандартных команд, которые прописаны в скетче, в голосовой модуль загружено распознавание таких слов, как: "домой", "быстро", "медленно" и "спать".
   >
   >Первые три команды требуют написания отдельного кода, как и для основных команд, а команда "спать" уже работает и позволяет выключить на время функцию распознования слов на голосовои модуле, также её можно использовать и для отправки в сон основного робота (в примере делать этого не будем).
  
#### Итоговый код
   В результате мы получили скетч, который обрабатывает запросы с голосового модуля через I2C и подаёт сигналы действий на моторчики.

   ```
#include <SoftwareSerial.h>

const int in1 = 7;             // Пин движения вперёд для колёс с правой стороны
const int in2 = 6;             // Пин движения назад для колёс с правой стороны (+ШИМ)
const int in3 = 5;             // Пин движения вперёд для колёс с левой стороны (+ШИМ)
const int in4 = 4;             // Пин движения назад для колёс с левой стороны
const int ble_rx = 3;          // Bluetooth module RX pin
const int ble_tx = 2;          // Bluetooth module TD pin
const int motion_delay = 550;  // Engine running time

SoftwareSerial mySerial(ble_tx, ble_rx);

void setup() {
  // Настройка UART
  Serial.begin(9600);  // Настраиваем Serial для вывода логов
  mySerial.begin(9600);

  // Инициализируем пины для управления двигателями как выходы
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
}

void loop() {
  if (mySerial.available()) {
    String command = "";
    command = mySerial.readString();
    Serial.println("DATA RECEIVED:");
    Serial.println(command);

    if (command.indexOf("forward") != -1) {
      moveForward();
    } else if (command.indexOf("backward") != -1) {
      moveBack();
    } else if (command.indexOf("right") != -1) {
      moveRight();
    } else if (command.indexOf("left") != -1) {
      moveLeft();
    }
  }
}

// Движение вперед
void moveForward() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
  delay(motion_delay);
  stopMotors();
}

// Движение назад
void moveBack() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  delay(motion_delay);
  stopMotors();
}

// Поворот вправо
void moveRight() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  delay(motion_delay);
  stopMotors();
}

// Поворот влево
void moveLeft() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
  delay(motion_delay);
  stopMotors();
}

// Остановка двигателей
void stopMotors() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}
   ```
  
####  Перенос кода на плату Arduino
   После написания всей программы, мы можем проверить скетч на наличие ошибок с помощью встроенной системы проверки Arduino IDE.
   Для этого нужно нажать на кнопку со значком "галочка" в верхнем левом углу. Если ошибок нет, то на вкладке "Консоль" будет указано "Готово", и можно переходить к следующему шагу — переносу кода на Arduino.
   
   Для переноса кода на Arduino необходимо подключить плату к компьютеру с помощью USB-кабеля.
   После этого нужно нажать на кнопку "Upload" с изображением стрелки направленной вправо (находится рядом с кнопкой проверки кода).
   В результате этого программа загрузится на плату, и можно будет перейти к подключению логической части.

   > [!WARNING]
   > Убедитесь в том, что вы загружайте скетч на порт COM3. В противном случаем, при загрузке программа выдаст ошибку.
   > Проверить и изменить порт можно в меню рядом с кнопкой Upload в верхнем углу Arduino IDE.

В результате вы получаете робота, который выполняет ваши голосовые команды. 
Не бойтесь экспериментировать с изменением скетча - скорость движения, угол поворота и так далее - зависят от многих факторов: насколько заряжены батареи, на каком покрытии перемещается робот. 

### Итоговый вид робота

#### С установкой модуля непосредственно на роботе
![image](https://github.com/user-attachments/assets/d5d7b11c-3bad-4e0b-81d1-a45e0713f09d)

#### С использованием модуля в качестве пульта управления

![image](https://github.com/user-attachments/assets/5c8a6e8e-fb8a-434e-be4a-b58c6f4843cf)




