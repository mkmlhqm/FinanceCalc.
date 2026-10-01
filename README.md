<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pengira Kewangan Bulanan</title>
    <!-- Import Library jsPDF untuk Cetak PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #f4f6f8;
            margin: 0;
            padding: 15px;
            color: #333;
        }
        .card {
            background: #ffffff;
            padding: 18px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            margin-bottom: 15px;
        }
        h2 {
            margin-top: 0;
            color: #1a202c;
            font-size: 1.1rem;
            border-bottom: 2px solid #edf2f7;
            padding-bottom: 8px;
        }
        .form-group {
            margin-bottom: 12px;
        }
        label {
            display: block;
            font-size: 0.85rem;
            color: #4a5568;
            margin-bottom: 4px;
            font-weight: 600;
        }
        input[type="number"], input[type="text"], input[type="month"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #cbd5e0;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 0.95rem;
            background-color: #fff;
        }
        
        /* Gaya Baris Penolakan Peratusan */
        .percent-item-box {
            background: #f7fafc;
            border: 1px solid #e2e8f0;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .percent-control-box {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-top: 8px;
        }
        .btn-step {
            background-color: #3182ce;
            color: white;
            border: none;
            width: 38px;
            height: 38px;
            border-radius: 6px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .btn-step:active {
            background-color: #2b6cb0;
        }
        .percent-val {
            font-weight: bold;
            color: #3182ce;
            min-width: 45px;
            text-align: center;
            font-size: 1rem;
        }

        .flex-row {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
            align-items: center;
        }
        .flex-row input[type="text"] { flex: 2; }
        .flex-row input[type="number"] { flex: 1.5; }
        .btn-remove {
            background-color: #e53e3e;
            color: white;
            border: none;
            padding: 10px 12px;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-add {
            background-color: #38a169;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 6px;
            font-weight: bold;
            width: 100%;
            cursor: pointer;
            margin-top: 5px;
        }
        .btn-submit {
            width: 100%;
            background-color: #3182ce;
            color: white;
            border: none;
            padding: 14px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
        }
        .btn-pdf {
            width: 100%;
            background-color: #dd6b20;
            color: white;
            border: none;
            padding: 14px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
        }
        .result-box {
            display: none;
        }
        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px dashed #e2e8f0;
            font-size: 0.95rem;
        }
        .result-subitem {
            font-size: 0.85rem;
            color: #718096;
            padding-left: 15px;
        }
        .result-total {
            font-weight: bold;
            font-size: 1.1rem;
            border-top: 2px solid #2d3748;
            border-bottom: none;
            margin-top: 10px;
            padding-top: 10px;
        }
        .status-badge {
            text-align: center;
            padding: 10px;
            border-radius: 6px;
            margin-top: 12px;
            font-weight: bold;
        }
        .status-surplus { background-color: #c6f6d5; color: #22543d; }
        .status-balanced { background-color: #e2e8f0; color: #2d3748; }
        .status-deficit { background-color: #fed7d7; color: #742a2a; }
    </style>
</head>
<body>

    <h1 style="text-align: center; font-size: 1.4rem; color: #2b6cb0;">Kalkulator Kewangan</h1>

    <!-- MAKLUMAT PERIBADI & BULAN -->
    <div class="card">
        <h2>Maklumat Peribadi & Tarikh</h2>
        <div class="form-group">
            <label>Nama Penuh</label>
            <input type="text" id="namaPengguna" placeholder="Masukkan nama anda">
        </div>
        <div class="flex-row">
            <div class="form-group" style="flex: 1; margin-bottom: 0;">
                <label>Umur (Tahun)</label>
                <input type="number" id="umurPengguna" placeholder="Contoh: 25">
            </div>
            <div class="form-group" style="flex: 1; margin-bottom: 0;">
                <label>Pilih Bulan & Tahun</label>
                <input type="month" id="bulanTahun">
            </div>
        </div>
    </div>

    <!-- 1. PENDAPATAN -->
    <div class="card">
        <h2>1. Pendapatan (RM)</h2>
        <div class="form-group">
            <label>Pendapatan Bersih (Gaji)</label>
            <input type="number" id="gaji" placeholder="0.00">
        </div>
        <div class="form-group">
            <label>Pendapatan Sampingan / Lain</label>
            <input type="number" id="sampingan" placeholder="0.00">
        </div>
    </div>

    <!-- PENOLAKAN PERATUSAN (URUSAN BEBAS & BOLEH TAMBAH) -->
    <div class="card">
        <h2>Penolakan Peratusan (Boleh Tambah Urusan)</h2>
        
        <div id="senaraiPeratus">
            <!-- Item Peratusan Pertama -->
            <div class="percent-item-box">
                <div class="flex-row" style="margin-bottom: 0;">
                    <input type="text" class="nama-peratus" placeholder="Nama urusan (Contoh: Zakat / Cukai / Tabung)">
                    <button type="button" class="btn-remove" onclick="padamBaris(this)">X</button>
                </div>
                <div class="percent-control-box">
                    <button type="button" class="btn-step" onclick="tukarPeratusItem(this, -1)">-</button>
                    <input type="range" class="kadar-peratus" min="0" max="100" value="0" style="flex:1;" oninput="kemaskiniPeratusItem(this)">
                    <button type="button" class="btn-step" onclick="tukarPeratusItem(this, 1)">+</button>
                    <span class="percent-val percent-label">0%</span>
                </div>
            </div>
        </div>

        <button class="btn-add" onclick="tambahBarisPeratus()">+ TAMBAH URUSAN PERATUSAN</button>
    </div>

    <!-- 2. SIMPANAN -->
    <div class="card">
        <h2>2. Simpanan & Pelaburan (RM)</h2>
        <div class="form-group">
            <label>Simpanan Kecemasan / ASB / Tabung Haji</label>
            <input type="number" id="simpanan" placeholder="0.00">
        </div>
    </div>

    <!-- 3. KOMITMEN TETAP -->
    <div class="card">
        <h2>3. Komitmen Tetap (RM)</h2>
        <div class="form-group">
            <label>Sewa / Pinjaman Rumah</label>
            <input type="number" id="rumah" placeholder="0.00">
        </div>
        <div class="form-group">
            <label>Pinjaman Kenderaan</label>
            <input type="number" id="kenderaan" placeholder="0.00">
        </div>
        <div class="form-group">
            <label>Bil-bil & Insurans/Takaful</label>
            <input type="number" id="bil" placeholder="0.00">
        </div>
    </div>

    <!-- 4. PERBELANJAAN FLEKSIBEL -->
    <div class="card">
        <h2>4. Perbelanjaan Fleksibel (Isi dan tambah)</h2>
        
        <div id="senaraiFleksibel">
            <div class="flex-row">
                <input type="text" class="kat-fleksibel" placeholder="Nama perbelanjaan bebas">
                <input type="number" class="val-fleksibel" placeholder="RM 0.00">
                <button type="button" class="btn-remove" onclick="padamBaris(this)">X</button>
            </div>
        </div>

        <button class="btn-add" onclick="tambahBarisFleksibel()">+ TAMBAH PERBELANJAAN</button>
    </div>

    <button class="btn-submit" onclick="kiraKewangan()">KIRA RINGKASAN</button>

    <!-- LAPORAN / KEPUTUSAN -->
    <div class="card result-box" id="keputusanBox">
        <h2 id="tajukLaporan">Laporan Kewangan Bulanan</h2>
        
        <div class="result-item" style="font-weight: 600;">
            <span>Nama: <span id="resNama" style="color: #3182ce;">-</span></span>
            <span>Umur: <span id="resUmur" style="color: #3182ce;">-</span></span>
        </div>

        <div class="result-item">
            <span>Jumlah Pendapatan:</span>
            <span id="resPendapatan">RM 0.00</span>
        </div>
        
        <!-- Pecahan Penolakan Peratusan -->
        <div id="senaraiPecahanPeratus"></div>

        <div class="result-item">
            <span>Simpanan & Pelaburan:</span>
            <span id="resSimpanan">RM 0.00</span>
        </div>
        <div class="result-item">
            <span>Perbelanjaan Tetap:</span>
            <span id="resTetap">RM 0.00</span>
        </div>
        
        <div class="result-item" style="border-bottom: none; font-weight: 600;">
            <span>Perbelanjaan Fleksibel:</span>
            <span id="resFleksibel">RM 0.00</span>
        </div>
        <div id="senaraiPecahanFleksibel"></div>

        <div class="result-item result-total">
            <span>Jumlah Perbelanjaan:</span>
            <span id="resTotalBelanja">RM 0.00</span>
        </div>
        <div class="result-item result-total" style="color: #2b6cb0;">
            <span>BAKI / LEBIHAN:</span>
            <span id="resBaki">RM 0.00</span>
        </div>

        <div id="statusBadge" class="status-badge"></div>

        <!-- BUTANG MUAT TURUN PDF -->
        <button class="btn-pdf" onclick="muatTurunPDF()">MUAT TURUN STATEMENT (PDF)</button>
    </div>

    <script>
        var dataLaporan = {};

        window.onload = function() {
            var today = new Date();
            var month = ('0' + (today.getMonth() + 1)).slice(-2);
            var year = today.getFullYear();
            document.getElementById('bulanTahun').value = year + '-' + month;
        };

        function kemaskiniPeratusItem(slider) {
            var box = slider.closest('.percent-item-box');
            box.querySelector('.percent-label').innerText = slider.value + '%';
        }

        function tukarPeratusItem(btn, delta) {
            var box = btn.closest('.percent-item-box');
            var slider = box.querySelector('.kadar-peratus');
            var nilaiSemasa = parseInt(slider.value) || 0;
            var nilaiBaru = nilaiSemasa + delta;

            if (nilaiBaru >= 0 && nilaiBaru <= 100) {
                slider.value = nilaiBaru;
                kemaskiniPeratusItem(slider);
            }
        }

        function tambahBarisPeratus() {
            var container = document.getElementById('senaraiPeratus');
            var div = document.createElement('div');
            div.className = 'percent-item-box';
            div.innerHTML = `
                <div class="flex-row" style="margin-bottom: 0;">
                    <input type="text" class="nama-peratus" placeholder="Nama urusan (Contoh: Zakat / Cukai)">
                    <button type="button" class="btn-remove" onclick="padamBaris(this)">X</button>
                </div>
                <div class="percent-control-box">
                    <button type="button" class="btn-step" onclick="tukarPeratusItem(this, -1)">-</button>
                    <input type="range" class="kadar-peratus" min="0" max="100" value="0" style="flex:1;" oninput="kemaskiniPeratusItem(this)">
                    <button type="button" class="btn-step" onclick="tukarPeratusItem(this, 1)">+</button>
                    <span class="percent-val percent-label">0%</span>
                </div>
            `;
            container.appendChild(div);
        }

        function val(id) {
            return parseFloat(document.getElementById(id).value) || 0;
        }

        function formatRM(amount) {
            return 'RM ' + amount.toFixed(2);
        }

        function formatBulan(inputMonth) {
            if (!inputMonth) return '';
            var parts = inputMonth.split('-');
            var tarikh = new Date(parts[0], parts[1] - 1);
            return tarikh.toLocaleString('ms-MY', { month: 'long', year: 'numeric' });
        }

        function tambahBarisFleksibel() {
            var container = document.getElementById('senaraiFleksibel');
            var div = document.createElement('div');
            div.className = 'flex-row';
            div.innerHTML = `
                <input type="text" class="kat-fleksibel" placeholder="Nama perbelanjaan bebas">
                <input type="number" class="val-fleksibel" placeholder="RM 0.00">
                <button type="button" class="btn-remove" onclick="padamBaris(this)">X</button>
            `;
            container.appendChild(div);
        }

        function padamBaris(btn) {
            var parent = btn.parentElement;
            if(parent.classList.contains('percent-item-box') || parent.classList.contains('flex-row')) {
                parent.remove();
            } else {
                btn.closest('.percent-item-box').remove();
            }
        }

        function kiraKewangan() {
            var nama = document.getElementById('namaPengguna').value.trim() || '-';
            var umur = document.getElementById('umurPengguna').value.trim() || '-';
            var bulanInput = document.getElementById('bulanTahun').value;
            var teksBulan = formatBulan(bulanInput);
            
            var pendapatan = val('gaji') + val('sampingan');

            // Pengiraan Semua Urusan Penolakan Peratusan
            var senaraiNamaPeratus = document.querySelectorAll('.nama-peratus');
            var senaraiKadarPeratus = document.querySelectorAll('.kadar-peratus');
            var jumlahNilaiPenolakanPeratus = 0;
            var htmlPecahanPeratus = '';
            var arrayPecahanPeratus = [];

            for (var p = 0; p < senaraiNamaPeratus.length; p++) {
                var namaP = senaraiNamaPeratus[p].value.trim() || 'Penolakan ' + (p + 1);
                var peratusP = parseFloat(senaraiKadarPeratus[p].value) || 0;
                
                if (peratusP > 0) {
                    var nilaiP = (pendapatan * peratusP) / 100;
                    jumlahNilaiPenolakanPeratus += nilaiP;
                    
                    htmlPecahanPeratus += `<div class="result-item result-subitem" style="color: #c53030;">
                        <span>• (-) ${namaP} (${peratusP}%)</span>
                        <span>${formatRM(nilaiP)}</span>
                    </div>`;
                    
                    arrayPecahanPeratus.push({ nama: namaP, peratus: peratusP, nilai: nilaiP });
                }
            }

            var simpanan = val('simpanan');
            var tetap = val('rumah') + val('kenderaan') + val('bil');

            // Pengiraan Fleksibel
            var kats = document.querySelectorAll('.kat-fleksibel');
            var vals = document.querySelectorAll('.val-fleksibel');
            var jumlahFleksibel = 0;
            var htmlPecahanFleksibel = '';
            var senaraiPecahanArray = [];

            for (var i = 0; i < kats.length; i++) {
                var namaKat = kats[i].value.trim() || 'Perbelanjaan ' + (i + 1);
                var nilaiKat = parseFloat(vals[i].value) || 0;
                
                if (nilaiKat > 0) {
                    jumlahFleksibel += nilaiKat;
                    htmlPecahanFleksibel += `<div class="result-item result-subitem">
                        <span>• ${namaKat}</span>
                        <span>${formatRM(nilaiKat)}</span>
                    </div>`;
                    senaraiPecahanArray.push({ nama: namaKat, nilai: nilaiKat });
                }
            }

            var totalBelanja = jumlahNilaiPenolakanPeratus + simpanan + tetap + jumlahFleksibel;
            var baki = pendapatan - totalBelanja;
            var statusTeks = "";

            if (baki > 0) statusTeks = "SURPLUS (Lebihan Wang)";
            else if (baki === 0) statusTeks = "SEIMBANG (Cukup-Cukup Makan)";
            else statusTeks = "DEFISIT (Belanja Lebih Dari Gaji)";

            // Simpan data untuk PDF
            dataLaporan = {
                nama: nama,
                umur: umur,
                bulan: teksBulan || 'Bulanan',
                pendapatan: pendapatan,
                pecahanPeratus: arrayPecahanPeratus,
                jumlahPenolakanPeratus: jumlahNilaiPenolakanPeratus,
                simpanan: simpanan,
                tetap: tetap,
                fleksibel: jumlahFleksibel,
                pecahanFleksibel: senaraiPecahanArray,
                totalBelanja: totalBelanja,
                baki: baki,
                status: statusTeks
            };

            // Kemas Kini Paparan Skrin
            document.getElementById('tajukLaporan').innerText = teksBulan ? 'Laporan Kewangan: ' + teksBulan : 'Laporan Kewangan Bulanan';
            document.getElementById('resNama').innerText = nama;
            document.getElementById('resUmur').innerText = umur !== '-' ? umur + ' Tahun' : '-';
            document.getElementById('resPendapatan').innerText = formatRM(pendapatan);
            document.getElementById('senaraiPecahanPeratus').innerHTML = htmlPecahanPeratus;
            document.getElementById('resSimpanan').innerText = formatRM(simpanan);
            document.getElementById('resTetap').innerText = formatRM(tetap);
            document.getElementById('resFleksibel').innerText = formatRM(jumlahFleksibel);
            document.getElementById('senaraiPecahanFleksibel').innerHTML = htmlPecahanFleksibel;
            document.getElementById('resTotalBelanja').innerText = formatRM(totalBelanja);
            document.getElementById('resBaki').innerText = formatRM(baki);

            var badge = document.getElementById('statusBadge');
            badge.innerText = "Status: " + statusTeks;
            if (baki > 0) badge.className = "status-badge status-surplus";
            else if (baki === 0) badge.className = "status-badge status-balanced";
            else badge.className = "status-badge status-deficit";

            document.getElementById('keputusanBox').style.display = 'block';
            document.getElementById('keputusanBox').scrollIntoView({ behavior: 'smooth' });
        }

        // FUNGSI MUAT TURUN PDF
        function muatTurunPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            let y = 20;

            doc.setFontSize(18);
            doc.setTextColor(43, 108, 176);
            doc.text("PENYATA KEWANGAN BULANAN", 105, y, { align: "center" });
            y += 8;

            doc.setFontSize(11);
            doc.setTextColor(100);
            doc.text(`Nama: ${dataLaporan.nama}  |  Umur: ${dataLaporan.umur}  |  Bulan: ${dataLaporan.bulan}`, 105, y, { align: "center" });
            y += 12;

            doc.setLineWidth(0.5);
            doc.setDrawColor(200);
            doc.line(15, y, 195, y);
            y += 10;

            doc.setFontSize(12);
            doc.setTextColor(0);
            
            doc.text("Jumlah Pendapatan:", 15, y);
            doc.text(formatRM(dataLaporan.pendapatan), 195, y, { align: "right" });
            y += 8;

            // Cetak Penolakan Peratusan jika ada
            if (dataLaporan.pecahanPeratus.length > 0) {
                doc.setTextColor(197, 48, 48);
                dataLaporan.pecahanPeratus.forEach(item => {
                    doc.text(`(-) ${item.nama} (${item.peratus}%):`, 15, y);
                    doc.text(formatRM(item.nilai), 195, y, { align: "right" });
                    y += 8;
                });
                doc.setTextColor(0);
            }

            doc.text("(-) Simpanan & Pelaburan:", 15, y);
            doc.text(formatRM(dataLaporan.simpanan), 195, y, { align: "right" });
            y += 8;

            doc.text("(-) Perbelanjaan Tetap:", 15, y);
            doc.text(formatRM(dataLaporan.tetap), 195, y, { align: "right" });
            y += 8;

            doc.text("(-) Perbelanjaan Fleksibel:", 15, y);
            doc.text(formatRM(dataLaporan.fleksibel), 195, y, { align: "right" });
            y += 6;

            doc.setFontSize(10);
            doc.setTextColor(100);
            dataLaporan.pecahanFleksibel.forEach(item => {
                doc.text(`   • ${item.nama}`, 20, y);
                doc.text(formatRM(item.nilai), 190, y, { align: "right" });
                y += 6;
            });

            y += 4;
            doc.line(15, y, 195, y);
            y += 8;

            doc.setFontSize(12);
            doc.setTextColor(0);
            doc.setFont(undefined, 'bold');
            doc.text("JUMLAH PERBELANJAAN:", 15, y);
            doc.text(formatRM(dataLaporan.totalBelanja), 195, y, { align: "right" });
            y += 10;

            doc.setTextColor(43, 108, 176);
            doc.text("BAKI / LEBIHAN:", 15, y);
            doc.text(formatRM(dataLaporan.baki), 195, y, { align: "right" });
            y += 12;

            doc.setFontSize(11);
            doc.setTextColor(0);
            doc.text(`Status Kewangan: ${dataLaporan.status}`, 15, y);

            var namaClean = dataLaporan.nama !== '-' ? dataLaporan.nama.replace(/ /g, '_') : 'Pengguna';
            var namaFail = `Penyata_Kewangan_${namaClean}_${dataLaporan.bulan.replace(/ /g, '_')}.pdf`;
            doc.save(namaFail);
        }
    </script>
</body>
</html>
