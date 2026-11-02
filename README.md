# InflationCalculator
An interactive inflation adjustment calculator designed to show you how inflation impacts your paycheck.
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>InflationScope - Sleek Inflation & Salary Growth Calculator</title>
  <!-- Tailwind CSS 3.4.17 -->
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', '-apple-system', 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'sans-serif'],
            mono: ['JetBrains Mono', 'SFMono-Regular', 'Menlo', 'Monaco', 'Consolas', 'monospace']
          },
          colors: {
            brand: {
              50: '#f0fdf4',
              100: '#dcfce7',
              400: '#4ade80',
              500: '#22c55e',
              600: '#16a34a'
            }
          }
        }
      }
    };
  </script>
  <style>
    /* Range Slider Styling */
    input[type=range] {
      -webkit-appearance: none;
      appearance: none;
      background: transparent;
      height: 8px;
      border-radius: 9999px;
    }
    input[type=range]:focus {
      outline: none;
    }
    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      height: 24px;
      width: 24px;
      border-radius: 50%;
      background: #38bdf8;
      border: 3px solid #0f172a;
      box-shadow: 0 0 14px rgba(56, 189, 248, 0.6);
      cursor: pointer;
      margin-top: -8px;
      transition: transform 0.15s ease, background-color 0.15s ease, box-shadow 0.15s ease;
    }
    .dark input[type=range]::-webkit-slider-thumb {
      border-color: #0f172a;
    }
    html:not(.dark) input[type=range]::-webkit-slider-thumb {
      border-color: #ffffff;
      box-shadow: 0 2px 8px rgba(14, 165, 233, 0.4);
    }
    input[type=range]::-webkit-slider-thumb:hover {
      transform: scale(1.18);
      background: #7dd3fc;
    }
    input[type=range]::-webkit-slider-thumb:active {
      transform: scale(0.95);
      background: #0284c7;
    }
    input[type=range]::-webkit-slider-runnable-track {
      width: 100%;
      height: 8px;
      cursor: pointer;
      border-radius: 9999px;
    }
    /* Firefox */
    input[type=range]::-moz-range-thumb {
      height: 22px;
      width: 22px;
      border-radius: 50%;
      background: #38bdf8;
      border: 3px solid #0f172a;
      box-shadow: 0 0 12px rgba(56, 189, 248, 0.6);
      cursor: pointer;
    }
    input[type=range]::-moz-range-track {
      width: 100%;
      height: 8px;
      cursor: pointer;
      border-radius: 9999px;
    }
  </style>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 min-h-screen flex flex-col antialiased selection:bg-sky-500 selection:text-white transition-colors duration-300">

  <!-- Navigation / Header -->
  <header class="border-b border-slate-200 dark:border-slate-800/80 bg-white/80 dark:bg-slate-900/70 backdrop-blur-md sticky top-0 z-40 transition-colors duration-300">
    <div class="max-w-5xl mx-auto px-4 sm:px-6 h-16 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-tr from-sky-500 to-emerald-400 flex items-center justify-center shadow-lg shadow-sky-500/20">
          <svg class="w-5 h-5 text-slate-950 font-bold" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
          </svg>
        </div>
        <div>
          <div class="flex items-center gap-2">
            <h1 class="text-base font-bold text-slate-900 dark:text-white tracking-tight">InflationScope</h1>
            <span class="text-[10px] font-semibold uppercase px-2 py-0.5 rounded-full bg-sky-500/10 text-sky-600 dark:text-sky-400 border border-sky-500/20">Interactive Calculator</span>
          </div>
          <p class="text-xs text-slate-500 dark:text-slate-400 hidden sm:block">Purchasing Power Analysis & Target Compensation Modeler</p>
        </div>
      </div>

      <!-- Controls: Method Toggle & Reset -->
      <div class="flex items-center space-x-2 sm:space-x-3">
        <button id="themeToggleBtn" type="button" aria-label="Toggle theme" class="p-2 rounded-lg bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 transition border border-slate-300 dark:border-slate-700/60 shadow-sm">
          <!-- Moon Icon (shown in dark) -->
          <svg id="moonIcon" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
          </svg>
          <!-- Sun Icon (hidden in dark) -->
          <svg id="sunIcon" class="w-4 h-4 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
          </svg>
        </button>

        <button id="resetBtn" type="button" class="text-xs font-medium px-3 py-2 rounded-lg bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white transition border border-slate-300 dark:border-slate-700/60 flex items-center gap-1.5 shadow-sm">
          <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
          </svg>
          Reset
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-1 max-w-5xl w-full mx-auto px-4 sm:px-6 py-6 sm:py-8 space-y-6 sm:space-y-8">

    <!-- Scenarios Quick Bar -->
    <div class="flex items-center gap-2 overflow-x-auto pb-1 text-xs">
      <span class="text-slate-500 dark:text-slate-400 font-medium whitespace-nowrap">Quick Scenarios:</span>
      <button type="button" class="scenario-btn px-2.5 py-1 rounded-full bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-600 dark:text-slate-300 hover:border-sky-400 dark:hover:border-sky-500 transition whitespace-nowrap" data-salary="75000" data-rate="3.1">
        Moderate / US Median ($75k @ 3.1%)
      </button>
      <button type="button" class="scenario-btn px-2.5 py-1 rounded-full bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-600 dark:text-slate-300 hover:border-sky-400 dark:hover:border-sky-500 transition whitespace-nowrap" data-salary="100000" data-rate="4.5">
        Current Baseline ($100k @ 4.5%)
      </button>
      <button type="button" class="scenario-btn px-2.5 py-1 rounded-full bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-600 dark:text-slate-300 hover:border-sky-400 dark:hover:border-sky-500 transition whitespace-nowrap" data-salary="140000" data-rate="6.5">
        High Inflation Wave ($140k @ 6.5%)
      </button>
      <button type="button" class="scenario-btn px-2.5 py-1 rounded-full bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-600 dark:text-slate-300 hover:border-sky-400 dark:hover:border-sky-500 transition whitespace-nowrap" data-salary="60000" data-rate="2.0">
        Low Fed Target ($60k @ 2.0%)
      </button>
    </div>

    <!-- Top Grid: Interactive Inputs -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-stretch">
      
      <!-- Input Card 1: Dollar Baseline -->
      <div class="lg:col-span-5 bg-white dark:bg-slate-900/70 border border-slate-200 dark:border-slate-800 rounded-2xl p-6 shadow-md dark:shadow-xl backdrop-blur-sm flex flex-col justify-between transition-colors duration-300">
        <div>
          <div class="flex items-center justify-between mb-2">
            <label for="salaryInput" class="text-xs font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400">Prior Year Dollar Amount</label>
            <span class="text-[11px] text-slate-400 dark:text-slate-500 font-medium">e.g. prior year salary</span>
          </div>
          
          <!-- Input field with currency symbol -->
          <div class="relative mt-2">
            <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none text-slate-400 font-semibold text-xl">
              $
            </div>
            <input 
              type="text" 
              id="salaryInput" 
              value="85,000" 
              inputmode="numeric"
              class="w-full pl-9 pr-4 py-3.5 bg-slate-50 dark:bg-slate-950/80 border border-slate-300 dark:border-slate-700/80 rounded-xl text-2xl font-bold font-mono text-slate-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-sky-500 focus:border-transparent transition shadow-inner"
              placeholder="0"
            />
          </div>

          <!-- Quick presets for dollar amount -->
          <div class="mt-4 flex flex-wrap items-center gap-2">
            <span class="text-xs text-slate-500">Presets:</span>
            <button type="button" class="preset-salary text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-value="50000">$50k</button>
            <button type="button" class="preset-salary text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-value="75000">$75k</button>
            <button type="button" class="preset-salary text-xs px-2.5 py-1 rounded-md bg-sky-500/20 border-sky-500/40 text-sky-600 dark:text-sky-300 border transition font-mono active" data-value="85000">$85k</button>
            <button type="button" class="preset-salary text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-value="100000">$100k</button>
            <button type="button" class="preset-salary text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-value="150000">$150k</button>
          </div>
        </div>

        <!-- Frequency selector (Annual vs Monthly vs Bi-Weekly) -->
        <div class="mt-6 pt-5 border-t border-slate-200 dark:border-slate-800/80 flex items-center justify-between">
          <span class="text-xs text-slate-500 dark:text-slate-400 font-medium">Display Breakdown:</span>
          <div class="inline-flex rounded-lg bg-slate-100 dark:bg-slate-950 p-1 border border-slate-200 dark:border-slate-800" role="group">
            <button type="button" id="freqAnnual" class="freq-btn text-xs px-3 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition shadow">Annual</button>
            <button type="button" id="freqMonthly" class="freq-btn text-xs px-3 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition">Monthly</button>
            <button type="button" id="freqBiweekly" class="freq-btn text-xs px-3 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition">Bi-weekly</button>
          </div>
        </div>
      </div>

      <!-- Input Card 2: Inflation Rate Slider -->
      <div class="lg:col-span-7 bg-white dark:bg-slate-900/70 border border-slate-200 dark:border-slate-800 rounded-2xl p-6 shadow-md dark:shadow-xl backdrop-blur-sm flex flex-col justify-between transition-colors duration-300">
        <div>
          <div class="flex items-center justify-between mb-2">
            <div>
              <label for="inflationSlider" class="text-xs font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400">Annual Inflation Rate</label>
              <div class="text-[11px] text-slate-400 dark:text-slate-500 font-medium">Slide to adjust annual cost-of-living increase</div>
            </div>
            <!-- Live numeric badge -->
            <div class="flex items-center space-x-2">
              <div id="rateBadge" class="px-2.5 py-1 rounded-lg text-xs font-medium border bg-amber-500/10 text-amber-600 dark:text-amber-400 border-amber-500/20">
                Moderate Inflation
              </div>
              <div class="flex items-center bg-slate-100 dark:bg-slate-950 border border-slate-300 dark:border-slate-700 rounded-xl px-3 py-1.5 shadow-inner">
                <input 
                  type="number" 
                  id="inflationNumber" 
                  value="4.5" 
                  min="0" 
                  max="20" 
                  step="0.1"
                  class="w-16 bg-transparent text-right font-mono font-bold text-xl text-sky-600 dark:text-sky-400 focus:outline-none"
                />
                <span class="text-sky-600 dark:text-sky-400 font-bold ml-0.5">%</span>
              </div>
            </div>
          </div>

          <!-- Interactive Slider -->
          <div class="mt-6 mb-2">
            <div class="relative flex items-center">
              <input 
                type="range" 
                id="inflationSlider" 
                min="0.0" 
                max="15.0" 
                step="0.1" 
                value="4.5"
                class="w-full z-10"
              />
            </div>
            <!-- Slider scale indicators -->
            <div class="flex justify-between text-[11px] font-mono text-slate-400 dark:text-slate-500 mt-2 px-1">
              <span>0%</span>
              <span>3%</span>
              <span>6%</span>
              <span>9%</span>
              <span>12%</span>
              <span>15%</span>
            </div>
          </div>

          <!-- Inflation Benchmarks Quick Select -->
          <div class="mt-4 flex flex-wrap items-center gap-2">
            <span class="text-xs text-slate-500">Benchmarks:</span>
            <button type="button" class="preset-rate text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-rate="2.0">2.0% Fed Target</button>
            <button type="button" class="preset-rate text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-rate="3.1">3.1% Long-Term Avg</button>
            <button type="button" class="preset-rate text-xs px-2.5 py-1 rounded-md bg-sky-500/20 border-sky-500/40 text-sky-600 dark:text-sky-300 border transition font-mono active" data-rate="4.5">4.5% Current Rate</button>
            <button type="button" class="preset-rate text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-rate="7.0">7.0% High Surge</button>
            <button type="button" class="preset-rate text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono" data-rate="9.1">9.1% 2022 Peak</button>
          </div>
        </div>

        <!-- Compounding Method Setting -->
        <div class="mt-6 pt-5 border-t border-slate-200 dark:border-slate-800/80 flex items-center justify-between text-xs">
          <div class="flex items-center space-x-1.5 text-slate-500 dark:text-slate-400">
            <span class="font-medium">Beating Inflation Formula:</span>
            <span id="formulaTooltip" title="Compounded calculates true purchasing power gain: Base × (1 + inflation) × (1 + real_growth). Additive does: Base × (1 + inflation + growth)." class="cursor-help text-slate-400 hover:text-slate-600 dark:hover:text-slate-200">
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
            </span>
          </div>
          <div class="inline-flex rounded-lg bg-slate-100 dark:bg-slate-950 p-1 border border-slate-200 dark:border-slate-800" role="group">
            <button type="button" id="calcCompounded" class="calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition">Compounded Real</button>
            <button type="button" id="calcAdditive" class="calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition">Additive</button>
          </div>
        </div>
      </div>

    </div>

    <!-- Middle Section: Primary Data Relays (2 Large Impact Cards) -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Impact Card 1: What Your Dollar Is Worth At Present -->
      <div class="relative overflow-hidden bg-white dark:bg-gradient-to-br dark:from-slate-900 dark:via-slate-900/90 dark:to-rose-950/20 border border-slate-200 dark:border-slate-800/90 rounded-2xl p-6 shadow-md dark:shadow-xl transition-colors duration-300">
        <div class="flex items-start justify-between">
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-rose-600 dark:text-rose-400 flex items-center gap-1.5">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path>
              </svg>
              Purchasing Power Today
            </span>
            <h3 class="text-slate-500 dark:text-slate-400 text-sm mt-1">If your dollar amount remained unadjusted</h3>
          </div>
          <span class="text-xs px-2.5 py-1 rounded-md bg-rose-500/10 text-rose-600 dark:text-rose-300 border border-rose-500/20 font-mono font-medium" id="devaluePctBadge">
            -4.31% purchasing power
          </span>
        </div>

        <div class="mt-6 flex items-baseline space-x-3">
          <span id="currentRealValue" class="text-3xl sm:text-4xl font-extrabold font-mono text-slate-900 dark:text-white tracking-tight">$81,340</span>
          <span class="text-xs text-slate-500 dark:text-slate-400 font-medium" id="currentPeriodLabel">real purchasing power</span>
        </div>

        <!-- Per Dollar Callout -->
        <div class="mt-4 p-3.5 rounded-xl bg-slate-50 dark:bg-slate-950/60 border border-slate-200 dark:border-slate-800/80 flex items-center justify-between">
          <div class="flex items-center space-x-3">
            <div class="w-8 h-8 rounded-lg bg-rose-500/10 text-rose-600 dark:text-rose-400 flex items-center justify-center font-bold text-sm">
              $1
            </div>
            <div>
              <div class="text-xs text-slate-800 dark:text-slate-200 font-medium">Your $1.00 is currently worth:</div>
              <div class="text-[11px] text-slate-500">In baseline buying power</div>
            </div>
          </div>
          <div class="text-right">
            <span id="singleDollarValue" class="text-lg font-bold font-mono text-rose-600 dark:text-rose-300">$0.957</span>
            <div id="dollarLoss" class="text-[11px] text-rose-500 dark:text-rose-400/80 font-mono">-$0.043 / dollar</div>
          </div>
        </div>

        <div class="mt-4 flex items-center justify-between text-xs text-slate-500 dark:text-slate-400">
          <span>Unrealized buying power eroded:</span>
          <span id="totalLostPower" class="font-mono font-semibold text-rose-600 dark:text-rose-400">-$3,660</span>
        </div>
      </div>

      <!-- Impact Card 2: How Much You Need to Keep Pace -->
      <div class="relative overflow-hidden bg-white dark:bg-gradient-to-br dark:from-slate-900 dark:via-slate-900/90 dark:to-sky-950/20 border border-slate-200 dark:border-slate-800/90 rounded-2xl p-6 shadow-md dark:shadow-xl transition-colors duration-300">
        <div class="flex items-start justify-between">
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-sky-600 dark:text-sky-400 flex items-center gap-1.5">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h14M12 5l7 7-7 7"></path>
              </svg>
              To Keep Pace with Inflation
            </span>
            <h3 class="text-slate-500 dark:text-slate-400 text-sm mt-1">Break-even target (0% real purchasing loss)</h3>
          </div>
          <span class="text-xs px-2.5 py-1 rounded-md bg-sky-500/10 text-sky-600 dark:text-sky-300 border border-sky-500/20 font-mono font-medium" id="keepPacePctBadge">
            +4.50% required
          </span>
        </div>

        <div class="mt-6 flex items-baseline space-x-3">
          <span id="keepPaceSalary" class="text-3xl sm:text-4xl font-extrabold font-mono text-slate-900 dark:text-white tracking-tight">$88,825</span>
          <span class="text-xs text-slate-500 dark:text-slate-400 font-medium" id="keepPeriodLabel">target compensation</span>
        </div>

        <!-- Breakdown delta callout -->
        <div class="mt-4 p-3.5 rounded-xl bg-slate-50 dark:bg-slate-950/60 border border-slate-200 dark:border-slate-800/80 flex items-center justify-between">
          <div class="flex items-center space-x-3">
            <div class="w-8 h-8 rounded-lg bg-sky-500/10 text-sky-600 dark:text-sky-400 flex items-center justify-center font-bold text-sm">
              +$
            </div>
            <div>
              <div class="text-xs text-slate-800 dark:text-slate-200 font-medium">Cost-of-Living Adjustment (COLA):</div>
              <div class="text-[11px] text-slate-500">To maintain exact purchasing parity</div>
            </div>
          </div>
          <div class="text-right">
            <span id="keepPaceRaise" class="text-lg font-bold font-mono text-sky-600 dark:text-sky-300">+$3,825</span>
            <div id="keepPaceMonthly" class="text-[11px] text-sky-500 dark:text-sky-400/80 font-mono">+$318.75 / mo</div>
          </div>
        </div>

        <div class="mt-4 flex items-center justify-between text-xs text-slate-500 dark:text-slate-400">
          <span>Net real wealth change:</span>
          <span class="font-mono font-semibold text-slate-600 dark:text-slate-300">$0.00 (Breakeven)</span>
        </div>
      </div>

    </div>

    <!-- Beat Inflation Tiers Section: 3%, 6%, 9% Real Growth -->
    <div class="space-y-4">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2">
        <div>
          <h2 class="text-lg font-bold text-slate-900 dark:text-white tracking-tight flex items-center gap-2">
            Target Earnings to Beat Inflation
            <span class="text-xs font-normal text-slate-500 dark:text-slate-400 font-sans">(Real Growth Milestones)</span>
          </h2>
          <p class="text-xs text-slate-500 dark:text-slate-400">How much you need to earn to stay ahead of rising costs and increase real discretionary income.</p>
        </div>
        <div class="flex items-center space-x-2">
          <button id="copySummaryBtn" type="button" class="text-xs px-3 py-1.5 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-300 dark:border-slate-700 flex items-center gap-1.5 transition shadow-sm">
            <svg class="w-3.5 h-3.5 text-sky-500 dark:text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"></path>
            </svg>
            <span id="copyBtnText">Copy Breakdown</span>
          </button>
        </div>
      </div>

      <!-- 3 Tier Cards -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">

        <!-- Tier 1: Beat by 3% -->
        <div class="group relative bg-white dark:bg-slate-900/80 hover:bg-slate-50 dark:hover:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-emerald-500/50 rounded-2xl p-5 shadow-md dark:shadow-lg transition-all duration-200 flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between">
              <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 dark:text-emerald-400 flex items-center gap-1.5">
                <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                Beat by +3%
              </span>
              <span class="text-[11px] font-mono px-2 py-0.5 rounded bg-emerald-500/10 text-emerald-600 dark:text-emerald-300 border border-emerald-500/20 font-medium" id="tier3RateBadge">
                +7.64% total
              </span>
            </div>
            
            <div class="mt-4">
              <div class="text-xs text-slate-500 dark:text-slate-400">Target Dollar Amount:</div>
              <div id="tier3Salary" class="text-2xl sm:text-3xl font-extrabold font-mono text-slate-900 dark:text-white mt-1">$91,490</div>
            </div>

            <!-- Details list -->
            <div class="mt-4 space-y-2 text-xs border-t border-slate-200 dark:border-slate-800/80 pt-3">
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Total raise from base:</span>
                <span id="tier3Raise" class="font-mono font-semibold text-emerald-600 dark:text-emerald-400">+$6,490</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Surplus beyond inflation:</span>
                <span id="tier3RealSurplus" class="font-mono text-slate-700 dark:text-slate-300">+$2,665</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Prorated addition:</span>
                <span id="tier3Monthly" class="font-mono text-slate-700 dark:text-slate-300">+$540.83 / mo</span>
              </div>
            </div>
          </div>

          <div class="mt-5 pt-3 border-t border-slate-200 dark:border-slate-800/80">
            <div class="text-[11px] text-slate-500 flex items-center justify-between">
              <span>Outcome</span>
              <span class="text-slate-700 dark:text-slate-300 font-medium">Modest Real Growth</span>
            </div>
          </div>
        </div>

        <!-- Tier 2: Beat by 6% -->
        <div class="group relative bg-white dark:bg-slate-900/80 hover:bg-slate-50 dark:hover:bg-slate-900 border border-emerald-500/40 hover:border-emerald-500/70 rounded-2xl p-5 shadow-md dark:shadow-lg transition-all duration-200 flex flex-col justify-between ring-1 ring-emerald-500/30">
          <!-- Featured Badge -->
          <div class="absolute -top-2.5 right-6 px-2.5 py-0.5 rounded-full bg-emerald-500 text-white dark:text-slate-950 font-bold text-[10px] tracking-wide uppercase shadow">
            Recommended Target
          </div>

          <div>
            <div class="flex items-center justify-between">
              <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 dark:text-emerald-400 flex items-center gap-1.5">
                <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                Beat by +6%
              </span>
              <span class="text-[11px] font-mono px-2 py-0.5 rounded bg-emerald-500/10 text-emerald-600 dark:text-emerald-300 border border-emerald-500/20 font-medium" id="tier6RateBadge">
                +10.77% total
              </span>
            </div>
            
            <div class="mt-4">
              <div class="text-xs text-slate-500 dark:text-slate-400">Target Dollar Amount:</div>
              <div id="tier6Salary" class="text-2xl sm:text-3xl font-extrabold font-mono text-slate-900 dark:text-white mt-1">$94,155</div>
            </div>

            <!-- Details list -->
            <div class="mt-4 space-y-2 text-xs border-t border-slate-200 dark:border-slate-800/80 pt-3">
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Total raise from base:</span>
                <span id="tier6Raise" class="font-mono font-semibold text-emerald-600 dark:text-emerald-400">+$9,155</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Surplus beyond inflation:</span>
                <span id="tier6RealSurplus" class="font-mono text-slate-700 dark:text-slate-300">+$5,330</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Prorated addition:</span>
                <span id="tier6Monthly" class="font-mono text-slate-700 dark:text-slate-300">+$762.88 / mo</span>
              </div>
            </div>
          </div>

          <div class="mt-5 pt-3 border-t border-slate-200 dark:border-slate-800/80">
            <div class="text-[11px] text-slate-500 flex items-center justify-between">
              <span>Outcome</span>
              <span class="text-emerald-600 dark:text-emerald-400 font-semibold">Strong Career Step</span>
            </div>
          </div>
        </div>

        <!-- Tier 3: Beat by 9% -->
        <div class="group relative bg-white dark:bg-slate-900/80 hover:bg-slate-50 dark:hover:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-emerald-500/50 rounded-2xl p-5 shadow-md dark:shadow-lg transition-all duration-200 flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between">
              <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 dark:text-emerald-400 flex items-center gap-1.5">
                <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                Beat by +9%
              </span>
              <span class="text-[11px] font-mono px-2 py-0.5 rounded bg-emerald-500/10 text-emerald-600 dark:text-emerald-300 border border-emerald-500/20 font-medium" id="tier9RateBadge">
                +13.91% total
              </span>
            </div>
            
            <div class="mt-4">
              <div class="text-xs text-slate-500 dark:text-slate-400">Target Dollar Amount:</div>
              <div id="tier9Salary" class="text-2xl sm:text-3xl font-extrabold font-mono text-slate-900 dark:text-white mt-1">$96,819</div>
            </div>

            <!-- Details list -->
            <div class="mt-4 space-y-2 text-xs border-t border-slate-200 dark:border-slate-800/80 pt-3">
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Total raise from base:</span>
                <span id="tier9Raise" class="font-mono font-semibold text-emerald-600 dark:text-emerald-400">+$11,819</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Surplus beyond inflation:</span>
                <span id="tier9RealSurplus" class="font-mono text-slate-700 dark:text-slate-300">+$7,994</span>
              </div>
              <div class="flex justify-between">
                <span class="text-slate-500 dark:text-slate-400">Prorated addition:</span>
                <span id="tier9Monthly" class="font-mono text-slate-700 dark:text-slate-300">+$984.94 / mo</span>
              </div>
            </div>
          </div>

          <div class="mt-5 pt-3 border-t border-slate-200 dark:border-slate-800/80">
            <div class="text-[11px] text-slate-500 flex items-center justify-between">
              <span>Outcome</span>
              <span class="text-slate-700 dark:text-slate-300 font-medium">Aggressive Wealth Expansion</span>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- Visual Interactive Comparison Ladder -->
    <div class="bg-white dark:bg-slate-900/70 border border-slate-200 dark:border-slate-800 rounded-2xl p-6 shadow-md dark:shadow-xl backdrop-blur-sm transition-colors duration-300">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-4 border-b border-slate-200 dark:border-slate-800 gap-2">
        <div>
          <h3 class="text-sm font-bold text-slate-900 dark:text-white tracking-tight">Compensation Ladder Visualizer</h3>
          <p class="text-xs text-slate-500 dark:text-slate-400">Relative scale comparing present eroded purchasing power vs baseline vs target milestones</p>
        </div>
        <div class="text-xs font-mono text-slate-500 dark:text-slate-400" id="ladderBaseLabel">
          Baseline: $85,000
        </div>
      </div>

      <!-- Comparison Progress Bars -->
      <div class="mt-6 space-y-4">
        
        <!-- Bar 1: Eroded Purchasing Power -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-rose-500"></span>
              Present Value (Eroded)
            </span>
            <span id="barLabelEroded" class="font-mono text-rose-600 dark:text-rose-400 font-semibold">$81,340 (-4.3%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barEroded" class="h-full bg-rose-500/80 rounded-full transition-all duration-300" style="width: 75%;"></div>
          </div>
        </div>

        <!-- Bar 2: Base Salary -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-slate-400"></span>
              Prior Year Baseline
            </span>
            <span id="barLabelBase" class="font-mono text-slate-700 dark:text-slate-300 font-semibold">$85,000 (100%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barBase" class="h-full bg-slate-400 dark:bg-slate-500/80 rounded-full transition-all duration-300" style="width: 80%;"></div>
          </div>
        </div>

        <!-- Bar 3: Keep Pace -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-sky-500"></span>
              Keep Pace with Inflation
            </span>
            <span id="barLabelKeep" class="font-mono text-sky-600 dark:text-sky-400 font-semibold">$88,825 (+4.5%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barKeep" class="h-full bg-sky-500/80 rounded-full transition-all duration-300" style="width: 85%;"></div>
          </div>
        </div>

        <!-- Bar 4: Beat by 3% -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-emerald-500"></span>
              Beat by +3%
            </span>
            <span id="barLabelBeat3" class="font-mono text-emerald-600 dark:text-emerald-400 font-semibold">$91,490 (+7.6%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barBeat3" class="h-full bg-emerald-500/80 rounded-full transition-all duration-300" style="width: 90%;"></div>
          </div>
        </div>

        <!-- Bar 5: Beat by 6% -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-emerald-400"></span>
              Beat by +6%
            </span>
            <span id="barLabelBeat6" class="font-mono text-emerald-600 dark:text-emerald-400 font-semibold">$94,155 (+10.8%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barBeat6" class="h-full bg-emerald-500 dark:bg-emerald-400/90 rounded-full transition-all duration-300" style="width: 95%;"></div>
          </div>
        </div>

        <!-- Bar 6: Beat by 9% -->
        <div>
          <div class="flex justify-between text-xs mb-1.5">
            <span class="text-slate-600 dark:text-slate-400 font-medium flex items-center gap-1.5">
              <span class="w-2.5 h-2.5 rounded-sm bg-emerald-300"></span>
              Beat by +9%
            </span>
            <span id="barLabelBeat9" class="font-mono text-emerald-600 dark:text-emerald-300 font-semibold">$96,819 (+13.9%)</span>
          </div>
          <div class="w-full bg-slate-100 dark:bg-slate-950 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-200 dark:border-slate-800">
            <div id="barBeat9" class="h-full bg-emerald-400 dark:bg-emerald-300 rounded-full transition-all duration-300" style="width: 100%;"></div>
          </div>
        </div>

      </div>
    </div>

    <!-- Quick Negotiation / Practical Tips Accordion -->
    <div class="border border-slate-200 dark:border-slate-800/80 rounded-xl bg-white dark:bg-slate-900/40 p-4 text-xs text-slate-500 dark:text-slate-400 space-y-2 transition-colors duration-300">
      <div class="flex items-center justify-between cursor-pointer select-none" id="toggleTipsBtn">
        <span class="font-semibold text-slate-800 dark:text-slate-300 flex items-center gap-2">
          <svg class="w-4 h-4 text-sky-500 dark:text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
          How to use these numbers in salary reviews & negotiations
        </span>
        <svg id="tipsChevron" class="w-4 h-4 text-slate-400 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
      </div>
      <div id="tipsContent" class="hidden pt-3 border-t border-slate-200 dark:border-slate-800 space-y-2 leading-relaxed text-slate-600 dark:text-slate-400">
        <p><strong class="text-slate-800 dark:text-slate-200">Cost-of-Living Adjustment (COLA):</strong> A raise equal to inflation (<span class="font-mono text-sky-600 dark:text-sky-400 font-semibold" id="tipInflationRate">4.5%</span>) is not a merit increase. It merely preserves your original purchasing power so you do not experience a real-dollar pay cut.</p>
        <p><strong class="text-slate-800 dark:text-slate-200">Merit Growth:</strong> True career progress requires an increase <em>above</em> the inflation baseline. Aiming for the <span class="font-mono text-emerald-600 dark:text-emerald-400 font-semibold">+6% tier</span> secures genuine wealth accumulation and protects your future savings.</p>
      </div>
    </div>

  </main>

  <!-- In-Page Toast Notification (Replaces alert) -->
  <div id="toast" class="fixed bottom-6 right-6 z-50 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none bg-slate-900 border border-emerald-500/40 text-emerald-300 px-4 py-3 rounded-xl shadow-2xl flex items-center gap-3">
    <svg class="w-5 h-5 text-emerald-400 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
    <span id="toastMsg" class="text-xs font-medium">Summary copied to clipboard!</span>
  </div>

  <!-- Footer -->
  <footer class="border-t border-slate-200 dark:border-slate-800/60 py-6 text-center text-xs text-slate-400 dark:text-slate-500 transition-colors duration-300">
    <p>InflationScope &bull; Sleek, interactive inflation and salary growth modeling</p>
  </footer>

  <!-- Application Logic -->
  <script>
    (function() {
      // State
      let baseSalary = 85000;
      let inflationRate = 4.5;
      let isCompounded = true; // true: Base * (1+r) * (1+g), false: Base * (1+r+g)
      let frequency = 'annual'; // 'annual', 'monthly', 'biweekly'
      let isDark = true;

      // DOM Elements
      const salaryInput = document.getElementById('salaryInput');
      const inflationSlider = document.getElementById('inflationSlider');
      const inflationNumber = document.getElementById('inflationNumber');
      const rateBadge = document.getElementById('rateBadge');
      
      const freqAnnual = document.getElementById('freqAnnual');
      const freqMonthly = document.getElementById('freqMonthly');
      const freqBiweekly = document.getElementById('freqBiweekly');
      
      const calcCompounded = document.getElementById('calcCompounded');
      const calcAdditive = document.getElementById('calcAdditive');
      
      const currentRealValue = document.getElementById('currentRealValue');
      const singleDollarValue = document.getElementById('singleDollarValue');
      const dollarLoss = document.getElementById('dollarLoss');
      const totalLostPower = document.getElementById('totalLostPower');
      const devaluePctBadge = document.getElementById('devaluePctBadge');
      const currentPeriodLabel = document.getElementById('currentPeriodLabel');

      const keepPaceSalary = document.getElementById('keepPaceSalary');
      const keepPaceRaise = document.getElementById('keepPaceRaise');
      const keepPaceMonthly = document.getElementById('keepPaceMonthly');
      const keepPacePctBadge = document.getElementById('keepPacePctBadge');
      const keepPeriodLabel = document.getElementById('keepPeriodLabel');

      const tier3Salary = document.getElementById('tier3Salary');
      const tier3Raise = document.getElementById('tier3Raise');
      const tier3RealSurplus = document.getElementById('tier3RealSurplus');
      const tier3Monthly = document.getElementById('tier3Monthly');
      const tier3RateBadge = document.getElementById('tier3RateBadge');

      const tier6Salary = document.getElementById('tier6Salary');
      const tier6Raise = document.getElementById('tier6Raise');
      const tier6RealSurplus = document.getElementById('tier6RealSurplus');
      const tier6Monthly = document.getElementById('tier6Monthly');
      const tier6RateBadge = document.getElementById('tier6RateBadge');

      const tier9Salary = document.getElementById('tier9Salary');
      const tier9Raise = document.getElementById('tier9Raise');
      const tier9RealSurplus = document.getElementById('tier9RealSurplus');
      const tier9Monthly = document.getElementById('tier9Monthly');
      const tier9RateBadge = document.getElementById('tier9RateBadge');

      const ladderBaseLabel = document.getElementById('ladderBaseLabel');
      const barEroded = document.getElementById('barEroded');
      const barBase = document.getElementById('barBase');
      const barKeep = document.getElementById('barKeep');
      const barBeat3 = document.getElementById('barBeat3');
      const barBeat6 = document.getElementById('barBeat6');
      const barBeat9 = document.getElementById('barBeat9');

      const barLabelEroded = document.getElementById('barLabelEroded');
      const barLabelBase = document.getElementById('barLabelBase');
      const barLabelKeep = document.getElementById('barLabelKeep');
      const barLabelBeat3 = document.getElementById('barLabelBeat3');
      const barLabelBeat6 = document.getElementById('barLabelBeat6');
      const barLabelBeat9 = document.getElementById('barLabelBeat9');

      const tipInflationRate = document.getElementById('tipInflationRate');
      const copySummaryBtn = document.getElementById('copySummaryBtn');
      const copyBtnText = document.getElementById('copyBtnText');
      const resetBtn = document.getElementById('resetBtn');
      const themeToggleBtn = document.getElementById('themeToggleBtn');
      const moonIcon = document.getElementById('moonIcon');
      const sunIcon = document.getElementById('sunIcon');
      const toggleTipsBtn = document.getElementById('toggleTipsBtn');
      const tipsContent = document.getElementById('tipsContent');
      const tipsChevron = document.getElementById('tipsChevron');
      const toast = document.getElementById('toast');
      const toastMsg = document.getElementById('toastMsg');

      // Helper: format currency
      function formatMoney(amount, decimals = 0) {
        return '$' + amount.toLocaleString('en-US', {
          minimumFractionDigits: decimals,
          maximumFractionDigits: decimals
        });
      }

      // Helper: parse sanitized number
      function parseNumber(str) {
        if (!str) return 0;
        const cleaned = str.toString().replace(/[^0-9.]/g, '');
        const val = parseFloat(cleaned);
        return isNaN(val) ? 0 : val;
      }

      // Update slider gradient track
      function updateSliderTrack(val) {
        const min = parseFloat(inflationSlider.min) || 0;
        const max = parseFloat(inflationSlider.max) || 15;
        const pct = Math.min(100, Math.max(0, ((val - min) / (max - min)) * 100));
        const trackColor = isDark ? '#334155' : '#cbd5e1';
        inflationSlider.style.background = `linear-gradient(to right, #38bdf8 0%, #38bdf8 ${pct}%, ${trackColor} ${pct}%, ${trackColor} 100%)`;
      }

      // Show toast
      let toastTimer = null;
      function showToast(msg) {
        toastMsg.textContent = msg;
        toast.classList.remove('translate-y-20', 'opacity-0');
        toast.classList.add('translate-y-0', 'opacity-100');
        if (toastTimer) clearTimeout(toastTimer);
        toastTimer = setTimeout(() => {
          toast.classList.remove('translate-y-0', 'opacity-100');
          toast.classList.add('translate-y-20', 'opacity-0');
        }, 3000);
      }

      // Update badge classification
      function updateRateBadge(r) {
        if (r <= 2.2) {
          rateBadge.textContent = 'Low / Target Inflation';
          rateBadge.className = 'px-2.5 py-1 rounded-lg text-xs font-medium border bg-emerald-500/10 text-emerald-600 dark:text-emerald-400 border-emerald-500/20';
        } else if (r <= 4.0) {
          rateBadge.textContent = 'Moderate Inflation';
          rateBadge.className = 'px-2.5 py-1 rounded-lg text-xs font-medium border bg-sky-500/10 text-sky-600 dark:text-sky-400 border-sky-500/20';
        } else if (r <= 6.5) {
          rateBadge.textContent = 'Elevated Inflation';
          rateBadge.className = 'px-2.5 py-1 rounded-lg text-xs font-medium border bg-amber-500/10 text-amber-600 dark:text-amber-400 border-amber-500/20';
        } else {
          rateBadge.textContent = 'Severe Inflationary Pressure';
          rateBadge.className = 'px-2.5 py-1 rounded-lg text-xs font-medium border bg-rose-500/10 text-rose-600 dark:text-rose-400 border-rose-500/20';
        }
      }

      // Core calculation & render
      function calculate() {
        const P = baseSalary;
        const r = inflationRate / 100;
        
        let freqDivisor = 1;
        let freqText = 'annual';
        let subFreqText = '/ mo';

        if (frequency === 'monthly') {
          freqDivisor = 12;
          freqText = 'monthly';
          subFreqText = '/ mo';
        } else if (frequency === 'biweekly') {
          freqDivisor = 26;
          freqText = 'bi-weekly';
          subFreqText = '/ paycheck';
        }

        const effectiveBase = P / freqDivisor;
        const decimals = (frequency === 'annual') ? 0 : 2;

        // 1. Present purchasing power of $1 and total amount
        const dollarVal = 1 / (1 + r);
        const singleLoss = 1 - dollarVal;
        
        const presentRealVal = effectiveBase / (1 + r);
        const totalLossVal = effectiveBase - presentRealVal;
        const devaluePercent = (1 - (1 / (1 + r))) * 100;

        // 2. Keep pace
        const keepPaceVal = effectiveBase * (1 + r);
        const keepRaiseVal = keepPaceVal - effectiveBase;
        const keepMonthlyDelta = (P * r) / (frequency === 'annual' ? 12 : freqDivisor);

        // 3. Beat inflation tiers (+3%, +6%, +9%)
        function getTier(realGainRate) {
          let targetVal = 0;
          let totalPercent = 0;
          if (isCompounded) {
            targetVal = effectiveBase * (1 + r) * (1 + realGainRate);
            totalPercent = ((1 + r) * (1 + realGainRate) - 1) * 100;
          } else {
            targetVal = effectiveBase * (1 + r + realGainRate);
            totalPercent = (r + realGainRate) * 100;
          }
          const totalRaise = targetVal - effectiveBase;
          const realSurplus = targetVal - keepPaceVal;
          const periodAddition = (P * (totalPercent / 100)) / (frequency === 'annual' ? 12 : freqDivisor);
          return { targetVal, totalRaise, realSurplus, periodAddition, totalPercent };
        }

        const t3 = getTier(0.03);
        const t6 = getTier(0.06);
        const t9 = getTier(0.09);

        // Update DOM
        currentPeriodLabel.textContent = `${freqText} buying power`;
        keepPeriodLabel.textContent = `${freqText} target`;

        currentRealValue.textContent = formatMoney(presentRealVal, decimals);
        singleDollarValue.textContent = '$' + dollarVal.toFixed(3);
        dollarLoss.textContent = `-$${singleLoss.toFixed(3)} / dollar`;
        totalLostPower.textContent = `-${formatMoney(totalLossVal, decimals)}`;
        devaluePctBadge.textContent = `-${devaluePercent.toFixed(2)}% purchasing power`;

        keepPaceSalary.textContent = formatMoney(keepPaceVal, decimals);
        keepPaceRaise.textContent = `+${formatMoney(keepRaiseVal, decimals)}`;
        keepPaceMonthly.textContent = `+${formatMoney(keepMonthlyDelta, 2)} ${subFreqText}`;
        keepPacePctBadge.textContent = `+${inflationRate.toFixed(1)}% required`;

        // Tiers
        tier3Salary.textContent = formatMoney(t3.targetVal, decimals);
        tier3Raise.textContent = `+${formatMoney(t3.totalRaise, decimals)}`;
        tier3RealSurplus.textContent = `+${formatMoney(t3.realSurplus, decimals)}`;
        tier3Monthly.textContent = `+${formatMoney(t3.periodAddition, 2)} ${subFreqText}`;
        tier3RateBadge.textContent = `+${t3.totalPercent.toFixed(2)}% total`;

        tier6Salary.textContent = formatMoney(t6.targetVal, decimals);
        tier6Raise.textContent = `+${formatMoney(t6.totalRaise, decimals)}`;
        tier6RealSurplus.textContent = `+${formatMoney(t6.realSurplus, decimals)}`;
        tier6Monthly.textContent = `+${formatMoney(t6.periodAddition, 2)} ${subFreqText}`;
        tier6RateBadge.textContent = `+${t6.totalPercent.toFixed(2)}% total`;

        tier9Salary.textContent = formatMoney(t9.targetVal, decimals);
        tier9Raise.textContent = `+${formatMoney(t9.totalRaise, decimals)}`;
        tier9RealSurplus.textContent = `+${formatMoney(t9.realSurplus, decimals)}`;
        tier9Monthly.textContent = `+${formatMoney(t9.periodAddition, 2)} ${subFreqText}`;
        tier9RateBadge.textContent = `+${t9.totalPercent.toFixed(2)}% total`;

        // Visual Comparison Bars
        ladderBaseLabel.textContent = `Baseline: ${formatMoney(effectiveBase, decimals)} (${freqText})`;
        
        const maxVal = Math.max(t9.targetVal, 1);
        const pctEroded = (presentRealVal / maxVal) * 100;
        const pctBase = (effectiveBase / maxVal) * 100;
        const pctKeep = (keepPaceVal / maxVal) * 100;
        const pctT3 = (t3.targetVal / maxVal) * 100;
        const pctT6 = (t6.targetVal / maxVal) * 100;
        const pctT9 = 100;

        barEroded.style.width = `${Math.max(8, pctEroded)}%`;
        barBase.style.width = `${Math.max(12, pctBase)}%`;
        barKeep.style.width = `${Math.max(16, pctKeep)}%`;
        barBeat3.style.width = `${Math.max(20, pctT3)}%`;
        barBeat6.style.width = `${Math.max(24, pctT6)}%`;
        barBeat9.style.width = `${pctT9}%`;

        barLabelEroded.textContent = `${formatMoney(presentRealVal, decimals)} (-${devaluePercent.toFixed(1)}%)`;
        barLabelBase.textContent = `${formatMoney(effectiveBase, decimals)} (100%)`;
        barLabelKeep.textContent = `${formatMoney(keepPaceVal, decimals)} (+${inflationRate.toFixed(1)}%)`;
        barLabelBeat3.textContent = `${formatMoney(t3.targetVal, decimals)} (+${t3.totalPercent.toFixed(1)}%)`;
        barLabelBeat6.textContent = `${formatMoney(t6.targetVal, decimals)} (+${t6.totalPercent.toFixed(1)}%)`;
        barLabelBeat9.textContent = `${formatMoney(t9.targetVal, decimals)} (+${t9.totalPercent.toFixed(1)}%)`;

        tipInflationRate.textContent = `${inflationRate.toFixed(1)}%`;
        updateRateBadge(inflationRate);
        updateSliderTrack(inflationRate);
      }

      // Event Listeners for Salary Input
      salaryInput.addEventListener('input', function() {
        const raw = parseNumber(this.value);
        baseSalary = raw;
        calculate();
      });

      salaryInput.addEventListener('blur', function() {
        this.value = baseSalary.toLocaleString('en-US');
      });

      // Salary Presets
      document.querySelectorAll('.preset-salary').forEach(btn => {
        btn.addEventListener('click', function() {
          document.querySelectorAll('.preset-salary').forEach(b => {
            b.className = 'preset-salary text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono';
          });
          this.className = 'preset-salary text-xs px-2.5 py-1 rounded-md bg-sky-500/20 border-sky-500/40 text-sky-600 dark:text-sky-300 border transition font-mono active';
          baseSalary = parseFloat(this.getAttribute('data-value'));
          salaryInput.value = baseSalary.toLocaleString('en-US');
          calculate();
        });
      });

      // Slider & Number Input Sync
      inflationSlider.addEventListener('input', function() {
        inflationRate = parseFloat(this.value);
        inflationNumber.value = inflationRate.toFixed(1);
        calculate();
      });

      inflationNumber.addEventListener('input', function() {
        let val = parseFloat(this.value);
        if (isNaN(val)) val = 0;
        if (val > 20) val = 20;
        if (val < 0) val = 0;
        inflationRate = val;
        inflationSlider.value = Math.min(15, val);
        calculate();
      });

      // Inflation Benchmarks
      document.querySelectorAll('.preset-rate').forEach(btn => {
        btn.addEventListener('click', function() {
          document.querySelectorAll('.preset-rate').forEach(b => {
            b.className = 'preset-rate text-xs px-2.5 py-1 rounded-md bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700/50 transition font-mono';
          });
          this.className = 'preset-rate text-xs px-2.5 py-1 rounded-md bg-sky-500/20 border-sky-500/40 text-sky-600 dark:text-sky-300 border transition font-mono active';
          const r = parseFloat(this.getAttribute('data-rate'));
          inflationRate = r;
          inflationSlider.value = Math.min(15, r);
          inflationNumber.value = r.toFixed(1);
          calculate();
        });
      });

      // Scenario quick buttons
      document.querySelectorAll('.scenario-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const s = parseFloat(this.getAttribute('data-salary'));
          const r = parseFloat(this.getAttribute('data-rate'));
          baseSalary = s;
          salaryInput.value = s.toLocaleString('en-US');
          inflationRate = r;
          inflationSlider.value = Math.min(15, r);
          inflationNumber.value = r.toFixed(1);
          calculate();
          showToast(`Loaded scenario: $${s.toLocaleString()} @ ${r}%`);
        });
      });

      // Frequency buttons
      function setFrequency(freq) {
        frequency = freq;
        [freqAnnual, freqMonthly, freqBiweekly].forEach(btn => {
          btn.className = 'freq-btn text-xs px-3 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition';
        });
        if (freq === 'annual') {
          freqAnnual.className = 'freq-btn text-xs px-3 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition shadow';
        } else if (freq === 'monthly') {
          freqMonthly.className = 'freq-btn text-xs px-3 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition shadow';
        } else if (freq === 'biweekly') {
          freqBiweekly.className = 'freq-btn text-xs px-3 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition shadow';
        }
        calculate();
      }

      freqAnnual.addEventListener('click', () => setFrequency('annual'));
      freqMonthly.addEventListener('click', () => setFrequency('monthly'));
      freqBiweekly.addEventListener('click', () => setFrequency('biweekly'));

      // Compounding method toggles
      calcCompounded.addEventListener('click', () => {
        isCompounded = true;
        calcCompounded.className = 'calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition';
        calcAdditive.className = 'calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition';
        calculate();
      });

      calcAdditive.addEventListener('click', () => {
        isCompounded = false;
        calcAdditive.className = 'calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium bg-sky-500 text-white dark:text-slate-950 transition';
        calcCompounded.className = 'calc-btn text-[11px] px-2.5 py-1 rounded-md font-medium text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition';
        calculate();
      });

      // Reset
      resetBtn.addEventListener('click', () => {
        baseSalary = 85000;
        inflationRate = 4.5;
        frequency = 'annual';
        isCompounded = true;
        salaryInput.value = '85,000';
        inflationSlider.value = '4.5';
        inflationNumber.value = '4.5';
        setFrequency('annual');
        calcCompounded.click();
        calculate();
        showToast('Reset to default values.');
      });

      // Copy Summary Action
      copySummaryBtn.addEventListener('click', () => {
        const textToCopy = `Inflation & Compensation Target Breakdown:
- Baseline Dollar Amount: ${formatMoney(baseSalary)} (${frequency})
- Annual Inflation Rate: ${inflationRate.toFixed(1)}%
- Present Purchasing Power: ${currentRealValue.textContent} (${singleDollarValue.textContent} per $1.00)
- Keep Pace Target: ${keepPaceSalary.textContent} (${keepPaceRaise.textContent} required raise)
- Beat Inflation by +3%: ${tier3Salary.textContent} (${tier3Raise.textContent} total raise, ${tier3RateBadge.textContent})
- Beat Inflation by +6%: ${tier6Salary.textContent} (${tier6Raise.textContent} total raise, ${tier6RateBadge.textContent})
- Beat Inflation by +9%: ${tier9Salary.textContent} (${tier9Raise.textContent} total raise, ${tier9RateBadge.textContent})`;

        if (navigator.clipboard && navigator.clipboard.writeText) {
          navigator.clipboard.writeText(textToCopy).then(() => {
            showToast('Breakdown copied to clipboard!');
          }).catch(() => {
            fallbackCopy(textToCopy);
          });
        } else {
          fallbackCopy(textToCopy);
        }
      });

      function fallbackCopy(text) {
        const ta = document.createElement('textarea');
        ta.value = text;
        ta.style.position = 'fixed';
        ta.style.opacity = '0';
        document.body.appendChild(ta);
        ta.select();
        try {
          document.execCommand('copy');
          showToast('Breakdown copied to clipboard!');
        } catch (e) {
          showToast('Could not auto-copy, please copy manually.');
        }
        document.body.removeChild(ta);
      }

      // Toggle Tips
      toggleTipsBtn.addEventListener('click', () => {
        const isHidden = tipsContent.classList.contains('hidden');
        if (isHidden) {
          tipsContent.classList.remove('hidden');
          tipsChevron.classList.add('rotate-180');
        } else {
          tipsContent.classList.add('hidden');
          tipsChevron.classList.remove('rotate-180');
        }
      });

      // Theme toggle (Dark / Light)
      themeToggleBtn.addEventListener('click', () => {
        isDark = !isDark;
        const html = document.documentElement;
        if (isDark) {
          html.classList.add('dark');
          moonIcon.classList.remove('hidden');
          sunIcon.classList.add('hidden');
        } else {
          html.classList.remove('dark');
          moonIcon.classList.add('hidden');
          sunIcon.classList.remove('hidden');
        }
        updateSliderTrack(inflationRate);
      });

      // Initial calculation & track setup
      calculate();
    })();
  </script>
</body>
</html>
