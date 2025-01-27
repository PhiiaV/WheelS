# WheelS
Ruleta
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rueda de la Fortuna</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-image:url('Fondo/fondousula1.jpg')
        }
        canvas {
            border: 15px solid white; /* Borde blanco */
            border-radius: 50%;
            position: relative;
            box-shadow: 0 6px 22px rgba(0, 0, 0, 0.8); /* Sombra */
        }
        button {
            margin-top: 30px;
            padding: 15px 25px;
            font-size: 20px;
            background-color: #198754; /* Color de fondo */
            color: white; /* Color del texto */
            border: none;
            border-radius: 25px; /* Bordes redondeados */
            cursor: pointer;
            transition: background-color 0.3s, transform 0.3s; /* Transiciones suaves */
        }   
        button:hover {
            background-color: #14532d; /* Color al pasar el mouse */
            transform: scale(1.05); /* Efecto de aumento */
        }
        .arrow {
            position: absolute;
            top: 50%; /* Centrar verticalmente */
            left: 50%; /* Centrar horizontalmente */
            transform: translate(-50%, -150%) rotate(180deg); /* Ajustar posición al borde */
            width: 0;
            height: 0;
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-bottom: 40px solid black; /* Color de la flecha */
            z-index: 10;
            filter: drop-shadow(0px 4px 6px rgba(0, 0, 0, 0.5)); /* Sombra */
        }

    </style>
</head>
<body>
<div class="arrow"></div>

<canvas id="wheel" width="400" height="400"></canvas>
<button id="spinButton">Girar la Rueda</button>

<script>
    const canvas = document.getElementById('wheel');
    const ctx = canvas.getContext('2d');
    const segments = 6;
    const angle = (2 * Math.PI) / segments;
    const prizes = ["Gorra", "Camisa", "Gira de nuevo", "Termo", "Buen Intento", "Gira de nuevo"];
    let rotation = 0;

    function drawWheel() {
        for (let i = 0; i < segments; i++) {
            const startAngle = i * angle + rotation;
            const endAngle = (i + 1) * angle + rotation;

            ctx.beginPath();
            ctx.moveTo(200, 200);
            ctx.arc(200, 200, 200, startAngle, endAngle);
            ctx.fillStyle = i % 2 === 0 ? '#093e7c' : '#74ab66'; // Colores alternos
            ctx.fill();
            ctx.strokeStyle = 'white'; // Contorno blanco
            ctx.stroke();

            // Dibujar el texto
            ctx.save();
            ctx.translate(200, 200);
            ctx.rotate(startAngle + angle / 2);
            ctx.fillStyle = '#FFFFFF'; // Texto blanco
            ctx.font = '20px Owsald, sans-serif';
            ctx.fillText(prizes[i], 60, 0);
            ctx.restore();
        }
    }

    function spinWheel() {
        const spinAngle = Math.random() * (360) + 3600; // Girar al menos 10 vueltas
        const spinDuration = 3000; // Duración del giro en milisegundos
        const startTime = performance.now();

        function animate(time) {
            const elapsed = time - startTime;
            const progress = Math.min(elapsed / spinDuration, 1);
            rotation = (spinAngle * progress) * (Math.PI / 180); // Convertir a radianes

            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawWheel();

            if (progress < 1) {
                requestAnimationFrame(animate);
            }
        }

        requestAnimationFrame(animate);
    }

    document.getElementById('spinButton').addEventListener('click', spinWheel);
    drawWheel();


    function positionArrow() {
    const arrow = document.querySelector('.arrow');
    const canvasRect = canvas.getBoundingClientRect();

    // Posicionar la flecha en el borde superior del círculo
    const arrowTop = canvasRect.top + canvasRect.height / 2 - 150; // Ajusta el valor 150 al radio del círculo
    const arrowLeft = canvasRect.left + canvasRect.width / 2;

    arrow.style.top = `${arrowTop}px`;
    arrow.style.left = `${arrowLeft}px`;
}

// Llama a la función al cargar la página y al redimensionar
window.addEventListener('resize', positionArrow);
positionArrow();



</script>

</body>
</html>
