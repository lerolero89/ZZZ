<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>¡Feliz Cumpleaños!</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { background-color: #ffeef2; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; perspective: 1000px; }

        /* --- 1. PANTALLA DE CARGA --- */
        #pantalla-carga { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: #f7cbd5; display: flex; justify-content: center; align-items: center; z-index: 200; transition: opacity 1s ease; }
        .loader { border: 8px solid #f3f3f3; border-top: 8px solid #ff6584; border-radius: 50%; width: 60px; height: 60px; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* --- 2. CONTENEDOR PRINCIPAL (Regalo y Corazones Latientes) --- */
        .contenedor { position: relative; width: 100%; max-width: 400px; height: 600px; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; padding: 20px; transition: opacity 0.8s ease; }
        .texto-aparicion { color: #ffb3c6; font-size: 1.5rem; font-weight: bold; margin-bottom: 20px; opacity: 0; animation: oscurecerTexto 4s forwards 1s; }
        @keyframes oscurecerTexto { 0% { opacity: 0; color: #ffb3c6; } 50% { opacity: 1; color: #ffb3c6; } 100% { opacity: 1; color: #d94060; } }
        .decoracion { position: absolute; pointer-events: none; }
        .cora-esquina { bottom: 40px; right: 40px; font-size: 50px; animation: latido 1.2s infinite alternate ease-in-out; }
        .cora-alas { top: 80px; left: 40px; font-size: 45px; animation: latido 1s infinite alternate ease-in-out; }
        @keyframes latido { 0% { transform: scale(1); } 100% { transform: scale(1.15); } }

        /* --- 3. CAJA DE REGALO (Con Tapa que Cae) --- */
        .caja-regalo { position: relative; width: 150px; height: 150px; cursor: pointer; margin-top: 20px; z-index: 10; }
        .tapa { position: absolute; top: 0; left: -5px; width: 160px; height: 40px; background-color: #ff4d6d; border-radius: 4px; z-index: 3; transition: transform 0.8s cubic-bezier(0.4, 0, 1, 1), opacity 0.8s; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .tapa::after { content: "💝"; position: absolute; top: -20px; left: 42%; font-size: 25px; }
        .cuerpo-caja { position: absolute; bottom: 0; left: 0; width: 150px; height: 120px; background-color: #ff758f; border-radius: 0 0 8px 8px; z-index: 1; box-shadow: 0 8px 15px rgba(0,0,0,0.1); }
        .caja-regalo.abierta .tapa { transform: translateY(150px) rotate(110deg); opacity: 0; }

        /* --- 4. SOBRE Y CARTA (Flotando con Corazones) --- */
        .sobre-contenedor { position: absolute; bottom: 50px; width: 140px; height: 100px; z-index: 2; opacity: 0; transform: scale(0.5); transition: all 0.8s ease-out; pointer-events: none; }
        .caja-regalo.abierta + .sobre-contenedor { opacity: 1; transform: scale(1); animation: flotarCarta 2.5s infinite alternate ease-in-out 0.8s; pointer-events: auto; }
        .sobre { position: absolute; width: 100%; height: 100%; background-color: #fff0f3; border: 2px solid #ffb3c6; border-radius: 4px; display: flex; justify-content: center; align-items: center; font-size: 30px; box-shadow: 0 4px 10px rgba(0,0,0,0.15); cursor: pointer; }
        .cora-sobre { position: absolute; font-size: 16px; animation: flotarCoraSobre 1.5s infinite alternate ease-in-out; }
        .cs1 { top: -20px; left: -10px; animation-delay: 0.2s; }
        .cs2 { top: -30px; right: -5px; animation-delay: 0.5s; }
        @keyframes flotarCarta { 0% { transform: translateY(0px); } 100% { transform: translateY(-15px); } }
        @keyframes flotarCoraSobre { 0% { transform: translateY(0) scale(0.9); opacity: 0.7; } 100% { transform: translateY(-8px) scale(1.1); opacity: 1; } }

        /* --- 5. PANTALLA DEL CORAZÓN DE ROSAS --- */
        #pantalla-corazon-rosas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: #fff0f3; display: flex; flex-direction: column; justify-content: center; align-items: center; opacity: 0; pointer-events: none; transition: opacity 1s ease; z-index: 50; padding: 20px; }
        #pantalla-corazon-rosas.activa { opacity: 1; pointer-events: auto; }
        .corazon-rosas-silueta { position: relative; width: 280px; height: 250px; }
        .rosa-cora { position: absolute; font-size: 28px; animation: pulsoSuave 2s infinite alternate; }
        .r1 { top: 10%; left: 25%; } .r2 { top: 5%; left: 40%; } .r3 { top: 10%; left: 55%; } .r4 { top: 5%; left: 70%; } .r5 { top: 25%; left: 10%; } .r6 { top: 25%; left: 85%; } .r7 { top: 45%; left: 5%; } .r8 { top: 45%; left: 90%; } .r9 { top: 65%; left: 15%; } .r10 { top: 65%; left: 80%; } .r11 { top: 80%; left: 30%; } .r12 { top: 80%; left: 65%; } .r13 { top: 92%; left: 48%; }
        .texto-rosas-centro { position: absolute; top: 35%; left: 15%; width: 70%; color: #d94060; font-size: 1.2rem; font-weight: bold; text-align: center; line-height: 1.4; }
        .mensaje-cumple-abajo { margin-top: 20px; color: #ff4d6d; font-size: 1.6rem; font-weight: bold; max-width: 320px; text-align: center; opacity: 0; transition: opacity 0.8s ease 1.2s; }
        #pantalla-corazon-rosas.activa .mensaje-cumple-abajo { opacity: 1; }
        .boton-final { margin-top: 30px; background-color: #ff6584; color: white; border: none; padding: 12px 24px; border-radius: 20px; font-size: 1rem; cursor: pointer; opacity: 0; transition: opacity 0.8s ease 1.8s; }
        #pantalla-corazon-rosas.activa .boton-final { opacity: 1; }
        @keyframes pulsoSuave { 0% { transform: scale(1); } 100% { transform: scale(1.08); } }

        /* --- 6. PANTALLA TARJETA MENSAJE FINAL --- */
        #pantalla-tarjeta-final { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: #f7cbd5; display: flex; justify-content: center; align-items: center; opacity: 0; pointer-events: none; transition: opacity 1s ease; z-index: 60; padding: 30px; }
        #pantalla-tarjeta-final.activa { opacity: 1; pointer-events: auto; }
        .tarjeta-final-papel { background: white; padding: 40px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); max-width: 350px; text-align: center; transform: translateY(30px); transition: transform 0.8s ease; }
        #pantalla-tarjeta-final.activa .tarjeta-final-papel { transform: translateY(0); }
        .texto-tarjeta-final { color: #d94060; font-size: 1.3rem; font-weight: 500; line-height: 1.6; }
        .corazon-final-decorativo { font-size: 40px; margin-top: 20px; animation: latido 1.5s infinite; }
    </style>
</head>
<body>

    <div id="pantalla-carga"><div class="loader"></div></div>

    <div class="contenedor" id="contenedorPrincipal">
        <div class="texto-aparicion">¡Tengo una sorpresa para vos!</div>
        <div class="decoracion cora-esquina">💖</div>
        <div class="decoracion cora-alas">👼💖</div>
        <div class="caja-regalo" id="regalo" onclick="abrirRegalo()">
            <div class="tapa"></div>
            <div class="cuerpo-caja"></div>
        </div>
        <div class="sobre-contenedor" id="sobre" onclick="mostrarCorazonRosas()">
            <div class="cora-sobre cs1">💗</div><div class="cora-sobre cs2">💓</div>
            <div class="sobre">✉️</div>
        </div>
    </div>

    <div id="pantalla-corazon-rosas">
        <div class="corazon-rosas-silueta">
            <div class="rosa-cora r1">🌹</div><div class="rosa-cora r2">🌹</div><div class="rosa-cora r3">🌹</div><div class="rosa-cora r4">🌹</div><div class="rosa-cora r5">🌹</div><div class="rosa-cora r6">🌹</div><div class="rosa-cora r7">🌹</div><div class="rosa-cora r8">🌹</div><div class="rosa-cora r9">🌹</div><div class="rosa-cora r10">🌹</div><div class="rosa-cora r11">🌹</div><div class="rosa-cora r12">🌹</div><div class="rosa-cora r13">🌹</div>
            <div class="texto-rosas-centro">Sos una persona increíble</div>
        </div>
        <div class="mensaje-cumple-abajo">¡Que pases el mejor de los cumpleaños! 🎉</div>
        <button class="boton-final" onclick="mostrarTarjetaFinal()">Ver mensaje final</button>
    </div>

    <div id="pantalla-tarjeta-final">
        <div class="tarjeta-final-papel">
            <p class="texto-tarjeta-final">Deseo que este año que comienza esté lleno de risas, amor y momentos inolvidables.</p>
            <div class="corazon-final-decorativo">❤️</div>
        </div>
    </div>

    <script>
        // Quita carga
        window.addEventListener('load', () => setTimeout(() => {
            const carga = document.getElementById('pantalla-carga');
            carga.style.opacity = '0';
            setTimeout(() => carga.style.display = 'none', 1000);
        }, 1500));

        // Abre regalo
        function abrirRegalo() { document.getElementById('regalo').classList.add('abierta'); }

        // Muestra corazón rosas
        function mostrarCorazonRosas() {
            document.getElementById('contenedorPrincipal').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('contenedorPrincipal').style.display = 'none';
                document.getElementById('pantalla-corazon-rosas').classList.add('activa');
            }, 800);
        }

        // Muestra tarjeta final
        function mostrarTarjetaFinal() {
            document.getElementById('pantalla-corazon-rosas').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('pantalla-corazon-rosas').style.display = 'none';
                document.getElementById('pantalla-tarjeta-final').classList.add('activa');
            }, 800);
        }
    </script>
</body>
</html>
