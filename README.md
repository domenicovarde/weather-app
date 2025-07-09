# weather-app


# Weather App - Temperature Città Italiane

Applicazione Spring Boot che mostra le temperature delle ultime 2 settimane per 8 città italiane utilizzando le API di open-meteo.com.

## Struttura del Progetto

```
weather-app/
├── src/
│   └── main/
│       ├── java/com/example/weatherapp/
│       │   ├── WeatherAppApplication.java      # Classe main Spring Boot
│       │   ├── controller/
│       │   │   └── WeatherController.java      # Controller web
│       │   ├── model/
│       │   │   └── WeatherData.java            # Entità JPA
│       │   ├── repository/
│       │   │   └── WeatherRepository.java      # Repository JPA
│       │   └── service/
│       │       └── WeatherService.java         # Logica business
│       └── resources/
│           ├── application.properties          # Configurazione app
│           ├── static/                         # Contenuti statici accessibili via HTTP
│           │   └── css/
│           │       └── style.css               # Foglio di stile esterno
│           └── templates/
│               └── index.html                  # Template Thymeleaf aggiornato con <link>
├── pom.xml                                      # Dipendenze Maven
├── Dockerfile                                   # Configurazione Docker
├── docker-compose.yml                           # Orchestrazione Docker
└── README.md                                       # Questo file (documentazione)
                             
```

## Componenti Principali

### 1. WeatherData (Model)
- Entità JPA per memorizzare i dati meteo
- Campi: id, city, date, temperature, latitude, longitude

### 2. WeatherRepository
- Repository JPA per accesso ai dati
- Query personalizzate per recuperare dati per data e città

### 3. WeatherService
- Logica per chiamare le API di open-meteo.com
- Gestisce 8 città italiane con coordinate predefinite
- Salva i dati nel database H2

### 4. WeatherController
- Endpoint "/" per visualizzare i dati
- Endpoint "/refresh" per aggiornare i dati

### 5. Template HTML
- Pagina web con CSS basico
- Tabelle per ogni città con temperature per data

## Città Incluse
- Roma, Milano, Napoli, Torino, Firenze, Bologna, Venezia, Bari

## Database
- H2 in-memory con persistenza su file
- Dati salvati nella cartella `h2-data/`

## Come Eseguire

### Metodo 1: Docker (Raccomandato)
```bash
# Build e start del container
docker-compose up --build

# Oppure manualmente:
mvn clean package
docker build -t weather-app .
docker run -p 8081:8081 weather-app
```

### Metodo 2: Locale
```bash
# Compilazione ed esecuzione
mvn clean spring-boot:run
```

## Utilizzo
1. Avvia l'applicazione
2. Vai su http://localhost:8081
3. Clicca "Aggiorna Dati" per scaricare le temperature
4. Visualizza i dati per ogni città

## Console H2 (Debug)
- URL: http://localhost:8080/h2-console
- JDBC URL: jdbc:h2:file:./h2-data/weatherdb
- Username: sa
- Password: (vuota)


### INSERIMENTO DI DATI NEL DB USANDO H2 HIBERNATE
il progetto è impostato in modo che i dati nel db vengano resettati ogni volta che viene lanciato,quindi bisogna entrare nella console h2 hibernate come mostrato sopra ed eseguire la seguente queri per popolare la tabella
```bash
# Compilazione ed esecuzione
INSERT INTO weather_data (DATE, LATITUDE, LONGITUDE, TEMPERATURE, ID, CITY) VALUES
('2025-07-01', 45.4642, 9.1900, 30.5, 1, 'Milano'),
('2025-07-01', 41.9028, 12.4964, 33.2, 2, 'Roma'),
('2025-07-01', 40.8518, 14.2681, 31.0, 3, 'Napoli'),
('2025-07-01', 44.4949, 11.3426, 29.8, 4, 'Bologna'),
('2025-07-01', 43.7696, 11.2558, 28.7, 5, 'Firenze');
('2025-07-01', 45.0703, 7.6869, 27.9, 6, 'Torino'),
('2025-07-01', 45.4408, 12.3155, 29.3, 7, 'Venezia');
('2025-07-01', 41.1171, 16.8719, 32.1, 8, 'Bari');

```

una volta inseriti i dati nella tabella,tornando sulla pagina principale basterà semplicemente aggiornare la pagina  cliccando il tasto aggiorna dati per visualizzare la pagina aggiornata con i dati inseriti
