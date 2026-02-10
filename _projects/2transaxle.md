---
layout: project
title: Hydrostatic Transaxle Dissection
description: Disassembpled a hydrostatic transaxle and studdied working fluid mechaisms
technologies: Fluid mechanics, CAD, power tools
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

<style>
  #pdf-container { width: 100%; }
  .pdf-page {
    display: block;
    margin: 0 auto 16px auto;
  }
  #pdf-error { color: #b00020; white-space: pre-wrap; }
</style>

<div id="pdf-error"></div>
<div id="pdf-container"></div>

<script>
(function () {
  const VER = "3.11.174";

  pdfjsLib.GlobalWorkerOptions.workerSrc =
    `https://cdnjs.cloudflare.com/ajax/libs/pdf.js/${VER}/pdf.worker.min.js`;

  const url = "{{ '/assets/images/Transaxle.pdf' | relative_url }}";
  const container = document.getElementById("pdf-container");
  const errBox = document.getElementById("pdf-error");

  function showError(e) {
    console.error(e);
    errBox.textContent = "PDF render error:\n" + (e?.message || String(e));
  }

  async function renderAllPages(pdf) {
    container.innerHTML = "";

    // Use DPR, but cap it to avoid insane sizes on some displays
    const dpr = Math.min(window.devicePixelRatio || 1, 2);

    // Measure container in CSS pixels
    const containerWidth = Math.max(container.clientWidth, 1);

    for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
      const page = await pdf.getPage(pageNum);

      // Fit-to-width in CSS pixels
      const viewport1 = page.getViewport({ scale: 1 });
      const fitScale = containerWidth / viewport1.width;

      const viewport = page.getViewport({ scale: fitScale });

      // Canvas setup
      const canvas = document.createElement("canvas");
      canvas.className = "pdf-page";
      const ctx = canvas.getContext("2d", { alpha: false });

      // CSS size (what user sees) — explicit to avoid browser interpolation surprises
      canvas.style.width = Math.floor(viewport.width) + "px";
      canvas.style.height = Math.floor(viewport.height) + "px";

      // Backing store size (actual pixels) — higher for sharpness
      canvas.width = Math.floor(viewport.width * dpr);
      canvas.height = Math.floor(viewport.height * dpr);

      // Map CSS pixels to backing pixels
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

      container.appendChild(canvas);

      // Render
      await page.render({
        canvasContext: ctx,
        viewport: viewport
      }).promise;
    }
  }

  pdfjsLib.getDocument({ url }).promise
    .then(async (pdf) => {
      await renderAllPages(pdf);

      // Re-render on resize (debounced)
      let t = null;
      window.addEventListener("resize", () => {
        clearTimeout(t);
        t = setTimeout(() => renderAllPages(pdf).catch(showError), 150);
      });
    })
    .catch(showError);
})();
</script>