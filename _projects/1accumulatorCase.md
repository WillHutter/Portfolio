---
layout: project
title: Accumulator Case, Cornell FSAE
description: Accumulator case for Cornell FSAE ARG25
technologies: Statics, Sheet Metal, CAD
--- 
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

<style>
  #pdf-container { width: 100%; }
  .pdf-page-svg {
    display: block;
    width: 100%;
    height: auto;
    margin: 0 auto 16px auto;
  }
  #pdf-error { color: #b00020; white-space: pre-wrap; }
</style>

<div id="pdf-error"></div>
<div id="pdf-container"></div>

<script>
(async function () {
  // Worker
  pdfjsLib.GlobalWorkerOptions.workerSrc =
    "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

  const url = "{{ '/assets/images/Accumulator_Case.pdf' | relative_url }}";
  const container = document.getElementById("pdf-container");
  const errBox = document.getElementById("pdf-error");

  function showError(e) {
    console.error(e);
    errBox.textContent = "PDF render error:\n" + (e && e.message ? e.message : String(e));
  }

  try {
    const pdf = await pdfjsLib.getDocument({ url }).promise;
    container.innerHTML = "";

    for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
      const page = await pdf.getPage(pageNum);

      // Use scale=1 for SVG; we’ll scale with CSS to fit width
      const viewport = page.getViewport({ scale: 1 });

      // Build SVG
      const opList = await page.getOperatorList();
      const svgGfx = new pdfjsLib.SVGGraphics(page.commonObjs, page.objs);
      svgGfx.embedFonts = true;

      const svg = await svgGfx.getSVG(opList, viewport);

      // Make it responsive: fit container width, preserve aspect ratio
      svg.classList.add("pdf-page-svg");
      svg.setAttribute("viewBox", `0 0 ${viewport.width} ${viewport.height}`);
      svg.setAttribute("preserveAspectRatio", "xMinYMin meet");

      container.appendChild(svg);
    }
  } catch (e) {
    showError(e);
  }
})();
</script>
