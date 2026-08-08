<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة الوقت الإضافي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Tajawal', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 flex flex-col justify-between">

    <div class="max-w-md w-full mx-auto p-4 sm:p-6 flex-grow flex flex-col justify-center">
        <!-- رأس التطبيق -->
        <div class="text-center mb-6">
            <div class="bg-indigo-600 text-white w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-3 shadow-lg shadow-indigo-200">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
            </div>
            <h1 class="text-2xl font-bold text-slate-900">حاسبة الوقت الإضافي</h1>
            <p class="text-sm text-slate-500 mt-1">احسب الساعات الإضافية بعد الساعة 2:00 ظهراً بكل سهولة</p>
        </div>

        <!-- بطاقة الإدخال -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-100 p-5 mb-5 space-y-4">
            <div>
                <label class="block text-sm font-medium text-slate-700 mb-1">وقت الانصراف الفعلي</label>
                <input type="time" id="departureTime" value="17:30" 
                    class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 text-lg font-semibold text-center">
            </div>

            <div class="flex items-center justify-between text-xs text-slate-400 px-1">
                <span>وقت الدوام الرسمي: 7:00 ص - 2:00 م</span>
                <span>نهاية الدوام: 14:00</span>
            </div>

            <button onclick="calculateOvertime()" 
                class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-3 rounded-xl transition shadow-md shadow-indigo-100 active:scale-95">
                احسب الساعات الإضافية
            </button>
        </div>

        <!-- بطاقة النتائج -->
        <div id="resultCard" class="bg-gradient-to-br from-indigo-900 to-slate-900 text-white rounded-2xl p-5 shadow-lg hidden">
            <h2 class="text-sm font-medium text-indigo-200 mb-3 border-b border-indigo-800 pb-2">نتائج الحساب</h2>
            <div class="grid grid-cols-2 gap-4 text-center">
                <div class="bg-white/10 rounded-xl p-3 backdrop-blur-sm">
                    <span class="block text-xs text-indigo-200 mb-1">إجمالي الساعات</span>
                    <span id="totalHours" class="text-2xl font-bold">--</span>
                </div>
                <div class="bg-white/10 rounded-xl p-3 backdrop-blur-sm">
                    <span class="block text-xs text-indigo-200 mb-1">تفصيل الوقت</span>
                    <span id="detailTime" class="text-sm font-semibold mt-1 block">--</span>
                </div>
            </div>
        </div>

        <!-- رسالة الخطأ -->
        <div id="errorCard" class="bg-rose-50 border border-rose-200 text-rose-700 p-4 rounded-xl text-sm hidden text-center">
            وقت الانصراف يجب أن يكون بعد الساعة 2:00 ظهراً (14:00).
        </div>
    </div>

    <!-- تذييل التطبيق مع التوقيع -->
    <footer class="text-center py-4 text-xs text-slate-500 space-y-1">
        <p>تطبيق بسيط ومفيد لحساب الساعات الإضافية &copy; 2026</p>
        <p class="font-bold text-indigo-600 text-sm">مع تحيات المبرمج روماني وجيه</p>
    </footer>

    <script>
        function calculateOvertime() {
            const timeInput = document.getElementById('departureTime').value;
            const resultCard = document.getElementById('resultCard');
            const errorCard = document.getElementById('errorCard');

            if (!timeInput) return;

            const [hours, minutes] = timeInput.split(':').map(Number);
            
            // وقت انتهاء الدوام الأساسي هو 14:00 (2 ظهراً)
            const standardEndHour = 14;
            const standardEndMinute = 0;

            const departureTotalMinutes = hours * 60 + minutes;
            const standardTotalMinutes = standardEndHour * 60 + standardEndMinute;

            if (departureTotalMinutes <= standardTotalMinutes) {
                errorCard.classList.remove('hidden');
                resultCard.classList.add('hidden');
                return;
            }

            errorCard.classList.add('hidden');
            resultCard.classList.remove('hidden');

            const diffMinutes = departureTotalMinutes - standardTotalMinutes;
            const diffHours = Math.floor(diffMinutes / 60);
            const remainingMinutes = diffMinutes % 60;

            document.getElementById('totalHours').innerText = `${diffHours}.${Math.round((remainingMinutes / 60) * 10)}`;
            
            let detailText = '';
            if (diffHours > 0) detailText += `${diffHours} ساعة `;
            if (remainingMinutes > 0) detailText += `${remainingMinutes} دقيقة`;
            document.getElementById('detailTime').innerText = detailText;
        }

        // حساب افتراضي عند التحميل
        window.onload = function() {
            calculateOvertime();
        }
    </script>
</body>
</html>
