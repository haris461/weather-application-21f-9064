
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        /* Add your CSS styles here */
        body {
            background: #CEF;
            font-family: Roboto, sans-serif;
            margin: 0;
            padding: 0;
        }

        .container {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 98vh;
            flex-direction: column; /* Align elements vertically */
        }

        .widget {
            background: linear-gradient(to bottom right, #3cc0fe 20%, #0066ff);
            width: 900px;
            height: 400px;
            border-radius: 6px;
            box-shadow: 0px 60px 80px -20px rgba(39, 165, 253, 0.4);
            position: relative;
            overflow: hidden;
        }

        .details {
            font-family: Roboto, sans-serif;
            display: flex;
            flex-direction: column;
            margin-top: 30px;
            margin-left: 60px;
        }

        .temperature {
            color: white;
            font-weight: 300;
            font-size: 10em;
        }

        .summary {
            color: #34373e;
            font-size: 2em;
            font-weight: 300;
            width: 260px;
            margin-top: -16px;
            padding-bottom: 16px;
            border-bottom: 2px solid #9cd0ff;
            margin-left: 8px;
        }

        .summaryText {
            margin: 0;
            margin-left: 56px;
        }

        .precipitation, .wind, .humidity {
            color: #34373e;
            font-size: 1.6em;
            font-weight: 300;
            margin-left: 8px;
        }

        .precipitation {
            margin-top: 16px;
        }

        .wind {
            margin-top: 4px;
        }
        
        .humidity {
            margin-top: 4px;
        }

        .pictoBackdrop {
            position: absolute;
            height: 560px;
            width: 560px;
            border-radius: 50%;
            background: linear-gradient(160deg, rgba(60, 192, 254, 0.7) 40%, rgba(0, 102, 255, 0.6));
            right: -40px;
            top: -90px;
        }

        .pictoFrame {
            position: absolute;
            background: #34373e;
            border-radius: 50%;
            box-shadow: 0px 50px 60px -20px #0066ff;
            height: 336px;
            width: 336px;
            right: 80px;
            top: 25px;
        }

        .pictoCloudBig {
            position: absolute;
            border-radius: 50%;
            background: #aacbe6;
            box-shadow: 20px 20px 80px -20px #aacbe6;
            height: 218.4px;
            width: 218.4px;
            top: 80px;
            right: 160px;
        }

        .pictoCloudFill {
            position: absolute;
            background: #aacbe6;
            box-shadow: 0px 20px 80px -20px #aacbe6;
            height: 152.88px;
            width: 152.88px;
            top: 191px;
            right: 265px;
        }

        .pictoCloudSmall {
            position: absolute;
            border-radius: 50%;
            background: #d2e9fa;
            height: 106.816px;
            width: 106.816px;
            top: 146px;
            right: 282px;
        }

        .iconCloudBig {
            position: absolute;
            border-radius: 50%;
            background: #9cd0ff;
            height: 36px;
            width: 36px;
            top: 200px;
            left: 80px;
        }

        .iconCloudSmall {
            position: absolute;
            border-radius: 50%;
            height: 23.4px;
            width: 23.4px;
            background: #d2e9fa;
            top: 213px;
            left: 70px;
        }

        .iconCloudFill {
            position: absolute;
            height: 16.38px;
            width: 16.38px;
            background: #9cd0ff;
            top: 220px;
            left: 80px;
        }

        .weather-icon {
            position: absolute;
            height: 100px;
            width: 100px;
            top: 280px;
            left: 750px;
        }

        .input-container {
            margin-bottom: 20px; /* Add some space between the input container and the widget */
        }

        .input-container input[type="text"],
        .input-container button {
            font-size: 1.2em;
            padding: 10px;
            margin-right: 10px;
            border: none;
            border-radius: 4px;
        }

        .input-container input[type="text"] {
            width: 200px;
        }

        .input-container button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }

        .input-container button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Weather Application</h1>
    <div class="input-container">
        <input type="text" id="cityInput" placeholder="Enter city name">
        <button onclick="fetchWeather()">Get Weather</button>
    </div>
    <div class="container">
        <div class="widget">
            <div class="details">
                <div class="temperature">Loading...</div>
                <div class="summary">
                    <p class="summaryText">Fetching weather data...</p>
                </div>
                <div class="precipitation"></div>
                <div class="wind"></div>
                <div class="humidity"></div> <!-- New humidity element -->
            </div>
            <div class="pictoBackdrop"></div>
            <div class="pictoFrame"></div>
            <div class="pictoCloudBig"></div>
            <div class="pictoCloudFill"></div>
            <div class="pictoCloudSmall"></div>
            <div class="iconCloudBig"></div>
            <div class="iconCloudFill"></div>
            <div class="iconCloudSmall"></div>
            <img class="weather-icon" src="" alt="Weather Icon">
        </div>
    </div>
    <script>
        // Function to make AJAX call to OpenWeather API
        function fetchWeather() {
            const apiKey = '6c101388096bf122d0fa0d764aa13d43'; // Your OpenWeather API key
            const city = document.getElementById('cityInput').value;
            const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`;

            const xhr = new XMLHttpRequest();
            xhr.open('GET', url, true);

            xhr.onload = function() {
                if (xhr.status === 200) {
                    const data = JSON.parse(xhr.responseText);
                    displayWeather(data);
                } else {
                    console.error('Error fetching weather data:', xhr.statusText);
                }
            };

            xhr.onerror = function() {
                console.error('Error fetching weather data.');
            };

            xhr.send();
        }

        // Function to display weather data on the frontend
        function displayWeather(data) {
            const temperatureElement = document.querySelector('.temperature');
            const summaryElement = document.querySelector('.summaryText');
            const precipitationElement = document.querySelector('.precipitation');
            const windElement = document.querySelector('.wind');
            const humidityElement = document.querySelector('.humidity'); // New humidity element
            const weatherIconElement = document.querySelector('.weather-icon');

            temperatureElement.textContent = `${Math.round(data.main.temp)}°C`;
            summaryElement.textContent = data.weather[0].description;
            precipitationElement.textContent = `Precipitation: ${data.clouds.all}%`;
            windElement.textContent = `Wind: ${data.wind.speed} m/s`;
            humidityElement.textContent = `Humidity: ${data.main.humidity}%`; // Set humidity

            // Set the weather icon
            const iconCode = data.weather[0].icon;
            const iconUrl = `http://openweathermap.org/img/wn/${iconCode}.png`;
            weatherIconElement.setAttribute('src', iconUrl);
        }
    </script>
</body>
</html>

![image](https://github.com/haris461/weather-application-21f-9064/assets/158203393/70458dae-0546-4ba0-a88b-5cae1865b45e)

