# 1 a. Data tentang penyebab - penyebab kematian di Indonesia
https://docs.google.com/spreadsheets/d/1QU1NnZl2l6fleGar7VF62qK5azeQ7XpqF3k9RlHntmE/edit#gid=1142725067

# 1 b. Data tentang jumlah kematian di Indonesia berdasarkan tahun dan tipe
https://docs.google.com/spreadsheets/d/1QU1NnZl2l6fleGar7VF62qK5azeQ7XpqF3k9RlHntmE/edit#gid=2003969226

# 1 c. Jelaskan proses pembuatan tabulasi silang tersebut
Langkah -langkah pembuatan data tabulasi silang  tentang penyebab kematian di Indonesia
1. Membuka google spreadsheets "Data Kematian si Indonesia" https://docs.google.com/spreadsheets/d/1QU1NnZl2l6fleGar7VF62qK5azeQ7XpqF3k9RlHntmE/edit#gid=188966321 

2. Kemudian klik "Ekstensi" lalu pilih "Apps Script"

3. selanjutnya masukkan kode dibawah ini pada "Apps Script"
```javascript
function causeAndYear() {
  let ss = SpreadsheetApp.getActiveSpreadsheet();
  let sheet = ss.getSheetByName('Penyebab Kematian di Indonesia yang Dilaporkan - Clean'); // Ganti 'Sheet1' dengan nama lembar kerja yang sesuai
  
  if (!sheet) {
    Logger.log("Lembar kerja tidak ditemukan. Pastikan nama lembar kerja sudah benar.");
    return;
  }
  
  let data = sheet.getDataRange().getValues();

  // Get unique causes and years
  let causes = [];
  let years = [];
  for (let i = 1; i < data.length; i++) { // Mulai dari baris kedua karena ada header di baris pertama
    let cause = data[i][0];
    let year = data[i][2];
    if (causes.indexOf(cause) == -1) causes.push(cause);
    if (years.indexOf(year) == -1) years.push(year);
  }
  
  // Sort years in ascending order
  years.sort((a, b) => a - b);

  // Create new sheet
  let newSheet = ss.insertSheet('Tabulasi Kematian');
  newSheet.getRange('A1').setValue('Cause');
  for (let i = 0; i < years.length; i++) {
    newSheet.getRange(1, i + 2).setValue(years[i]);
  }

  // Calculate totals and add to sheet
  for (let i = 0; i < causes.length; i++) {
    let cause = causes[i];
    newSheet.getRange(i + 2, 1).setValue(cause);
    for (let j = 0; j < years.length; j++) {
      let year = years[j];
      let totalDeaths = 0;
      for (let k = 1; k < data.length; k++) { // Mulai dari baris kedua karena ada header di baris pertama
        if (data[k][0] == cause && data[k][2] == year) {
          totalDeaths += Number(data[k][4]);
        }
      }
      newSheet.getRange(i + 2, j + 2).setValue(totalDeaths);
    }
  }
}
```

4. klik simpan dan jalankan kode tersebut

5. Jika eksekusi berhasil dijalankan maka pada spreadsheet akan otomatis menambah sheet dengan nama "Kematian Berdasarkan Tahun dan Tipe"
