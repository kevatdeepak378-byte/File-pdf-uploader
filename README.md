      font: 500 16px/1.5 system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, "Helvetica Neue", Arial;
    }

    .stars {
      width: 1px; height: 1px; position: fixed; inset: 0; margin: auto; background: white;
      box-shadow: 2vw 5vh 2px white, 10vw 8vh 2px white, 15vw 15vh 1px white,
        22vw 22vh 1px white, 28vw 12vh 2px white, 32vw 32vh 1px white,
        38vw 18vh 2px white, 42vw 35vh 1px white, 48vw 25vh 2px white,
        53vw 42vh 1px white, 58vw 15vh 2px white, 63vw 38vh 1px white,
        68vw 28vh 2px white, 73vw 45vh 1px white, 78vw 32vh 2px white,
        83vw 48vh 1px white, 88vw 20vh 2px white, 93vw 52vh 1px white,
        98vw 35vh 2px white, 5vw 60vh 1px white, 12vw 65vh 2px white,
        18vw 72vh 1px white, 25vw 78vh 2px white, 30vw 85vh 1px white;
      filter: drop-shadow(0 0 6px rgba(255,255,255,.3)); pointer-events: none;
    }
    .shooting-star {
      position: fixed; top: -10vh; left: -10vw;
      width: 2px; height: 2px; background: white; border-radius: 50%;
      box-shadow: 0 0 6px 2px rgba(255,255,255,.7);
      animation: shoot 6s linear infinite; opacity: .9;
    }
    .shooting-star:nth-child(2){ animation-delay: 1.5s; }
    .shooting-star:nth-child(3){ animation-delay: 3s; }
    .shooting-star:nth-child(4){ animation-delay: 4.5s; }
    .shooting-star:nth-child(5){ animation-delay: 6s; }
    .shooting-star:nth-child(6){ animation-delay: 7.5s; }
    @keyframes shoot {
      0% { transform: translate(-5vw,-5vh) scale(1); opacity: 0; }
      8% { opacity: 1; }
      50% { transform: translate(105vw, 65vh) scale(1.2); opacity: .9; }
      100% { transform: translate(150vw, 100vh) scale(.8); opacity: 0; }
    }

    .wrap { position: relative; z-index: 1; padding: 32px 20px 48px; max-width: 800px; margin: 0 auto; text-align:center; }
    header { margin-bottom: 40px; }
    .title { font-size: clamp(28px, 5vw, 40px); font-weight: 700; letter-spacing: 1px; color: var(--accent); }
    .subtitle { color: var(--muted); font-size: 1rem; margin-top: 8px; }

    .panel {
      background: var(--glass); border: 1px solid var(--stroke);
      backdrop-filter: blur(10px); border-radius: var(--radius);
      padding: 20px; box-shadow: 0 15px 40px rgba(0,0,0,.4);
      margin-bottom: 30px;
    }
    .uploader {
      display:flex; align-items:center; justify-content:center;
      gap:16px; flex-wrap:wrap; margin-bottom:20px;
    }
    input[type="file"]{
      appearance:none; border:1px dashed var(--stroke);
      border-radius: 12px; padding: 14px; color: var(--muted);
      background: rgba(255,255,255,.02); font-weight:600; cursor:pointer;
    }
    .btn {
      appearance:none; border:1px solid var(--stroke);
      background: var(--glass-strong); color: var(--accent);
      padding: 12px 18px; border-radius: 14px; cursor:pointer;
      font-weight:700; font-size:1rem; transition: .3s;
    }
    .btn:hover{ transform: translateY(-2px) scale(1.02); box-shadow: 0 8px 20px rgba(0,0,0,.35); }
    .btn:active{ transform: translateY(0) scale(1); }
    .btn.danger{ border-color: rgba(255,0,0,.3); color: var(--danger); }

    .table-wrap{ overflow:auto; border-radius: 16px; border:1px solid var(--stroke); }
    table{ width:100%; border-collapse: separate; border-spacing:0; font-weight:500; }
    thead th{
      background: rgba(143,211,255,0.15); color: var(--accent);
      text-align:left; padding: 14px; font-size:1rem; font-weight:700;
    }
    tbody td{
      padding: 14px; border-bottom: 1px solid rgba(255,255,255,.1);
      color: var(--text); transition: background .3s, color .3s;
    }
    tbody tr:hover td{ background: var(--highlight); color: #fff; }
    tbody tr:first-child td{ border-top-left-radius: 12px; border-top-right-radius: 12px; }
    tbody tr:last-child td{ border-bottom-left-radius: 12px; border-bottom-right-radius: 12px; }

    .toast {
      position: fixed; right: 16px; bottom: 16px;
      background: rgba(0,0,0,.65); color:#fff; padding: 10px 14px;
      border-radius: 12px; opacity:0; transform: translateY(8px);
      transition:.3s; pointer-events:none;
    }
    .toast.on{ opacity:1; transform: translateY(0); }
  </style>
</head>
<body>
  <div class="stars"></div>
  <div class="shooting-star"></div>
  <div class="shooting-star"></div>
  <div class="shooting-star"></div>
  <div class="shooting-star"></div>
  <div class="shooting-star"></div>

  <div class="wrap">
    <header>
      <div class="title">📄 PDF Upload & Viewer ✨</div>
      <div class="subtitle">🚀 Upload your PDFs and manage them below!</div>
    </header>

    <div class="panel">
      <div class="uploader">
        <input id="fileInput" type="file" accept="application/pdf" />
        <button class="btn" id="saveBtn">⬆️ Upload PDF</button>
      </div>
      <p class="hint">❌ Other formats (jpg, png, doc, csv) are not allowed!</p>
    </div>

    <div class="panel table-wrap">
      <table id="filesTable">
        <thead>
          <tr>
            <th>🔢 S. No.</th>
            <th>📂 File Name</th>
            <th>💾 Size</th>
            <th>🕒 Uploaded At</th>
            <th>⚡ Actions</th>
          </tr>
        </thead>
        <tbody id="filesBody">
          <tr class="empty-row">
            <td colspan="5" class="empty">No files yet. Upload a PDF to get started.</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="toast" id="toast"></div>

  <script>
    const DB_NAME = 'pdfDB'; 
    const STORE_NAME = 'files'; 
    let db;

    function openDB(){
      return new Promise((resolve,reject)=>{
        const req = indexedDB.open(DB_NAME,1);
        req.onupgradeneeded = e=>{
          const db=e.target.result;
          if(!db.objectStoreNames.contains(STORE_NAME))
            db.createObjectStore(STORE_NAME,{keyPath:'id',autoIncrement:true});
        };
        req.onsuccess = ()=>resolve(req.result);
        req.onerror = ()=>reject(req.error);
      });
    }

    function addFile(record){
      return new Promise((resolve,reject)=>{
        const tx=db.transaction([STORE_NAME],'readwrite');
        tx.objectStore(STORE_NAME).add(record).onsuccess = e=>resolve(e.target.result);
        tx.onabort=()=>reject(tx.error);
      });
    }

    function getAllFiles(){
      return new Promise((resolve,reject)=>{
        const tx=db.transaction([STORE_NAME],'readonly');
        const req=tx.objectStore(STORE_NAME).getAll();
        req.onsuccess=()=>resolve(req.result);
        req.onerror=()=>reject(req.error);
      });
    }

    function getFile(id){
      return new Promise((resolve,reject)=>{
        const tx=db.transaction([STORE_NAME],'readonly');
        const req=tx.objectStore(STORE_NAME).get(id);
        req.onsuccess=()=>resolve(req.result);
        req.onerror=()=>reject(req.error);
      });
    }

    function deleteFile(id){
      return new Promise((resolve,reject)=>{
        const tx=db.transaction([STORE_NAME],'readwrite');
        tx.objectStore(STORE_NAME).delete(id).onsuccess=()=>resolve();
        tx.onabort=()=>reject(tx.error);
      });
    }

    function showToast(msg, ok=false){
      const toast=document.getElementById('toast');
      toast.textContent=msg;
      toast.style.background=ok?'rgba(0,128,0,.7)':'rgba(128,0,0,.7)';
      toast.classList.add('on');
      setTimeout(()=>toast.classList.remove('on'),2000);
    }

    function renderTable(rows){
      const tbody=document.getElementById('filesBody');
      tbody.innerHTML='';
      if(!rows.length){
        tbody.innerHTML='<tr class="empty-row"><td colspan="5" class="empty">No files yet. Upload a PDF to get started.</td></tr>';
        return;
      }
      rows.forEach((r,idx)=>{
        const tr=document.createElement('tr');
        tr.innerHTML=`
          <td>${idx+1}</td>
          <td>${r.name}</td>
          <td>${(r.size/1024).toFixed(1)} KB</td>
          <td>${new Date(r.uploadedAt).toLocaleString()}</td>
          <td>
            <button data-action="view" data-id="${r.id}" class="btn">👁️ View</button>
            <button data-action="delete" data-id="${r.id}" class="btn danger">🗑️ Delete</button>
          </td>`;
        tbody.appendChild(tr);
      });
    }

    async function refresh(){
      const all=await getAllFiles();
      all.sort((a,b)=>b.uploadedAt-a.uploadedAt);
      renderTable(all);
    }

    document.getElementById('saveBtn').addEventListener('click',async()=>{
      const fileInput=document.getElementById('fileInput');
      const file=fileInput.files[0];
      if(!file){ showToast('Please choose a file'); return;}
      if(file.type!=='application/pdf'){ showToast('Wrong file format'); fileInput.value=''; return;}
      const rec={name:file.name,size:file.size,uploadedAt:Date.now(),blob:file};
      await addFile(rec);
      fileInput.value='';
      showToast('✅ Saved!');
      await refresh();
    });

    document.getElementById('filesBody').addEventListener('click',async e=>{
      const btn=e.target.closest('button[data-action]');
      if(!btn) return;
      const id=Number(btn.dataset.id);

      if(btn.dataset.action==='delete'){
        await deleteFile(id);
        await refresh();
      } 
      else if(btn.dataset.action==='view'){
        const rec = await getFile(id);
        if(!rec){ showToast('File not found! ❌'); return; }
        const pdfUrl = URL.createObjectURL(rec.blob);
        window.open(pdfUrl, '_blank'); // open in new tab
        showToast(`Opening: ${rec.name} 📄`, true);
      }
    });

    (async function(){ db=await openDB(); await refresh();})();
  </script>
</body>
</html>
