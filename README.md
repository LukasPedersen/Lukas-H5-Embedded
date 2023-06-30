# HeatWatcherLite
## Introduktion

Dette projekt kombinerer InfluxDB, en populær Blobstorage, med Blazor Server, et frameworksområde der tilbyder .NET-applikationer til at køre på serveren. Ved at kombinere disse teknologier kan du oprette en webapplikation, der kan gemme og præsentere telemetri data fra forskellige kilder.

## Krav

Før du begynder at arbejde med dette projekt, skal du sikre dig, at du har følgende krav opfyldt:

1. En Raspberry Pi-enhed (helst en Pi 3 eller nyere) kørende med et understøttet operativsystem (f.eks. RaspBerry PI OS eller RaspBerry PI OS Light).
2. .NET SDK installeret på din Raspberry Pi. Du kan finde instruktioner til installation af .NET på Raspberry Pi på Microsofts officielle dokumentationsside.
3. InfluxDB installeret og konfigureret på din Raspberry Pi. Du kan følge InfluxDB's dokumentation for at få hjælp til installation og konfiguration.
4. Grundlæggende kendskab til C# og webudvikling.

## Opsætning

Følg disse trin for at sætte dit projekt op:  
Sørg for at docker er installeret og køre. ellers er der en guide her du kan følge 

#### Installer docker
```Bash
sudo apt install docker docker.io docker-compose  
```
#### Start docker
```Bash
sudo systemctl start docker
```
#### Giv din bruger adgang til docker
```Bash
sudo usermod -a -G docker <username>
```
#### Genstart
```Bash
sudo reboot
```
Hvis du tror du har docker installeret kan du bekræfte ved at køre:
#### Test om Docker er installeret
```Bash
docker ps
```
Hvis terminalen skriver følgende, er Docker installeret:

```CONTAINER ID     IMAGE   COMMAND   CREATED    STATUS   PORTS   NAMES```
## Installation af projekt
1. Klon projektets repository fra GitHub til din Raspberry Pi eller pc.
1.1 Ved pc skal der bruges andet software (f.eks. WinSCP) til at flytte projektet over til din Raspberry Pi
    Hurtig installation af WinSCP i Windows Terminal:
   ```Bash
    winget install -e --id WinSCP.WinSCP.RC 
    ```
2. Opret forbindelse til din Raspberry Pi.
Følg de nedenstående guides til de forskellige dele af projektet.

4. Åbn projektet på din Raspberry Pi i din foretrukne IDE (f.eks. Visual Studio Code).
5. Konfigurer InfluxDB-forbindelsesoplysningerne i projektets konfigurationsfiler.
6. Kompilér og kør Blazor Server-applikationen ved hjælp af .NET SDK'et.
7. Åbn din webbrowser og navigér til den lokale IP-adresse eller værtsnavn for din Raspberry Pi for at få adgang til den kørende applikation.

## InfluxDB API

Hvis InfluxDB ikke er installeret kan det gøres med kommandoen:
```Bash  
docker run -d -p 8086:8086 -v influxdb:/var/lib/influxdb -v influxdb2:/var/lib/influxdb2 influxdb:2.0
```
1. Opret forbindelse til din InfluxDB via en browser på ```<HostIP or Host Name or localhost:8086>```  
2. Opret et login  
3. Opret en ny Organisation  
4. Opret en ny Bucket
5. Opret en ny Token

Så er InfluxDB klar til brug

## API

Nu da du har sat din InfluxDb op kan du få sat din API op og se om du kan få hul igennem  
Da vi allerede har fået overført API og Blazor Server til vores Raspberry Pi skal vi bare have kørt den op  

1. Lav et Docker Image med kommandoen:  
```Bash
docker build -t api:latest -f WebApiMqtt/Dockerfile .
```  
2. Lan en Docker Container med kommandoen:  
```Bash
docker run -p 32768:80 -d api
```
3. Giv den nogle min før du prøver at forbinde men det kan gøres på:
```<IP or hostname or localhost:32768/swagger/index.html>```
4. Test via swagger om der er forbindelse til InfluxDb ved at prøve nogle af de endpoints der er.
   
## Blazor Server  
Nu da du har sat din API op kan du få sat din Blazor Server op og se om du kan få hul igennem  
Da vi allerede har fået overført API og Blazor Server til vores Raspberry Pi skal vi bare have kørt den op 
1. Lav et Docker Image med kommandoen:  
```Bash  
docker build -t blazor-server:latest -f BlazorWeb/Dockerfile .
```  
2. Lan en Docker Container med kommandoen:  
```Bash  
docker run -p 31369:80 -d blazor-server
```  
3. Giv den nogle min før du prøver at forbinde men det kan gøres på:  
```<IP or hostname or localhost:32768/swagger/index.html>```  

## Extra hjælp
Hvis du har brug for yderligere hjælp til InfluxDB eller Blazor Server, anbefales det at gennemgå dokumentationen og ressourcerne fra følgende kilder:

- InfluxDB-dokumentation: [https://docs.influxdata.com/](https://docs.influxdata.com/)
- Blazor-dokumentation: [https://docs.microsoft.com/blazor/](https://docs.microsoft.com/blazor/)
- .NET-dokumentation: [https://docs.microsoft.com/dotnet/](https://docs.microsoft.com/dotnet/)
  
### Built With
___
|  MAUI  |  C#  |  MQTT  |  Minimal API  |  InfluxDB  |
|---|---|---|---|---|
|  <img src="https://github.com/LukasPedersen/H5AppProgrammering-III-Project/assets/61869988/bdaa047a-7892-4593-9627-39e5e18ecbe9" alt="Logo" width="120" height="120">  |  <img src="https://github.com/LukasPedersen/H5AppProgrammering-III-Project/assets/61869988/b2da569a-06aa-4c5b-95c0-8259c661d7f6" alt="Logo" width="120" height="120">  |  <img src="https://github.com/LukasPedersen/H5AppProgrammering-III-Project/assets/61869988/5b74a28d-3711-42c7-b695-46c6ab36d9d3" alt="Logo" width="155" height="155">| <img src="https://github.com/LukasPedersen/H5AppProgrammering-III-Project/assets/61869988/a6e03e24-9096-4dab-b82e-324a5183ddc2" alt="Logo" width="120" height="120">  |  <img src="https://github.com/LukasPedersen/H5AppProgrammering-III-Project/assets/61869988/a5a46e98-d1fd-486c-9189-0c35e66b3537" alt="Logo" width="120" height="120">  |
___

## Topics
---
| Topic | Type | Body | Description |
|---|---|---|---|
| devices/```deviceId```/messages/devicebound | Subscribe | ```{ "LED": "ON" }``` | Tell device to turn LED ON or OFF |
| devices/```deviceId```/messages/devicebound | Subscribe | ```{ "Servo": 120 }``` | Tell device to turn Servo to x degrees |
| devices/```deviceId```/messages/telemetry | Publish | ```{ "temperature": 25.5, "humidity": 68.2 }``` | Send telemetry to broker |
---

## Api Endpoints
| Endpoint | Type |
|---|---|
| /setLED | POST |
| /setServo | POST |
| /createTelemetry | POST |
| /getTelemetry | GET |
---

## Roadmap

- [x] Kan vise aktuel (seneste måling) af temperatur og humidity og de målte tidspunkter i lokal tid
- [x] Kan vise en graf over målingerne, hvor man kan vælge mellem seneste time, dag og uge.
- [X] Der skal være en knap, der via MQTT kan aktivere en servo (og simulere at man åbner et vindue eller tænder for ventilationen).
- [X] App'en skal opbygges med MVVM design pattern og Dependency Injection.
- [X] Kan vise seneste data, hvis nettet afbrydes. 
- [X] Er robust overfor ustabil netforbindelse.
- [X] Projektet afleveres i Github med en god Readme-fil og præsenteres for klassen. Readme-filen markerer også hvilke mål, der er nået.
