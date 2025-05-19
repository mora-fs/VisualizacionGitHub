<script>
  import JavaScript from './JavaScript.svelte';
  import CSS from './CSS.svelte';
  import C from './C.svelte';
  import Svelte from './Svelte.svelte';
  import Python from './Py.svelte';
  import Html from './Html.svelte';
  import Cmasmas from './Cmasmas.svelte';
  import StarIcon from './StarIcon.svelte';

  import * as d3 from "d3";
  import todos from './todos.json'

  const lenguajeIcons = [
    { nombre: 'javascript', componente: JavaScript },
    { nombre: 'c++', componente: Cmasmas },
    { nombre: 'css', componente: CSS },
    { nombre: 'c', componente: C },
    { nombre: 'svelte', componente: Svelte },
    { nombre: 'python', componente: Python },
    { nombre: 'html', componente: Html }
  ];

  function getIcons(lenguajes) {
    if (!Array.isArray(lenguajes)) return [];
    return lenguajeIcons
      .filter(icon => lenguajes.includes(icon.nombre))
  }

  /*  
  Combinaciones:
  JavaScript, CSS, svelte   -> 'CSS', 'JavaScript', 'Svelte'
  Html  -> 'Cmasmas'
  python, CSS, Html  -> 'Python', 'JavaScript', 'Cmasmas'
  python, Html  -> 'Python', 'Cmasmas'
  python  -> 'Python',
  python, c++  -> 'Python', 'Html'
  python, c  -> 'Python', 'C'
  c  -> 'C'

  'JavaScript',   JavaScript    
  'DoubleDiamondIcon', 
  'CSS',         CSS 
  'C',              
  'Svelte',        Svelte
  'Python',          
  'SemiArcsIcon',        
  'Html',   Html
  'Cmasmas'   c++

  'WavyLineIcon',   peso   
  'StarIcon',       commit   
  */

  const iconLayout = {
    "javascript":        { x: '45%', y: '0%',    zIndex: 2, scale: 4},
    /*DoubleDiamondIcon: { x: '15%',  y: '50%',  zIndex: 1, scale: 1.5 },*/
    "css":               { x: '30%',  y: '75%',  zIndex: 3, scale: 5 },
    "c":                 { x: '50%',  y: '50%',  zIndex: 2, scale: 6},
    "svelte":        { x: '80%',  y: '95%',   zIndex: 0, scale: 8.2 },
    "python":            { x: '34%',  y: '50%',  zIndex: 0, scale: 7 },
    /*SemiArcsIcon:      { x: '90%',  y: '85%',  zIndex: 2, scale: 1.5 },  // big one!*/
    "html":               { x: '90%',   y: '70%',  zIndex: 2, scale: 6 },
    "c++":          { x: '40%',   y: '40%',   zIndex: 1, scale: 7 },
    "star":          { x: '85%',  y: '15%',  zIndex: 3, scale: 1.5 }
  }; 

  const baseIconSize = 50;  // for example, 50px base size
   
</script>

<main class="page">
  <h1 class="title">Repositorios GitHub</h1>
  <div class="cajas">
    {#each todos as t}
      <div class="caja">
        <div class="borde-extra" style= "border-color: {t.peso>=5 ? '#4B86AA' : '#000000'};">
        <div class="borde-extra" style= "border-color: {t.peso>=5 ? '#669BBC' : '#000000'};">
        <div class="borde-extra" style= "border-color: {t.peso>=5 ? '#8EB5CD' : '#000000'}; padding:5px;">
          <div class="colab-box-BIG">
            <div class="icon-layer-BIG">
              {#each getIcons(t.lenguajes) as icon}
                      {#if iconLayout[icon.nombre]}
                        <svelte:component
                          this={icon.componente}
                          size={baseIconSize * iconLayout[icon.nombre].scale}
                          class="icon-item-BIG"
                          style={`position: absolute;
                                  left: ${iconLayout[icon.nombre].x};
                                  top: ${iconLayout[icon.nombre].y};
                                  transform: translate(-50%, -50%);
                                  z-index: ${iconLayout[icon.nombre].zIndex ?? 0};
                                  pointer-events: none;`}
                        />
                      {/if}
              {/each}
              <svelte:component
                this={StarIcon}
                size={baseIconSize * iconLayout["star"].scale * t.commits*0.1}
                class="icon-item-BIG"
                style={`position: absolute;
                        left: ${iconLayout["star"].x};
                        top: ${iconLayout["star"].y};
                        transform: translate(-50%, -50%);
                        z-index: ${iconLayout["star"].zIndex ?? 0};
                        pointer-events: none;`}
              />
            </div>
          </div>
        </div>
        </div>
        </div>
        <div class="nombre_fecha">
          <p class="nombre-repo">{t.nombre}</p>
          <p class="fecha-repo">{t.fecha}</p>
        </div>
      </div>
    {/each}
  </div>

  <div class="guia">
    <h2 class="leyenda-titulo">Leyenda de íconos</h2>
    <div class="leyenda">
      <p class="leyenda-subtitulo">Métricas: </p>
      <div class="leyenda1">
        <div>
          <div class="borde-extra" style= "border-color:'#4B86AA';">
            <div class="borde-extra" style= "border-color:'#669BBC';">
              <div class="borde-extra" style= "border-color:'#8EB5CD';">
                <div class="borde-extra borde-leyenda" style= "border-color: '#AAC7DA';">
                </div>
              </div>
            </div>
          </div> <span>Peso</span>
        </div>
        <div><StarIcon size={100}/> <span>Cantidad de commits</span></div>
      </div>
      <p class="leyenda-subtitulo">Lenguajes usados: </p>
      <div class="leyenda2">
        <div><Html size={100}/> <span>HTML</span></div>
        <div><Python size={100}/> <span>Python</span></div>
        <div><JavaScript size={100}/> <span>JavaScript</span></div>
        <div><Cmasmas size={100}/> <span>C++</span></div>
        <div><C size={100}/> <span>C</span></div>
        <div><Svelte size={100}/> <span>Svelte</span></div>
        <div><CSS size={100}/> <span>CSS</span></div>
      </div>
    </div>
  </div>
</main>
<footer class="footer">
  <a href="https://github.com/Sofiik1/Visual" target="_blank">Repositorio en GitHub</a>
  <a href="https://www.linkedin.com/in/sofia-kutter" target="_blank">Sofía Kutter</a>
  <a href="https://www.linkedin.com/in/ezequiel-mautner" target="_blank">Ezequiel Mautner</a>
</footer>

<style>
  
  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
  }

</style>
