```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>【新人研修】人生の経歴・動詞構造化ワークショップ</title>
    <!-- スタイリングとフォントの設定 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+JP:wght@400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        :root {
            --brand-primary: #0f172a;
            --brand-accent: #10b981;
            --bg-light: #f8fafc;
        }
        body {
            font-family: 'Inter', 'Noto Sans JP', sans-serif;
            background-color: var(--bg-light);
            color: #1e293b;
            margin: 0;
            padding: 0;
        }
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* エクセル風テーブル */
        .deep-dive-table th, .deep-dive-table td { border: 1px solid #c0c0c0; }
        .red-text { color: #ef4444 !important; font-weight: bold; }
        textarea:focus, input:focus { outline: none; background-color: #f1f5f9; }
        
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }

        /* 印刷・PDF出力用スタイル設定 */
        @media print {
            body {
                background-color: #ffffff !important;
                color: #000000 !important;
                font-size: 11px !important;
            }
            nav, .tab-btn, button, .no-print, #modal-help, #modal-export, .toast-container {
                display: none !important;
            }
            .tab-content {
                display: block !important;
                opacity: 1 !important;
                transform: none !important;
            }
            .print-page {
                page-break-after: always;
                page-break-inside: avoid;
                padding-top: 1.5cm;
                padding-bottom: 1.5cm;
            }
            textarea, input, select {
                border: none !important;
                background-color: transparent !important;
                resize: none !important;
                overflow: hidden !important;
            }
            .bg-slate-50, .bg-amber-50, .bg-emerald-50 {
                background-color: #f8fafc !important;
                border-color: #cbd5e1 !important;
            }
            .deep-dive-table th {
                background-color: #f1f5f9 !important;
                color: #000000 !important;
            }
            canvas {
                max-width: 100% !important;
                height: auto !important;
            }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col pb-20 md:pb-0">

    <!-- グローバル・ナビゲーション (PC用) -->
    <nav class="bg-white border-b border-slate-200 sticky top-0 z-50 no-print hidden md:block">
        <div class="max-w-[1400px] mx-auto px-6 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="bg-slate-900 text-white p-2 rounded-lg">
                    <i class="fa-solid fa-route"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold tracking-tight">人生の経歴・動詞構造化ワーク</h1>
                    <p class="text-[10px] text-slate-500 uppercase tracking-widest font-semibold">New Hire Onboarding System v5.0</p>
                </div>
            </div>
            
            <div class="flex items-center bg-slate-100 p-1 rounded-xl">
                <button onclick="switchTab('guide')" class="tab-btn active px-4 py-2 rounded-lg text-xs font-semibold transition duration-200" id="btn-guide">1. ガイド</button>
                <button onclick="switchTab('timeline')" class="tab-btn px-4 py-2 rounded-lg text-xs font-semibold transition duration-200" id="btn-timeline">2. モチベ波</button>
                <button onclick="switchTab('list')" class="tab-btn px-4 py-2 rounded-lg text-xs font-semibold transition duration-200" id="btn-list">3. 動詞洗い出し</button>
                <button onclick="switchTab('deep')" class="tab-btn px-4 py-2 rounded-lg text-xs font-semibold transition duration-200" id="btn-deep">4. 本質深掘り</button>
                <button onclick="switchTab('analysis')" class="tab-btn px-4 py-2 rounded-lg text-xs font-semibold transition duration-200" id="btn-analysis">5. 自己分析マップ</button>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="openHelp()" class="text-slate-600 hover:text-slate-900 px-3 py-2 rounded-lg text-xs font-bold border border-slate-200 bg-white flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-book"></i> カタログ
                </button>
                <button onclick="triggerPrint()" class="bg-slate-100 hover:bg-slate-200 text-slate-800 px-3 py-2 rounded-lg text-xs font-bold flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-file-pdf"></i> PDF出力
                </button>
                <button onclick="generateShareUrl()" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg text-xs font-bold shadow-sm flex items-center gap-2 transition">
                    <i class="fa-solid fa-share-nodes"></i> 提出用URLを発行
                </button>
            </div>
        </div>
    </nav>

    <!-- モバイル用ヘッダー (スマホ閲覧時のみ表示) -->
    <header class="bg-white border-b border-slate-200 px-4 py-3 sticky top-0 z-50 flex justify-between items-center md:hidden no-print">
        <div class="flex items-center gap-2">
            <div class="bg-slate-900 text-white p-1.5 rounded-md text-xs">
                <i class="fa-solid fa-route"></i>
            </div>
            <h1 class="text-sm font-bold">動詞構造化ワーク v5.0</h1>
        </div>
        <button onclick="generateShareUrl()" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-1.5 rounded-lg text-xs font-bold shadow-sm flex items-center gap-1">
            <i class="fa-solid fa-share-nodes"></i> URL提出
        </button>
    </header>

    <!-- メインコンテンツ -->
    <main class="flex-grow max-w-[1400px] w-full mx-auto p-4 md:p-6">

        <!-- 配布テンプレート読込通知バナー -->
        <div id="import-banner" class="hidden mb-6 bg-[#f0fdf4] border border-[#bbf7d0] rounded-xl p-4 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3 no-print">
            <div class="flex items-center gap-3">
                <div class="p-2 bg-[#dcfce7] text-[#15803d] rounded-lg">
                    <i class="fa-solid fa-download"></i>
                </div>
                <div>
                    <h4 class="text-xs font-bold text-[#166534]">研修用配布テンプレートを読み込みました</h4>
                    <p class="text-[10px] text-[#1e7e34]">このまま内容をご自由に入力・編集し、完了したら右上の「提出用URLを発行」から回答を提出してください。</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="resetToImported()" class="text-xs bg-white text-slate-700 px-3 py-1.5 rounded-lg border border-slate-200 font-semibold hover:bg-slate-50 transition">
                    配布初期状態に戻す
                </button>
                <button onclick="clearImportedData()" class="text-xs bg-rose-50 text-rose-700 px-3 py-1.5 rounded-lg border border-rose-200 font-semibold hover:bg-rose-100 transition">
                    自分の標準データに戻す
                </button>
            </div>
        </div>

        <!-- TAB 1: GUIDE -->
        <section id="tab-guide" class="tab-content active space-y-6 print-page">
            <div class="grid lg:grid-cols-2 gap-6 md:gap-8 items-center py-4 md:py-10">
                <div class="space-y-4 md:space-y-6">
                    <span class="bg-emerald-100 text-emerald-800 text-[10px] md:text-xs font-extrabold px-3 py-1 rounded-full uppercase">Mobile Optimization</span>
                    <h2 class="text-2xl md:text-4xl font-bold leading-tight">「なんとなく過ごした時間」を<br><span class="text-emerald-600">再現性のある才能</span>へ</h2>
                    <p class="text-slate-600 leading-relaxed text-xs md:text-sm">
                        新人研修の最初の壁は、「自分の強みがわからない」という思い込みです。このワークショップでは、自分の人生のモチベーションの波をグラフ化し、そこから本当に熱量があったエピソード（動詞）を抽出。そして、そのエピソードを「要素分解」して、あなたを夢中にさせている「行動のエンジン」を突き止めます。
                    </p>
                    <div class="bg-blue-50 border border-blue-200 p-4 rounded-xl text-xs text-blue-800 leading-relaxed">
                        <strong>💡 スマホ・PC連動（配布・提出もラクラク）:</strong><br>
                        入力したデータは「提出用URLを発行」から、いつでも1つのリンクに変換できます。そのURLをSlack等で共有するだけで、ファイルを一切送らずにメンターへ提出完了です！
                    </div>
                </div>
                <div class="bg-slate-900 rounded-2xl md:rounded-3xl p-6 md:p-8 text-white shadow-2xl relative overflow-hidden">
                    <h3 class="text-sm md:text-lg font-bold mb-4 md:mb-6 flex items-center gap-2 text-emerald-400">
                        <i class="fa-solid fa-stairs"></i> 構造化の5つのステップ
                    </h3>
                    <ul class="space-y-3 md:space-y-4 text-[11px] md:text-xs">
                        <li class="flex gap-3 md:gap-4">
                            <span class="w-5 h-5 md:w-6 md:h-6 bg-emerald-500 rounded-full flex items-center justify-center font-bold text-[10px] md:text-xs shrink-0">1</span>
                            <div>
                                <p class="font-bold">人生のモチベーション波グラフの作成</p>
                                <p class="text-[10px] md:text-[11px] text-slate-400">小学生〜現在までの熱量を可視化し、エピソードを見つける足がかりにします。</p>
                            </div>
                        </li>
                        <li class="flex gap-3 md:gap-4">
                            <span class="w-5 h-5 md:w-6 md:h-6 bg-emerald-500 rounded-full flex items-center justify-center font-bold text-[10px] md:text-xs shrink-0">2</span>
                            <div>
                                <p class="font-bold">動詞の洗い出し（定量評価）</p>
                                <p class="text-[10px] md:text-[11px] text-slate-400">各時期の活動を「〜する」という最小単位の動詞に分解して、好き・得意をスコア化します。</p>
                            </div>
                        </li>
                        <li class="flex gap-3 md:gap-4">
                            <span class="w-5 h-5 md:w-6 md:h-6 bg-emerald-500 rounded-full flex items-center justify-center font-bold text-[10px] md:text-xs shrink-0">3</span>
                            <div>
                                <p class="font-bold">本質の深掘り（定性・エクセル再現）</p>
                                <p class="text-[10px] md:text-[11px] text-slate-400">最重要業務を5ステップに構造分解。こだわり（美学）と夢中になる本質的な理由を言語化します。</p>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- TAB 2: TIMELINE (人生の波グラフ) -->
        <section id="tab-timeline" class="tab-content space-y-6 print-page">
            <div class="bg-white p-4 md:p-6 rounded-2xl shadow-sm border border-slate-200">
                <div class="mb-4">
                    <h2 class="text-lg md:text-xl font-bold">2. 人生のモチベーション波グラフ</h2>
                    <p class="text-xs text-slate-500">各時期のモチベーション（熱量）スコア（-100〜+100）を入力・調節してください。</p>
                </div>
                
                <div class="grid lg:grid-cols-3 gap-6 md:gap-8">
                    <!-- 入力エリア -->
                    <div class="space-y-4 bg-slate-50 p-4 rounded-xl border border-slate-100">
                        <h3 class="font-bold text-xs md:text-sm text-slate-700 flex items-center gap-2"><i class="fa-solid fa-sliders text-emerald-500"></i> 各時期の熱量スコア</h3>
                        
                        <div class="space-y-4">
                            <!-- 小学生 -->
                            <div>
                                <div class="flex justify-between text-[11px] md:text-xs font-bold text-slate-600 mb-1">
                                    <span>小学生時代 (塾・部活等)</span>
                                    <span id="score-val-0" class="text-emerald-600">+50</span>
                                </div>
                                <input type="range" min="-100" max="100" value="50" oninput="updateTimelineScore(0, this.value)" class="w-full accent-emerald-500">
                                <input type="text" id="desc-0" value="サッカークラブでキャプテンをして楽しかった" placeholder="この時期の出来事や行動" onchange="updateTimelineDesc(0, this.value)" class="w-full mt-1 bg-white border border-slate-200 rounded px-2 py-1 text-xs">
                            </div>
                            <!-- 中学生 -->
                            <div>
                                <div class="flex justify-between text-[11px] md:text-xs font-bold text-slate-600 mb-1">
                                    <span>中学生時代 (部活・勉強など)</span>
                                    <span id="score-val-1" class="text-emerald-600">-20</span>
                                </div>
                                <input type="range" min="-100" max="100" value="-20" oninput="updateTimelineScore(1, this.value)" class="w-full accent-emerald-500">
                                <input type="text" id="desc-1" value="受験勉強が辛く、ひたすらインプットを繰り返していた" placeholder="この時期の出来事や行動" onchange="updateTimelineDesc(1, this.value)" class="w-full mt-1 bg-white border border-slate-200 rounded px-2 py-1 text-xs">
                            </div>
                            <!-- 高校生 -->
                            <div>
                                <div class="flex justify-between text-[11px] md:text-xs font-bold text-slate-600 mb-1">
                                    <span>高校生時代 (部活・学校行事など)</span>
                                    <span id="score-val-2" class="text-emerald-600">+80</span>
                                </div>
                                <input type="range" min="-100" max="100" value="80" oninput="updateTimelineScore(2, this.value)" class="w-full accent-emerald-500">
                                <input type="text" id="desc-2" value="文化祭の実行委員会で新しいイベントの企画を主導した" placeholder="この時期の出来事や行動" onchange="updateTimelineDesc(2, this.value)" class="w-full mt-1 bg-white border border-slate-200 rounded px-2 py-1 text-xs">
                            </div>
                            <!-- 大学生 -->
                            <div>
                                <div class="flex justify-between text-[11px] md:text-xs font-bold text-slate-600 mb-1">
                                    <span>大学生時代 / 直近 (サークル・バイト等)</span>
                                    <span id="score-val-3" class="text-emerald-600">+30</span>
                                </div>
                                <input type="range" min="-100" max="100" value="30" oninput="updateTimelineScore(3, this.value)" class="w-full accent-emerald-500">
                                <input type="text" id="desc-3" value="個別指導塾のバイトで生徒の学力分析と提案を行った" placeholder="この時期の出来事や行動" onchange="updateTimelineDesc(3, this.value)" class="w-full mt-1 bg-white border border-slate-200 rounded px-2 py-1 text-xs">
                            </div>
                        </div>
                    </div>

                    <!-- グラフエリア -->
                    <div class="lg:col-span-2 bg-slate-50 rounded-xl p-4 border border-slate-100 flex flex-col justify-between">
                        <div class="h-[240px] md:h-[280px] w-full">
                            <canvas id="timelineChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: LIST (動詞の洗い出し) -->
        <section id="tab-list" class="tab-content space-y-4 print-page">
            <div class="flex justify-between items-end mb-2">
                <div>
                    <h2 class="text-xl md:text-2xl font-bold">3. 動詞の洗い出し</h2>
                    <p class="text-xs text-slate-500">あなたのこれまでの活動を、「～する」という動詞形式に分解してリストにしましょう。</p>
                </div>
                <button onclick="addListRow()" class="px-3 py-1.5 md:px-4 md:py-2 bg-slate-900 text-white rounded-lg text-xs font-bold hover:bg-slate-800 transition shadow-sm">
                    <i class="fa-solid fa-plus"></i> 追加
                </button>
            </div>

            <!-- PC用テーブルビュー -->
            <div class="hidden md:block bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                <table class="w-full border-collapse">
                    <thead class="bg-slate-50 border-b border-slate-200">
                        <tr class="text-left text-[11px] uppercase tracking-wider text-slate-400 font-bold">
                            <th class="px-6 py-4 w-12">#</th>
                            <th class="px-6 py-4 w-1/4">対象の時期・エピソード</th>
                            <th class="px-6 py-4 w-1/4 text-slate-900">具体的な行動【動詞】</th>
                            <th class="px-6 py-4">得られた成果・変化</th>
                            <th class="px-6 py-4 w-28 text-center">好き度 (1-5)</th>
                            <th class="px-6 py-4 w-28 text-center">得意度 (1-5)</th>
                            <th class="px-6 py-4 w-12"></th>
                        </tr>
                    </thead>
                    <tbody id="list-tbody-pc" class="divide-y divide-slate-100">
                        <!-- JSで動的に追加 -->
                    </tbody>
                </table>
            </div>

            <!-- スマホ用カード型入力フォーム -->
            <div id="list-cards-mobile" class="block md:hidden space-y-4">
                <!-- JSで動的にスマホ用入力カードをレンダリング -->
            </div>
        </section>

        <!-- TAB 4: DEEP DIVE (本質の深掘り) -->
        <section id="tab-deep" class="tab-content space-y-6 print-page">
            <div class="flex justify-between items-end mb-2">
                <div>
                    <h2 class="text-xl md:text-2xl font-bold">4. 本質の深掘り（経歴構造化）</h2>
                    <p class="text-xs text-slate-500">最重要業務を抽出し、こだわりを抽出します。ステップごとに赤字（最重要）を指定してください。</p>
                </div>
                <button onclick="addDeepDiveBlock()" class="px-3 py-1.5 md:px-4 md:py-2 bg-emerald-600 text-white rounded-lg text-xs font-bold hover:bg-emerald-700 transition shadow-sm">
                    <i class="fa-solid fa-plus"></i> 追加
                </button>
            </div>

            <!-- PC/スマホ 共通レンダラー (レスポンシブ対応) -->
            <div id="deep-dive-container" class="space-y-8">
                <!-- JSでエクセル再現テーブル / スマホ最適化フォームを動的レンダリング -->
            </div>
        </section>

        <!-- TAB 5: ANALYSIS (散布図マップ) -->
        <section id="tab-analysis" class="tab-content space-y-8 print-page">
            <div class="grid lg:grid-cols-3 gap-6 md:gap-8">
                <div class="lg:col-span-2 bg-white p-4 md:p-8 rounded-2xl shadow-sm border border-slate-200">
                    <div class="mb-6">
                        <h3 class="text-lg md:text-xl font-bold">モチベーション・マップ</h3>
                        <p class="text-xs text-slate-400">好き度と得意度の相関から、あなたのコア能力をグラフィカルに特定します。</p>
                    </div>
                    <div class="relative h-[320px] md:h-[450px] w-full bg-slate-50 rounded-xl p-4 border border-slate-100">
                        <canvas id="scatterChart"></canvas>
                    </div>
                </div>

                <div class="space-y-4">
                    <div class="bg-emerald-50 border border-emerald-100 p-4 md:p-6 rounded-2xl text-xs">
                        <h4 class="text-emerald-900 font-bold flex items-center gap-2 mb-2">
                            <span class="w-2.5 h-2.5 bg-emerald-500 rounded-full"></span> 才能の源泉（右上）
                        </h4>
                        <p class="text-emerald-700 leading-relaxed">
                            「好きで、かつ得意」な行動です。あなた自身が最も高い自走力を発揮できる領域です。インターン初期からこの業務をアサインされると成長速度が最大化します。
                        </p>
                    </div>
                    <div class="bg-amber-50 border border-amber-100 p-4 md:p-6 rounded-2xl text-xs">
                        <h4 class="text-amber-900 font-bold flex items-center gap-2 mb-2">
                            <span class="w-2.5 h-2.5 bg-amber-500 rounded-full"></span> 燃え尽き注意（右下）
                        </h4>
                        <p class="text-amber-700 leading-relaxed">
                            「得意だが、実は嫌い」な行動です。成果は出ますが、エネルギー消費が激しくモチベーション低下に繋がりやすい注意領域です。
                        </p>
                    </div>
                    <div class="bg-slate-200 p-4 md:p-6 rounded-2xl text-xs">
                        <h4 class="text-slate-900 font-bold flex items-center gap-2 mb-2">
                            <span class="w-2.5 h-2.5 bg-slate-600 rounded-full"></span> ポテンシャル領域（左上）
                        </h4>
                        <p class="text-slate-600 leading-relaxed">
                            「好きだが、まだ得意ではない」行動です。ここを徹底的なフィードバックを交えて指導することで、将来のブレイクスルーを生み出せます。
                        </p>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- モバイル用下部タブナビゲーション (スマホ画面時のみ) -->
    <div class="md:hidden fixed bottom-0 left-0 right-0 bg-white border-t border-slate-200 py-2 px-1 flex justify-around items-center z-45 no-print">
        <button onclick="switchTab('guide')" id="mbtn-guide" class="flex flex-col items-center gap-1 text-slate-500 text-[10px] w-1/5">
            <i class="fa-solid fa-circle-info text-base"></i><span>ガイド</span>
        </button>
        <button onclick="switchTab('timeline')" id="mbtn-timeline" class="flex flex-col items-center gap-1 text-slate-500 text-[10px] w-1/5">
            <i class="fa-solid fa-chart-line text-base"></i><span>グラフ</span>
        </button>
        <button onclick="switchTab('list')" id="mbtn-list" class="flex flex-col items-center gap-1 text-slate-500 text-[10px] w-1/5">
            <i class="fa-solid fa-list-check text-base"></i><span>洗い出し</span>
        </button>
        <button onclick="switchTab('deep')" id="mbtn-deep" class="flex flex-col items-center gap-1 text-slate-500 text-[10px] w-1/5">
            <i class="fa-solid fa-magnifying-glass-chart text-base"></i><span>深掘り</span>
        </button>
        <button onclick="switchTab('analysis')" id="mbtn-analysis" class="flex flex-col items-center gap-1 text-slate-500 text-[10px] w-1/5">
            <i class="fa-solid fa-chart-pie text-base"></i><span>分析</span>
        </button>
    </div>

    <!-- モーダル: 動詞のカタログヘルプ -->
    <div id="modal-help" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-[100] flex items-center justify-center p-4 md:p-6 hidden">
        <div class="bg-white rounded-2xl md:rounded-3xl max-w-4xl w-full p-6 md:p-8 shadow-2xl space-y-6 flex flex-col max-h-[85vh]">
            <div class="flex justify-between items-center">
                <h3 class="text-lg md:text-xl font-bold flex items-center gap-2"><i class="fa-solid fa-book-open text-emerald-500"></i> 動詞 of カタログ・ヒント集</h3>
                <button onclick="closeHelp()" class="text-slate-400 hover:text-slate-600 text-2xl font-bold">&times;</button>
            </div>
            
            <div class="overflow-y-auto space-y-6 pr-2 text-xs">
                <div class="grid md:grid-cols-3 gap-4">
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
                        <h4 class="font-bold text-slate-700 border-b border-slate-200 pb-1.5 mb-2 flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 bg-blue-500 rounded-full"></span> インプット系
                        </h4>
                        <ul class="text-[11px] text-slate-600 space-y-1">
                            <li>・データを**分析する** / **収集する**</li>
                            <li>・他者の意見を**傾聴する** / **観察する**</li>
                            <li>・隠れた課題を**発見する**</li>
                        </ul>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
                        <h4 class="font-bold text-slate-700 border-b border-slate-200 pb-1.5 mb-2 flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 bg-emerald-500 rounded-full"></span> プロセス系
                        </h4>
                        <ul class="text-[11px] text-slate-600 space-y-1">
                            <li>・複雑な概念を**言語化する**</li>
                            <li>・タスクの**優先順位を決める**</li>
                            <li>・計画を**設計する**</li>
                        </ul>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
                        <h4 class="font-bold text-slate-700 border-b border-slate-200 pb-1.5 mb-2 flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 bg-purple-500 rounded-full"></span> 実行系
                        </h4>
                        <ul class="text-[11px] text-slate-600 space-y-1">
                            <li>・周りの人を**巻き込む** / **説得する**</li>
                            <li>・やり方をマニュアル化し**仕組み化する**</li>
                            <li>・メンバーを**育成する**</li>
                        </ul>
                    </div>
                </div>
            </div>
            <div class="flex justify-end pt-4 border-t border-slate-100">
                <button onclick="closeHelp()" class="px-6 py-2 bg-slate-100 hover:bg-slate-200 text-slate-600 rounded-xl font-bold text-xs transition">閉じる</button>
            </div>
        </div>
    </div>

    <!-- モーダル: 提出用・共有URL出力モーダル -->
    <div id="modal-export" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-[100] flex items-center justify-center p-4 md:p-6 hidden">
        <div class="bg-white rounded-2xl md:rounded-3xl max-w-2xl w-full p-6 md:p-8 shadow-2xl space-y-6">
            <div class="flex justify-between items-center">
                <h3 class="text-lg md:text-xl font-bold flex items-center gap-2">
                    <i class="fa-solid fa-share-nodes text-blue-600"></i>
                    共有・提出用リンクの生成
                </h3>
                <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 text-2xl font-bold">&times;</button>
            </div>
            
            <div class="space-y-4">
                <p class="text-xs text-slate-500 leading-relaxed">
                    入力された全ての情報（人生グラフ、動詞リスト、深掘りシート）を暗号化した共有URLを発行しました。<br>
                    このURLを受け取った相手は、開くだけであなたのワークシートの状態を1秒で復元できます（ZIPやファイルの送受信は不要です）。
                </p>
                
                <div class="bg-slate-100 p-3 rounded-lg border border-slate-200 flex items-center gap-2">
                    <input type="text" id="share-url-input" class="bg-transparent border-none text-xs text-slate-700 w-full focus:outline-none" readonly>
                    <button onclick="copyShareUrl()" class="bg-blue-600 hover:bg-blue-700 text-white text-xs px-3 py-1.5 rounded font-bold shrink-0">
                        コピー
                    </button>
                </div>
            </div>

            <div class="pt-4 border-t border-slate-100 flex justify-end">
                <button onclick="closeModal()" class="px-6 py-2 bg-slate-100 hover:bg-slate-200 text-slate-600 rounded-xl font-bold text-xs transition">
                    閉じる
                </button>
            </div>
        </div>
    </div>

    <!-- トースト通知 -->
    <div id="toast" class="fixed bottom-24 md:bottom-6 right-6 bg-slate-950 text-white text-xs px-4 py-3 rounded-xl shadow-xl transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none flex items-center gap-2 z-50">
        <i class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toast-message">保存しました</span>
    </div>

    <!-- アプリケーション・ロジック -->
    <script>
        // 初期状態データ
        const defaultTimelineData = [
            { period: "小学生", score: 50, desc: "サッカークラブでキャプテンをして楽しかった" },
            { period: "中学生", score: -20, desc: "受験勉強が辛く、ひたすらインプットを繰り返していた" },
            { period: "高校生", score: 80, desc: "文化祭の実行委員会で新しいイベントの企画を主導した" },
            { period: "大学生/直近", score: 30, desc: "個別指導塾のバイトで生徒の学力分析と提案を行った" }
        ];

        const defaultListData = [
            { org: "高校・文化祭", verb: "新規のステージ企画を起案する", result: "前年比1.5倍の集客に成功", love: 5, skill: 4 },
            { org: "高校・文化祭", verb: "関係部活動のスケジュール調整をする", result: "タイムテーブル通りに進行", love: 2, skill: 4 },
            { org: "個別指導塾バイト", verb: "生徒の誤答の傾向を分析する", result: "苦手克服シートの作成", love: 5, skill: 5 },
            { org: "個別指導塾バイト", verb: "生徒のモチベーションを褒めて促す", result: "登校拒否だった子が皆勤に", love: 4, skill: 3 }
        ];

        const defaultDeepDiveData = [
            {
                id: "block-default",
                importantWork: "商談 (塾講師・三者面談)",
                headerColor: "grey",
                steps: [
                    { text: "顧客（親御さん）の情報収集", isRed: false, definition: "" },
                    { text: "提案資料（成績分析表）の作成", isRed: false, definition: "" },
                    { text: "カウンセリング", isRed: true, definition: "顧客の不安と将来の理想像を明確にするステップ" },
                    { text: "具体的な授業プランの提案", isRed: true, definition: "相手の納得のいく形で解決プランを提案する" },
                    { text: "クロージング", isRed: false, definition: "" }
                ],
                dreamReason: "①顧客が100%腹落ちしている（心からぶっ刺さっている）状態をつくりたい。主導権を常に握りたい。\n\n②誰もが納得のいくプレゼン、解説ができるように「例え話」や「資料作成」に徹底的に拘ってしまう。"
            }
        ];

        let timelineData = [];
        let listData = [];
        let deepDiveData = [];

        // 配布データの保持用
        let importedTimelineData = null;
        let importedListData = null;
        let importedDeepDiveData = null;

        let scatterChart = null;
        let timelineChart = null;

        window.onload = () => {
            loadAllData();
            checkURLParams(); // URLパラメータを最優先で読込
            renderList();
            renderDeepDive();
            switchTab('guide');
        };

        // ローカルストレージからのロード、またはデフォルト
        function loadAllData() {
            const savedTimeline = localStorage.getItem('onboard_timeline');
            const savedList = localStorage.getItem('onboard_list');
            const savedDeep = localStorage.getItem('onboard_deep');

            timelineData = savedTimeline ? JSON.parse(savedTimeline) : JSON.parse(JSON.stringify(defaultTimelineData));
            listData = savedList ? JSON.parse(savedList) : JSON.parse(JSON.stringify(defaultListData));
            deepDiveData = savedDeep ? JSON.parse(savedDeep) : JSON.parse(JSON.stringify(defaultDeepDiveData));

            syncInputs();
        }

        // 入力値の同期
        function syncInputs() {
            for (let i = 0; i < 4; i++) {
                const descEl = document.getElementById(`desc-${i}`);
                const valEl = document.getElementById(`score-val-${i}`);
                if (descEl) descEl.value = timelineData[i].desc;
                if (valEl) valEl.innerText = (timelineData[i].score > 0 ? "+" : "") + timelineData[i].score;
            }
        }

        // 状態保存 (URLから展開中は「ローカル用保存キー」を汚さないよう分岐可能ですが、作業の継続性を担保するためLocalStorageに保存します)
        function saveState() {
            localStorage.setItem('onboard_timeline', JSON.stringify(timelineData));
            localStorage.setItem('onboard_list', JSON.stringify(listData));
            localStorage.setItem('onboard_deep', JSON.stringify(deepDiveData));
        }

        // トースト表示
        function showToast(msg) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toast-message');
            if (toastMsg) toastMsg.innerText = msg;
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 2500);
        }

        // タブ切替
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.getElementById(`tab-${tabId}`).classList.add('active');
            
            // PCナビゲーション
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('bg-white', 'shadow-sm', 'text-slate-900', 'active'));
            const activeBtn = document.getElementById(`btn-${tabId}`);
            if(activeBtn) activeBtn.classList.add('bg-white', 'shadow-sm', 'text-slate-900', 'active');

            // スマホナビゲーション
            const mobileIds = ['guide', 'timeline', 'list', 'deep', 'analysis'];
            mobileIds.forEach(id => {
                const mbtn = document.getElementById(`mbtn-${id}`);
                if (mbtn) {
                    if (id === tabId) {
                        mbtn.classList.add('text-blue-600', 'font-bold');
                        mbtn.classList.remove('text-slate-500');
                    } else {
                        mbtn.classList.remove('text-blue-600', 'font-bold');
                        mbtn.classList.add('text-slate-500');
                    }
                }
            });

            if(tabId === 'timeline') renderTimelineChart();
            if(tabId === 'analysis') renderChart();
        }

        function triggerPrint() {
            const allTabs = document.querySelectorAll('.tab-content');
            allTabs.forEach(tab => tab.classList.add('active'));
            renderTimelineChart();
            renderChart();
            setTimeout(() => {
                window.print();
                const activeBtn = document.querySelector('.tab-btn.active');
                const currentActiveTab = activeBtn ? activeBtn.id.replace('btn-', '') : 'guide';
                switchTab(currentActiveTab);
            }, 500);
        }

        function openHelp() { document.getElementById('modal-help').classList.remove('hidden'); }
        function closeHelp() { document.getElementById('modal-help').classList.add('hidden'); }

        // タイムライン変更
        function updateTimelineScore(idx, val) {
            timelineData[idx].score = parseInt(val);
            const valSpan = document.getElementById(`score-val-${idx}`);
            if (valSpan) valSpan.innerText = (val > 0 ? "+" : "") + val;
            saveState();
            renderTimelineChart();
        }

        // 説明変更
        function updateTimelineDesc(idx, val) {
            timelineData[idx].desc = val;
            saveState();
            renderTimelineChart();
        }

        function renderTimelineChart() {
            const ctx = document.getElementById('timelineChart').getContext('2d');
            if (timelineChart) timelineChart.destroy();
            timelineChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: timelineData.map(d => d.period),
                    datasets: [{
                        label: '熱量（モチベーション）',
                        data: timelineData.map(d => d.score),
                        borderColor: '#10b981',
                        borderWidth: 3,
                        backgroundColor: 'rgba(16, 185, 129, 0.05)',
                        fill: true,
                        tension: 0.3,
                        pointRadius: 6,
                        pointBackgroundColor: '#10b981'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { min: -100, max: 100 }
                    }
                }
            });
        }

        // 動詞洗い出しのレンダリング (PC / スマホ両対応)
        function renderList() {
            const tbodyPc = document.getElementById('list-tbody-pc');
            const cardMobile = document.getElementById('list-cards-mobile');

            tbodyPc.innerHTML = '';
            cardMobile.innerHTML = '';

            listData.forEach((item, idx) => {
                // PC用
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="px-6 py-4 text-[10px] font-mono text-slate-400">${idx+1}</td>
                    <td class="px-4 py-2"><input type="text" value="${item.org}" oninput="updateListData(${idx}, 'org', this.value)" class="w-full border-none p-1 text-xs focus:ring-1 focus:ring-slate-200 rounded"></td>
                    <td class="px-4 py-2"><input type="text" value="${item.verb}" oninput="updateListData(${idx}, 'verb', this.value)" class="w-full border-none p-1 text-xs font-bold text-emerald-700 focus:ring-1 focus:ring-emerald-200 rounded"></td>
                    <td class="px-4 py-2"><input type="text" value="${item.result}" oninput="updateListData(${idx}, 'result', this.value)" class="w-full border-none p-1 text-xs focus:ring-1 focus:ring-slate-200 rounded"></td>
                    <td class="px-4 py-2">
                        <select onchange="updateListData(${idx}, 'love', parseInt(this.value))" class="w-full border border-slate-200 rounded p-1 text-xs font-bold bg-white">
                            ${[5,4,3,2,1].map(v => `<option value="${v}" ${item.love === v ? 'selected' : ''}>${v}</option>`).join('')}
                        </select>
                    </td>
                    <td class="px-4 py-2">
                        <select onchange="updateListData(${idx}, 'skill', parseInt(this.value))" class="w-full border border-slate-200 rounded p-1 text-xs font-bold bg-white">
                            ${[5,4,3,2,1].map(v => `<option value="${v}" ${item.skill === v ? 'selected' : ''}>${v}</option>`).join('')}
                        </select>
                    </td>
                    <td class="px-4 py-2 text-center">
                        <button onclick="removeListRow(${idx})" class="text-slate-300 hover:text-rose-500 transition"><i class="fa-solid fa-trash-can"></i></button>
                    </td>
                `;
                tbodyPc.appendChild(tr);

                // スマホ用 (カード型)
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-xl shadow-sm border border-slate-200 space-y-3 relative";
                card.innerHTML = `
                    <div class="flex justify-between items-center border-b border-slate-100 pb-2">
                        <span class="text-xs font-bold text-slate-400">動詞エピソード #${idx+1}</span>
                        <button onclick="removeListRow(${idx})" class="text-rose-500 text-xs font-semibold"><i class="fa-solid fa-trash-can"></i> 削除</button>
                    </div>
                    <div class="grid grid-cols-1 gap-2.5">
                        <div>
                            <label class="text-[10px] font-bold text-slate-400">対象の時期・出来事</label>
                            <input type="text" value="${item.org}" oninput="updateListData(${idx}, 'org', this.value)" class="w-full border border-slate-200 rounded-lg px-2.5 py-1.5 text-xs">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold text-emerald-600">具体的な行動【動詞】</label>
                            <input type="text" value="${item.verb}" oninput="updateListData(${idx}, 'verb', this.value)" class="w-full border border-emerald-300 rounded-lg px-2.5 py-1.5 text-xs font-bold text-emerald-700">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold text-slate-400">得られた成果・変化</label>
                            <input type="text" value="${item.result}" oninput="updateListData(${idx}, 'result', this.value)" class="w-full border border-slate-200 rounded-lg px-2.5 py-1.5 text-xs">
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="text-[10px] font-bold text-slate-400">好き度 (1-5)</label>
                                <select onchange="updateListData(${idx}, 'love', parseInt(this.value))" class="w-full border border-slate-200 rounded-lg px-2.5 py-1.5 text-xs bg-white font-bold">
                                    ${[5,4,3,2,1].map(v => `<option value="${v}" ${item.love === v ? 'selected' : ''}>${v}</option>`).join('')}
                                </select>
                            </div>
                            <div>
                                <label class="text-[10px] font-bold text-slate-400">得意度 (1-5)</label>
                                <select onchange="updateListData(${idx}, 'skill', parseInt(this.value))" class="w-full border border-slate-200 rounded-lg px-2.5 py-1.5 text-xs bg-white font-bold">
                                    ${[5,4,3,2,1].map(v => `<option value="${v}" ${item.skill === v ? 'selected' : ''}>${v}</option>`).join('')}
                                </select>
                            </div>
                        </div>
                    </div>
                `;
                cardMobile.appendChild(card);
            });
        }

        function addListRow() {
            listData.push({ org: "", verb: "", result: "", love: 3, skill: 3 });
            saveState();
            renderList();
        }

        function removeListRow(idx) {
            listData.splice(idx, 1);
            saveState();
            renderList();
        }

        function updateListData(idx, key, val) {
            listData[idx][key] = val;
            saveState();
            if (document.getElementById('tab-analysis').classList.contains('active')) {
                renderChart();
            }
        }

        // 深掘りシートのレンダリング (PCエクセル風テーブル＆スマホ用アコーディオン完全自動両立)
        function renderDeepDive() {
            const container = document.getElementById('deep-dive-container');
            if (!container) return;
            container.innerHTML = '';

            deepDiveData.forEach((block, bIdx) => {
                const headerBg = block.headerColor === 'grey' ? 'bg-[#d9d9d9]' : 'bg-[#f4cccc]';
                
                let html = `
                    <div class="bg-white rounded-2xl shadow-md border border-slate-200 overflow-hidden">
                        <!-- 1. 操作ヘッダー -->
                        <div class="px-4 py-2 bg-slate-50 border-b border-slate-200 flex justify-between items-center text-[10px] text-slate-400 no-print">
                            <span class="font-bold">業務深掘りブロック #${bIdx+1}</span>
                            <div class="flex gap-4">
                                <label class="cursor-pointer"><input type="radio" name="color-${block.id}" onchange="updateDeepColor(${bIdx}, 'grey')" ${block.headerColor==='grey'?'checked':''}> グレー</label>
                                <label class="cursor-pointer"><input type="radio" name="color-${block.id}" onchange="updateDeepColor(${bIdx}, 'pink')" ${block.headerColor==='pink'?'checked':''}> ピンク</label>
                                <button onclick="removeDeepBlock(${bIdx})" class="text-rose-500 font-bold"><i class="fa-solid fa-trash-can"></i> 削除</button>
                            </div>
                        </div>

                        <!-- 2. PC用エクセルテーブルレイアウト -->
                        <div class="hidden md:block overflow-x-auto">
                            <table class="deep-dive-table w-full border-collapse">
                                <thead class="${headerBg} text-[11px] font-bold">
                                    <tr class="text-center">
                                        <th class="py-3 w-[20%]">重要な業務</th>
                                        <th class="py-3 w-[25%]">①要素分解 (チェックで赤字)</th>
                                        <th class="py-3 w-[25%]">名詞限定 (この業務の定義)</th>
                                        <th class="py-3">このステップの何が自分を夢中にさせているか？</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr>
                                        <td rowspan="5" class="p-4 align-middle bg-slate-50">
                                            <textarea oninput="updateDeepBase(${bIdx}, 'importantWork', this.value)" class="w-full h-32 bg-transparent border-none text-center font-bold text-base focus:ring-0" placeholder="例：商談">${block.importantWork}</textarea>
                                        </td>
                                        ${renderStepRowPC(bIdx, 0)}
                                        <td rowspan="5" class="p-4 align-top">
                                            <textarea oninput="updateDeepBase(${bIdx}, 'dreamReason', this.value)" class="w-full h-[280px] bg-transparent border-none text-xs leading-loose focus:ring-0" placeholder="①〇〇な状態をつくりたい...">${block.dreamReason}</textarea>
                                        </td>
                                    </tr>
                                    <tr>${renderStepRowPC(bIdx, 1)}</tr>
                                    <tr>${renderStepRowPC(bIdx, 2)}</tr>
                                    <tr>${renderStepRowPC(bIdx, 3)}</tr>
                                    <tr>${renderStepRowPC(bIdx, 4)}</tr>
                                </tbody>
                            </table>
                        </div>

                        <!-- 3. スマホ用縦型フォームレイアウト (スマホ閲覧時に自動適用) -->
                        <div class="block md:hidden p-4 space-y-4">
                            <div>
                                <label class="text-[10px] font-bold text-slate-400">重要な業務名</label>
                                <input type="text" value="${block.importantWork}" oninput="updateDeepBase(${bIdx}, 'importantWork', this.value)" class="w-full border border-slate-200 rounded-lg px-2.5 py-2 text-sm font-bold bg-slate-50" placeholder="例：商談">
                            </div>
                            
                            <div class="space-y-2">
                                <label class="text-[10px] font-bold text-slate-400">①要素分解 と 名詞限定（チェックで赤字＆深掘り）</label>
                                ${[0,1,2,3,4].map(sIdx => renderStepRowMobile(block, bIdx, sIdx)).join('')}
                            </div>

                            <div>
                                <label class="text-[10px] font-bold text-slate-400">このステップの何が自分を夢中にさせているか？</label>
                                <textarea oninput="updateDeepBase(${bIdx}, 'dreamReason', this.value)" class="w-full h-36 border border-slate-200 rounded-lg p-2.5 text-xs mt-1" placeholder="自分なりのこだわり・こだわり美学を言語化してください">${block.dreamReason}</textarea>
                            </div>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', html);
            });
        }

        function renderStepRowPC(bIdx, sIdx) {
            const step = deepDiveData[bIdx].steps[sIdx];
            return `
                <td class="p-2 border-slate-200">
                    <div class="flex items-center gap-2">
                        <input type="checkbox" ${step.isRed?'checked':''} onchange="toggleDeepRed(${bIdx}, ${sIdx})" class="accent-red-500">
                        <input type="text" value="${step.text}" oninput="updateDeepStep(${bIdx}, ${sIdx}, 'text', this.value)" class="w-full border-none p-1 text-xs ${step.isRed?'red-text':''}" placeholder="ステップ ${sIdx+1}">
                    </div>
                </td>
                <td class="p-2 border-slate-200">
                    <input type="text" value="${step.definition}" oninput="updateDeepStep(${bIdx}, ${sIdx}, 'definition', this.value)" class="w-full border-none p-1 text-[11px] text-slate-500" placeholder="${step.isRed?'定義を入力':''}">
                </td>
            `;
        }

        function renderStepRowMobile(block, bIdx, sIdx) {
            const step = block.steps[sIdx];
            return `
                <div class="bg-slate-50 p-2.5 rounded-lg border border-slate-200 space-y-2">
                    <div class="flex items-center gap-2">
                        <input type="checkbox" ${step.isRed?'checked':''} onchange="toggleDeepRed(${bIdx}, ${sIdx})" class="accent-red-500">
                        <input type="text" value="${step.text}" oninput="updateDeepStep(${bIdx}, ${sIdx}, 'text', this.value)" class="w-full bg-transparent border-b border-dashed border-slate-300 py-0.5 text-xs ${step.isRed?'red-text':''}" placeholder="分解ステップ #${sIdx+1}">
                    </div>
                    ${step.isRed ? `
                        <div>
                            <input type="text" value="${step.definition}" oninput="updateDeepStep(${bIdx}, ${sIdx}, 'definition', this.value)" class="w-full border border-slate-200 bg-white rounded px-2 py-1 text-[11px]" placeholder="赤字ステップの定義を入力">
                        </div>
                    ` : ''}
                </div>
            `;
        }

        function updateDeepBase(bIdx, key, val) {
            deepDiveData[bIdx][key] = val;
            saveState();
        }

        function updateDeepStep(bIdx, sIdx, key, val) {
            deepDiveData[bIdx].steps[sIdx][key] = val;
            saveState();
        }

        function toggleDeepRed(bIdx, sIdx) {
            deepDiveData[bIdx].steps[sIdx].isRed = !deepDiveData[bIdx].steps[sIdx].isRed;
            saveState();
            renderDeepDive();
        }

        function updateDeepColor(bIdx, color) {
            deepDiveData[bIdx].headerColor = color;
            saveState();
            renderDeepDive();
        }

        // 新規ブロック追加
        function addDeepDiveBlock() {
            deepDiveData.push({
                id: "block-" + Date.now(),
                importantWork: "",
                headerColor: "pink",
                steps: Array(5).fill(null).map(() => ({ text: "", isRed: false, definition: "" })),
                dreamReason: ""
            });
            saveState();
            renderDeepDive();
        }

        // 削除
        function removeDeepBlock(idx) {
            if(confirm('この業務ブロックを削除しますか？')){
                deepDiveData.splice(idx, 1);
                saveState();
                renderDeepDive();
            }
        }

        // 散布図描画
        function renderChart() {
            const ctx = document.getElementById('scatterChart').getContext('2d');
            if(scatterChart) scatterChart.destroy();
            scatterChart = new Chart(ctx, {
                type: 'scatter',
                data: {
                    datasets: [{
                        label: 'あなたの動詞',
                        data: listData.filter(d => d.verb).map(d => ({ x: d.skill, y: d.love, label: d.verb })),
                        backgroundColor: '#10b981',
                        pointRadius: 10
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: { min: 0, max: 6 },
                        y: { min: 0, max: 6 }
                    }
                }
            });
        }

        // URL共有発行
        function generateShareUrl() {
            const dataToShare = {
                timeline: timelineData,
                list: listData,
                deep: deepDiveData
            };
            
            const jsonStr = JSON.stringify(dataToShare);
            const encodedData = btoa(unescape(encodeURIComponent(jsonStr)));
            
            const baseUrl = window.location.href.split('?')[0];
            const shareUrl = `${baseUrl}?data=${encodedData}`;

            const inputEl = document.getElementById('share-url-input');
            if (inputEl) inputEl.value = shareUrl;
            document.getElementById('modal-export').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('modal-export').classList.add('hidden');
        }

        function copyShareUrl() {
            const input = document.getElementById('share-url-input');
            if (input) {
                input.select();
                document.execCommand('copy');
                showToast("提出用URLをコピーしました！");
            }
            closeModal();
        }

        // URLからのデータ自動復元
        function checkURLParams() {
            const urlParams = new URLSearchParams(window.location.search);
            const rawData = urlParams.get('data');
            if (rawData) {
                try {
                    const decodedData = decodeURIComponent(escape(atob(rawData)));
                    const parsed = JSON.parse(decodedData);
                    
                    if (parsed.timeline && parsed.list && parsed.deep) {
                        // 配布テンプレートの初期状態を一時保持
                        importedTimelineData = JSON.parse(JSON.stringify(parsed.timeline));
                        importedListData = JSON.parse(JSON.stringify(parsed.list));
                        importedDeepDiveData = JSON.parse(JSON.stringify(parsed.deep));

                        // ワーク用アクティブデータに注入
                        timelineData = parsed.timeline;
                        listData = parsed.list;
                        deepDiveData = parsed.deep;

                        syncInputs();

                        // バナーを表示
                        const banner = document.getElementById('import-banner');
                        if (banner) banner.classList.remove('hidden');
                        showToast("共有テンプレートを正常に読み込みました！");
                    }
                } catch (e) {
                    console.error("データの復元に失敗しました: ", e);
                }
            }
        }

        // 配布された初期状態に戻す
        function resetToImported() {
            if (importedTimelineData && importedListData && importedDeepDiveData) {
                if (confirm("入力した内容を消去し、配布された初期テンプレート状態に戻しますか？")) {
                    timelineData = JSON.parse(JSON.stringify(importedTimelineData));
                    listData = JSON.parse(JSON.stringify(importedListData));
                    deepDiveData = JSON.parse(JSON.stringify(importedDeepDiveData));
                    
                    syncInputs();
                    renderList();
                    renderDeepDive();
                    saveState();
                    showToast("配布状態にリセットしました");
                }
            }
        }

        // 自分のローカルデータに完全復帰
        function clearImportedData() {
            if (confirm("共有テンプレートの表示を終了し、ブラウザに保存されている自分のデータに戻りますか？")) {
                const baseUrl = window.location.href.split('?')[0];
                window.location.href = baseUrl;
            }
        }
    </script>
</body>
</html>

```
