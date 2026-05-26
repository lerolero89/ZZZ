<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>¡Feliz Cumpleaños!</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #ffeef2;
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }

        /* PANTALLA DE CARGA (Img 1) */
        #pantalla-carga {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #1a1a1a; /* Oscuro como en la primera imagen */
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            transition: opacity 0.8s ease;
        }

        .loader-rosa {
            border: 6px solid #f3f3f3;
            border-top: 6px solid #e05275;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* FONDO PRINCIPAL DE NUBES Y FLORES (Img 2 en adelante) */
        .escenario-cumple {
            position: relative;
            width: 100vw;
            height: 100vh;
            /* Gradiente que simula el cielo rosa/blanco de fondo */
            background: linear-gradient(to bottom, #fff0f3 0%, #fbc4d4 50%, #f9a7c0 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        /* Decoración de flores abajo a la izquierda */
        .flores-abajo {
            position: absolute;
            bottom: -20px;
            left: -20px;
            font-size: 80px;
            opacity: 0.85;
            pointer-events: none;
            z-index: 2;
            animation: balanceo 4s ease-in-out infinite alternate;
        }

        @keyframes balanceo {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(5deg); }
        }

        /* CORAZÓN GRANDE DEL FONDO (Bombea despacito) */
        .corazon-fondo {
            position: absolute;
            bottom: -50px;
            right: -20px;
            font-size: 180px;
            opacity: 0.4;
            pointer-events: none;
            z-index: 1;
            transform-origin: center;
            animation: bombeo 2s infinite alternate ease-in-out;
        }

        /* CORAZÓN CON ALITAS (Bombea y flota de costado) */
        .corazon-alas {
            position: absolute;
            top: 35%;
            right: 15%;
            font-size: 60px;
            z-index: 3;
            pointer-events: none;
            animation: bombeoAlas 1.8s infinite alternate ease-in-out;
        }

        @keyframes bombeo {
            0% { transform: scale(1); }
            100% { transform: scale(1.05); }
        }

        @keyframes bombeoAlas {
            0% { transform: scale(1) translateY(0); }
            100% { transform: scale(1.1) translateY(-10px); }
        }

        /* TEXTO PRINCIPAL ANIMADO (Img 3) */
        .texto-saludo {
            position: absolute;
            top: 15%;
            width: 100%;
            text-align: center;
            font-size: 2.2rem;
            font-weight: bold;
            font-family: 'Georgia', serif;
            opacity: 0;
            z-index: 4;
            animation: aparicionTexto 4s forwards 0.5s;
        }

        @keyframes aparicionTexto {
            0% { opacity: 0; color: #ffccd5; }
            50% { opacity: 1; color: #ffccd5; }
            100% { opacity: 1; color: #c92a4a; } /* Se oscurece a rosa fuerte */
        }

        /* CAJA DE REGALO INTERACTIVA (Img 4 y 5) */
        .regalo-contenedor {
            position: relative;
            width: 100px;
            height: 100px;
            cursor: pointer;
            z-index: 5;
            transition: transform 0.3s;
        }

        .regalo-emoji {
            font-size: 80px;
            text-align: center;
            line-height: 100px;
        }

        /* TRANSICIÓN DE APERTURA: la tapa/caja desaparece con animación */
        .regalo-contenedor.abierto {
            transform: scale(0);
            opacity: 0;
            transition: all 0.5s ease;
            pointer-events: none;
        }

        /* SOBRE QUE ENTRA Y SALE SUAVEMENTE (Img 6) */
        .sobre-carta {
            position: absolute;
            font-size: 110px;
            cursor: pointer;
            z-index: 6;
            display: none;
            transform-origin: center;
        }

        .sobre-carta.activo {
            display: block;
            animation: entradaSobre 0.8s ease-out forwards, flotarSobre 2s infinite alternate ease-in-out 0.8s;
        }

        @keyframes entradaSobre {
            0% { transform: scale(0) translateY(100px); opacity: 0; }
            100% { transform: scale(1) translateY(0); opacity: 1; }
        }

        @keyframes flotarSobre {
            0% { transform: translateY(0) scale(1); }
            100% { transform: translateY(-12px) scale(1.03); } /* Efecto entra y sale poquito */
        }

        /* CORAZONCITOS ALREDEDOR DEL SOBRE */
        .mini-corazon {
            position: absolute;
            font-size: 20px;
            opacity: 0;
        }
        .sobre-carta.activo .mini-corazon {
            animation: flotarMinis 1.5s infinite alternate ease-in-out;
        }
        .mc1 { top: -10px; left: -10px; animation-delay: 0.2s; }
        .mc2 { top: -20px; right: -10px; animation-delay: 0.6s; }

        @keyframes flotarMinis {
            0% { transform: translateY(0); opacity: 0.6; }
            100% { transform: translateY(-10px); opacity: 1; }
        }

        /* TEXTO CURVADO "HAPPY BIRTHDAY" QUE RODEA (Img 7 y 8) */
        .texto-curvo-svg {
            position: absolute;
            width: 320px;
            height: 320px;
            z-index: 4;
            pointer-events: none;
            opacity: 0;
            transform: scale(0.8);
            transition: all 1s ease;
        }
        .texto-curvo-svg.visible {
            opacity: 1;
            transform: scale(1);
        }

        .texto-path {
            font-family: 'Arial', sans-serif;
            font-size: 22px;
            font-weight: bold;
            fill: #d94060;
            letter-spacing: 3px;
        }

        /* TARJETA DE MENSAJE FINAL MODAL (Img 9 y 10) */
        .modal-tarjeta {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.4); /* Fondo oscurecido */
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            z-index: 100;
            transition: opacity 0.6s ease;
        }

        .modal-tarjeta.abierto {
            opacity: 1;
            pointer-events: auto;
        }

        .tarjeta-blanca {
            background: white;
            width: 85%;
            max-width: 340px;
            padding: 35px 25px;
            border-radius: 20px;
            border: 2px dashed #ffb3c6;
            box-shadow: 0 12px 30px rgba(0,0,0,0.15);
            text-align: center;
            position: relative;
            transform: translateY(50px);
            transition: transform 0.6s ease;
        }

        .modal-tarjeta.abierto .tarjeta-blanca {
            transform: translateY(0);
        }

        .globo-decorativo {
            font-size: 45px;
            margin-bottom: 15px;
            display: inline-block;
            animation: flotarSobre 1.5s infinite alternate ease-in-out;
        }

        .texto-carta-final {
            color: #4a4a4a;
            font-size: 1.15rem;
            line-height: 1.6;
            font-family: 'Georgia', serif;
            margin-bottom: 15px;
        }

        .firma-final {
            color: #d94060;
            font-size: 1rem;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div id="pantalla-carga">
        <div class="loader-rosa"></div>
    </div>

    <div class="escenario-cumple">
        
        <div class="texto-saludo">¡Hola linda! Tengo una sorpresa para vos</div>
        <div class="flores-abajo">🌸🌷🌹</div>
        <div class="corazon-fondo">💖</div>
        <div class="corazon-alas">👼💖</div>

        <svg class="texto-curvo-svg" id="circuloTexto" viewBox="0 0 300 300">
            <path id="curva" d="M 150, 40 A 110,110 0 0,1 260,150 A 110,110 0 0,1 150,260 A 110,110 0 0,1 40,150 A 110,110 0 0,1 150,40" fill="none"/>
            <text>
                <textPath href="#curva" class="texto-path" startOffset="0%">
                    ¡Feliz Cumpleaños! • ¡Feliz Cumpleaños! • 
                </textPath>
            </text>
        </svg>

        <div class="regalo-contenedor" id="cajaRegalo" onclick="abrirRegaloExacto()">
            <div class="regalo-emoji">🎁</div>
        </div>

        <div class="sobre-carta" id="sobreCarta" onclick="abrirMensajeTarjeta()">
            <div class="mini-corazon mc1">💗</div>
            <div class="mini-corazon mc2">💕</div>
            ✉️
        </div>

    </div>

    <div class="modal-tarjeta" id="modalFinal">
        <div class="tarjeta-blanca">
            <div class="globo-decorativo">🎈❤️</div>
            <p class="texto-carta-final">
                Deseo que este año que comienza esté lleno de risas, amor y momentos inolvidables.
            </p>
            <p class="firma-final">¡Con todo mi cariño! ✨</p>
        </div>
    </div>

    <script>
        // Simulamos la carga exacta de la primera imagen
        window.addEventListener('load', () => {
            setTimeout(() => {
                const carga = document.getElementById('pantalla-carga');
                carga.style.opacity = '0';
                setTimeout(() => carga.style.display = 'none', 800);
            }, 2000); // 2 segundos de carga fija
        });

        // Al tocar el regalo, se esconde y aparece el sobre flotante
        function abrirRegaloExacto() {
            document.getElementById('cajaRegalo').classList.add('abierto');
            
            setTimeout(() => {
                document.getElementById('sobreCarta').classList.add('activo');
                document.getElementById('circuloTexto').classList.add('visible');
            }, 500);
        }

        // Al tocar el sobre, se despliega la tarjeta blanca final con el fondo oscurecido
        function abrirMensajeTarjeta() {
            document.getElementById('modalFinal').classList.add('abierto');
        }
    </script>
</body>
</html>
