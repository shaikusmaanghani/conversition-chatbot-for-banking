# conversition-chatbot-for-banking
<!DOCTYPE html>
<html lang="en" class="h-full bg-slate-900 text-slate-100">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FinAI - Intelligent Banking Assistant</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            900: '#1e3a8a',
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Custom scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.6);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(51, 65, 85, 0.8);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(71, 85, 105, 1);
        }

        /* Typing indicator animation */
        .typing-dot {
            animation: typing 1.4s infinite ease-in-out both;
        }
        .typing-dot:nth-child(1) { animation-delay: 0s; }
        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes typing {
            0%, 80%, 100% { transform: scale(0.6); opacity: 0.4; }
            40% { transform: scale(1); opacity: 1; }
        }

        /* Mic wave animation */
        .mic-wave {
            animation: pulse-wave 1.2s infinite ease-out;
        }
        @keyframes pulse-wave {
            0% { transform: scale(0.95); opacity: 0.8; }
            50% { transform: scale(1.15); opacity: 0.3; }
            100% { transform: scale(0.95); opacity: 0.8; }
        }
    </style>
</head>
<body class="h-full flex flex-col font-sans overflow-hidden bg-slate-950 text-slate-100">

    <!-- Top Header Navigation -->
    <header class="h-16 border-b border-slate-800 bg-slate-900/80 backdrop-blur-md px-4 lg:px-6 flex items-center justify-between z-20 shrink-0">
        <div class="flex items-center gap-3">
            <button id="sidebar-toggle" class="lg:hidden p-2 text-slate-400 hover:text-white rounded-lg hover:bg-slate-800">
                <i data-lucide="menu" class="w-5 h-5"></i>
            </button>
            <div class="flex items-center gap-2.5">
                <div class="w-9 h-9 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-500 flex items-center justify-center shadow-lg shadow-blue-500/20">
                    <i data-lucide="bot" class="w-5 h-5 text-white"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <span class="font-bold text-lg text-white tracking-wide">FinAI</span>
                        <span class="text-xs px-2 py-0.5 rounded-full bg-blue-500/10 text-blue-400 border border-blue-500/20 font-medium">v3.2 Live</span>
                    </div>
                    <p class="text-xs text-slate-400 hidden sm:block">Conversational AI Banking Assistant</p>
                </div>
            </div>
        </div>

        <div class="flex items-center gap-3">
            <!-- User Profile Summary Pill -->
            <div class="hidden sm:flex items-center gap-3 pl-3 pr-4 py-1.5 bg-slate-800/80 rounded-full border border-slate-700/60">
                <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100&auto=format&fit=crop&q=80" alt="Avatar" class="w-7 h-7 rounded-full object-cover ring-2 ring-blue-500/50">
                <div class="text-xs">
                    <p class="font-semibold text-slate-200">Alex Morgan</p>
                    <p class="text-slate-400 text-[10px]">Premier Member</p>
                </div>
            </div>

            <!-- Global Action Trigger -->
            <button onclick="switchView('chat'); triggerQuickAction('transfer');" class="bg-blue-600 hover:bg-blue-500 text-white text-xs font-medium px-3.5 py-2 rounded-lg flex items-center gap-1.5 transition shadow-lg shadow-blue-600/20">
                <i data-lucide="send" class="w-3.5 h-3.5"></i>
                <span>Quick Transfer</span>
            </button>
        </div>
    </header>

    <!-- Main Content Wrapper -->
    <div class="flex-1 flex overflow-hidden relative">
        
        <!-- Navigation Sidebar -->
        <aside id="sidebar" class="fixed lg:relative inset-y-0 left-0 z-30 w-64 bg-slate-900 border-r border-slate-800 flex flex-col transition-transform duration-300 -translate-x-full lg:translate-x-0 shrink-0">
            <!-- Navigation Items -->
            <div class="p-4 space-y-1">
                <p class="px-3 text-[11px] font-semibold text-slate-500 uppercase tracking-wider mb-2">Main Console</p>
                
                <button onclick="switchView('chat')" id="nav-chat" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition bg-blue-600/10 text-blue-400 border border-blue-500/20">
                    <i data-lucide="message-square" class="w-4 h-4"></i>
                    <span>AI Chat Console</span>
                    <span class="ml-auto w-2 h-2 rounded-full bg-blue-500"></span>
                </button>

                <button onclick="switchView('accounts')" id="nav-accounts" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-slate-400 hover:text-slate-200 hover:bg-slate-800/60 transition">
                    <i data-lucide="wallet" class="w-4 h-4"></i>
                    <span>Accounts & Cards</span>
                </button>

                <button onclick="switchView('capability')" id="nav-capability" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-slate-400 hover:text-slate-200 hover:bg-slate-800/60 transition">
                    <i data-lucide="sparkles" class="w-4 h-4"></i>
                    <span>AI Capability Hub</span>
                    <span class="ml-auto text-[10px] bg-purple-500/10 text-purple-400 px-1.5 py-0.5 rounded border border-purple-500/20">Interactive</span>
                </button>

                <button onclick="switchView('analytics')" id="nav-analytics" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-slate-400 hover:text-slate-200 hover:bg-slate-800/60 transition">
                    <i data-lucide="pie-chart" class="w-4 h-4"></i>
                    <span>Smart Financial Insights</span>
                </button>

                <button onclick="switchView('faq')" id="nav-faq" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-slate-400 hover:text-slate-200 hover:bg-slate-800/60 transition">
                    <i data-lucide="help-circle" class="w-4 h-4"></i>
                    <span>Knowledge Base & FAQs</span>
                </button>
            </div>

            <!-- Recent AI Conversations History List -->
            <div class="px-4 py-3 border-t border-slate-800 flex-1 overflow-y-auto">
                <div class="flex items-center justify-between mb-2 px-1">
                    <p class="text-[11px] font-semibold text-slate-500 uppercase tracking-wider">Recent Topics</p>
                    <button onclick="clearChatHistory()" class="text-[11px] text-slate-400 hover:text-rose-400 transition">Clear</button>
                </div>
                <div class="space-y-1 text-xs" id="chat-history-list">
                    <a href="#" onclick="presetChatTopic('Mortgage rates')" class="block px-3 py-2 rounded-lg text-slate-400 hover:text-slate-200 hover:bg-slate-800/40 truncate transition">Mortgage Inquiry #941</a>
                    <a href="#" onclick="presetChatTopic('Fraud alert review')" class="block px-3 py-2 rounded-lg text-slate-400 hover:text-slate-200 hover:bg-slate-800/40 truncate transition">Suspicious Charge Check</a>
                    <a href="#" onclick="presetChatTopic('International transfer fee')" class="block px-3 py-2 rounded-lg text-slate-400 hover:text-slate-200 hover:bg-slate-800/40 truncate transition">Wire Transfer Rules</a>
                </div>
            </div>

            <!-- System Status -->
            <div class="p-4 border-t border-slate-800 bg-slate-900/50">
                <div class="flex items-center justify-between text-xs text-slate-400">
                    <span class="flex items-center gap-1.5">
                        <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                        NLP Core Online
                    </span>
                    <span class="text-[10px] bg-slate-800 px-2 py-0.5 rounded text-slate-400">Latency: 24ms</span>
                </div>
            </div>
        </aside>

        <!-- Sidebar Overlay for Mobile -->
        <div id="sidebar-overlay" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-20 hidden lg:hidden"></div>

        <!-- Center Content Views Container -->
        <main class="flex-1 flex flex-col h-full overflow-hidden bg-slate-950 relative">

            <!-- VIEW 1: AI Chatbot Console (Default View) -->
            <section id="view-chat" class="view-panel flex-1 flex flex-col h-full overflow-hidden">
                
                <!-- AI Assistant Context Bar -->
                <div class="bg-slate-900/70 border-b border-slate-800/80 px-4 py-2.5 flex items-center justify-between shrink-0">
                    <div class="flex items-center gap-2 text-xs">
                        <span class="text-slate-400">Session Mode:</span>
                        <span class="font-medium text-slate-200 bg-slate-800 px-2 py-0.5 rounded border border-slate-700">Authenticated Banking Workspace</span>
                    </div>

                    <!-- AI Intent Detection Bar Debug Box -->
                    <div id="intent-badge-container" class="hidden sm:flex items-center gap-2 text-[11px]">
                        <span class="text-slate-500">Live AI Diagnostics:</span>
                        <span id="intent-tag" class="bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 px-2 py-0.5 rounded font-mono">Intent: Welcome</span>
                        <span id="sentiment-tag" class="bg-emerald-500/10 text-emerald-400 border border-emerald-500/20 px-2 py-0.5 rounded font-mono">Sentiment: Positive (0.92)</span>
                    </div>
                </div>

                <!-- Chat Messages Scroll Container -->
                <div id="chat-messages-container" class="flex-1 overflow-y-auto p-4 lg:p-6 space-y-6">
                    
                    <!-- Welcome AI Bot Message -->
                    <div class="flex gap-3 max-w-3xl">
                        <div class="w-8 h-8 rounded-lg bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center shrink-0 mt-0.5 shadow-md shadow-blue-500/20">
                            <i data-lucide="bot" class="w-4 h-4 text-white"></i>
                        </div>
                        <div class="space-y-3 flex-1">
                            <div class="flex items-center gap-2">
                                <span class="text-xs font-semibold text-slate-200">FinAI Banking Assistant</span>
                                <span class="text-[10px] text-slate-500">Just now</span>
                            </div>
                            <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl rounded-tl-none text-sm text-slate-200 space-y-3 leading-relaxed shadow-sm">
                                <p>Hello Alex! I am your AI banking assistant. I can help you check balances, transfer funds, analyze your spending, block cards, or answer complex financial queries.</p>
                                <p class="text-xs text-slate-400">What would you like to do today?</p>
                            </div>

                            <!-- Interactive Quick Suggestion Pills -->
                            <div class="flex flex-wrap gap-2 pt-1">
                                <button onclick="sendQuickPrompt('Check my balance')" class="text-xs bg-slate-900/90 hover:bg-slate-800 text-slate-300 hover:text-white border border-slate-800 hover:border-slate-700 px-3 py-1.5 rounded-full transition flex items-center gap-1.5">
                                    <i data-lucide="wallet" class="w-3.5 h-3.5 text-blue-400"></i> Check my balance
                                </button>
                                <button onclick="sendQuickPrompt('Transfer $200 to Sarah')" class="text-xs bg-slate-900/90 hover:bg-slate-800 text-slate-300 hover:text-white border border-slate-800 hover:border-slate-700 px-3 py-1.5 rounded-full transition flex items-center gap-1.5">
                                    <i data-lucide="arrow-right-left" class="w-3.5 h-3.5 text-emerald-400"></i> Transfer $200 to Sarah
                                </button>
                                <button onclick="sendQuickPrompt('Freeze my credit card')" class="text-xs bg-slate-900/90 hover:bg-slate-800 text-slate-300 hover:text-white border border-slate-800 hover:border-slate-700 px-3 py-1.5 rounded-full transition flex items-center gap-1.5">
                                    <i data-lucide="shield-alert" class="w-3.5 h-3.5 text-amber-400"></i> Freeze my card
                                </button>
                                <button onclick="sendQuickPrompt('Show spending breakdown')" class="text-xs bg-slate-900/90 hover:bg-slate-800 text-slate-300 hover:text-white border border-slate-800 hover:border-slate-700 px-3 py-1.5 rounded-full transition flex items-center gap-1.5">
                                    <i data-lucide="pie-chart" class="w-3.5 h-3.5 text-purple-400"></i> Spending breakdown
                                </button>
                            </div>
                        </div>
                    </div>

                </div>

                <!-- Chat Dynamic Typing Indicator (Hidden by default) -->
                <div id="typing-indicator" class="px-6 py-2 hidden">
                    <div class="flex items-center gap-3">
                        <div class="w-8 h-8 rounded-lg bg-blue-600/20 border border-blue-500/30 flex items-center justify-center shrink-0">
                            <i data-lucide="bot" class="w-4 h-4 text-blue-400"></i>
                        </div>
                        <div class="bg-slate-900 border border-slate-800 px-4 py-2.5 rounded-2xl rounded-tl-none flex items-center gap-1.5">
                            <span class="w-2 h-2 rounded-full bg-blue-400 typing-dot"></span>
                            <span class="w-2 h-2 rounded-full bg-blue-400 typing-dot"></span>
                            <span class="w-2 h-2 rounded-full bg-blue-400 typing-dot"></span>
                            <span class="text-xs text-slate-400 ml-2">FinAI is processing...</span>
                        </div>
                    </div>
                </div>

                <!-- Chat Input Controls Section -->
                <div class="p-4 bg-slate-900/80 border-t border-slate-800 shrink-0">
                    <!-- Voice Active Visualizer Bar (Hidden by default) -->
                    <div id="voice-visualizer" class="hidden mb-3 bg-blue-950/60 border border-blue-500/30 p-3 rounded-xl flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div class="relative flex items-center justify-center">
                                <span class="w-3 h-3 bg-red-500 rounded-full animate-ping absolute"></span>
                                <span class="w-3 h-3 bg-red-500 rounded-full relative"></span>
                            </div>
                            <span class="text-xs font-medium text-slate-200">Listening to voice input...</span>
                        </div>
                        <div class="flex items-center gap-1">
                            <span class="w-1 h-4 bg-blue-400 rounded mic-wave"></span>
                            <span class="w-1 h-7 bg-blue-500 rounded mic-wave" style="animation-delay: 0.1s"></span>
                            <span class="w-1 h-3 bg-blue-400 rounded mic-wave" style="animation-delay: 0.2s"></span>
                            <span class="w-1 h-8 bg-indigo-500 rounded mic-wave" style="animation-delay: 0.3s"></span>
                            <span class="w-1 h-5 bg-blue-400 rounded mic-wave" style="animation-delay: 0.15s"></span>
                        </div>
                        <button onclick="stopVoiceInput()" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 px-2.5 py-1 rounded">Cancel</button>
                    </div>

                    <form id="chat-form" onsubmit="handleChatSubmit(event)" class="flex items-center gap-2">
                        <div class="relative flex-1">
                            <input type="text" id="chat-input" placeholder="Ask FinAI anything (e.g. 'Transfer $150 to John' or 'Mortgage application')" 
                                class="w-full bg-slate-950 border border-slate-800 focus:border-blue-500 focus:ring-1 focus:ring-blue-500 rounded-xl px-4 py-3 text-sm text-slate-100 placeholder-slate-500 outline-none transition">
                        </div>

                        <!-- Voice Mic Button -->
                        <button type="button" id="mic-btn" onclick="toggleVoiceInput()" class="p-3 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl border border-slate-700 transition" title="Voice Input Simulation">
                            <i data-lucide="mic" class="w-4 h-4"></i>
                        </button>

                        <!-- Send Button -->
                        <button type="submit" class="p-3 bg-blue-600 hover:bg-blue-500 text-white rounded-xl shadow-lg shadow-blue-600/20 transition flex items-center justify-center">
                            <i data-lucide="send" class="w-4 h-4"></i>
                        </button>
                    </form>
                    <p class="text-[10px] text-slate-500 mt-2 text-center">Protected by 256-bit Bank Grade Encryption • AI responses are simulated for demonstration</p>
                </div>
            </section>

            <!-- VIEW 2: Accounts & Quick Action Overview -->
            <section id="view-accounts" class="view-panel hidden flex-1 overflow-y-auto p-4 lg:p-6 space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-100">Account Management & Security</h2>
                    <p class="text-xs text-slate-400">View live balances, quick actions, and card security controls.</p>
                </div>

                <!-- Account Balance Summary Cards Grid -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="bg-gradient-to-br from-slate-900 to-slate-900/90 border border-slate-800 p-5 rounded-2xl relative overflow-hidden">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-slate-400 font-medium">Checking Account</p>
                                <p class="text-2xl font-bold text-white mt-1">$12,450.80</p>
                            </div>
                            <span class="p-2 bg-blue-500/10 text-blue-400 rounded-xl"><i data-lucide="credit-card" class="w-5 h-5"></i></span>
                        </div>
                        <p class="text-[11px] text-slate-500 mt-4">**** **** **** 8842</p>
                    </div>

                    <div class="bg-gradient-to-br from-slate-900 to-slate-900/90 border border-slate-800 p-5 rounded-2xl relative overflow-hidden">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-slate-400 font-medium">High Yield Savings</p>
                                <p class="text-2xl font-bold text-emerald-400 mt-1">$48,210.15</p>
                            </div>
                            <span class="p-2 bg-emerald-500/10 text-emerald-400 rounded-xl"><i data-lucide="trending-up" class="w-5 h-5"></i></span>
                        </div>
                        <p class="text-[11px] text-emerald-500/80 mt-4">+4.25% APY Interest Earned</p>
                    </div>

                    <div class="bg-gradient-to-br from-slate-900 to-slate-900/90 border border-slate-800 p-5 rounded-2xl relative overflow-hidden">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-slate-400 font-medium">Sapphire Credit Card</p>
                                <p class="text-2xl font-bold text-purple-400 mt-1">$1,840.20</p>
                            </div>
                            <span class="p-2 bg-purple-500/10 text-purple-400 rounded-xl"><i data-lucide="shield-check" class="w-5 h-5"></i></span>
                        </div>
                        <p class="text-[11px] text-slate-500 mt-4">Limit: $15,000.00 • Due in 12 days</p>
                    </div>
                </div>

                <!-- Digital Card Control & Quick Security -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 space-y-4">
                        <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                            <i data-lucide="lock" class="w-4 h-4 text-blue-400"></i> Card Security Controls
                        </h3>
                        <div class="space-y-3">
                            <div class="flex items-center justify-between p-3 bg-slate-950 rounded-xl border border-slate-800">
                                <div>
                                    <p class="text-xs font-medium text-slate-200">Freeze Debit Card</p>
                                    <p class="text-[10px] text-slate-400">Instantly block new transactions</p>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" id="card-freeze-toggle" class="sr-only peer" onchange="toggleCardFreeze(this.checked)">
                                    <div class="w-9 h-5 bg-slate-800 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-blue-600"></div>
                                </label>
                            </div>

                            <div class="flex items-center justify-between p-3 bg-slate-950 rounded-xl border border-slate-800">
                                <div>
                                    <p class="text-xs font-medium text-slate-200">International Online Transactions</p>
                                    <p class="text-[10px] text-slate-400">Allow foreign payments</p>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" checked class="sr-only peer">
                                    <div class="w-9 h-5 bg-slate-800 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-blue-600"></div>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- Quick Transfer Tool -->
                    <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 space-y-4">
                        <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                            <i data-lucide="send" class="w-4 h-4 text-emerald-400"></i> Instant Money Transfer
                        </h3>
                        <div class="space-y-3">
                            <input type="text" id="quick-recipient" placeholder="Recipient Name or Email" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-slate-200 outline-none focus:border-blue-500">
                            <input type="number" id="quick-amount" placeholder="Amount ($)" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-slate-200 outline-none focus:border-blue-500">
                            <button onclick="executeQuickTransfer()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-medium py-2 rounded-xl text-xs transition">Send Money via AI Assist</button>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW 3: AI Capability Hub -->
            <section id="view-capability" class="view-panel hidden flex-1 overflow-y-auto p-4 lg:p-6 space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-100">AI Banking Capability Demos</h2>
                    <p class="text-xs text-slate-400">Test specialized AI modules embedded in the conversational banking core.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Demo 1: Smart Loan Eligibility Predictor -->
                    <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 space-y-4">
                        <div class="flex items-center gap-3">
                            <div class="p-2 bg-blue-500/10 text-blue-400 rounded-xl"><i data-lucide="calculator" class="w-5 h-5"></i></div>
                            <div>
                                <h3 class="text-sm font-semibold text-slate-100">Loan Eligibility Predictor</h3>
                                <p class="text-[11px] text-slate-400">Real-time creditworthiness AI engine</p>
                            </div>
                        </div>

                        <div class="space-y-3 text-xs">
                            <div>
                                <label class="text-slate-400 block mb-1">Monthly Income ($)</label>
                                <input type="number" id="loan-income" value="6500" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-slate-200 outline-none">
                            </div>
                            <div>
                                <label class="text-slate-400 block mb-1">Credit Score Range</label>
                                <select id="loan-credit" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-slate-200 outline-none">
                                    <option value="750">Excellent (750+)</option>
                                    <option value="680">Good (670-740)</option>
                                    <option value="600">Fair (580-660)</option>
                                </select>
                            </div>
                            <button onclick="calculateLoanEligibility()" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-medium py-2 rounded-lg transition">Run AI Evaluation</button>
                        </div>

                        <div id="loan-result" class="hidden p-3 bg-slate-950 border border-blue-500/30 rounded-xl text-xs space-y-1">
                            <p class="font-semibold text-emerald-400">Pre-Approved Limit: $45,000</p>
                            <p class="text-slate-400 text-[11px]">Estimated Interest Rate: 5.4% APR</p>
                        </div>
                    </div>

                    <!-- Demo 2: Fraud Alert Diagnostic Engine -->
                    <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 space-y-4">
                        <div class="flex items-center gap-3">
                            <div class="p-2 bg-rose-500/10 text-rose-400 rounded-xl"><i data-lucide="shield-alert" class="w-5 h-5"></i></div>
                            <div>
                                <h3 class="text-sm font-semibold text-slate-100">Fraud Security Scanner</h3>
                                <p class="text-[11px] text-slate-400">Anomaly detection simulation</p>
                            </div>
                        </div>

                        <div class="p-3 bg-slate-950 border border-rose-500/20 rounded-xl text-xs space-y-2">
                            <div class="flex justify-between items-center text-slate-300">
                                <span>Recent Flagged Event:</span>
                                <span class="text-rose-400 font-mono">$480.00 @ TechStore London</span>
                            </div>
                            <p class="text-[11px] text-slate-400">AI Risk Score: <span class="text-amber-400 font-bold">84/100 (High Risk)</span></p>
                        </div>

                        <div class="grid grid-cols-2 gap-2">
                            <button onclick="resolveFraudDemo(true)" class="bg-emerald-600/20 text-emerald-400 border border-emerald-500/30 py-2 rounded-lg text-xs font-medium hover:bg-emerald-600/30">Verify "It Was Me"</button>
                            <button onclick="resolveFraudDemo(false)" class="bg-rose-600/20 text-rose-400 border border-rose-500/30 py-2 rounded-lg text-xs font-medium hover:bg-rose-600/30">Report & Block Card</button>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW 4: Smart Financial Analytics -->
            <section id="view-analytics" class="view-panel hidden flex-1 overflow-y-auto p-4 lg:p-6 space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-100">AI Financial Insights & PFM</h2>
                    <p class="text-xs text-slate-400">Autonomous budget analysis and spending habits visualizer.</p>
                </div>

                <!-- Spending Chart SVG -->
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-sm font-semibold text-slate-200">Monthly Expense Breakdown</h3>
                        <span class="text-xs text-slate-400">Total Spent: $3,420.00</span>
                    </div>

                    <!-- SVG Bar Chart -->
                    <div class="h-44 w-full flex items-end justify-between gap-3 pt-6 px-4 border-b border-slate-800">
                        <div class="flex-1 flex flex-col items-center gap-2">
                            <div class="w-full bg-blue-500/20 hover:bg-blue-500/30 rounded-t transition relative group" style="height: 70%;">
                                <span class="opacity-0 group-hover:opacity-100 absolute -top-6 left-1/2 -translate-x-1/2 bg-slate-800 text-[10px] px-1.5 py-0.5 rounded text-white">$1,200</span>
                            </div>
                            <span class="text-[10px] text-slate-400">Housing</span>
                        </div>
                        <div class="flex-1 flex flex-col items-center gap-2">
                            <div class="w-full bg-indigo-500/20 hover:bg-indigo-500/30 rounded-t transition relative group" style="height: 45%;">
                                <span class="opacity-0 group-hover:opacity-100 absolute -top-6 left-1/2 -translate-x-1/2 bg-slate-800 text-[10px] px-1.5 py-0.5 rounded text-white">$750</span>
                            </div>
                            <span class="text-[10px] text-slate-400">Food/Dining</span>
                        </div>
                        <div class="flex-1 flex flex-col items-center gap-2">
                            <div class="w-full bg-emerald-500/20 hover:bg-emerald-500/30 rounded-t transition relative group" style="height: 30%;">
                                <span class="opacity-0 group-hover:opacity-100 absolute -top-6 left-1/2 -translate-x-1/2 bg-slate-800 text-[10px] px-1.5 py-0.5 rounded text-white">$420</span>
                            </div>
                            <span class="text-[10px] text-slate-400">Utilities</span>
                        </div>
                        <div class="flex-1 flex flex-col items-center gap-2">
                            <div class="w-full bg-purple-500/20 hover:bg-purple-500/30 rounded-t transition relative group" style="height: 55%;">
                                <span class="opacity-0 group-hover:opacity-100 absolute -top-6 left-1/2 -translate-x-1/2 bg-slate-800 text-[10px] px-1.5 py-0.5 rounded text-white">$890</span>
                            </div>
                            <span class="text-[10px] text-slate-400">Shopping</span>
                        </div>
                        <div class="flex-1 flex flex-col items-center gap-2">
                            <div class="w-full bg-amber-500/20 hover:bg-amber-500/30 rounded-t transition relative group" style="height: 20%;">
                                <span class="opacity-0 group-hover:opacity-100 absolute -top-6 left-1/2 -translate-x-1/2 bg-slate-800 text-[10px] px-1.5 py-0.5 rounded text-white">$160</span>
                            </div>
                            <span class="text-[10px] text-slate-400">Travel</span>
                        </div>
                    </div>

                    <!-- AI Generated Recommendation Box -->
                    <div class="bg-indigo-950/40 border border-indigo-500/20 p-4 rounded-xl flex items-start gap-3">
                        <i data-lucide="sparkles" class="w-5 h-5 text-indigo-400 shrink-0 mt-0.5"></i>
                        <div class="text-xs space-y-1">
                            <p class="font-semibold text-slate-200">AI Budget Observation</p>
                            <p class="text-slate-400">Dining out increased by <span class="text-amber-400 font-medium">18%</span> compared to last month. Setting a $600 weekly cap could save you approximately $180/mo towards your High-Yield Savings account.</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW 5: Knowledge Base & FAQs -->
            <section id="view-faq" class="view-panel hidden flex-1 overflow-y-auto p-4 lg:p-6 space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-100">Knowledge Base & FAQ Search</h2>
                    <p class="text-xs text-slate-400">Find quick answers or trigger AI guidance directly.</p>
                </div>

                <!-- Search Input -->
                <div class="relative">
                    <i data-lucide="search" class="w-4 h-4 absolute left-3.5 top-3.5 text-slate-500"></i>
                    <input type="text" id="faq-search" oninput="filterFAQs()" placeholder="Search banking queries (e.g. wire transfer, routing number, lost card)..." class="w-full bg-slate-900 border border-slate-800 rounded-xl pl-10 pr-4 py-3 text-xs text-slate-200 outline-none focus:border-blue-500">
                </div>

                <div class="space-y-3" id="faq-list">
                    <div class="faq-item bg-slate-900 border border-slate-800 p-4 rounded-xl space-y-2">
                        <h3 class="text-xs font-semibold text-slate-200">How do I find my account and routing numbers?</h3>
                        <p class="text-xs text-slate-400 leading-relaxed">You can locate your Routing Number directly under the Accounts tab or ask FinAI chat 'Show routing number' for instant verification.</p>
                        <button onclick="sendQuickPrompt('What is my routing number?'); switchView('chat');" class="text-[11px] text-blue-400 hover:underline">Ask AI about this →</button>
                    </div>

                    <div class="faq-item bg-slate-900 border border-slate-800 p-4 rounded-xl space-y-2">
                        <h3 class="text-xs font-semibold text-slate-200">What is the daily wire transfer limit?</h3>
                        <p class="text-xs text-slate-400 leading-relaxed">Standard checking accounts have a $10,000 daily digital wire transfer limit. Premier accounts can request temporary increases via the AI Chat console.</p>
                        <button onclick="sendQuickPrompt('Increase my wire transfer limit'); switchView('chat');" class="text-[11px] text-blue-400 hover:underline">Ask AI about this →</button>
                    </div>

                    <div class="faq-item bg-slate-900 border border-slate-800 p-4 rounded-xl space-y-2">
                        <h3 class="text-xs font-semibold text-slate-200">What happens if I lose my debit card?</h3>
                        <p class="text-xs text-slate-400 leading-relaxed">Instantly freeze your card using the Accounts tab or tell FinAI chat 'Freeze card'. A replacement card will be issued automatically.</p>
                        <button onclick="sendQuickPrompt('Freeze my credit card'); switchView('chat');" class="text-[11px] text-blue-400 hover:underline">Ask AI about this →</button>
                    </div>
                </div>
            </section>
        </main>
    </div>

    <script>
        // Lucide Icons Initialization
        lucide.createIcons();

        // State Management
        let isListening = false;
        let activeView = 'chat';

        // Predefined AI Knowledge Base Engine for Dynamic Chat
        const aiKnowledgeBase = [
            {
                keywords: ['balance', 'checking', 'savings', 'money', 'how much'],
                intent: 'Account Balance Query',
                sentiment: 'Neutral (0.95)',
                responseHTML: `
                    <p>Here is your current balance snapshot:</p>
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 space-y-2 my-2 text-xs">
                        <div class="flex justify-between">
                            <span class="text-slate-400">Checking (*8842):</span>
                            <span class="font-bold text-white">$12,450.80</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-400">High-Yield Savings:</span>
                            <span class="font-bold text-emerald-400">$48,210.15</span>
                        </div>
                    </div>
                    <p class="text-xs text-slate-400">Would you like to perform a transfer or view recent transactions?</p>
                `
            },
            {
                keywords: ['transfer', 'send', 'sarah', 'john', 'pay'],
                intent: 'Money Transfer Workflow',
                sentiment: 'Positive (0.88)',
                responseHTML: `
                    <p>I can help you process that transfer immediately.</p>
                    <div class="bg-slate-950 p-4 rounded-xl border border-blue-500/30 space-y-3 my-2 text-xs">
                        <p class="font-semibold text-blue-400">Confirm Transfer Details</p>
                        <div class="space-y-1 text-slate-300">
                            <p>Recipient: <span class="text-white font-medium">Sarah Jenkins</span></p>
                            <p>Amount: <span class="text-emerald-400 font-bold">$200.00</span></p>
                            <p>From: Checking (*8842)</p>
                        </div>
                        <button onclick="confirmTransferAction(this)" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-medium py-2 rounded-lg transition">Authorize Transfer Now</button>
                    </div>
                `
            },
            {
                keywords: ['freeze', 'block', 'lost', 'stolen', 'card'],
                intent: 'Card Security Control',
                sentiment: 'Urgent (-0.40)',
                responseHTML: `
                    <p class="text-amber-400 font-semibold">Security Action Initiated</p>
                    <p>Your Sapphire Debit Card (*8842) has been temporarily <span class="font-bold text-white">FROZEN</span> to prevent unauthorized charges.</p>
                    <div class="bg-slate-950 p-3 rounded-xl border border-amber-500/30 my-2 text-xs space-y-2">
                        <p class="text-slate-400">No new purchases will be approved. You can unfreeze it anytime from the Accounts tab.</p>
                    </div>
                `
            },
            {
                keywords: ['spending', 'breakdown', 'analytics', 'budget'],
                intent: 'Financial Analytics Query',
                sentiment: 'Neutral (0.91)',
                responseHTML: `
                    <p>Here is your spending summary for this month:</p>
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 space-y-1.5 my-2 text-xs">
                        <p>Total Spent: <span class="font-bold text-white">$3,420.00</span></p>
                        <p class="text-slate-400">Top Category: <span class="text-blue-400">Housing ($1,200)</span></p>
                        <p class="text-slate-400">Dining Out: <span class="text-amber-400">$750 (+18% vs last mo)</span></p>
                    </div>
                    <button onclick="switchView('analytics')" class="text-xs text-blue-400 hover:underline">View full visual chart in Analytics tab →</button>
                `
            },
            {
                keywords: ['mortgage', 'loan', 'rate', 'apply'],
                intent: 'Loan Inquiry',
                sentiment: 'Positive (0.90)',
                responseHTML: `
                    <p>Our current 30-year fixed mortgage rates start at <span class="text-emerald-400 font-bold">5.85% APR</span>.</p>
                    <p class="text-xs text-slate-400 mt-1">Based on your account history, you are pre-qualified for streamlined approval.</p>
                    <button onclick="switchView('capability')" class="mt-2 text-xs bg-slate-800 hover:bg-slate-700 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700">Open AI Loan Predictor Tool</button>
                `
            }
        ];

        // Sidebar Navigation Switcher
        function switchView(viewId) {
            activeView = viewId;
            document.querySelectorAll('.view-panel').forEach(panel => panel.classList.add('hidden'));
            document.getElementById(`view-${viewId}`).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('bg-blue-600/10', 'text-blue-400', 'border', 'border-blue-500/20');
                btn.classList.add('text-slate-400');
            });

            const activeBtn = document.getElementById(`nav-${viewId}`);
            if (activeBtn) {
                activeBtn.classList.add('bg-blue-600/10', 'text-blue-400', 'border', 'border-blue-500/20');
                activeBtn.classList.remove('text-slate-400');
            }

            // Mobile sidebar auto-close
            document.getElementById('sidebar').classList.add('-translate-x-full');
            document.getElementById('sidebar-overlay').classList.add('hidden');
        }

        // Mobile Sidebar Toggle
        document.getElementById('sidebar-toggle').addEventListener('click', () => {
            document.getElementById('sidebar').classList.toggle('-translate-x-full');
            document.getElementById('sidebar-overlay').classList.toggle('hidden');
        });

        document.getElementById('sidebar-overlay').addEventListener('click', () => {
            document.getElementById('sidebar').classList.add('-translate-x-full');
            document.getElementById('sidebar-overlay').classList.add('hidden');
        });

        // Chat Form Handler
        function handleChatSubmit(e) {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            const message = input.value.trim();

            if (!message) return;

            appendUserMessage(message);
            input.value = '';

            processAIResponse(message);
        }

        function sendQuickPrompt(promptText) {
            appendUserMessage(promptText);
            processAIResponse(promptText);
        }

        function presetChatTopic(topic) {
            switchView('chat');
            sendQuickPrompt(topic);
        }

        // Render User Message
        function appendUserMessage(text) {
            const container = document.getElementById('chat-messages-container');
            const userMsgHTML = `
                <div class="flex gap-3 max-w-3xl ml-auto justify-end">
                    <div class="space-y-1 text-right">
                        <div class="flex items-center justify-end gap-2">
                            <span class="text-[10px] text-slate-500">Just now</span>
                            <span class="text-xs font-semibold text-slate-200">You</span>
                        </div>
                        <div class="bg-blue-600 text-white p-3.5 rounded-2xl rounded-tr-none text-sm space-y-1 shadow-md shadow-blue-600/10">
                            <p>${escapeHTML(text)}</p>
                        </div>
                    </div>
                </div>
            `;
            container.insertAdjacentHTML('beforeend', userMsgHTML);
            scrollToBottom();
        }

        // Process AI Response with Simulation Delay
        function processAIResponse(userText) {
            const typingIndicator = document.getElementById('typing-indicator');
            typingIndicator.classList.remove('hidden');
            scrollToBottom();

            // Match Knowledge Base
            const lowerText = userText.toLowerCase();
            let matchedEntry = aiKnowledgeBase.find(item => item.keywords.some(kw => lowerText.includes(kw)));

            if (!matchedEntry) {
                matchedEntry = {
                    intent: 'General Inquiry',
                    sentiment: 'Neutral (0.85)',
                    responseHTML: `
                        <p>I have processed your request regarding: "<em>${escapeHTML(userText)}</em>".</p>
                        <p class="text-xs text-slate-400">Our customer support core is monitoring this session. Is there a specific account, transaction, or security setting you would like to adjust?</p>
                    `
                };
            }

            // Update Intent Badge Diagnostics
            document.getElementById('intent-badge-container').classList.remove('hidden');
            document.getElementById('intent-tag').innerText = `Intent: ${matchedEntry.intent}`;
            document.getElementById('sentiment-tag').innerText = `Sentiment: ${matchedEntry.sentiment}`;

            setTimeout(() => {
                typingIndicator.classList.add('hidden');

                const container = document.getElementById('chat-messages-container');
                const aiMsgHTML = `
                    <div class="flex gap-3 max-w-3xl">
                        <div class="w-8 h-8 rounded-lg bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center shrink-0 mt-0.5 shadow-md shadow-blue-500/20">
                            <i data-lucide="bot" class="w-4 h-4 text-white"></i>
                        </div>
                        <div class="space-y-2 flex-1">
                            <div class="flex items-center gap-2">
                                <span class="text-xs font-semibold text-slate-200">FinAI Assistant</span>
                                <span class="text-[10px] text-slate-500">Just now</span>
                            </div>
                            <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl rounded-tl-none text-sm text-slate-200 space-y-2 leading-relaxed shadow-sm">
                                ${matchedEntry.responseHTML}
                            </div>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', aiMsgHTML);
                lucide.createIcons();
                scrollToBottom();
            }, 1000);
        }

        // Scroll Helper
        function scrollToBottom() {
            const container = document.getElementById('chat-messages-container');
            container.scrollTop = container.scrollHeight;
        }

        // Utility HTML Escaper
        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }

        // Voice Input Simulation
        function toggleVoiceInput() {
            isListening = !isListening;
            const visualizer = document.getElementById('voice-visualizer');
            if (isListening) {
                visualizer.classList.remove('hidden');
                // Simulate voice transcription after 3 seconds
                setTimeout(() => {
                    if (isListening) {
                        document.getElementById('chat-input').value = "Check my checking account balance";
                        stopVoiceInput();
                    }
                }, 3000);
            } else {
                visualizer.classList.add('hidden');
            }
        }

        function stopVoiceInput() {
            isListening = false;
            document.getElementById('voice-visualizer').classList.add('hidden');
        }

        // Interactive Transfer Authorize Action
        function confirmTransferAction(buttonElement) {
            buttonElement.disabled = true;
            buttonElement.innerText = "Processing Auth...";
            buttonElement.classList.remove('bg-blue-600', 'hover:bg-blue-500');
            buttonElement.classList.add('bg-slate-700', 'text-slate-400');

            setTimeout(() => {
                buttonElement.parentElement.innerHTML = `
                    <div class="flex items-center gap-2 text-emerald-400 font-semibold">
                        <i data-lucide="check-circle" class="w-4 h-4"></i>
                        <span>Transfer of $200.00 to Sarah Jenkins Completed!</span>
                    </div>
                    <p class="text-[11px] text-slate-400 mt-1">Ref ID: TXN-9948201 • Updated Checking Balance: $12,250.80</p>
                `;
                lucide.createIcons();
            }, 1200);
        }

        // Card Freeze Toggle
        function toggleCardFreeze(isFrozen) {
            if (isFrozen) {
                sendQuickPrompt('Freeze my card');
                switchView('chat');
            }
        }

        // Quick Transfer Action from Accounts View
        function executeQuickTransfer() {
            const name = document.getElementById('quick-recipient').value || "Recipient";
            const amt = document.getElementById('quick-amount').value || "100";
            switchView('chat');
            sendQuickPrompt(`Transfer $${amt} to ${name}`);
        }

        // Capability Hub: Loan Calculator
        function calculateLoanEligibility() {
            const income = parseFloat(document.getElementById('loan-income').value) || 0;
            const credit = parseInt(document.getElementById('loan-credit').value);
            const resultBox = document.getElementById('loan-result');

            const maxLoan = Math.round((income * 12) * (credit / 700) * 0.6);

            resultBox.classList.remove('hidden');
            resultBox.innerHTML = `
                <p class="font-semibold text-emerald-400">Pre-Approved AI Limit: $${maxLoan.toLocaleString()}</p>
                <p class="text-slate-400 text-[11px]">Calculated based on DTI ratio & $${income.toLocaleString()}/mo income.</p>
            `;
        }

        // Capability Hub: Fraud Demo
        function resolveFraudDemo(isUser) {
            if (isUser) {
                alert("Fraud Alert Resolved: Transaction marked as authorized by user.");
            } else {
                switchView('chat');
                sendQuickPrompt("Freeze my credit card and issue a replacement");
            }
        }

        // FAQ Filter logic
        function filterFAQs() {
            const query = document.getElementById('faq-search').value.toLowerCase();
            const items = document.querySelectorAll('.faq-item');

            items.forEach(item => {
                const text = item.innerText.toLowerCase();
                if (text.includes(query)) {
                    item.style.display = 'block';
                } else {
                    item.style.display = 'none';
                }
            });
        }

        // Clear Chat History
        function clearChatHistory() {
            document.getElementById('chat-history-list').innerHTML = '<p class="text-[11px] text-slate-500 italic px-2">History cleared</p>';
        }
    </script>
</body>
</html>
