<!DOCTYPE html>
a.download = 'hostel_entries.csv';
document.body.appendChild(a); a.click(); a.remove();
});


// Reset form
resetBtn.addEventListener('click', ()=>{
if(!confirm('Clear form?')) return;
form.reset();
calcTotal();
renderSummary();
receiptBtn.disabled = true;
});


// Receipt
receiptBtn.addEventListener('click', ()=>{
const data = getFormData();
if(!data.fullName){ alert('Submit the form first.'); return; }
const receiptHTML = buildReceiptHTML(data);
const blob = new Blob([receiptHTML], {type: 'text/html'});
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = `Hostel_Receipt_${(data.fullName||'Student').replace(/\s+/g,'_')}.html`;
document.body.appendChild(a); a.click(); a.remove();
});


function buildReceiptHTML(d){
const toINR = n => Number(n||0).toLocaleString('en-IN');
return `<!DOCTYPE html><html><head><meta charset='utf-8'><title>Receipt</title>
<style>
body{font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial; background:#f6f7fb; color:#0f172a; margin:0;}
.sheet{max-width:800px;margin:24px auto;padding:24px}
.card{background:#fff;border:1px solid #e5e7eb;border-radius:14px;box-shadow:0 8px 20px rgba(0,0,0,.08);overflow:hidden}
.head{display:flex;align-items:center;justify-content:space-between;background:linear-gradient(135deg,#22c55e,#06b6d4);color:#06261c;padding:16px 18px}
h1{font-size:18px;margin:0}
.body{padding:18px}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.grid div{padding:8px 10px;background:#f9fafb;border:1px solid #eef2f7;border-radius:10px}
.grid strong{display:block;font-size:12px;color:#6b7280}
.total{margin-top:12px;padding:12px 14px;background:#ecfdf5;border:1px solid #a7f3d0;border-radius:10px}
.foot{padding:16px 18px;border-top:1px dashed #e5e7eb;display:flex;justify-content:space-between;font-size:12px;color:#6b7280}
.muted{color:#6b7280}
.btn{display:inline-block;margin-top:10px;padding:10px 12px;border-radius:10px;background:#0ea5e9;color:#fff;text-decoration:none}
@media print {.btn{display:none}}
</style></head>
<body><div class='sheet'><div class='card'>
<div class='head'><h1>Hostel Receipt</h1><div>${new Date().toLocaleString()}</div></div>
<div class='body'>
<div class='grid'>
<div><strong>Student</strong>${escapeHTML(d.fullName)} (${escapeHTML(d.studentId||'-')})</div>
<div><strong>Phone</strong>${escapeHTML(d.phone||'-')}</div>
<div><strong>Hostel / Room</strong>${escapeHTML(d.hostel||'-')} / ${escapeHTML(d.roomNo||'-')} (${escapeHTML(d.roomType||'-')})</div>
<div><strong>Stay</strong>${escapeHTML(d.checkIn||'-')} → ${escapeHTML(d.checkOut||'-')} (${d.nights} nights)</div>
<div><strong>Meal / AC</strong>${escapeHTML(d.mealPlan||'-')} / AC: ${escapeHTML(d.ac||'-')}</div>
<div><strong>Payment</strong>${escapeHTML(d.paymentMethod||'-')} ${escapeHTML(d.txnId? '('+d.txnId+')':'')}</div>
</div>
<div class='total'><strong>Amount</strong>
<div>Rate × Nights: ₹${toINR(d.rate)} × ${d.nights} = ₹${toINR(d.rate * d.nights)}</div>
<div>Security Deposit: ₹${toINR(d.deposit)}</div>
<div><b>Total: ₹${toINR(d.total)}</b></div>
</div>
<a class='btn' href='javascript:window.print()'>Print Receipt</a>
</div>
<div class='foot'><span class='muted'>Auto‑generated receipt</span><span>Signature: ___________</span></div>
</div></div></body></html>`;
}


function escapeHTML(str){
return (str==null?'' : String(str))
.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;')
.replaceAll('"','&quot;').replaceAll("'",'&#039;');
}


// Initialize
calcTotal();
renderSummary();


// Set sensible defaults for dates (today & tomorrow)
const today = new Date();
const tomorrow = new Date(Date.now() + 24*60*60*1000);
fields.checkIn.valueAsDate = today;
fields.checkOut.valueAsDate = tomorrow;
calcTotal();
</script>
</body>
</html>