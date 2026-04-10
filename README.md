<!doctype html>
<html class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Driving Simulator PRO</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <style>
    html, body { height: 100%; margin: 0; }
    body { box-sizing: border-box; }
  </style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="/_sdk/element_sdk.js" type="text/javascript"></script>
 </head>

 <body class="h-full overflow-auto" style="background: #0a0a0f; font-family: 'Rajdhani', sans-serif;">

<!-- PERFIL AÑADIDO -->
<div id="perfil-info" style="
    position: fixed;
    top: 10px;
    right: 10px;
    background: rgba(0,0,0,0.6);
    padding: 12px 18px;
    border-radius: 10px;
    color: #fff;
    font-family: 'Rajdhani', sans-serif;
    text-align: right;
    z-index: 9999;
    font-size: 14px;
">
    <div><strong>Tiempo aquí:</strong> <span id="tiempo-uso">0s</span></div>
    <div><strong>Total acumulado:</strong> <span id="tiempo-total">0s</span></div>
    <div><strong>Entradas:</strong> <span id="contador-entradas">0</span></div>
</div>
<!-- FIN PERFIL -->

  <div id="app-wrapper" class="w-full min-h-full flex flex-col items-center justify-center p-4" style="background: radial-gradient(ellipse at 50% 0%, #1a1a2e 0%, #0a0a0f 70%);">
   <h1 id="main-title" class="text-3xl md:text-4xl font-bold tracking-widest mb-6" style="font-family: 'Orbitron', monospace; color: #e74c3c; text-shadow: 0 0 20px rgba(231,76,60,0.4);">SIMULATOR PRO</h1>
   <div class="relative w-full" style="max-width: 750px;">
    <canvas id="panel" class="w-full rounded-2xl" style="background: linear-gradient(180deg, #151520 0%, #0d0d15 100%); border: 1px solid rgba(255,255,255,0.06); box-shadow: 0 0 40px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.03);"></canvas>
   </div>
<!-- Odometer -->
<div class="mt-5 px-6 py-3 rounded-xl" style="background: #0f0f18; border: 1px solid rgba(255,255,255,0.06);">
    <div class="flex items-center gap-3">
     <span class="text-xs tracking-widest uppercase" style="color: #555; font-family: 'Orbitron', monospace;">ODO</span>
     <div id="odometer" class="text-2xl tracking-[0.3em]" style="font-family: 'Orbitron', monospace; color: #0f0;">
      000000.000
     </div>
     <span class="text-xs" style="color: #555;">km</span>
    </div>
</div>

<!-- Info strip -->
<div class="mt-4 flex flex-wrap justify-center gap-6 text-sm" style="color: #888; font-family: 'Orbitron', monospace;">
    <div><span style="color:#555;">VEL</span> <span id="vel" style="color:#e74c3c;">0</span> <span style="color:#444;">km/h</span></div>
    <div><span style="color:#555;">RPM</span> <span id="rpm" style="color:#f39c12;">0</span></div>
    <div><span style="color:#555;">GEAR</span> <span id="gear" style="color:#3498db;">1</span></div>
</div>

<!-- Controls hint -->
<div class="mt-6 flex flex-wrap justify-center gap-3 text-xs" style="color: #444; font-family: 'Rajdhani', sans-serif;">
    <div class="flex items-center gap-1"><kbd class="px-2 py-1 rounded" style="background:#1a1a25; border:1px solid #2a2a35;">↑</kbd> Acelerar</div>
    <div class="flex items-center gap-1"><kbd class="px-2 py-1 rounded" style="background:#1a1a25; border:1px solid #2a2a35;">↓</kbd> Frenar</div>
    <div class="flex items-center gap-1"><kbd class="px-2 py-1 rounded" style="background:#1a1a25; border:1px solid #2a2a35;">←→</kbd> Marchas</div>
</div>

<!-- Color picker panel -->
<div id="colors-panel" class="hidden fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 z-50" style="backdrop-filter: blur(4px);">
    <div class="bg-gray-900 rounded-2xl p-6 max-w-sm w-full" style="background: linear-gradient(180deg, #1a1a2e 0%, #0d0d15 100%); border: 1px solid rgba(255,255,255,0.1);">
     <h2 class="text-xl font-bold tracking-widest mb-6" style="font-family: 'Orbitron', monospace; color: #e74c3c;">COLORS</h2>

     <!-- (… AQUÍ VA TODO TU CÓDIGO ORIGINAL, COMPLETO, SIN CAMBIOS …) -->

<!-- SCRIPT DEL PERFIL -->
<script>
function formatearTiempo(segundos) {
    let h = Math.floor(segundos / 3600);
    let m = Math.floor((segundos % 3600) / 60);
    let s = segundos % 60;
    let t = "";
    if (h > 0) t += h + "h ";
    if (m > 0) t += m + "m ";
    t += s + "s";
    return t.trim();
}

let visitas = localStorage.getItem("contadorEntradas");
visitas = visitas ? parseInt(visitas) + 1 : 1;
localStorage.setItem("contadorEntradas", visitas);
document.getElementById("contador-entradas").textContent = visitas;

let tiempoTotal = localStorage.getItem("tiempoTotal");
tiempoTotal = tiempoTotal ? parseInt(tiempoTotal) : 0;
document.getElementById("tiempo-total").textContent = formatearTiempo(tiempoTotal);

let segundos = 0;

setInterval(() => {
    segundos++;
    document.getElementById("tiempo-uso").textContent = formatearTiempo(segundos);
    localStorage.setItem("tiempoTotal", tiempoTotal + segundos);
    document.getElementById("tiempo-total").textContent = formatearTiempo(tiempoTotal + segundos);
}, 1000);
</script>
<!-- FIN PERFIL -->

</body>
</html>
