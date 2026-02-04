---
layout: project
title: Accumulator Design, Cornell FSAE
description: Laser welded pouch cell battery pack for Cornell FSAE ARG26
technologies: MATLAB, Laser Welding, ANSYS, CAD, High Voltage, Pouch Cells
--- 


<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

<style>
  #pdf-container { width: 100%; }
  .pdf-page { display: block; margin: 0 auto 16px auto; max-width: 100%; height: auto; }
  #pdf-error { color: #b00020; white-space: pre-wrap; }
</style>

<div id="pdf-error"></div>
<div id="pdf-container"></div>

<script>
  (function () {
    // IMPORTANT: configure worker (common cause of "nothing renders")
    pdfjsLib.GlobalWorkerOptions.workerSrc =
      "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

    const url = "{{ '/assets/images/Accumulator.pdf' | relative_url }}";

    const container = document.getElementById("pdf-container");
    const errBox = document.getElementById("pdf-error");

    function showError(e) {
      console.error(e);
      errBox.textContent = "PDF render error:\n" + (e && e.message ? e.message : String(e));
    }

    pdfjsLib.getDocument({ url }).promise
      .then(async (pdf) => {
        for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
          const page = await pdf.getPage(pageNum);

          // Scale to fit container width nicely
          const viewport0 = page.getViewport({ scale: 1 });
          const targetWidth = Math.min(container.clientWidth || 900, 1000);
          const scale = targetWidth / viewport0.width;

          const viewport = page.getViewport({ scale });

          const canvas = document.createElement("canvas");
          canvas.className = "pdf-page";
          const context = canvas.getContext("2d");

          canvas.width = Math.floor(viewport.width);
          canvas.height = Math.floor(viewport.height);

          container.appendChild(canvas);

          await page.render({ canvasContext: context, viewport }).promise;
        }
      })
      .catch(showError);
  })();
</script>
