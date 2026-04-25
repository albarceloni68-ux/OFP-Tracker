<!DOCTYPE html>
<html lang="ar" dir="rtl" id="html-tag">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Risks | OFP Tracker</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <style>
        :root { --cr-blue: #002A54; --cr-blue-light: #184275; --cr-dark: #1a1a1a; --cr-light: #f4f7f1; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--cr-light); margin: 0; display: flex; flex-direction: column; min-height: 100vh; }
        #sidebar { width: 260px; background-color: var(--cr-dark); color: white; position: fixed; height: 100%; z-index: 1000; overflow-y: auto;}
        .sidebar-header { background: white; padding: 20px 15px; text-align: center; border-bottom: 3px solid var(--cr-blue); }
        #sidebar ul { list-style: none; padding: 0; margin-top: 10px; }
        #sidebar ul li a { padding: 15px 20px; display: block; color: #cbd5e0; text-decoration: none; border-bottom: 1px solid #2d3748; cursor: pointer; transition: 0.3s; }
        #sidebar ul li a:hover, #sidebar ul li a.active { background: var(--cr-blue-light); color: white; font-weight: bold;}
        .sub-menu { display: none; background-color: #0f0f0f; }
        .sub-menu a { padding: 10px 30px !important; font-size: 0.9rem; border-bottom: none !important; }
        .sub-menu a.active-sub { color: white !important; border-right: 4px solid var(--cr-blue) !important; }
        .content { margin-right: 260px; flex: 1; display: flex; flex-direction: column; }
        .top-navbar { background: white; padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .btn-cr { background-color: var(--cr-blue); color: white; border: none; font-weight: bold;}
        .btn-cr:hover { background-color: var(--cr-blue-light); color: white; }
        .custom-card { background: white; border-radius: 12px; padding: 20px; box-shadow: 0 4px 20px rgba(0,0,0,0.04); margin-bottom: 20px; }
        #login-page { height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--cr-dark) 0%, var(--cr-blue) 100%); position: fixed; width: 100%; z-index: 2000; top: 0; left: 0; }
        .login-card { background: white; padding: 40px; border-radius: 20px; width: 100%; max-width: 420px; text-align: center; }
        .filter-box { background: #f8f9fa; padding: 10px 15px; border-radius: 6px; border: 1px solid #dee2e6; margin-bottom: 15px; }
        .filter-box .form-control, .filter-box .form-select { font-size: 0.8rem; height: 32px; }
        .filter-label { font-size: 0.75rem; color: #6c757d; font-weight: bold; }
        .names-preview { font-size: 0.75rem; background: #fafafa; border: 1px solid #eee; padding: 8px; border-radius: 6px; max-height: 90px; overflow-y: auto; text-align: start; }
        .pagination .page-link { color: var(--cr-blue); font-weight: bold; cursor: pointer; }
        .pagination .page-item.active .page-link { background-color: var(--cr-blue); border-color: var(--cr-blue); color: white; }
        .notif-dropdown { min-width: 320px; max-height: 400px; overflow-y: auto; }
        .cr-footer { background-color: var(--cr-blue); color: white; padding: 15px; text-align: center; margin-top: auto; font-size: 0.85rem; }
        .ltr-mode .content { margin-right: 0; margin-left: 260px; }
        .ltr-mode #sidebar { right: auto; left: 0; }
    </style>
</head>
<body class="rtl-mode text-start">

    <!-- Login -->
    <div id="login-page">
        <div class="login-card shadow-lg">
            <img src="https://parisarbitrationweek.com/wp-content/uploads/gravity_forms/5-8c91b4cf34801a0f50f49fc1e3b236a3/2024/02/control-risks-logo-dark-blue-rgb.jpg" alt="Logo" style="width: 80%; margin-bottom: 15px;">
            <p class="text-muted mb-4 fw-bold">OFP Application Tracker</p>
            <div id="login-error" class="alert alert-danger p-2 small fw-bold d-none">اليوزر أو الرمز خطأ.</div>
            <input type="text" id="userInput" class="form-control mb-3" placeholder="اسم المستخدم">
            <input type="password" id="passInput" class="form-control mb-4" placeholder="كلمة المرور" onkeypress="if(event.key === 'Enter') loginSystem()">
            <button onclick="loginSystem()" class="btn btn-cr w-100 p-2 fs-5">دخول</button>
            <div class="mt-3"><a href="#" class="text-muted small" onclick="alert('compad.southiraq@controlrisks.com')">هل نسيت كلمة السر؟</a></div>
        </div>
    </div>

    <!-- Dashboard -->
    <div id="main-dashboard" style="display: none;">
        <nav id="sidebar">
            <div class="sidebar-header"><img src="https://parisarbitrationweek.com/wp-content/uploads/gravity_forms/5-8c91b4cf34801a0f50f49fc1e3b236a3/2024/02/control-risks-logo-dark-blue-rgb.jpg" alt="Logo" style="width: 100%;"></div>
            <ul>
                <li><a onclick="showTab('auth')" id="link-auth" class="active"><i class="fa fa-folder-open ms-2"></i> الطلبات للهيئات</a></li>
                <li><a onclick="showTab('permits')" id="link-permits"><i class="fa fa-id-card-clip ms-2"></i> التصاريح المستلمة</a></li>
                <li>
                    <a onclick="toggleMaster()"><i class="fa fa-database ms-2"></i> Master OFP <i class="fa fa-chevron-down ms-auto float-end mt-1" style="font-size: 10px;"></i></a>
                    <ul class="sub-menu" id="master-submenu">
                        <li><a onclick="showMaster('e')" id="sub-e"><i class="fa fa-user ms-2"></i> الأجانب</a></li>
                        <li><a onclick="showMaster('l')" id="sub-l"><i class="fa fa-user-tie ms-2"></i> العراقيين</a></li>
                        <li><a onclick="showMaster('w')" id="sub-w"><i class="fa fa-car ms-2"></i> العجلات</a></li>
                    </ul>
                </li>
                <li id="admin-menu" class="d-none"><a onclick="showTab('users')" id="link-users"><i class="fa fa-users-gear ms-2"></i> إدارة الحسابات</a></li>
                <li><a onclick="location.reload()"><i class="fa fa-power-off ms-2"></i> خروج</a></li>
            </ul>
        </nav>

        <div class="content">
            <header class="top-navbar">
                <div class="d-flex align-items-center">
                    <button class="btn btn-sm btn-outline-secondary fw-bold ms-3" onclick="toggleLang()">EN / AR</button>
                    <!-- Notifications -->
                    <div class="dropdown">
                        <button class="btn btn-light position-relative border" data-bs-toggle="dropdown" onclick="readNotifs()">
                            <i class="fa fa-bell text-warning fs-5"></i>
                            <span id="notif-count" class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger d-none">0</span>
                        </button>
                        <ul id="notif-list" class="dropdown-menu notif-dropdown p-0"></ul>
                    </div>
                </div>
                <div><span class="text-muted small">مرحباً:</span> <strong id="user-display" style="color: var(--cr-blue);"></strong></div>
            </header>

            <div class="p-4" style="flex: 1;">
                
                <!-- Authorities -->
                <div id="auth-section">
                    <div class="d-flex justify-content-between mb-3"><h3 class="fw-bold">سجل الطلبات للهيئات</h3><button class="btn btn-cr" onclick="openModal('auth')"><i class="fa fa-plus"></i> إضافة طلب</button></div>
                    <div class="filter-box row g-2">
                        <div class="col-md-2"><input type="text" id="f-auth-bn" class="form-control" placeholder="رقم الكتاب" onkeyup="currPageA=1;render()"></div>
                        <div class="col-md-3"><input type="text" id="f-auth-list" class="form-control" placeholder="بحث بالأسماء" onkeyup="currPageA=1;render()"></div>
                        <div class="col-md-2"><input type="date" id="f-auth-from" class="form-control" onchange="currPageA=1;render()"></div>
                        <div class="col-md-2"><input type="date" id="f-auth-to" class="form-control" onchange="currPageA=1;render()"></div>
                        <div class="col-md-2"><select id="f-auth-status" class="form-select" onchange="currPageA=1;render()"><option value="">الكل</option><option value="Pending">Pending</option><option value="Done">Completed</option></select></div>
                        <div class="col-md-1"><button class="btn btn-secondary w-100" onclick="clearF('auth')"><i class="fa fa-eraser"></i></button></div>
                    </div>
                    <div class="custom-card table-responsive">
                        <table class="table align-middle text-center"><thead class="table-dark"><tr><th>Ref</th><th style="width:25%;">الكتاب والأسماء</th><th>الهيئة/النوع</th><th>المذكرة</th><th>إرسال</th><th>استلام</th><th>بواسطة</th><th>الحالة</th><th>إجراء</th></tr></thead><tbody id="auth-body"></tbody></table>
                        <div class="d-flex justify-content-between mt-2"><button class="btn btn-success btn-sm" onclick="exportD('auth')">تصدير</button><ul class="pagination pagination-sm mb-0" id="auth-pag"></ul></div>
                    </div>
                </div>

                <!-- Permits -->
                <div id="permits-section" class="d-none">
                    <div class="d-flex justify-content-between mb-3"><h3 class="fw-bold" style="color:var(--cr-blue);">التصاريح المستلمة</h3></div>
                    <div class="filter-box row g-2">
                        <div class="col-md-2"><input type="text" id="f-perm-bn" class="form-control" placeholder="رقم الكتاب" onkeyup="currPageP=1;render()"></div>
                        <div class="col-md-2"><input type="text" id="f-perm-ofp" class="form-control" placeholder="OFP/المذكرة" onkeyup="currPageP=1;render()"></div>
                        <div class="col-md-3"><input type="text" id="f-perm-list" class="form-control" placeholder="بحث بالأسماء" onkeyup="currPageP=1;render()"></div>
                        <div class="col-md-2"><input type="date" id="f-perm-from" class="form-control" onchange="currPageP=1;render()"></div>
                        <div class="col-md-2"><input type="date" id="f-perm-to" class="form-control" onchange="currPageP=1;render()"></div>
                        <div class="col-md-1"><button class="btn btn-secondary w-100" onclick="clearF('perm')"><i class="fa fa-eraser"></i></button></div>
                    </div>
                    <div class="custom-card table-responsive">
                        <table class="table table-bordered align-middle text-center"><thead class="table-primary border-primary"><tr><th>Ref/OFP</th><th style="width:25%;">التصريح والأسماء</th><th>الهيئة/النوع</th><th>المذكرة</th><th>إرسال</th><th>استلام</th><th>بواسطة</th><th>الحالة</th><th>إجراء</th></tr></thead><tbody id="perm-body"></tbody></table>
                        <div class="d-flex justify-content-between mt-2"><button class="btn btn-success btn-sm" onclick="exportD('perm')">تصدير</button><ul class="pagination pagination-sm mb-0" id="perm-pag"></ul></div>
                    </div>
                </div>

                <!-- Master OFP -->
                <div id="master-section" class="d-none">
                    <div class="d-flex justify-content-between mb-3">
                        <h3 class="fw-bold text-dark"><i class="fa fa-database"></i> بيانات <span id="m-title" class="text-primary">الأجانب</span></h3>
                        <div><button class="btn btn-warning btn-sm fw-bold me-2" onclick="bulkModal.show()"><i class="fa fa-sync"></i> تحديث المحددين</button><button class="btn btn-cr btn-sm" onclick="openMaster()"><i class="fa fa-plus"></i> إضافة للقاعدة</button></div>
                    </div>
                    <div class="filter-box row g-2">
                        <div class="col-md-3"><input type="text" id="f-m-name" class="form-control" placeholder="الاسم/العجلة" onkeyup="renderM()"></div>
                        <div class="col-md-2"><input type="text" id="f-m-ofp" class="form-control" placeholder="OFP" onkeyup="renderM()"></div>
                        <div class="col-md-2"><select id="f-m-proj" class="form-select" onchange="renderM()"><option value="">كل المشاريع</option><option value="BGC">BGC</option><option value="ROO">ROO</option><option value="SRC">SRC</option></select></div>
                        <div class="col-md-2"><input type="date" id="f-m-end" class="form-control" onchange="renderM()"></div>
                        <div class="col-md-2"><button class="btn btn-secondary w-100" onclick="['f-m-name','f-m-ofp','f-m-proj','f-m-end'].forEach(i=>document.getElementById(i).value='');renderM()"><i class="fa fa-eraser"></i></button></div>
                    </div>
                    <div class="custom-card table-responsive">
                        <table class="table border text-center align-middle"><thead class="table-dark"><tr><th><input type="checkbox" id="scb" onchange="document.querySelectorAll('.mcb').forEach(c=>c.checked=this.checked)"></th><th id="m-th-name">الاسم</th><th>المشروع</th><th>ISCO</th><th>المذكرة</th><th>OFP</th><th>تاريخ البدء</th><th>تاريخ الانتهاء</th><th>الصلاحية</th><th>حذف</th></tr></thead><tbody id="master-body"></tbody></table>
                        <div class="mt-2 text-end"><button class="btn btn-success btn-sm" onclick="exportM()">تصدير</button></div>
                    </div>
                </div>

                <!-- Users -->
                <div id="users-section" class="d-none">
                    <h3 class="fw-bold mb-4">إدارة الحسابات</h3>
                    <div class="custom-card">
                        <div class="row g-2 mb-4 bg-light p-3 rounded border">
                            <input type="hidden" id="edit-u-idx" value="-1">
                            <div class="col-md-4"><input id="nu" class="form-control" placeholder="اسم المستخدم"></div>
                            <div class="col-md-4"><input id="np" class="form-control" placeholder="كلمة المرور"></div>
                            <div class="col-md-4"><button id="btn-u" class="btn btn-cr w-100 fw-bold" onclick="addU()">حفظ</button></div>
                        </div>
                        <table class="table table-striped text-center"><thead class="table-dark"><tr><th>المستخدم</th><th>الباسورد</th><th>الرتبة</th><th>إجراء</th></tr></thead><tbody id="utb"></tbody></table>
                    </div>
                </div>
            </div>
            <footer class="cr-footer">&copy; 2024 Control Risks OFP Tracker</footer>
        </div>
    </div>

    <!-- Modals -->
    <div class="modal fade" id="mainModal" tabindex="-1">
        <div class="modal-dialog modal-xl">
            <div class="modal-content border-0">
                <div class="modal-header text-white" style="background:var(--cr-blue);"><h5 class="fw-bold">بيانات الطلب</h5><button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button></div>
                <div class="modal-body p-4 text-start">
                    <input type="hidden" id="form-t"><input type="hidden" id="edit-id">
                    <div class="row g-3">
                        <div class="col-md-3"><label class="small fw-bold">رقم الكتاب</label><input type="text" id="m-bn" class="form-control" required></div>
                        <div class="col-md-3"><label class="small fw-bold">نوع التصريح</label><select id="m-pt" class="form-select"><option value="مؤقت">مؤقت</option><option value="دائمي">دائمي</option></select></div>
                        <div class="col-md-3"><label class="small fw-bold">الهيئة</label><select id="m-ta" class="form-select"><option value="BGC">BGC</option><option value="ROO">ROO</option><option value="SRC">SRC</option></select></div>
                        <div class="col-md-3" id="ofp-box"><label class="small fw-bold text-primary">OFP Number</label><input type="text" id="m-ofp" class="form-control"></div>
                        <div class="col-md-3"><label class="small fw-bold">إرسال</label><input type="date" id="m-sd" class="form-control"></div>
                        <div class="col-md-3"><label class="small fw-bold">استلام</label><input type="date" id="m-rd" class="form-control"></div>
                        <div class="col-md-3"><label class="small fw-bold">رقم المذكرة</label><input type="text" id="m-memo" class="form-control"></div>
                        <div class="col-md-3"><label class="small fw-bold">مرفق (اختياري)</label><input type="file" id="m-file" class="form-control" accept=".pdf, image/*"></div>
                        <div class="col-12"><div class="alert alert-secondary small text-center fw-bold p-1">انسخ القوائم من الوورد والصقها هنا</div></div>
                        <div class="col-md-3"><textarea id="m-w" class="form-control" rows="4" placeholder="عجلات"></textarea></div>
                        <div class="col-md-3"><textarea id="m-e" class="form-control" rows="4" placeholder="أجانب"></textarea></div>
                        <div class="col-md-3"><textarea id="m-l" class="form-control" rows="4" placeholder="عراقيين"></textarea></div>
                        <div class="col-md-3"><textarea id="m-wp" class="form-control" rows="4" placeholder="أسلحة"></textarea></div>
                    </div>
                </div>
                <div class="modal-footer"><button onclick="saveOrd()" class="btn btn-cr px-5">حفظ</button></div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="addMasterModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header text-white" style="background:var(--cr-blue);"><h5 class="fw-bold">إضافة للماستر</h5><button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button></div>
                <div class="modal-body text-start">
                    <select id="m-type" class="form-select mb-2"><option value="e">أجانب</option><option value="l">عراقيين</option><option value="w">عجلات</option></select>
                    <select id="m-proj" class="form-select mb-2"><option value="BGC">BGC</option><option value="ROO">ROO</option><option value="SRC">SRC</option></select>
                    <textarea id="m-names" class="form-control" rows="6" placeholder="الصق الأسماء/العجلات هنا"></textarea>
                </div>
                <div class="modal-footer"><button onclick="saveM()" class="btn btn-cr w-100">إضافة</button></div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="bulkUpdateModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header bg-warning"><h5 class="fw-bold">تحديث جماعي</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
                <div class="modal-body text-start">
                    <div class="row g-2">
                        <div class="col-6"><input type="text" id="b-isco" class="form-control" placeholder="ISCO"></div>
                        <div class="col-6"><input type="text" id="b-memo" class="form-control" placeholder="المذكرة"></div>
                        <div class="col-12"><input type="text" id="b-ofp" class="form-control" placeholder="OFP Number"></div>
                        <div class="col-6"><label class="small">بدء</label><input type="date" id="b-start" class="form-control"></div>
                        <div class="col-6"><label class="small">انتهاء</label><input type="date" id="b-end" class="form-control"></div>
                    </div>
                </div>
                <div class="modal-footer"><button onclick="applyBulk()" class="btn btn-warning w-100 fw-bold">تطبيق</button></div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Data Initialization
        let authData = JSON.parse(localStorage.getItem('CR_AUTH')) || [];
        let permData = JSON.parse(localStorage.getItem('CR_PERM')) || [];
        let masterData = JSON.parse(localStorage.getItem('CR_MASTER')) || [];
        let users = JSON.parse(localStorage.getItem('CR_USERS')) || [{u:'admin', p:'1234', r:'admin'}];
        let readNotifs = JSON.parse(localStorage.getItem('CR_NOTIFS')) || {};
        let currentUser = null; let lang = 'ar'; let currTabM = 'e';
        let currPageA = 1, currPageP = 1, limit = 15;
        
        const modal = new bootstrap.Modal(document.getElementById('mainModal'));
        const mModal = new bootstrap.Modal(document.getElementById('addMasterModal'));
        const bulkModal = new bootstrap.Modal(document.getElementById('bulkUpdateModal'));

        function saveDb() {
            try { localStorage.setItem('CR_AUTH', JSON.stringify(authData)); localStorage.setItem('CR_PERM', JSON.stringify(permData)); localStorage.setItem('CR_MASTER', JSON.stringify(masterData)); localStorage.setItem('CR_USERS', JSON.stringify(users)); localStorage.setItem('CR_NOTIFS', JSON.stringify(readNotifs)); } 
            catch(e){ alert("الذاكرة ممتلئة!"); }
        }

        function loginSystem() {
            const u = document.getElementById('user').value, p = document.getElementById('pass').value;
            const f = users.find(x => x.u === u && x.p === p);
            if(f) {
                currentUser = f; document.getElementById('login-page').classList.add('d-none'); document.getElementById('main-dashboard').style.display = 'block';
                document.getElementById('user-display').innerText = f.u; if(f.r === 'admin') document.getElementById('admin-menu').classList.remove('d-none');
                if(!readNotifs[currentUser.u]) readNotifs[currentUser.u] = [];
                render(); checkNotifs();
            } else document.getElementById('login-error').classList.remove('d-none');
        }

        function showTab(t) {
            ['auth','permits','users','master'].forEach(s => document.getElementById(s+'-section').classList.add('d-none'));
            document.getElementById(t+'-section').classList.remove('d-none');
            document.querySelectorAll('#sidebar a').forEach(a => a.classList.remove('active','active-sub'));
            document.getElementById('link-'+t).classList.add('active'); document.getElementById('master-submenu').style.display = 'none';
        }
        function toggleMaster() { const sm = document.getElementById('master-submenu'); sm.style.display = sm.style.display==='block'?'none':'block'; }
        function showMaster(type) {
            ['auth','permits','users'].forEach(s => document.getElementById(s+'-section').classList.add('d-none'));
            document.getElementById('master-section').classList.remove('d-none');
            document.querySelectorAll('#sidebar a').forEach(a => a.classList.remove('active','active-sub'));
            document.getElementById('link-master').classList.add('active'); document.getElementById('sub-'+type).classList.add('active-sub');
            currTabM = type; document.getElementById('m-title').innerText = type==='e'?'الأجانب':(type==='l'?'العراقيين':'العجلات');
            document.getElementById('m-th-name').innerText = type==='w'?'رقم العجلة':'الاسم'; renderM();
        }

        // Auth/Permits Logic
        function openModal(t, id=null) {
            document.getElementById('form-t').value=t; document.getElementById('edit-id').value=id||"";
            document.getElementById('m-bn').value=""; document.getElementById('m-pt').value="مؤقت"; document.getElementById('m-ta').value="BGC";
            document.getElementById('m-ofp').value=""; document.getElementById('m-sd').value=""; document.getElementById('m-rd').value=""; document.getElementById('m-memo').value="";
            document.getElementById('m-w').value=""; document.getElementById('m-e').value=""; document.getElementById('m-l').value=""; document.getElementById('m-wp').value="";
            document.getElementById('m-file').value=""; document.getElementById('m-file').removeAttribute('data-file');
            document.getElementById('ofp-box').style.display = t==='permits'?'block':'none';
            if(id) {
                const item = (t==='auth'?authData:permData).find(x=>x.id==id);
                if(item) { document.getElementById('m-bn').value=item.b; document.getElementById('m-pt').value=item.p; document.getElementById('m-ta').value=item.t; document.getElementById('m-ofp').value=item.o||""; document.getElementById('m-sd').value=item.s; document.getElementById('m-rd').value=item.r; document.getElementById('m-memo').value=item.m; document.getElementById('m-w').value=item.w.join('\n'); document.getElementById('m-e').value=item.e.join('\n'); document.getElementById('m-l').value=item.l.join('\n'); document.getElementById('m-wp').value=item.wp.join('\n'); if(item.f) document.getElementById('m-file').setAttribute('data-file',item.f); }
            } modal.show();
        }

        async function saveOrd() {
            const b = document.getElementById('m-bn').value; if(!b) return;
            const t = document.getElementById('form-t').value, id = document.getElementById('edit-id').value;
            const gl = (x) => document.getElementById(x).value.split('\n').filter(l=>l.trim()!=="");
            let ex = id ? (t==='auth'?authData:permData).find(x=>x.id==id) : null;
            
            const fi = document.getElementById('m-file'); let fb = fi.getAttribute('data-file');
            if(fi.files.length>0) fb = await new Promise(r=>{const rd=new FileReader(); rd.onload=()=>r(rd.result); rd.readAsDataURL(fi.files[0]);});

            const entry = { id: id?parseInt(id):Date.now(), b:b, p:document.getElementById('m-pt').value, t:document.getElementById('m-ta').value, m:document.getElementById('m-memo').value, o:document.getElementById('m-ofp').value, s:document.getElementById('m-sd').value, r:document.getElementById('m-rd').value, w:gl('m-w'), e:gl('m-e'), l:gl('m-l'), wp:gl('m-wp'), c: ex?ex.c:currentUser.u, ref: ex?ex.ref:'REF-'+Math.floor(1000+Math.random()*9000), f:fb };
            const stats = `${entry.e.length} أجانب - ${entry.w.length} عجلات - ${entry.l.length} عراقيين`;

            if(t==='auth') {
                entry.fb = `TEMP - ${entry.b} - ${entry.p} - ${stats}`; entry.st = entry.r ? 'Done' : 'Pending';
                if(id) { authData[authData.findIndex(x=>x.id==id)] = entry; let px=permData.findIndex(p=>p.ref===entry.ref); if(px!==-1) { permData[px].p=entry.p; permData[px].t=entry.t; permData[px].w=entry.w; permData[px].e=entry.e; permData[px].l=entry.l; permData[px].wp=entry.wp; permData[px].fb=`ISCO - ${permData[px].b||""} - ${entry.p} - ${stats}`; } } else authData.push(entry);
                if(entry.st==='Done' && !permData.find(x=>x.ref===entry.ref)) { let cpy=JSON.parse(JSON.stringify(entry)); cpy.id=Date.now()+1; cpy.b=""; cpy.fb=""; cpy.o=""; cpy.s=""; cpy.r=""; cpy.st="Pending"; permData.push(cpy); }
            } else {
                entry.st = entry.r ? 'Completed' : 'Pending'; entry.fb = `ISCO - ${entry.b} - ${entry.p} - ${stats}`;
                permData[permData.findIndex(x=>x.id==id)] = entry;
            }
            saveDb(); render(); modal.hide();
        }

        // Master Logic
        function openMaster() { document.getElementById('m-type').value=currTabM; document.getElementById('m-names').value=""; mModal.show(); }
        function saveM() {
            const t=document.getElementById('m-type').value, p=document.getElementById('m-proj').value, n=document.getElementById('m-names').value.split('\n').filter(l=>l.trim()!=="");
            n.forEach(x => masterData.push({id:Date.now()+Math.random(), n:x.trim(), t:t, p:p, i:"", m:"", o:"", s:"", e:""}));
            saveDb(); renderM(); mModal.hide();
        }
        function applyBulk() {
            const i=document.getElementById('b-isco').value, m=document.getElementById('b-memo').value, o=document.getElementById('b-ofp').value, s=document.getElementById('b-start').value, e=document.getElementById('b-end').value;
            document.querySelectorAll('.mcb:checked').forEach(cb=>{
                const idx = masterData.findIndex(x=>x.id==cb.value);
                if(idx!==-1) { if(i) masterData[idx].i=i; if(m) masterData[idx].m=m; if(o) masterData[idx].o=o; if(s) masterData[idx].s=s; if(e) masterData[idx].e=e; }
            });
            saveDb(); renderM(); bulkModal.hide(); document.getElementById('scb').checked=false; checkNotifs();
        }

        // Notifications
        function checkNotifs() {
            if(!currentUser) return; let count=0, html=''; const today = new Date(); today.setHours(0,0,0,0);
            masterData.forEach(x => {
                if(x.e) {
                    const diff = Math.ceil((new Date(x.e) - today) / (1000*3600*24));
                    if(diff <= 11 && diff >= 0) {
                        if(!readNotifs[currentUser.u].includes(x.id)) count++;
                        html += `<li class="notif-item text-end"><i class="fa fa-warning text-danger me-2"></i><strong>${x.n}</strong><br><small>OFP: ${x.o||'-'} | ينتهي بعد: <span class="text-danger fw-bold">${diff}</span> أيام</small></li>`;
                    }
                }
            });
            const b = document.getElementById('notif-count'); b.className = `position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger ${count>0?'':'d-none'}`; b.innerText=count;
            document.getElementById('notif-list').innerHTML = html || `<li class="p-3 text-center text-muted small">لا توجد إشعارات</li>`;
        }
        function readNotifs() {
            if(!currentUser) return; document.getElementById('notif-count').classList.add('d-none'); const today = new Date(); today.setHours(0,0,0,0);
            masterData.forEach(x => { if(x.e) { const diff = Math.ceil((new Date(x.e) - today) / (1000*3600*24)); if(diff<=11 && diff>=0 && !readNotifs[currentUser.u].includes(x.id)) readNotifs[currentUser.u].push(x.id); } });
            saveDb();
        }

        // Renders
        function namesHtml(i) { let h=''; if(i.w.length||i.e.length||i.l.length||i.wp.length){ h+=`<div class="names-preview">`; if(i.w.length)h+=`<strong>🚗 العجلات:</strong> ${i.w.join('، ')}<br>`; if(i.e.length)h+=`<strong>👤 الأجانب:</strong> ${i.e.join('، ')}<br>`; if(i.l.length)h+=`<strong>👨🏽 العراقيين:</strong> ${i.l.join('، ')}<br>`; if(i.wp.length)h+=`<strong>🔫 الأسلحة:</strong> ${i.wp.join('، ')}<br>`; h+=`</div>`;} return h;}
        function fileHtml(f) { return f?`<a href="#" onclick="openFile('${f}')" class="text-danger ms-1"><i class="fa fa-paperclip"></i></a>`:''; }
        function openFile(b) { let w=window.open(); w.document.write(`<iframe src="${b}" frameborder="0" style="border:0;width:100%;height:100%;"></iframe>`); }
        function clearF(t) { if(t==='auth'){['f-auth-bn','f-auth-list','f-auth-from','f-auth-to','f-auth-status'].forEach(x=>document.getElementById(x).value='');currPageA=1;}else{['f-perm-bn','f-perm-ofp','f-perm-list','f-perm-from','f-perm-to'].forEach(x=>document.getElementById(x).value='');currPageP=1;} render();}
        function pg(tot, curr, t) { const p=Math.ceil(tot/limit), u=document.getElementById(t==='auth'?'auth-pag':'perm-pag'); u.innerHTML=''; if(p<=1)return; u.innerHTML+=`<li class="page-item ${curr===1?'disabled':''}"><a class="page-link" onclick="currPage${t==='auth'?'A':'P'}=${curr-1};render()">السابق</a></li>`; for(let i=1;i<=p;i++) u.innerHTML+=`<li class="page-item ${curr===i?'active':''}"><a class="page-link" onclick="currPage${t==='auth'?'A':'P'}=${i};render()">${i}</a></li>`; u.innerHTML+=`<li class="page-item ${curr===p?'disabled':''}"><a class="page-link" onclick="currPage${t==='auth'?'A':'P'}=${curr+1};render()">التالي</a></li>`; }

        function render() {
            // Auth
            const abn=document.getElementById('f-auth-bn').value.toLowerCase(), al=document.getElementById('f-auth-list').value.toLowerCase(), af=document.getElementById('f-auth-from').value, at=document.getElementById('f-auth-to').value, as=document.getElementById('f-auth-status').value;
            const fa = authData.filter(i=>{ let md=true; if(af||at){const sd=i.s?new Date(i.s):null, rd=i.r?new Date(i.r):null, df=af?new Date(af):new Date('1900-01-01'), dt=at?new Date(at):new Date('2100-01-01'); md=(sd&&sd>=df&&sd<=dt)||(rd&&rd>=df&&rd<=dt);} return (i.b||'').toLowerCase().includes(abn) && (al===''||[...i.w,...i.e,...i.l,...i.wp].some(n=>n.toLowerCase().includes(al))) && (as===''?true:i.st===as) && md; }).reverse();
            const sa=(currPageA-1)*limit, pa=fa.slice(sa,sa+limit), abd=document.getElementById('auth-body'); abd.innerHTML='';
            pa.forEach(i=>{ abd.innerHTML+=`<tr><td><span class="badge bg-secondary shadow-sm">${i.ref}</span></td><td class="text-start"><strong style="color:var(--cr-blue);">${i.fb}</strong>${namesHtml(i)}</td><td><span class="badge bg-dark">${i.t}</span><br><small>${i.p}</small></td><td class="fw-bold">${i.m||'-'}${fileHtml(i.f)}</td><td><small>${i.s||'-'}</small></td><td><small>${i.r||'-'}</small></td><td><span class="badge border text-dark"><i class="fa fa-user"></i> ${i.c}</span></td><td><span class="badge ${i.st==='Done'?'bg-success':'bg-warning text-dark'}">${i.st}</span></td><td><div class="btn-group"><button class="btn btn-sm btn-light border" onclick="openModal('auth',${i.id})"><i class="fa fa-edit text-primary"></i></button><button class="btn btn-sm btn-light border" onclick="if(confirm('حذف؟')){authData=authData.filter(x=>x.id!=${i.id});saveDb();render();}"><i class="fa fa-trash text-danger"></i></button></div></td></tr>`; }); pg(fa.length, currPageA, 'auth');

            // Perm
            const pbn=document.getElementById('f-perm-bn').value.toLowerCase(), po=document.getElementById('f-perm-ofp').value.toLowerCase(), pl=document.getElementById('f-perm-list').value.toLowerCase(), pf=document.getElementById('f-perm-from').value, pt=document.getElementById('f-perm-to').value;
            const fp = permData.filter(i=>{ let md=true; if(pf||pt){const sd=i.s?new Date(i.s):null, rd=i.r?new Date(i.r):null, df=pf?new Date(pf):new Date('1900-01-01'), dt=pt?new Date(pt):new Date('2100-01-01'); md=(sd&&sd>=df&&sd<=dt)||(rd&&rd>=df&&rd<=dt);} return (i.b||'').toLowerCase().includes(pbn) && ((i.o||'')+" "+(i.m||'')).toLowerCase().includes(po) && (pl===''||[...i.w,...i.e,...i.l,...i.wp].some(n=>n.toLowerCase().includes(pl))) && md; }).reverse();
            const sp=(currPageP-1)*limit, pp=fp.slice(sp,sp+limit), pbd=document.getElementById('perm-body'); pbd.innerHTML='';
            pp.forEach(i=>{ pbd.innerHTML+=`<tr><td><span class="badge bg-secondary shadow-sm mb-1">${i.ref}</span><br><strong class="text-primary small">${i.o||'-'}</strong></td><td class="text-start"><strong style="color:var(--cr-blue);">${i.fb}</strong>${namesHtml(i)}</td><td><span class="badge bg-primary">${i.t}</span><br><small>${i.p}</small></td><td class="fw-bold">${i.m||'-'}${fileHtml(i.f)}</td><td><small>${i.s||'-'}</small></td><td><small>${i.r||'-'}</small></td><td><span class="badge border text-dark"><i class="fa fa-user"></i> ${i.c}</span></td><td><span class="badge ${i.st==='Completed'?'bg-success':'bg-warning text-dark'}">${i.st}</span></td><td><div class="btn-group"><button class="btn btn-sm btn-light border" onclick="openModal('permits',${i.id})"><i class="fa fa-edit text-primary"></i></button><button class="btn btn-sm btn-light border" onclick="if(confirm('حذف؟')){permData=permData.filter(x=>x.id!=${i.id});saveDb();render();}"><i class="fa fa-trash text-danger"></i></button></div></td></tr>`; }); pg(fp.length, currPageP, 'permits');
            
            // Users
            const utb=document.getElementById('utb'); utb.innerHTML=''; users.forEach((u,i)=>{ utb.innerHTML+=`<tr><td class="fw-bold">${u.u}</td><td>${u.p}</td><td><span class="badge ${u.r==='admin'?'bg-danger':'bg-primary'}">${u.r}</span></td><td>${u.r!=='admin'?`<button class="btn btn-sm btn-light border" onclick="document.getElementById('nu').value='${u.u}';document.getElementById('np').value='${u.p}';document.getElementById('edit-u-idx').value=${i};document.getElementById('btn-u').innerText='تحديث';"><i class="fa fa-edit text-warning"></i></button> <button class="btn btn-sm btn-light border" onclick="users.splice(${i},1);saveDb();render();"><i class="fa fa-trash text-danger"></i></button>`:'-'}</td></tr>`; });
        }

        function renderM() {
            const fn=document.getElementById('f-m-name').value.toLowerCase(), fo=document.getElementById('f-m-ofp').value.toLowerCase(), fp=document.getElementById('f-m-proj').value, fe=document.getElementById('f-m-end').value, today=new Date(); today.setHours(0,0,0,0);
            const f = masterData.filter(x => x.t===currTabM && x.n.toLowerCase().includes(fn) && (x.o||'').toLowerCase().includes(fo) && (fp===''?true:x.p===fp) && (fe===''?true:x.e===fe)).reverse();
            const mb = document.getElementById('master-body'); mb.innerHTML='';
            f.forEach(i=>{ let sb='<span class="badge bg-secondary">غير مفعل</span>'; if(i.e){ const d=Math.ceil((new Date(i.e)-today)/(1000*3600*24)); if(d<0)sb='<span class="badge bg-danger">منتهي</span>'; else if(d<=10)sb=`<span class="badge bg-warning text-dark">ينتهي بعد ${d} يوم</span>`; else sb='<span class="badge bg-success">فعال</span>'; } mb.innerHTML+=`<tr><td><input type="checkbox" class="mcb form-check-input" value="${i.id}"></td><td class="text-primary fw-bold text-start">${i.n}</td><td><span class="badge bg-dark">${i.p}</span></td><td class="fw-bold">${i.i||'-'}</td><td>${i.m||'-'}</td><td class="text-success fw-bold">${i.o||'-'}</td><td><small>${i.s||'-'}</small></td><td><small>${i.e||'-'}</small></td><td>${sb}</td><td><button class="btn btn-sm btn-outline-danger" onclick="if(confirm('حذف؟')){masterData=masterData.filter(x=>x.id!=${i.id});saveDb();renderM();}"><i class="fa fa-trash"></i></button></td></tr>`; });
        }

        function addU() { const u=document.getElementById('nu').value, p=document.getElementById('np').value, x=parseInt(document.getElementById('edit-u-idx').value); if(!u||!p)return; if(x!==-1){users[x].u=u; users[x].p=p; document.getElementById('edit-u-idx').value=-1; document.getElementById('btn-u').innerText="حفظ";}else users.push({u,p,r:'user'}); document.getElementById('nu').value=''; document.getElementById('np').value=''; saveDb(); render(); }
        function exportD(t) { const d=t==='auth'?authData:permData, ws=XLSX.utils.json_to_sheet(d.map(i=>({Ref:i.ref, Book_No:i.b, Full_Name:i.fb, Authority:i.ta, Type:i.pt, Memo:i.m||'', OFP:i.o||'', Sent:i.s||'', Received:i.r||'', Status:i.st, Creator:i.c, Wheels:i.w.join(' | '), Expats:i.e.join(' | '), Locals:i.l.join(' | '), Weapons:i.wp.join(' | ')}))), wb=XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb,ws,"Data"); XLSX.writeFile(wb, `ControlRisks_${t}.xlsx`); }
        function exportM() { const ws=XLSX.utils.json_to_sheet(masterData.map(i=>({Type:i.t==='e'?'Expat':(i.t==='l'?'Local':'Vehicle'), Name:i.n, Project:i.p, ISCO:i.i, Memo:i.m, OFP:i.o, Start:i.s, End:i.e}))), wb=XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb,ws,"Master"); XLSX.writeFile(wb, `MasterDB.xlsx`); }
        function toggleLanguage() { lang=lang==='ar'?'en':'ar'; document.getElementById('html-tag').dir=lang==='en'?'ltr':'rtl'; document.body.className=lang==='en'?'ltr-mode text-start':'rtl-mode text-start'; }
    </script>
</body>
</html>
