<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Galería</title>
<style>
  :root{
    --fondo:#0f1115;
    --superficie:#1a1d23;
    --texto:#e8eaed;
    --texto-suave:#9aa0a6;
    --acento:#4f8cff;
  }

  *{box-sizing:border-box;}

  body{
    margin:0;
    min-height:100vh;
    background:var(--fondo);
    color:var(--texto);
    font-family:system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    -webkit-font-smoothing:antialiased;
  }

  /* ---------- CABECERA ---------- */
  header{
    text-align:center;
    padding:clamp(40px,8vw,80px) 20px 10px;
  }
  h1{
    margin:0 0 12px;
    font-size:clamp(1.9rem,5vw,3.2rem);
    font-weight:700;
    letter-spacing:-0.03em;
    background:linear-gradient(90deg,#ffffff,#9ec1ff);
    -webkit-background-clip:text;
    background-clip:text;
    color:transparent;
  }
  header p{
    margin:0;
    color:var(--texto-suave);
    font-size:1rem;
  }

  /* ---------- CONTENEDOR ---------- */
  main{
    max-width:1200px;
    margin:0 auto;
    padding:0 clamp(16px,4vw,32px) 80px;
  }
  section{ margin-top:clamp(40px,6vw,64px); }

  h2{
    margin:0 0 18px;
    padding-left:12px;
    font-size:.85rem;
    font-weight:600;
    text-transform:uppercase;
    letter-spacing:.18em;
    color:var(--texto-suave);
    border-left:3px solid var(--acento);
  }

  /* ---------- VÍDEOS ---------- */
  .videos{
    display:grid;
    gap:24px;
    grid-template-columns:repeat(auto-fit,minmax(320px,1fr));
  }
  .video-card{
    background:var(--superficie);
    border:1px solid rgba(255,255,255,.06);
    border-radius:16px;
    overflow:hidden;
  }
  .video-card video{
    display:block;
    width:100%;
    height:auto;
    background:#000;
  }
  .video-card p{
    margin:0;
    padding:12px 16px;
    font-size:.9rem;
    color:var(--texto-suave);
  }

  /* ---------- GALERÍA DE FOTOS ---------- */
  .galeria{
    display:grid;
    gap:14px;
    grid-template-columns:repeat(auto-fill,minmax(200px,1fr));
  }
  .galeria figure{
    position:relative;
    margin:0;
    aspect-ratio:1/1;
    overflow:hidden;
    border-radius:14px;
    border:1px solid rgba(255,255,255,.06);
    background:var(--superficie);
    cursor:zoom-in;
  }
  .galeria figure:focus-visible{
    outline:2px solid var(--acento);
    outline-offset:2px;
  }
  .galeria img{
    display:block;
    width:100%;
    height:100%;
    object-fit:cover;
    transition:transform .45s cubic-bezier(.2,.7,.3,1);
  }
  .galeria figure:hover img,
  .galeria figure:focus-visible img{ transform:scale(1.07); }

  .galeria figcaption{
    position:absolute;
    inset:auto 0 0 0;
    padding:26px 12px 10px;
    font-size:.8rem;
    color:#fff;
    background:linear-gradient(to top,rgba(0,0,0,.85),transparent);
    opacity:0;
    transition:opacity .3s;
    pointer-events:none;
  }
  .galeria figure:hover figcaption,
  .galeria figure:focus-visible figcaption{ opacity:1; }

  /* Aviso cuando una imagen no se encuentra */
  .galeria figure.sin-imagen{
    display:flex;
    align-items:center;
    justify-content:center;
    padding:10px;
    text-align:center;
    cursor:default;
  }
  .galeria figure.sin-imagen span{
    font-size:.75rem;
    color:var(--texto-suave);
    line-height:1.4;
  }

  .vacio{
    grid-column:1/-1;
    margin:0;
    padding:24px;
    text-align:center;
    font-size:.9rem;
    color:var(--texto-suave);
    background:var(--superficie);
    border:1px dashed rgba(255,255,255,.12);
    border-radius:14px;
  }

  /* ---------- VISOR DE FOTOS (LIGHTBOX) ---------- */
  .lightbox{
    position:fixed;
    inset:0;
    z-index:100;
    display:none;
    align-items:center;
    justify-content:center;
    padding:clamp(12px,4vw,48px);
    background:rgba(5,6,8,.94);
    backdrop-filter:blur(4px);
  }
  .lightbox.abierto{ display:flex; }

  .lightbox img{
    max-width:92vw;
    max-height:86vh;
    border-radius:10px;
    box-shadow:0 20px 60px rgba(0,0,0,.6);
    user-select:none;
  }

  .lb-cerrar,
  .lb-flecha{
    position:absolute;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:0;
    color:#fff;
    background:rgba(255,255,255,.08);
    border:1px solid rgba(255,255,255,.15);
    border-radius:50%;
    cursor:pointer;
    transition:background .2s, transform .2s;
  }
  .lb-cerrar:hover,
  .lb-flecha:hover{ background:rgba(255,255,255,.22); }

  .lb-cerrar{
    top:18px; right:18px;
    width:44px; height:44px;
    font-size:18px;
  }
  .lb-flecha{
    top:50%;
    width:52px; height:52px;
    font-size:32px;
    line-height:1;
    transform:translateY(-50%);
  }
  .lb-flecha:hover{ transform:translateY(-50%) scale(1.08); }
  .lb-prev{ left:18px; }
  .lb-next{ right:18px; }

  .lb-contador{
    position:absolute;
    bottom:20px;
    left:50%;
    transform:translateX(-50%);
    font-size:.85rem;
    color:var(--texto-suave);
    letter-spacing:.05em;
  }

  .lightbox.solo .lb-flecha{ display:none; }

  @media (max-width:600px){
    .lb-flecha{ width:42px; height:42px; font-size:26px; }
    .lb-prev{ left:8px; }
    .lb-next{ right:8px; }
  }
</style>
</head>
<body>

<header>
  <h1 id="titulo"></h1>
  <p id="subtitulo"></p>
</header>

<main>
  <section>
    <h2>Vídeos</h2>
    <div id="videos" class="videos"></div>
  </section>

  <section>
    <h2>Fotos</h2>
    <div id="galeria" class="galeria"></div>
  </section>
</main>

<!-- Visor de fotos a pantalla completa -->
<div class="lightbox" id="lightbox" role="dialog" aria-modal="true">
  <button class="lb-cerrar" id="lbCerrar" aria-label="Cerrar">✕</button>
  <button class="lb-flecha lb-prev" id="lbPrev" aria-label="Foto anterior">‹</button>
  <img id="lbImagen" src="" alt="">
  <button class="lb-flecha lb-next" id="lbNext" aria-label="Foto siguiente">›</button>
  <div class="lb-contador" id="lbContador"></div>
</div>

<script>
/* ============================================================
   1) TÍTULO PERSONALIZABLE
   Cambia el texto entre comillas y listo.
   ============================================================ */
const TITULO    = "Mi Subransa";
const SUBTITULO = "el mejor lugar Para descansar";


/* ============================================================
   2) VÍDEOS
   Para añadir otro vídeo, copia un bloque { ... } y pégalo
   justo debajo, separado por una coma.

   src    -> ruta del archivo de vídeo   (ej: "videos/video2.mp4")
   poster -> imagen de portada opcional  (déjalo "" si no quieres)
   titulo -> texto que aparece debajo del vídeo
   ============================================================ */
const VIDEOS = [
  {
    src: "videos/video1.mp4",
    poster: "",
    titulo: "Vídeo 1"
  }

  // 👇👇 AÑADE MÁS VÍDEOS AQUÍ ABAJO 👇👇
  // ,
  // {
  //   src: "videos/video2.mp4",
  //   poster: "fotos/portada.jpg",
  //   titulo: "Vídeo 2"
  // }
];


/* ============================================================
   3) FOTOS
   Para añadir otra foto, copia una línea { ... } y pégala
   justo debajo, separada por una coma.

   src    -> ruta de la imagen  (ej: "fotos/foto2.jpg")
   titulo -> texto al pasar el ratón (opcional)
   ============================================================ */
const FOTOS = [
  { src: "fotos/foto1.jpg", titulo: "Foto 1" },
  { src: "fotos/foto2.jpg", titulo: "Foto 2" },
  { src: "fotos/foto3.jpg", titulo: "Foto 3" }

  // 👇👇 AÑADE MÁS FOTOS AQUÍ ABAJO 👇👇
  // ,
  // { src: "fotos/foto4.jpg", titulo: "Foto 4" },
  // { src: "fotos/foto5.jpg", titulo: "" }
];


/* ============================================================
   A PARTIR DE AQUÍ NO HACE FALTA TOCAR NADA
   ============================================================ */

document.addEventListener("DOMContentLoaded", () => {
  document.title = TITULO;
  document.getElementById("titulo").textContent = TITULO;
  document.getElementById("subtitulo").textContent = SUBTITULO;
  pintarVideos();
  pintarFotos();
});

/* ---------- VÍDEOS ---------- */
function pintarVideos(){
  const cont = document.getElementById("videos");
  cont.innerHTML = "";

  if (VIDEOS.length === 0){
    cont.innerHTML = '<p class="vacio">Todavía no hay vídeos. Añádelos en la lista <strong>VIDEOS</strong> del código.</p>';
    return;
  }

  VIDEOS.forEach(v => {
    const card = document.createElement("div");
    card.className = "video-card";

    const video = document.createElement("video");
    video.src = v.src;
    video.controls = true;
    video.preload = "metadata";
    video.playsInline = true;
    if (v.poster) video.poster = v.poster;

    card.appendChild(video);

    if (v.titulo){
      const p = document.createElement("p");
      p.textContent = v.titulo;
      card.appendChild(p);
    }

    cont.appendChild(card);
  });
}

/* ---------- FOTOS ---------- */
function pintarFotos(){
  const galeria = document.getElementById("galeria");
  galeria.innerHTML = "";

  if (FOTOS.length === 0){
    galeria.innerHTML = '<p class="vacio">Todavía no hay fotos. Añádelas en la lista <strong>FOTOS</strong> del código.</p>';
    return;
  }

  FOTOS.forEach((f, i) => {
    const fig = document.createElement("figure");
    fig.tabIndex = 0;
    fig.setAttribute("role", "button");
    fig.setAttribute("aria-label", f.titulo || "Ver foto " + (i + 1));

    const img = document.createElement("img");
    img.src = f.src;
    img.alt = f.titulo || "Foto " + (i + 1);
    img.loading = "lazy";

    // Si la imagen no existe, mostramos un aviso en su lugar
    img.addEventListener("error", () => {
      fig.classList.add("sin-imagen");
      fig.innerHTML = "<span>Imagen no encontrada<br>" + f.src + "</span>";
    });

    fig.appendChild(img);

    if (f.titulo){
      const cap = document.createElement("figcaption");
      cap.textContent = f.titulo;
      fig.appendChild(cap);
    }

    const abrir = () => {
      if (!fig.classList.contains("sin-imagen")) abrirLightbox(i);
    };
    fig.addEventListener("click", abrir);
    fig.addEventListener("keydown", e => {
      if (e.key === "Enter" || e.key === " "){ e.preventDefault(); abrir(); }
    });

    galeria.appendChild(fig);
  });
}

/* ---------- VISOR (LIGHTBOX) ---------- */
const lightbox   = document.getElementById("lightbox");
const lbImagen   = document.getElementById("lbImagen");
const lbContador = document.getElementById("lbContador");
let indiceActual = 0;

function abrirLightbox(i){
  if (!FOTOS[i]) return;
  indiceActual = i;
  pintarLightbox();
  lightbox.classList.add("abierto");
  lightbox.classList.toggle("solo", FOTOS.length < 2);
  document.body.style.overflow = "hidden";
}

function pintarLightbox(){
  const foto = FOTOS[indiceActual];
  lbImagen.src = foto.src;
  lbImagen.alt = foto.titulo || "Foto " + (indiceActual + 1);
  lbContador.textContent = (indiceActual + 1) + " / " + FOTOS.length;
}

function cerrarLightbox(){
  lightbox.classList.remove("abierto");
  document.body.style.overflow = "";
}

function moverLightbox(paso){
  indiceActual = (indiceActual + paso + FOTOS.length) % FOTOS.length;
  pintarLightbox();
}

document.getElementById("lbCerrar").addEventListener("click", cerrarLightbox);
document.getElementById("lbPrev").addEventListener("click", e => { e.stopPropagation(); moverLightbox(-1); });
document.getElementById("lbNext").addEventListener("click", e => { e.stopPropagation(); moverLightbox(1); });
lightbox.addEventListener("click", e => { if (e.target === lightbox) cerrarLightbox(); });

document.addEventListener("keydown", e => {
  if (!lightbox.classList.contains("abierto")) return;
  if (e.key === "Escape") cerrarLightbox();
  if (e.key === "ArrowLeft")  moverLightbox(-1);
  if (e.key === "ArrowRight") moverLightbox(1);
});
</script>
</body>
</html>
