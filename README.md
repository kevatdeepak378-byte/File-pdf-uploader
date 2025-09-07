  
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
  <    }

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
