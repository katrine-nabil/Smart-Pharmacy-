# Smart-Pharmacy-
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>الصيدلية الذكية</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: 'Arial', sans-serif;
      background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), 
                  url('https://i.imgur.com/pharmacy-bg.jpg') no-repeat center center fixed;
      background-size: cover;
      min-height: 100vh;
      color: white;
      text-align: center;
    }
    
    .container {
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    
    .header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 20px;
      margin-bottom: 30px;
      padding: 15px;
    }
    
    .header img {
      width: 80px;
      height: 80px;
      object-fit: contain;
      filter: drop-shadow(0 0 10px #3498db);
    }
    
    h1 {
      color: white;
      text-shadow: 0 2px 4px rgba(0,0,0,0.5);
      font-size: 2rem;
    }
    
    .drug-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 15px;
      margin: 30px auto;
    }
    
    .drug {
      background: rgba(52, 152, 219, 0.8);
      color: white;
      padding: 15px;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      border: 2px solid rgba(255,255,255,0.3);
    }
    
    .drug:hover {
      background: rgba(41, 128, 185, 0.9);
      transform: translateY(-5px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.2);
    }
    
    #status {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      padding: 15px 30px;
      border-radius: 50px;
      max-width: 90%;
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
      animation: fadeIn 0.5s ease-out;
      display: none;
      z-index: 100;
    }
    
    .search-container {
      margin: 20px auto;
      max-width: 500px;
    }
    
    #search {
      padding: 12px 20px;
      width: 100%;
      border-radius: 50px;
      border: none;
      background: rgba(255,255,255,0.9);
      font-size: 16px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      text-align: center;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; bottom: -50px; }
      to { opacity: 1; bottom: 20px; }
    }
    
    @media (max-width: 768px) {
      .header {
        flex-direction: column;
      }
      
      .drug-grid {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <img src="https://i.imgur.com/pharmacy-logo.png" alt="شعار الصيدلية">
      <h1>نظام الصيدلية الذكية</h1>
    </div>
    
    <div class="search-container">
      <input type="text" id="search" placeholder="ابحث عن الدواء..." oninput="filterDrugs()">
    </div>
    
    <div class="drug-grid" id="drug-list">
      <!-- قائمة الأدوية -->
      <div class="drug" onclick="dispense('باراسيتامول')">باراسيتامول</div>
      <div class="drug" onclick="dispense('بنادول')">بنادول</div>
      <div class="drug" onclick="dispense('أوجمنتين')">أوجمنتين</div>
      <div class="drug" onclick="dispense('كتافلام')">كتافلام</div>
      <div class="drug" onclick="dispense('روفيناك')">روفيناك</div>
      <div class="drug" onclick="dispense('أدول')">أدول</div>
      <div class="drug" onclick="dispense('إيبوبروفين')">إيبوبروفين</div>
      <div class="drug" onclick="dispense('ديكلوفيناك')">ديكلوفيناك</div>
      <div class="drug" onclick="dispense('كونجستال')">كونجستال</div>
      <div class="drug" onclick="dispense('أوتريفين')">أوتريفين</div>
      <div class="drug" onclick="dispense('فولتارين')">فولتارين</div>
      <div class="drug" onclick="dispense('زيرتك')">زيرتك</div>
      <div class="drug" onclick="dispense('سيتريزين')">سيتريزين</div>
      <div class="drug" onclick="dispense('لورين')">لورين</div>
      <div class="drug" onclick="dispense('كلافوكس')">كلافوكس</div>
      <div class="drug" onclick="dispense('تاميفلو')">تاميفلو</div>
      <div class="drug" onclick="dispense('رابيدوس')">رابيدوس</div>
      <div class="drug" onclick="dispense('نيوروتون')">نيوروتون</div>
      <div class="drug" onclick="dispense('كارديكور')">كارديكور</div>
      <div class="drug" onclick="dispense('نوفالدول')">نوفالدول</div>
      <div class="drug" onclick="dispense('سيبروفلوكساسين')">سيبروفلوكساسين</div>
      <div class="drug" onclick="dispense('كلاريثروميسين')">كلاريثروميسين</div>
      <div class="drug" onclick="dispense('دوكسيسايكلين')">دوكسيسايكلين</div>
      <div class="drug" onclick="dispense('أموكسيسيللين')">أموكسيسيللين</div>
    </div>
  </div>
  
  <div id="status"></div>
  
  <script>
    // دالة لعرض الرسائل
    function showStatus(message, color = '#3498db', duration = 3000) {
      const statusElement = document.getElementById('status');
      statusElement.style.display = 'block';
      statusElement.textContent = message;
      statusElement.style.backgroundColor = color;
      
      setTimeout(() => {
        statusElement.style.display = 'none';
      }, duration);
    }

    // التحقق من الاتصال عند تحميل الصفحة
    async function checkConnection() {
      try {
        const response = await fetch('http://192.168.4.1/', {
          mode: 'no-cors',
          cache: 'no-store'
        });
        showStatus('✓ متصل بالصيدلية الذكية', '#2ecc71', 2000);
      } catch (error) {
        showStatus('⚠️ تأكد من الاتصال بشبكة SmartPharmacy', '#e74c3c', 5000);
        setTimeout(checkConnection, 3000); // إعادة المحاولة كل 3 ثواني
      }
    }

    // دالة صرف الدواء
    async function dispense(drugName) {
      showStatus(`جارٍ صرف ${drugName}...`, '#f39c12');
      
      try {
        const response = await fetch(
          `http://192.168.4.1/dispense?drug=${encodeURIComponent(drugName)}`, 
          { mode: 'no-cors' }
        );
        
        showStatus(`✓ تم صرف ${drugName}`, '#2ecc71');
      } catch (error) {
        showStatus('✗ فشل في الصرف. تأكد من تشغيل الجهاز', '#e74c3c');
      }
    }

    // دالة البحث
    function filterDrugs() {
      const searchTerm = document.getElementById('search').value.toLowerCase();
      document.querySelectorAll('.drug').forEach(drug => {
        const drugName = drug.textContent.toLowerCase();
        drug.style.display = drugName.includes(searchTerm) ? 'block' : 'none';
      });
    }

    // بدء التشغيل
    window.addEventListener('load', () => {
      checkConnection();
      // اختبار اتصال كل 10 ثواني
      setInterval(checkConnection, 10000);
    });
  </script>
</body>
</html><!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>الصيدلية الذكية</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: 'Arial', sans-serif;
      background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), 
                  url('https://i.imgur.com/pharmacy-bg.jpg') no-repeat center center fixed;
      background-size: cover;
      min-height: 100vh;
      color: white;
      text-align: center;
    }
    
    .container {
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    
    .header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 20px;
      margin-bottom: 30px;
      padding: 15px;
    }
    
    .header img {
      width: 80px;
      height: 80px;
      object-fit: contain;
      filter: drop-shadow(0 0 10px #3498db);
    }
    
    h1 {
      color: white;
      text-shadow: 0 2px 4px rgba(0,0,0,0.5);
      font-size: 2rem;
    }
    
    .drug-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 15px;
      margin: 30px auto;
    }
    
    .drug {
      background: rgba(52, 152, 219, 0.8);
      color: white;
      padding: 15px;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      border: 2px solid rgba(255,255,255,0.3);
    }
    
    .drug:hover {
      background: rgba(41, 128, 185, 0.9);
      transform: translateY(-5px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.2);
    }
    
    #status {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      padding: 15px 30px;
      border-radius: 50px;
      max-width: 90%;
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
      animation: fadeIn 0.5s ease-out;
      display: none;
      z-index: 100;
    }
    
    .search-container {
      margin: 20px auto;
      max-width: 500px;
    }
    
    #search {
      padding: 12px 20px;
      width: 100%;
      border-radius: 50px;
      border: none;
      background: rgba(255,255,255,0.9);
      font-size: 16px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      text-align: center;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; bottom: -50px; }
      to { opacity: 1; bottom: 20px; }
    }
    
    @media (max-width: 768px) {
      .header {
        flex-direction: column;
      }
      
      .drug-grid {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <img src="https://i.imgur.com/pharmacy-logo.png" alt="شعار الصيدلية">
      <h1>نظام الصيدلية الذكية</h1>
    </div>
    
    <div class="search-container">
      <input type="text" id="search" placeholder="ابحث عن الدواء..." oninput="filterDrugs()">
    </div>
    
    <div class="drug-grid" id="drug-list">
      <!-- قائمة الأدوية -->
      <div class="drug" onclick="dispense('باراسيتامول')">باراسيتامول</div>
      <div class="drug" onclick="dispense('بنادول')">بنادول</div>
      <div class="drug" onclick="dispense('أوجمنتين')">أوجمنتين</div>
      <div class="drug" onclick="dispense('كتافلام')">كتافلام</div>
      <div class="drug" onclick="dispense('روفيناك')">روفيناك</div>
      <div class="drug" onclick="dispense('أدول')">أدول</div>
      <div class="drug" onclick="dispense('إيبوبروفين')">إيبوبروفين</div>
      <div class="drug" onclick="dispense('ديكلوفيناك')">ديكلوفيناك</div>
      <div class="drug" onclick="dispense('كونجستال')">كونجستال</div>
      <div class="drug" onclick="dispense('أوتريفين')">أوتريفين</div>
      <div class="drug" onclick="dispense('فولتارين')">فولتارين</div>
      <div class="drug" onclick="dispense('زيرتك')">زيرتك</div>
      <div class="drug" onclick="dispense('سيتريزين')">سيتريزين</div>
      <div class="drug" onclick="dispense('لورين')">لورين</div>
      <div class="drug" onclick="dispense('كلافوكس')">كلافوكس</div>
      <div class="drug" onclick="dispense('تاميفلو')">تاميفلو</div>
      <div class="drug" onclick="dispense('رابيدوس')">رابيدوس</div>
      <div class="drug" onclick="dispense('نيوروتون')">نيوروتون</div>
      <div class="drug" onclick="dispense('كارديكور')">كارديكور</div>
      <div class="drug" onclick="dispense('نوفالدول')">نوفالدول</div>
      <div class="drug" onclick="dispense('سيبروفلوكساسين')">سيبروفلوكساسين</div>
      <div class="drug" onclick="dispense('كلاريثروميسين')">كلاريثروميسين</div>
      <div class="drug" onclick="dispense('دوكسيسايكلين')">دوكسيسايكلين</div>
      <div class="drug" onclick="dispense('أموكسيسيللين')">أموكسيسيللين</div>
    </div>
  </div>
  
  <div id="status"></div>
  
  <script>
    // دالة لعرض الرسائل
    function showStatus(message, color = '#3498db', duration = 3000) {
      const statusElement = document.getElementById('status');
      statusElement.style.display = 'block';
      statusElement.textContent = message;
      statusElement.style.backgroundColor = color;
      
      setTimeout(() => {
        statusElement.style.display = 'none';
      }, duration);
    }

    // التحقق من الاتصال عند تحميل الصفحة
    async function checkConnection() {
      try {
        const response = await fetch('http://192.168.4.1/', {
          mode: 'no-cors',
          cache: 'no-store'
        });
        showStatus('✓ متصل بالصيدلية الذكية', '#2ecc71', 2000);
      } catch (error) {
        showStatus('⚠️ تأكد من الاتصال بشبكة SmartPharmacy', '#e74c3c', 5000);
        setTimeout(checkConnection, 3000); // إعادة المحاولة كل 3 ثواني
      }
    }

    // دالة صرف الدواء
    async function dispense(drugName) {
      showStatus(`جارٍ صرف ${drugName}...`, '#f39c12');
      
      try {
        const response = await fetch(
          `http://192.168.4.1/dispense?drug=${encodeURIComponent(drugName)}`, 
          { mode: 'no-cors' }
        );
        
        showStatus(`✓ تم صرف ${drugName}`, '#2ecc71');
      } catch (error) {
        showStatus('✗ فشل في الصرف. تأكد من تشغيل الجهاز', '#e74c3c');
      }
    }

    // دالة البحث
    function filterDrugs() {
      const searchTerm = document.getElementById('search').value.toLowerCase();
      document.querySelectorAll('.drug').forEach(drug => {
        const drugName = drug.textContent.toLowerCase();
        drug.style.display = drugName.includes(searchTerm) ? 'block' : 'none';
      });
    }

    // بدء التشغيل
    window.addEventListener('load', () => {
      checkConnection();
      // اختبار اتصال كل 10 ثواني
      setInterval(checkConnection, 10000);
    });
  </script>
</body>
</html>0
