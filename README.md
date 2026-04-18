<!DOCTYPE html>
<html lang="ar" dir="rtl" id="html-tag">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Risks | OFP Tracker PRO</title>
    <!-- Bootstrap & Fonts -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" id="bootstrap-rtl">
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <style>
        :root { --cr-olive: #3D4928; --cr-olive-light: #556B2F; --cr-dark: #1a1a1a; --cr-light: #f4f7f1; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--cr-light); margin: 0; transition: all 0.3s ease; display: flex; flex-direction: column; min-height: 100vh; }
        
        #sidebar { width: 260px; background-color: var(--cr-dark); color: white; position: fixed; height: 100%; z-index: 1000; transition: 0.3s; }
        .sidebar-header { background: var(--cr-olive); padding: 30px 15px; text-align: center; border-bottom: 2px solid rgba(255,255,255,0.1); }
        .sidebar-header h5 { color: white; margin: 0; font-weight: bold; letter-spacing: 1px; }
        #sidebar ul { list-style: none; padding: 0; margin-top: 10px; }
        #sidebar ul li a { padding: 15px 20px; display: block; color: #cbd5e0; text-decoration: none; border-bottom: 1px solid #2d3748; cursor: pointer; transition: 0.3s; border-right: 4px solid transparent; }
        #sidebar ul li a:hover, #sidebar ul li a.active { background: var(--cr-olive-light); color: white; border-right: 4px solid white; padding-right: 25px; }
        
        .content { margin-right: 260px; flex: 1; transition: 0.3s; display: flex; flex-direction: column; }
        .top-navbar { background: white; padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        
        .btn-cr { background-color: var(--cr-olive); color: white; border: none; transition: 0.3s; font-weight: bold;}
        .btn-cr:hover { background-color: var(--cr-olive-light); transform: translateY(-2px); box-shadow: 0 4px 8px rgba(0,0,0,0.2); color: white; }
        
        .custom-card { background: white; border-radius: 12px; padding: 20px; box-shadow: 0 4px 20px rgba(0,0,0,0.04); margin-bottom: 20px; }
        
        #login-page { height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--cr-dark) 0%, var(--cr-olive) 100%); position: fixed; width: 100%; z-index: 2000; top: 0; left: 0; }
        .login-card { background: white; padding: 50px; border-radius: 20px; width: 100%; max-width: 420px; text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .login-card h2 { color: var(--cr-olive); font-weight: bold; margin-bottom: 5px; }
        
        .ltr-mode .content { margin-right: 0; margin-left: 260px; }
        .ltr-mode #sidebar { right: auto; left: 0; }
        .ref-badge { font-size: 0.75rem; padding: 2px 8px; border-radius: 4px; background: #f0f4e8; color: var(--cr-olive); border: 1px solid var(--cr-olive); font-weight: bold; }
        
        .filter-box { background: #f8f9fa; padding: 10px 15px; border-radius: 6px; border: 1px solid #dee2e6; margin-bottom: 15px; }
        .filter-box .form-control, .filter-box .form-select { font-size: 0.8rem; padding: 4px 8px; height: 32px; }
        .filter-label { font-size: 0.75rem; color: #6c757d; margin-bottom: 2px; display: block; font-weight: bold; }

        .names-preview { font-size: 0.75rem; background: #fafafa; border: 1px solid #eee; padding: 8px; border-radius: 6px; margin-top: 8px; max-height: 90px; overflow-y: auto; color: #444; line-height: 1.6; text-align: start; }
        
        /* Pagination */
        .pagination .page-link { color: var(--cr-olive); font-weight: bold; cursor: pointer; }
        .pagination .page-item.active .page-link { background-color: var(--cr-olive); border-color: var(--cr-olive); color: white; }

        /* Notifications */
        .notification-dropdown { min-width: 300px; max-height: 400px; overflow-y: auto; box-shadow: 0 10px 30px rgba(0,0,0,0.1); border: 1px solid #eee; }
        .notif-item { padding: 10px 15px; border-bottom: 1px solid #eee; font-size: 0.85rem; text-align: right; }
        .notif-item:hover { background-color: #f8f9fa; }

        /* Footer */
        .cr-footer { background-color: var(--cr-olive); color: white; padding: 15px; text-align: center; margin-top: auto; font-size: 0.85rem; }
    </style>
</head>
<body class="rtl-mode text-start">

    <!-- صفحة تسجيل الدخول -->
    <div id="login-page">
        <div class="login-card shadow-lg">
            <h2>CONTROL RISKS</h2>
            <p class="text-muted mb-4 fw-bold">OFP Application Tracker</p>
            <div id="login-error" class="alert alert-danger p-2 small fw-bold" style="display: none;"><i class="fa fa-exclamation-circle"></i> اليوزر أو الرمز خطأ.</div>
            <div class="mb-3 text-start"><label class="fw-bold small">اسم المستخدم</label><input type="text" id="userInput" class="form-control form-control-lg"></div>
            <div class="mb-4 text-start"><label class="fw-bold small">كلمة المرور</label><input type="password" id="passInput" class="form-control form-control-lg" onkeypress="if(event.key === 'Enter') loginSystem()"></div>
            <button onclick="loginSystem()" class="btn btn-cr w-100 p-3 fs-5">دخول</button>
            <div class="mt-4"><a href="#" class="text-muted small text-decoration-none" onclick="alert('compad.southiraq@controlrisks.com')">نسيت كلمة السر؟</a></div>
        </div>
    </div>

    <!-- الواجهة الرئيسية -->
    <div id="main-dashboard" style="display: none;">
        <nav id="sidebar">
            <div class="sidebar-header"><h5>CONTROL RISKS</h5></div>
            <ul>
                <li><a onclick="showTab('auth')" id="link-auth" class="active"><i class="fa fa-folder-open ms-2"></i> الطلبات للهيئات</a></li>
                <li><a onclick="showTab('permits')" id="link-permits"><i class="fa fa-id-card-clip ms-2"></i> التصاريح المستلمة</a></li>
                <li id="admin-menu" style="display: none;"><a onclick="showTab('users')" id="link-users"><i class="fa fa-users-gear ms-2"></i> إدارة الحسابات</a></li>
                <li><a onclick="logout()"><i class="fa fa-power-off ms-2"></i> خروج</a></li>
            </ul>
        </nav>

        <div class="content">
            <header class="top-navbar">
                <div class="d-flex align-items-center">
                    <button class="btn btn-sm btn-outline-secondary fw-bold ms-3" onclick="toggleLanguage()">EN / AR</button>
                    <!-- الإشعارات -->
                    <div class="dropdown">
                        <button class="btn btn-light position-relative border" data-bs-toggle="dropdown" aria-expanded="false" onclick="openNotifications()">
                            <i class="fa fa-bell text-warning fs-5"></i>
                            <span id="notif-count" class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger" style="display: none;">0</span>
                        </button>
                        <ul id="notif-list" class="dropdown-menu notification-dropdown text-end p-0">
                            <li class="p-3 text-center text-muted small fw-bold">لا توجد تصاريح قريبة الانتهاء</li>
                        </ul>
                    </div>
                </div>
                <div><span class="text-muted small">مرحباً:</span> <strong id="user-display" style="color: var(--cr-olive);"></strong> <i class="fa fa-user-circle ms-1"></i></div>
            </header>

            <div class="p-4" style="flex: 1;">
                <!-- قسم الهيئات -->
                <div id="auth-section">
                    <div class="d-flex justify-content-between mb-3 align-items-center">
                        <h3 class="fw-bold mb-0">سجل الطلبات للهيئات</h3>
                        <button class="btn btn-cr shadow-sm px-4" onclick="openModal('auth')"><i class="fa fa-plus me-1"></i> إضافة طلب</button>
                    </div>
                    <div class="filter-box row g-2 align-items-end">
                        <div class="col-md-2"><span class="filter-label">رقم الكتاب</span><input type="text" id="f-auth-bn" class="form-control" onkeyup="currPageAuth=1;render()"></div>
                        <div class="col-md-3"><span class="filter-label">البحث بالأسماء</span><input type="text" id="f-auth-list" class="form-control" onkeyup="currPageAuth=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">من تاريخ</span><input type="date" id="f-auth-from" class="form-control" onchange="currPageAuth=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">إلى تاريخ</span><input type="date" id="f-auth-to" class="form-control" onchange="currPageAuth=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">الحالة</span><select id="f-auth-status" class="form-select" onchange="currPageAuth=1;render()"><option value="">الكل</option><option value="Pending">Pending</option><option value="Done">Completed</option></select></div>
                        <div class="col-md-1"><button class="btn btn-secondary w-100" onclick="clearFilters('auth')"><i class="fa fa-eraser"></i></button></div>
                    </div>
                    <div class="custom-card table-responsive">
                        <table class="table align-middle text-center">
                            <thead class="table-dark"><tr><th>Ref</th><th style="width:25%;">الكتاب والأسماء</th><th>الهيئة/النوع</th><th>المذكرة</th><th>إرسال</th><th>استلام</th><th>بواسطة</th><th>الحالة</th><th>إجراء</th></tr></thead>
                            <tbody id="auth-body"></tbody>
                        </table>
                        <nav class="mt-3 d-flex justify-content-between align-items-center">
                            <button class="btn btn-success btn-sm shadow-sm" onclick="exportData('auth')"><i class="fa fa-file-excel"></i> تصدير</button>
                            <ul class="pagination pagination-sm mb-0" id="auth-pagination"></ul>
                        </nav>
                    </div>
                </div>

                <!-- قسم التصاريح -->
                <div id="permits-section" style="display: none;">
                    <h3 class="fw-bold mb-3" style="color: var(--cr-olive);">التصاريح المستلمة</h3>
                    <div class="filter-box row g-2 align-items-end">
                        <div class="col-md-2"><span class="filter-label">رقم الكتاب</span><input type="text" id="f-perm-bn" class="form-control" onkeyup="currPagePerm=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">OFP/المذكرة</span><input type="text" id="f-perm-ofp" class="form-control" onkeyup="currPagePerm=1;render()"></div>
                        <div class="col-md-3"><span class="filter-label">البحث بالأسماء</span><input type="text" id="f-perm-list" class="form-control" onkeyup="currPagePerm=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">من تاريخ</span><input type="date" id="f-perm-from" class="form-control" onchange="currPagePerm=1;render()"></div>
                        <div class="col-md-2"><span class="filter-label">إلى تاريخ</span><input type="date" id="f-perm-to" class="form-control" onchange="currPagePerm=1;render()"></div>
                        <div class="col-md-1"><button class="btn btn-secondary w-100" onclick="clearFilters('perm')"><i class="fa fa-eraser"></i></button></div>
                    </div>
                    <div class="custom-card table-responsive">
                        <table class="table table-bordered align-middle text-center">
                            <thead class="table-success"><tr><th>Ref/OFP</th><th style="width:25%;">التصريح والأسماء</th><th>الهيئة/النوع</th><th>المذكرة</th><th>إرسال</th><th>استلام</th><th>بواسطة</th><th>الحالة</th><th>إجراء</th></tr></thead>
                            <tbody id="perm-body"></tbody>
                        </table>
                        <nav class="mt-3 d-flex justify-content-between align-items-center">
                            <button class="btn btn-success btn-sm shadow-sm" onclick="exportData('perm')"><i class="fa fa-file-excel"></i> تصدير</button>
                            <ul class="pagination pagination-sm mb-0" id="perm-pagination"></ul>
                        </nav>
                    </div>
                </div>

                <!-- قسم الحسابات -->
                <div id="users-section" style="display: none;">
                    <h3 class="fw-bold mb-4">إدارة الحسابات</h3>
                    <div class="custom-card">
                        <div class="row g-2 mb-4 bg-light p-3 rounded border">
                            <input type="hidden" id="edit-user-idx" value="-1">
                            <div class="col-md-4"><label class="small fw-bold">اسم المستخدم</label><input id="nu" class="form-control"></div>
                            <div class="col-md-4"><label class="small fw-bold">كلمة المرور</label><input id="np" class="form-control"></div>
                            <div class="col-md-4 d-flex align-items-end"><button id="btn-save-user" class="btn btn-cr w-100 fw-bold" onclick="handleUser()">حفظ الحساب</button></div>
                        </div>
                        <table class="table table-striped border text-center">
                            <thead class="table-dark"><tr><th>المستخدم</th><th>الباسورد</th><th>الرتبة</th><th>الإجراء</th></tr></thead>
                            <tbody id="utb"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Footer -->
            <footer class="cr-footer">
                &copy; 2024 Control Risks - OFP Application Tracker. All Rights Reserved.
            </footer>
        </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="mainModal" tabindex="-1">
        <div class="modal-dialog modal-xl">
            <div class="modal-content border-0 shadow-lg">
                <div class="modal-header text-white" style="background: var(--cr-olive);"><h5 class="modal-title fw-bold">بيانات الطلب</h5><button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button></div>
                <div class="modal-body p-4 text-start">
                    <input type="hidden" id="form-type"><input type="hidden" id="edit-id">
                    <div class="row g-3">
                        <div class="col-md-3"><label class="fw-bold small">رقم الكتاب</label><input type="text" id="m-bn" class="form-control" required></div>
                        <div class="col-md-3"><label class="fw-bold small">نوع التصريح</label><select id="m-pt" class="form-select"><option value="مؤقت">مؤقت</option><option value="دائمي">دائمي</option></select></div>
                        <div class="col-md-3"><label class="fw-bold small">الهيئة</label><select id="m-ta" class="form-select"><option value="BGC">BGC</option><option value="ROO">ROO</option><option value="SRC">SRC</option></select></div>
                        <div class="col-md-3" id="ofp-box"><label class="fw-bold small text-primary">OFP Number</label><input type="text" id="m-ofp" class="form-control"></div>
                        <div class="col-md-3"><label class="fw-bold small">إرسال</label><input type="date" id="m-sd" class="form-control"></div>
                        <div class="col-md-3"><label class="fw-bold small">استلام</label><input type="date" id="m-rd" class="form-control"></div>
                        <div class="col-md-3"><label class="fw-bold small">رقم المذكرة</label><input type="text" id="m-memo" class="form-control"></div>
                        <div class="col-md-3"><label class="fw-bold small">مرفق (اختياري)</label><input type="file" id="m-file" class="form-control"></div>
                        <div class="col-12 mt-4"><div class="alert alert-secondary border small text-center fw-bold">انسخ القوائم من الوورد والصقها مباشرة هنا</div></div>
                        <div class="col-md-3"><label class="fw-bold small">العجلات</label><textarea id="m-w" class="form-control" rows="4"></textarea></div>
                        <div class="col-md-3"><label class="fw-bold small">الأجانب</label><textarea id="m-e" class="form-control" rows="4"></textarea></div>
                        <div class="col-md-3"><label class="fw-bold small">العراقيين</label><textarea id="m-l" class="form-control" rows="4"></textarea></div>
                        <div class="col-md-3"><label class="fw-bold small">الأسلحة</label><textarea id="m-wp" class="form-control" rows="4"></textarea></div>
                    </div>
                </div>
                <div class="modal-footer"><button onclick="saveOrder()" class="btn btn-cr px-5">حفظ</button></div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        let authData = JSON.parse(localStorage.getItem('CR_AUTH')) || [];
        let permData = JSON.parse(localStorage.getItem('CR_PERM')) || [];
        let users = JSON.parse(localStorage.getItem('CR_USERS')) || [{u:'admin', p:'1234', r:'admin'}];
        let readNotifs = JSON.parse(localStorage.getItem('CR_NOTIFS')) || {};
        
        let currentUser = null; let lang = 'ar';
        const modal = new bootstrap.Modal(document.getElementById('mainModal'));
        
        let currPageAuth = 1; let currPagePerm = 1; const rowsPerPage = 15;

        function saveToStorage() {
            try { 
                localStorage.setItem('CR_AUTH', JSON.stringify(authData)); 
                localStorage.setItem('CR_PERM', JSON.stringify(permData)); 
                localStorage.setItem('CR_USERS', JSON.stringify(users)); 
                localStorage.setItem('CR_NOTIFS', JSON.stringify(readNotifs)); 
            } catch(e) { alert("⚠️ الذاكرة ممتلئة! يرجى حذف بعض الملفات المرفقة."); }
        }

        function loginSystem() {
            const u = document.getElementById('userInput').value, p = document.getElementById('passInput').value;
            const f = users.find(x => x.u === u && x.p === p);
            if(f) {
                currentUser = f; 
                document.getElementById('login-page').style.display = 'none'; 
                document.getElementById('main-dashboard').style.display = 'block';
                document.getElementById('user-display').innerText = f.u; 
                if(f.r === 'admin') document.getElementById('admin-menu').style.display = 'block';
                if(!readNotifs[currentUser.u]) readNotifs[currentUser.u] = [];
                render(); 
                checkNotifications();
            } else document.getElementById('login-error').style.display = 'block';
        }

        function showTab(t) {
            ['auth','permits','users'].forEach(s => document.getElementById(s+'-section').style.display = 'none');
            document.getElementById(t+'-section').style.display = 'block';
            document.querySelectorAll('#sidebar a').forEach(a => a.classList.remove('active'));
            document.getElementById('link-'+t).classList.add('active');
        }

        function logout() { location.reload(); }

        function openModal(type, id = null) {
            document.getElementById('m-bn').value=""; document.getElementById('m-pt').value="مؤقت"; document.getElementById('m-ta').value="BGC"; document.getElementById('m-memo').value=""; 
            document.getElementById('m-ofp').value=""; document.getElementById('m-sd').value=""; document.getElementById('m-rd').value=""; document.getElementById('m-w').value=""; 
            document.getElementById('m-e').value=""; document.getElementById('m-l').value=""; document.getElementById('m-wp').value=""; document.getElementById('m-file').value="";
            document.getElementById('form-type').value = type; document.getElementById('edit-id').value = id || "";
            document.getElementById('ofp-box').style.display = (type === 'permits') ? 'block' : 'none';
            document.getElementById('m-file').removeAttribute('data-file');

            if(id) {
                const item = (type==='auth'?authData:permData).find(x => x.id == id);
                if(item){
                    document.getElementById('m-bn').value = item.bn_orig; document.getElementById('m-pt').value = item.pt; document.getElementById('m-ta').value = item.ta;
                    document.getElementById('m-memo').value = item.memo; document.getElementById('m-ofp').value = item.ofp || ""; document.getElementById('m-sd').value = item.sd;
                    document.getElementById('m-rd').value = item.rd; document.getElementById('m-w').value = item.w.join('\n'); document.getElementById('m-e').value = item.e.join('\n');
                    document.getElementById('m-l').value = item.l.join('\n'); document.getElementById('m-wp').value = item.wp.join('\n');
                    if(item.file) document.getElementById('m-file').setAttribute('data-file', item.file);
                }
            } modal.show();
        }

        async function saveOrder() {
            const bnInput = document.getElementById('m-bn').value; if(!bnInput) return alert("يرجى إدخال رقم الكتاب");
            const type = document.getElementById('form-type').value, id = document.getElementById('edit-id').value;
            const getL = (eid) => document.getElementById(eid).value.split('\n').filter(l => l.trim() !== "");
            let ex = id ? (type==='auth'?authData:permData).find(x=>x.id==id) : null;
            
            const fInput = document.getElementById('m-file'); let fileBase64 = fInput.getAttribute('data-file') || null;
            if(fInput.files.length > 0) fileBase64 = await new Promise(r => { const rd = new FileReader(); rd.onload=()=>r(rd.result); rd.readAsDataURL(fInput.files[0]); });

            const entry = {
                id: id ? parseInt(id) : Date.now(), bn_orig: bnInput, pt: document.getElementById('m-pt').value, ta: document.getElementById('m-ta').value,
                memo: document.getElementById('m-memo').value, ofp: document.getElementById('m-ofp').value, sd: document.getElementById('m-sd').value, rd: document.getElementById('m-rd').value,
                w: getL('m-w'), e: getL('m-e'), l: getL('m-l'), wp: getL('m-wp'), creator: ex ? ex.creator : currentUser.u, ref: ex ? ex.ref : 'REF-'+Math.floor(1000+Math.random()*9000), file: fileBase64
            };

            const stats = `${entry.e.length} أجانب - ${entry.w.length} عجلات - ${entry.l.length} عراقيين`;
            if(type === 'auth') {
                entry.full_bn = `TEMP - ${entry.bn_orig} - ${entry.pt} - ${stats}`; entry.status = entry.rd ? 'Done' : 'Pending';
                if(id) { authData[authData.findIndex(x=>x.id == id)] = entry; let pIdx = permData.findIndex(p => p.ref === entry.ref);
                    if(pIdx !== -1) { permData[pIdx].pt=entry.pt; permData[pIdx].ta=entry.ta; permData[pIdx].w=entry.w; permData[pIdx].e=entry.e; permData[pIdx].l=entry.l; permData[pIdx].wp=entry.wp; permData[pIdx].full_bn = `ISCO - ${permData[pIdx].bn_orig} - ${entry.pt} - ${stats}`; }
                } else authData.push(entry);

                if(entry.status === 'Done' && !permData.find(p=>p.ref === entry.ref)) { let copy = JSON.parse(JSON.stringify(entry)); copy.id = Date.now()+1; copy.bn_orig = ""; copy.full_bn = ""; copy.ofp = ""; copy.sd = ""; copy.rd = ""; copy.status = "Pending"; permData.push(copy); }
            } else {
                entry.status = entry.rd ? 'Completed' : 'Pending'; entry.full_bn = `ISCO - ${entry.bn_orig} - ${entry.pt} - ${stats}`;
                permData[permData.findIndex(x=>x.id == id)] = entry;
            }
            saveToStorage(); render(); checkNotifications(); modal.hide();
        }

        function checkNotifications() {
            if(!currentUser) return;
            let alerts = []; let unreadCount = 0; const today = new Date();
            
            permData.forEach(p => {
                if(p.rd) {
                    const rDate = new Date(p.rd); const diffTime = Math.abs(today - rDate); const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                    const daysLeft = 21 - diffDays;
                    if(daysLeft <= 10 && daysLeft >= 0) {
                        alerts.push({id: p.id, bn: p.full_bn, left: daysLeft});
                        if(!readNotifs[currentUser.u].includes(p.id)) unreadCount++;
                    }
                }
            });
            const b = document.getElementById('notif-count'); const l = document.getElementById('notif-list');
            if(alerts.length > 0) {
                if(unreadCount > 0) { b.style.display = 'inline-block'; b.innerText = unreadCount; } else { b.style.display = 'none'; }
                l.innerHTML = '';
                alerts.forEach(a => l.innerHTML += `<li class="notif-item"><i class="fa fa-exclamation-triangle text-danger me-2"></i><strong>${a.bn}</strong><br><small class="text-muted">ينتهي بعد: <span class="text-danger fw-bold">${a.left} أيام</span></small></li>`);
            } else { b.style.display = 'none'; l.innerHTML = `<li class="p-3 text-center text-muted small fw-bold">لا توجد إشعارات</li>`; }
        }

        // دالة تفتح القائمة وتخفي الإشعار فوراً وتسجله بالذاكرة
        function openNotifications() {
            if(!currentUser) return;
            
            // إخفاء العداد الأحمر فوراً عبر Style
            document.getElementById('notif-count').style.display = 'none';
            
            const today = new Date();
            permData.forEach(p => {
                if(p.rd) {
                    const diffDays = Math.ceil(Math.abs(today - new Date(p.rd)) / (1000 * 60 * 60 * 24));
                    if((21 - diffDays) <= 10 && (21 - diffDays) >= 0) {
                        if(!readNotifs[currentUser.u].includes(p.id)) readNotifs[currentUser.u].push(p.id);
                    }
                }
            });
            saveToStorage();
        }

        function generateNamesPreview(i) {
            let html = '';
            if(i.w.length>0||i.e.length>0||i.l.length>0||i.wp.length>0) {
                html += `<div class="names-preview">`;
                if(i.w.length>0) html+=`<strong>🚗 العجلات:</strong> ${i.w.join('، ')}<br>`; if(i.e.length>0) html+=`<strong>👤 الأجانب:</strong> ${i.e.join('، ')}<br>`;
                if(i.l.length>0) html+=`<strong>👨🏽 العراقيين:</strong> ${i.l.join('، ')}<br>`; if(i.wp.length>0) html+=`<strong>🔫 الأسلحة:</strong> ${i.wp.join('، ')}<br>`;
                html += `</div>`;
            } return html;
        }

        function getFilteredData(data, type) {
            const bn = document.getElementById(type==='auth'?'f-auth-bn':'f-perm-bn').value.toLowerCase();
            const list = document.getElementById(type==='auth'?'f-auth-list':'f-perm-list').value.toLowerCase();
            const from = document.getElementById(type==='auth'?'f-auth-from':'f-perm-from').value;
            const to = document.getElementById(type==='auth'?'f-auth-to':'f-perm-to').value;
            const status = type==='auth' ? document.getElementById('f-auth-status').value : null;
            const ofp = type==='permits' ? document.getElementById('f-perm-ofp').value.toLowerCase() : '';

            return data.filter(i => {
                const mBn = (i.bn_orig||'').toLowerCase().includes(bn);
                const mList = list==='' || [...i.w,...i.e,...i.l,...i.wp].some(n=>n.toLowerCase().includes(list));
                const mStatus = (type==='auth' && status!=='') ? i.status === status : true;
                const mOfp = type==='permits' ? ((i.ofp||'')+" "+(i.memo||'')).toLowerCase().includes(ofp) : true;
                let mDate = true;
                if(from||to) { const sd = i.sd?new Date(i.sd):null; const rd = i.rd?new Date(i.rd):null; const df = from?new Date(from):new Date('1900-01-01'); const dt = to?new Date(to):new Date('2100-01-01'); mDate = (sd&&sd>=df&&sd<=dt)||(rd&&rd>=df&&rd<=dt); }
                return mBn && mList && mStatus && mOfp && mDate;
            });
        }

        function renderPagination(total, currPage, type) {
            const totalPages = Math.ceil(total / rowsPerPage); const ul = document.getElementById(type==='auth'?'auth-pagination':'perm-pagination'); ul.innerHTML = '';
            if(totalPages <= 1) return;
            ul.innerHTML += `<li class="page-item ${currPage===1?'disabled':''}"><a class="page-link" onclick="${type==='auth'?'currPageAuth':'currPagePerm'}=${currPage-1};render()">السابق</a></li>`;
            for(let i=1; i<=totalPages; i++) ul.innerHTML += `<li class="page-item ${currPage===i?'active':''}"><a class="page-link" onclick="${type==='auth'?'currPageAuth':'currPagePerm'}=${i};render()">${i}</a></li>`;
            ul.innerHTML += `<li class="page-item ${currPage===totalPages?'disabled':''}"><a class="page-link" onclick="${type==='auth'?'currPageAuth':'currPagePerm'}=${currPage+1};render()">التالي</a></li>`;
        }

        function render() {
            // Auth Render
            const fAuth = getFilteredData(authData, 'auth').reverse();
            const startAuth = (currPageAuth - 1) * rowsPerPage; const paginatedAuth = fAuth.slice(startAuth, startAuth + rowsPerPage);
            const ab = document.getElementById('auth-body'); ab.innerHTML = '';
            paginatedAuth.forEach(i => {
                const sBadge = i.status==='Done'?'<span class="badge bg-success">Completed</span>':'<span class="badge bg-warning text-dark">Pending</span>';
                const fileIco = i.file?`<a href="#" onclick="openFile('${i.file}')" class="text-danger ms-1"><i class="fa fa-paperclip fa-lg"></i></a>`:'';
                ab.innerHTML += `<tr><td><span class="badge bg-secondary shadow-sm">${i.ref}</span></td><td class="text-start"><span class="fw-bold" style="color:var(--cr-olive);">${i.full_bn}</span>${generateNamesPreview(i)}</td><td><span class="badge bg-dark">${i.ta}</span><br><small class="fw-bold text-muted">${i.pt}</small></td><td class="fw-bold">${i.memo||'-'}${fileIco}</td><td><small>${i.sd||'-'}</small></td><td><small>${i.rd||'-'}</small></td><td><span class="badge bg-light text-dark border"><i class="fa fa-user me-1"></i> ${i.creator}</span></td><td>${sBadge}</td><td><div class="btn-group"><button class="btn btn-sm btn-light border" onclick="openModal('auth',${i.id})"><i class="fa fa-edit text-primary"></i></button><button class="btn btn-sm btn-light border" onclick="deleteRec('auth',${i.id})"><i class="fa fa-trash text-danger"></i></button></div></td></tr>`;
            });
            renderPagination(fAuth.length, currPageAuth, 'auth');

            // Perm Render
            const fPerm = getFilteredData(permData, 'permits').reverse();
            const startPerm = (currPagePerm - 1) * rowsPerPage; const paginatedPerm = fPerm.slice(startPerm, startPerm + rowsPerPage);
            const pb = document.getElementById('perm-body'); pb.innerHTML = '';
            paginatedPerm.forEach(i => {
                const sBadge = i.status==='Completed'?'<span class="badge bg-success">Completed</span>':'<span class="badge bg-warning text-dark">Pending</span>';
                const fileIco = i.file?`<a href="#" onclick="openFile('${i.file}')" class="text-danger ms-1"><i class="fa fa-paperclip fa-lg"></i></a>`:'';
                pb.innerHTML += `<tr><td><span class="badge bg-secondary shadow-sm mb-1">${i.ref}</span><br><strong class="text-primary small">${i.ofp||'-'}</strong></td><td class="text-start"><span class="fw-bold" style="color:var(--cr-olive);">${i.full_bn}</span>${generateNamesPreview(i)}</td><td><span class="badge bg-dark">${i.ta}</span><br><small class="fw-bold text-muted">${i.pt}</small></td><td class="fw-bold">${i.memo||'-'}${fileIco}</td><td><small>${i.sd||'-'}</small></td><td><small>${i.rd||'-'}</small></td><td><span class="badge bg-light text-dark border"><i class="fa fa-user me-1"></i> ${i.creator}</span></td><td>${sBadge}</td><td><div class="btn-group"><button class="btn btn-sm btn-light border" onclick="openModal('permits',${i.id})"><i class="fa fa-edit text-primary"></i></button><button class="btn btn-sm btn-light border" onclick="deleteRec('perm',${i.id})"><i class="fa fa-trash text-danger"></i></button></div></td></tr>`;
            });
            renderPagination(fPerm.length, currPagePerm, 'permits');
            renderUsers();
        }

        function deleteRec(t, id) { if(!confirm("حذف؟")) return; if(t==='auth') authData = authData.filter(x=>x.id!=id); else permData = permData.filter(x=>x.id!=id); saveToStorage(); render(); }
        function openFile(b64) { let win = window.open(); win.document.write(`<iframe src="${b64}" frameborder="0" style="border:0; top:0; left:0; bottom:0; right:0; width:100%; height:100%;" allowfullscreen></iframe>`); }
        function clearFilters(t) { if(t==='auth'){ ['f-auth-bn','f-auth-list','f-auth-from','f-auth-to','f-auth-status'].forEach(id=>document.getElementById(id).value=''); currPageAuth=1; } else { ['f-perm-bn','f-perm-ofp','f-perm-list','f-perm-from','f-perm-to'].forEach(id=>document.getElementById(id).value=''); currPagePerm=1; } render(); }

        function renderUsers() {
            const utb = document.getElementById('utb'); utb.innerHTML = '';
            users.forEach((u, i) => { utb.innerHTML += `<tr><td class="fw-bold">${u.u}</td><td>${u.p}</td><td><span class="badge ${u.r==='admin'?'bg-danger':'bg-primary'}">${u.r}</span></td><td>${u.r!=='admin'?`<button class="btn btn-sm btn-light border" onclick="editUser(${i})"><i class="fa fa-edit text-warning"></i></button> <button class="btn btn-sm btn-light border" onclick="users.splice(${i},1);saveToStorage();render();"><i class="fa fa-trash text-danger"></i></button>`:'-'}</td></tr>`; });
        }
        function editUser(i) { document.getElementById('nu').value=users[i].u; document.getElementById('np').value=users[i].p; document.getElementById('edit-user-idx').value=i; document.getElementById('btn-save-user').innerText="تحديث"; }
        function handleUser() { const u=document.getElementById('nu').value, p=document.getElementById('np').value, idx=parseInt(document.getElementById('edit-user-idx').value); if(!u||!p) return; if(idx!==-1){ users[idx].u=u; users[idx].p=p; document.getElementById('edit-user-idx').value=-1; document.getElementById('btn-save-user').innerText="إضافة"; } else users.push({u,p,r:'user'}); document.getElementById('nu').value=''; document.getElementById('np').value=''; saveToStorage(); render(); }
        function exportData(t) { const d = t==='auth'?authData:permData; const ws = XLSX.utils.json_to_sheet(d.map(i=>({ Ref: i.ref, Book_No: i.bn_orig, Full_Name: i.full_bn, Authority: i.ta, Type: i.pt, Memo: i.memo||'', OFP: i.ofp||'', Sent: i.sd||'', Received: i.rd||'', Status: i.status, Creator: i.creator, Wheels: i.w.join(' | '), Expats: i.e.join(' | '), Locals: i.l.join(' | '), Weapons: i.wp.join(' | ') }))); const wb = XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb, ws, "Data"); XLSX.writeFile(wb, `CR_${t}.xlsx`); }
        function toggleLanguage() { lang = lang==='ar'?'en':'ar'; document.getElementById('html-tag').dir = lang==='en'?'ltr':'rtl'; document.body.className = lang==='en'?'ltr-mode text-start':'rtl-mode text-start'; }
    </script>
</body>
</html>
