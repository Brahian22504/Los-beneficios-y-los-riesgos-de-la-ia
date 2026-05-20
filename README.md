<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recordatorio de Medicación</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 40px;
        }
        label {
            display: block;
            margin-top: 20px;
        }
        input {
            margin-top: 10px;
            padding: 10px;
            width: 100%;
            max-width: 300px;
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <h1>Recordatorio de Medicación</h1>
    <form id="medicationForm">
        <label for="dose">Dosis:</label>
        <input type="text" id="dose" required>

        <label for="time">Hora (HH:MM):</label>
        <input type="time" id="time" required>

        <button type="submit">Iniciar Recordatorio</button>
    </form>

    <script>
        document.getElementById('medicationForm').addEventListener('submit', function(e) {
            e.preventDefault();
            if (!('Notification' in window)) {
                alert('Tu navegador no soporta notificaciones.');
                return;
            }
            Notification.requestPermission().then(function(permission) {
                if (permission === 'granted') {
                    // Obtener dosis y hora del formulario
                    const dose = document.getElementById('dose').value;
                    const time = document.getElementById('time').value;
                    // Convertir la hora a milisegundos
                    const [hour, minute] = time.split(':').map(Number);
                    const now = new Date();
                    let nextReminder = new Date(now.getFullYear(), now.getMonth(), now.getDate(), hour, minute);
                    if (nextReminder <= now) {
                        nextReminder.setDate(nextReminder.getDate() + 1); // Siguiente día
                    }
                    // Calcular intervalo
                    const interval = nextReminder.getTime() - now.getTime();
                    // Esperar hasta la hora
                    setTimeout(function() {
                        new Notification('Toma tu medicación', {
                            body: `Dosis: ${dose}`,
                        });
                    }, interval);
                    alert('Recordatorio configurado. Recibirás una notificación a la hora indicada.');
                } else {
                    alert('Permiso de notificación denegado.');
                }
            });
        });
    </script>
</body>
</html>
