<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relógio Digital</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            margin-top: 100px;
            font-size: 1.5em;
        }

        #clock {
            display: inline-block;
            margin-right: 20px;
        }

        #date {
            display: inline-block;
        }

        #stopwatch {
            margin-top: 20px;
        }

        #timezone {
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <div id="clock"></div>
    <div id="date"></div>
    <div id="stopwatch">00:00:00</div>
    <div id="timezone">
        <label for="timezoneSelect">Fuso Horário:</label>
        <select id="timezoneSelect"></select>
    </div>

    <script>
        function updateClock() {
            var now = new Date();
            var hours = now.getHours();
            var minutes = now.getMinutes();
            var seconds = now.getSeconds();
            var day = now.getDate();
            var month = now.getMonth() + 1;
            var year = now.getFullYear();

            // Adiciona um zero à esquerda se for menor que 10
            hours = hours < 10 ? "0" + hours : hours;
            minutes = minutes < 10 ? "0" + minutes : minutes;
            seconds = seconds < 10 ? "0" + seconds : seconds;
            day = day < 10 ? "0" + day : day;
            month = month < 10 ? "0" + month : month;

            var timeString = hours + ":" + minutes + ":" + seconds;
            var dateString = day + "/" + month + "/" + year;

            document.getElementById("clock").innerHTML = timeString;
            document.getElementById("date").innerHTML = dateString;
        }

        function updateStopwatch() {
            var stopwatch = document.getElementById("stopwatch");
            var currentTime = stopwatch.innerHTML.split(":");
            var hours = parseInt(currentTime[0]);
            var minutes = parseInt(currentTime[1]);
            var seconds = parseInt(currentTime[2]);

            seconds++;

            if (seconds === 60) {
                seconds = 0;
                minutes++;
            }

            if (minutes === 60) {
                minutes = 0;
                hours++;
            }

            // Adiciona um zero à esquerda se for menor que 10
            hours = hours < 10 ? "0" + hours : hours;
            minutes = minutes < 10 ? "0" + minutes : minutes;
            seconds = seconds < 10 ? "0" + seconds : seconds;

            stopwatch.innerHTML = hours + ":" + minutes + ":" + seconds;
        }

        function updateTimezoneSelect() {
            var timezoneSelect = document.getElementById("timezoneSelect");
            var timezones = Intl.DateTimeFormat().resolvedOptions().timeZone;
            var timezoneList = timezones.split('/');
            
            for (var i = 0; i < timezoneList.length; i++) {
                var option = document.createElement("option");
                option.text = timezoneList[i];
                option.value = timezoneList[i];
                timezoneSelect.add(option);
            }
        }

        function updateClockWithTimezone() {
            var selectedTimezone = document.getElementById("timezoneSelect").value;
            var options = { timeZone: selectedTimezone, hour12: false };
            var timeString = new Date().toLocaleTimeString('en-US', options);
            document.getElementById("clock").innerHTML = timeString;
        }

        setInterval(updateClock, 1000);
        setInterval(updateStopwatch, 1000);
        setInterval(updateClockWithTimezone, 1000);

        updateTimezoneSelect();
        document.getElementById("timezoneSelect").addEventListener("change", updateClockWithTimezone);
    </script>

</body>
</html>
