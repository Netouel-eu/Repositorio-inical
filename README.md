(function() {
  // Proprietário: Netouel-eu
  const userId = document.querySelector('meta[name="user-id"]').content || "defaultUser";
  const storageKeyProg = `${userId}_prog`;
  const storageKeyMeta = `${userId}_meta`;

  const config = {
    progresso: parseInt(localStorage[storageKeyProg]) || 0,
    meta: parseInt(localStorage[storageKeyMeta]) || 200
  };

  const $ = id => document.getElementById(id);
  const painel = document.createElement('div');
  painel.innerHTML = `
    <div>Meta: <input id="sh_meta" type="number" value="${config.meta}"></div>
    <div style="background:#222;height:10px;"><div id="sh_bar" style="width:0%;height:100%;background:lime;"></div></div>
    <div>Progresso: <span id="sh_prog">${config.progresso}</span> / <span id="sh_meta_val">${config.meta}</span></div>
    <button id="sh_btn">INICIAR</button>
  `;
  document.body.appendChild(painel);

  let rodando = false;
  $('sh_btn').onclick = () => {
    rodando = !rodando;
    config.meta = parseInt($('sh_meta').value);
    $('sh_btn').innerText = rodando ? "PAUSAR" : "INICIAR";
    if (rodando) workflow();
  };

  function workflow() {
    if (!rodando || config.progresso >= config.meta) {
      console.log("OBG POR USAR!");
      return;
    }
    document.querySelectorAll('button').forEach(b => {
      if (b.innerText === "SEGUIR" && config.progresso < config.meta) {
        b.click();
        config.progresso++;
        localStorage[storageKeyProg] = config.progresso;
        $('sh_prog').innerText = config.progresso;
        $('sh_bar').style.width = (config.progresso / config.meta * 100) + "%";
      }
    });
    setTimeout(workflow, 3000);
  }
})();
