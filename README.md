<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>健身房預約管理系統 (Gym Booking Management System)</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    .page { display: none; }
    .page.active { display: block; }
  </style>
</head>
<body class="bg-gray-100 text-gray-800 flex flex-col min-h-screen">

  <!-- 導覽列 Navbar -->
  <nav class="bg-slate-900 text-white shadow-md sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <div class="flex items-center space-x-3 cursor-pointer" onclick="showPage('index')">
          <i class="fa-solid fa-dumbbell text-amber-500 text-2xl"></i>
          <span class="font-bold text-xl tracking-wider">POWERFIT 健身預約系統</span>
        </div>
        <div class="hidden md:flex space-x-4">
          <button onclick="showPage('index')" class="px-3 py-2 rounded-md text-sm font-medium hover:bg-slate-800 transition">
            <i class="fa-solid fa-house mr-1"></i> 首頁 (index.php)
          </button>
          <button onclick="showPage('booking')" class="px-3 py-2 rounded-md text-sm font-medium hover:bg-slate-800 transition">
            <i class="fa-solid fa-calendar-check mr-1"></i> 線上預約 (course_booking.php)
          </button>
          <button onclick="showPage('admin')" class="px-3 py-2 rounded-md text-sm font-medium hover:bg-slate-800 transition">
            <i class="fa-solid fa-user-gear mr-1"></i> 後台管理 (admin_gym.php)
          </button>
          <button onclick="showPage('add-course')" class="bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold px-3 py-2 rounded-md text-sm transition">
            <i class="fa-solid fa-plus mr-1"></i> 新增課程 (course_add.php)
          </button>
        </div>
      </div>
    </div>
  </nav>

  <!-- 主要內容區 Main Content Area -->
  <main class="flex-grow max-w-7xl w-full mx-auto p-4 sm:p-6 lg:p-8">

    <!-- 1. 首頁 View: index.php -->
    <section id="index" class="page active space-y-8">
      <div class="bg-slate-800 text-white rounded-2xl p-8 sm:p-12 shadow-xl flex flex-col md:flex-row items-center justify-between gap-8">
        <div class="space-y-4 md:w-1/2">
          <span class="bg-amber-500 text-slate-900 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider">專業鍛鍊 ‧ 智能管理</span>
          <h1 class="text-3xl sm:text-4xl font-extrabold tracking-tight">超越極限，打造專屬您的健康計畫</h1>
          <p class="text-slate-300 text-base">提供最專業的一對一重訓指導、燃脂有氧與核心皮拉提斯。線上即時查詢時段與預約，輕鬆管理您的運動目標。</p>
          <div class="pt-2 flex space-x-4">
            <button onclick="showPage('booking')" class="bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold px-6 py-3 rounded-lg shadow-lg transition">立即預約課程</button>
            <button onclick="showPage('admin')" class="border border-slate-500 hover:bg-slate-700 text-white font-medium px-6 py-3 rounded-lg transition">進入管理後台</button>
          </div>
        </div>
        <div class="md:w-1/2 w-full flex justify-center">
          <div class="bg-slate-700 p-6 rounded-xl border border-slate-600 w-full max-w-sm text-center shadow-inner">
            <i class="fa-solid fa-heart-pulse text-5xl text-amber-500 mb-3 animate-pulse"></i>
            <h3 class="text-xl font-bold mb-2">本週預約狀況</h3>
            <div class="grid grid-cols-2 gap-4 mt-4">
              <div class="bg-slate-800 p-3 rounded-lg">
                <div class="text-2xl font-bold text-amber-400" id="stat-courses">3</div>
                <div class="text-xs text-slate-400">開放中課程</div>
              </div>
              <div class="bg-slate-800 p-3 rounded-lg">
                <div class="text-2xl font-bold text-emerald-400" id="stat-bookings">3</div>
                <div class="text-xs text-slate-400">已預約人次</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 熱門課程列表 -->
      <div>
        <h2 class="text-2xl font-bold text-slate-800 mb-6 flex items-center">
          <i class="fa-solid fa-fire text-amber-500 mr-2"></i> 熱門推薦課程
        </h2>
        <div id="index-course-list" class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <!-- 由 JS 動態繪製 -->
        </div>
      </div>
    </section>

    <!-- 2. 線上預約 View: course_booking.php -->
    <section id="booking" class="page space-y-6">
      <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-6 rounded-xl shadow-sm border border-gray-200">
        <div>
          <h2 class="text-2xl font-bold text-gray-800">線上課程預約 (course_booking.php)</h2>
          <p class="text-gray-500 text-sm">選擇喜歡的課程與教練，點擊即可進行線上時段預約</p>
        </div>
        <div class="w-full sm:w-auto">
          <input type="text" id="booking-search" oninput="renderBookingCourses()" placeholder="搜尋課程或教練..." class="w-full sm:w-64 px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
        </div>
      </div>

      <div id="booking-course-grid" class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <!-- 由 JS 動態繪製 -->
      </div>
    </section>

    <!-- 3. 後台管理 View: admin_gym.php -->
    <section id="admin" class="page space-y-8">
      <div class="bg-white p-6 rounded-xl shadow-sm border border-gray-200 space-y-4">
        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
          <div>
            <h2 class="text-2xl font-bold text-gray-800">後台管理總覽 (admin_gym.php)</h2>
            <p class="text-gray-500 text-sm">檢視會員預約紀錄 (JOIN 查詢結果)、修改與刪除功能</p>
          </div>
          <button onclick="showPage('add-course')" class="bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold px-4 py-2 rounded-lg text-sm transition">
            <i class="fa-solid fa-plus mr-1"></i> 新增課程
          </button>
        </div>

        <!-- 預約紀錄清單 (SQL JOIN 成果) -->
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse">
            <thead>
              <tr class="bg-slate-100 text-slate-700 text-sm">
                <th class="p-3 border-b">預約編號</th>
                <th class="p-3 border-b">會員姓名</th>
                <th class="p-3 border-b">聯絡電話</th>
                <th class="p-3 border-b">預約課程 (JOIN)</th>
                <th class="p-3 border-b">授課教練</th>
                <th class="p-3 border-b">上課日期與時段</th>
                <th class="p-3 border-b">費用</th>
                <th class="p-3 border-b text-center">操作 (DML)</th>
              </tr>
            </thead>
            <tbody id="admin-booking-tbody" class="text-sm divide-y divide-gray-200">
              <!-- 由 JS 動態繪製 -->
            </tbody>
          </table>
        </div>
      </div>

      <!-- 器材狀態表 -->
      <div class="bg-white p-6 rounded-xl shadow-sm border border-gray-200 space-y-4">
        <h3 class="text-xl font-bold text-gray-800 flex items-center">
          <i class="fa-solid fa-gears text-slate-600 mr-2"></i> 器材與場地狀態 (equipment 表格)
        </h3>
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse">
            <thead>
              <tr class="bg-slate-100 text-slate-700 text-sm">
                <th class="p-3 border-b">器材編號 (PK)</th>
                <th class="p-3 border-b">對應課程 ID (FK)</th>
                <th class="p-3 border-b">器材類別</th>
                <th class="p-3 border-b">目前狀態</th>
              </tr>
            </thead>
            <tbody id="admin-equipment-tbody" class="text-sm divide-y divide-gray-200">
              <!-- 由 JS 動態繪製 -->
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- 4. 新增課程 View: course_add.php -->
    <section id="add-course" class="page space-y-6 max-w-2xl mx-auto">
      <div class="bg-white p-6 sm:p-8 rounded-xl shadow-sm border border-gray-200 space-y-6">
        <div>
          <h2 class="text-2xl font-bold text-gray-800">新增健身課程 (course_add.php)</h2>
          <p class="text-gray-500 text-sm">輸入新課程資訊後，將自動執行 SQL INSERT 並更新至系統庫存</p>
        </div>

        <form id="add-course-form" onsubmit="handleCourseSubmit(event)" class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">課程名稱 (course_name)</label>
            <input type="text" id="fc-name" required placeholder="例如：空中瑜珈基礎班" class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1">授課教練 (coach_name)</label>
              <input type="text" id="fc-coach" required placeholder="例如：David 教練" class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1">單堂費用 NT$ (price)</label>
              <input type="number" id="fc-price" required placeholder="1000" class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
            </div>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">課程介紹 (description)</label>
            <textarea id="fc-desc" rows="3" required placeholder="請簡述課程特色與訓練目標..." class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none"></textarea>
          </div>

          <!-- 即時 SQL 語法預覽區 -->
          <div class="bg-slate-900 text-amber-400 p-4 rounded-lg font-mono text-xs overflow-x-auto">
            <span class="text-gray-400">// 即時 SQL DML 語法預覽：</span><br>
            <span id="sql-preview">INSERT INTO course (course_id, course_name, coach_name, price, description) VALUES (NULL, '...', '...', 0, '...');</span>
          </div>

          <div class="flex justify-end space-x-3 pt-2">
            <button type="button" onclick="showPage('admin')" class="px-5 py-2 border border-gray-300 rounded-lg text-sm font-medium text-gray-600 hover:bg-gray-50 transition">取消</button>
            <button type="submit" class="px-5 py-2 bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold rounded-lg text-sm transition">新增課程 (INSERT)</button>
          </div>
        </form>
      </div>
    </section>

  </main>

  <!-- 預約對話框 Modal -->
  <div id="booking-modal" class="fixed inset-0 bg-slate-900/60 hidden items-center justify-center p-4 z-50">
    <div class="bg-white rounded-xl shadow-2xl max-w-md w-full p-6 space-y-4">
      <div class="flex justify-between items-center border-b pb-3">
        <h3 class="text-lg font-bold text-gray-800" id="modal-title">線上課程預約</h3>
        <button onclick="closeBookingModal()" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark text-xl"></i></button>
      </div>
      
      <form id="modal-form" onsubmit="handleBookingSubmit(event)" class="space-y-4">
        <input type="hidden" id="modal-course-id">
        <div class="bg-amber-50 p-3 rounded-lg border border-amber-200 text-xs text-amber-800" id="modal-course-info"></div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">會員姓名</label>
          <input type="text" id="bm-name" required placeholder="請輸入姓名" class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">聯絡電話</label>
          <input type="tel" id="bm-phone" required placeholder="0912-345-678" class="w-full px-4 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
        </div>
        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">預約日期</label>
            <input type="date" id="bm-date" required class="w-full px-3 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">預約時段</label>
            <select id="bm-slot" required class="w-full px-3 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-amber-500 focus:outline-none">
              <option value="09:00 - 10:00">09:00 - 10:00</option>
              <option value="14:00 - 15:00">14:00 - 15:00</option>
              <option value="18:00 - 19:00">18:00 - 19:00</option>
              <option value="19:00 - 20:00">19:00 - 20:00</option>
              <option value="20:00 - 21:00">20:00 - 21:00</option>
            </select>
          </div>
        </div>

        <div class="flex justify-end space-x-2 pt-3">
          <button type="button" onclick="closeBookingModal()" class="px-4 py-2 border border-gray-300 rounded-lg text-sm font-medium text-gray-600 hover:bg-gray-50">取消</button>
          <button type="submit" class="px-4 py-2 bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold rounded-lg text-sm">確認預約</button>
        </div>
      </form>
    </div>
  </div>

  <footer class="bg-slate-900 text-gray-400 text-center py-4 text-xs mt-auto">
    <p>© 2026 健身房預約管理系統 (Gym Booking Management System)</p>
  </footer>

  <script>
    let courses = [
      { course_id: 1, course_name: "一對一重訓指導", coach_name: "Alex 教練", price: 1200, description: "針對深蹲、臥推、硬舉等大重量訓練進行動作雕塑與肌力發揮。" },
      { course_id: 2, course_name: "燃脂有氧核心", coach_name: "Bella 教練", price: 800, description: "透過高強度間歇訓練 (HIIT) 快速燃燒卡路里並強化核心穩定度。" },
      { course_id: 3, course_name: "舒壓皮拉提斯", coach_name: "Chloe 教練", price: 1000, description: "強化深層核心肌群、改善身體柔軟度與姿態不良體態。" }
    ];

    let equipments = [
      { equipment_id: "EQ-01", course_id: 1, category: "奧林匹克槓鈴與深蹲架", status: "Available" },
      { equipment_id: "EQ-02", course_id: 2, category: "多功能有氧踏板與壺鈴", status: "Available" },
      { equipment_id: "EQ-03", course_id: 3, category: "皮拉提斯核心床 (Reformer)", status: "Maintenance" }
    ];

    let bookings = [
      { booking_id: 5001, course_id: 1, member_name: "張小豪", member_phone: "0912-333-444", booking_date: "2026-11-15", time_slot: "19:00 - 20:00" },
      { booking_id: 5002, course_id: 2, member_name: "林美玲", member_phone: "0988-555-666", booking_date: "2026-11-16", time_slot: "18:00 - 19:00" },
      { booking_id: 5003, course_id: 3, member_name: "王大明", member_phone: "0933-777-888", booking_date: "2026-11-18", time_slot: "20:00 - 21:00" }
    ];

    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
      window.scrollTo({ top: 0, behavior: 'smooth' });

      renderStats();
      renderIndexCourses();
      renderBookingCourses();
      renderAdminTables();
    }

    function renderStats() {
      document.getElementById('stat-courses').textContent = courses.length;
      document.getElementById('stat-bookings').textContent = bookings.length;
    }

    function renderIndexCourses() {
      const container = document.getElementById('index-course-list');
      container.innerHTML = courses.map(c => `
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 overflow-hidden flex flex-col justify-between">
          <div class="p-5 space-y-3">
            <div class="flex justify-between items-start">
              <span class="bg-slate-100 text-slate-700 text-xs font-semibold px-2.5 py-1 rounded">ID: ${c.course_id}</span>
              <span class="text-amber-600 font-bold text-lg">NT$ ${c.price}</span>
            </div>
            <h3 class="text-lg font-bold text-gray-800">${c.course_name}</h3>
            <p class="text-xs text-slate-500 font-medium"><i class="fa-solid fa-user-ninja mr-1"></i>${c.coach_name}</p>
            <p class="text-gray-600 text-sm line-clamp-2">${c.description}</p>
          </div>
          <div class="p-5 bg-gray-50 border-t border-gray-100">
            <button onclick="openBookingModal(${c.course_id})" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-medium py-2 rounded-lg text-sm transition">預約此課程</button>
          </div>
        </div>
      `).join('');
    }

    function renderBookingCourses() {
      const query = document.getElementById('booking-search').value.toLowerCase();
      const filtered = courses.filter(c => c.course_name.toLowerCase().includes(query) || c.coach_name.toLowerCase().includes(query));
      const container = document.getElementById('booking-course-grid');
      
      container.innerHTML = filtered.map(c => `
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-5 flex flex-col justify-between space-y-4">
          <div class="space-y-2">
            <div class="flex justify-between items-center">
              <h3 class="text-lg font-bold text-gray-800">${c.course_name}</h3>
              <span class="text-amber-600 font-bold">NT$ ${c.price}</span>
            </div>
            <div class="text-xs text-amber-600 font-medium bg-amber-50 inline-block px-2 py-0.5 rounded">教練：${c.coach_name}</div>
            <p class="text-gray-600 text-sm">${c.description}</p>
          </div>
          <button onclick="openBookingModal(${c.course_id})" class="w-full bg-amber-500 hover:bg-amber-600 text-slate-900 font-bold py-2 rounded-lg text-sm transition">
            <i class="fa-solid fa-calendar-plus mr-1"></i> 立即預約
          </button>
        </div>
      `).join('');
    }

    function renderAdminTables() {
      const tbodyBooking = document.getElementById('admin-booking-tbody');
      tbodyBooking.innerHTML = bookings.map(b => {
        const course = courses.find(c => c.course_id === b.course_id) || { course_name: '未知課程', coach_name: '-', price: 0 };
        return `
          <tr class="hover:bg-slate-50">
            <td class="p-3 font-mono font-medium">${b.booking_id}</td>
            <td class="p-3 font-semibold text-slate-800">${b.member_name}</td>
            <td class="p-3 text-slate-600">${b.member_phone}</td>
            <td class="p-3 font-medium text-amber-600">${course.course_name}</td>
            <td class="p-3 text-slate-600">${course.coach_name}</td>
            <td class="p-3 text-slate-600">${b.booking_date} (${b.time_slot})</td>
            <td class="p-3 font-medium">NT$ ${course.price}</td>
            <td class="p-3 text-center">
              <button onclick="deleteBooking(${b.booking_id})" class="text-red-500 hover:text-red-700 font-medium text-xs px-2 py-1 bg-red-50 rounded border border-red-200">
                <i class="fa-solid fa-trash mr-1"></i>取消預約
              </button>
            </td>
          </tr>
        `;
      }).join('');

      const tbodyEq = document.getElementById('admin-equipment-tbody');
      tbodyEq.innerHTML = equipments.map(eq => `
        <tr class="hover:bg-slate-50">
          <td class="p-3 font-mono font-medium">${eq.equipment_id}</td>
          <td class="p-3 text-slate-600">Course #${eq.course_id}</td>
          <td class="p-3 font-medium text-slate-800">${eq.category}</td>
          <td class="p-3">
            <span class="px-2.5 py-0.5 rounded-full text-xs font-bold ${eq.status === 'Available' ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'}">
              ${eq.status}
            </span>
          </td>
        </tr>
      `).join('');
    }

    function openBookingModal(courseId) {
      const course = courses.find(c => c.course_id === courseId);
      if (!course) return;

      document.getElementById('modal-course-id').value = course.course_id;
      document.getElementById('modal-title').textContent = `預約課程：${course.course_name}`;
      document.getElementById('modal-course-info').innerHTML = `
        <div class="font-bold">${course.course_name} (NT$ ${course.price})</div>
        <div>授課教練：${course.coach_name}</div>
      `;

      const today = new Date().toISOString().split('T')[0];
      document.getElementById('bm-date').value = today;

      document.getElementById('booking-modal').classList.remove('hidden');
      document.getElementById('booking-modal').classList.add('flex');
    }

    function closeBookingModal() {
      document.getElementById('booking-modal').classList.add('hidden');
      document.getElementById('booking-modal').classList.remove('flex');
    }

    function handleBookingSubmit(e) {
      e.preventDefault();
      const course_id = parseInt(document.getElementById('modal-course-id').value);
      const member_name = document.getElementById('bm-name').value;
      const member_phone = document.getElementById('bm-phone').value;
      const booking_date = document.getElementById('bm-date').value;
      const time_slot = document.getElementById('bm-slot').value;

      const newBooking = {
        booking_id: 5000 + bookings.length + 1,
        course_id,
        member_name,
        member_phone,
        booking_date,
        time_slot
      };

      bookings.push(newBooking);
      closeBookingModal();
      document.getElementById('modal-form').reset();
      alert(`預約成功！您的預約編號為：${newBooking.booking_id}`);
      showPage('admin');
    }

    function deleteBooking(bookingId) {
      if (confirm(`確定要取消/刪除預約單號 #${bookingId} 嗎？`)) {
        bookings = bookings.filter(b => b.booking_id !== bookingId);
        renderAdminTables();
        renderStats();
      }
    }

    function handleCourseSubmit(e) {
      e.preventDefault();
      const course_name = document.getElementById('fc-name').value;
      const coach_name = document.getElementById('fc-coach').value;
      const price = parseInt(document.getElementById('fc-price').value);
      const description = document.getElementById('fc-desc').value;

      const newCourse = {
        course_id: courses.length + 1,
        course_name,
        coach_name,
        price,
        description
      };

      courses.push(newCourse);
      document.getElementById('add-course-form').reset();
      alert(`課程「${newCourse.course_name}」已成功新增！`);
      showPage('booking');
    }

    document.getElementById('add-course-form').addEventListener('input', () => {
      const name = document.getElementById('fc-name').value || '...';
      const coach = document.getElementById('fc-coach').value || '...';
      const price = document.getElementById('fc-price').value || 0;
      const desc = document.getElementById('fc-desc').value || '...';

      document.getElementById('sql-preview').textContent = 
        `INSERT INTO course (course_id, course_name, coach_name, price, description) VALUES (NULL, '${name}', '${coach}', ${price}, '${desc}');`;
    });

    showPage('index');
  </script>
</body>
</html>
