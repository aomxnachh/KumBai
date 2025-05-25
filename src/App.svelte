<script lang="ts">
  import { onMount } from 'svelte';
  
  interface Hint {
    text: string;
    image?: string;
    color: string;
  }
  
  let currentHint: Hint | null = null;
  let clickCount: number = 0;
  let isAnimating: boolean = false;
  let terminalLines: string[] = [];
  
  const hints: Hint[] = [
    { text: "มัธยมศิลป์แห่งคณิตศาสตร์บัณฑิต นักปราชญ์สรรค์สร้างขั้นตอนวิธี สมองกลจักรกลประมวลผลเทิดทูน ชาติกาลเวลาแปรเปลี่ยนไปตามกัน ยุคแห่งข้อมูลใหญ่ไหลเวียนวน อัลกอริทึมแทรกซ้อนซับซ้อนงาม องค์ความรู้ปัญญาประดิษฐ์คิดคำนวณ มิได้หยุดหย่อนแห่งความก้าวหน้าคำ", color: "text-red-400" },
    { text: "Still clicking? Really? 🙄 / ยังจะกดอีกเหรอ?", color: "text-yellow-400" },
    { text: "You're persistent... 😏 / ดื้อจริงๆ นะ...", color: "text-blue-400", image: "https://shorturl.asia/2nmMZ" },
    { text: "Fine, here's a hint: It starts with 'H' / โอเค ใบ้ให้: ขึ้นต้นด้วย 'H'", color: "text-green-400" },
    { text: "HINT: Think greeting! / ใบ้: คิดถึงการทักทาย!", color: "text-purple-400" },
    { text: "🤦‍♂️ Hello? Really? / สวัสดี? จริงเหรอ?", color: "text-pink-400" },
    { text: "You got it! But why did you keep clicking? 😂 / ได้แล้ว! แต่ทำไมกดต่อ?", color: "text-cyan-400" },
    { text: "Okay, I'm impressed by your dedication 👏 / โอเค ชื่นชมความมุ่งมั่น", color: "text-orange-400" },
    { text: "🎉 BONUS: You're awesome! / โบนัส: คุณเจ๋งมาก!", color: "text-indigo-400" },
    { text: "Last hint: Stop clicking! 😆 / ใบ้สุดท้าย: หยุดกด!", color: "text-rose-400" }
  ];
  
  const terminalStartup = [
    "CS Comedy Science v9.9.9 - Kumbai System Initialized",
    "Loading Kumbai database...",
    "WARNING: Unauthorized clicking detected!",
    "Sawasdee Nong Rahus Kong Chun!",
    "Get ready for Kumbai..."
  ];
  
  onMount(() => {
    terminalStartup.forEach((line, index) => {
      setTimeout(() => {
        terminalLines = [...terminalLines, `> ${line}`];
      }, index * 300);
    });
  });
  
  function handleClick(): void {
    if (isAnimating) return;
    
    isAnimating = true;
    clickCount++;
    
    terminalLines = [...terminalLines, `> CLICK_EVENT_${clickCount} detected at ${new Date().toLocaleTimeString()}`];
    
    const hintIndex = Math.min(clickCount - 1, hints.length - 1);
    currentHint = hints[hintIndex];
    
    setTimeout(() => {
      terminalLines = [...terminalLines, `> HINT_${clickCount}: ${currentHint?.text}`];
      if (currentHint?.image) {
        terminalLines = [...terminalLines, `> IMAGE_LOADED`];
      }
    }, 500);
    
    setTimeout(() => {
      isAnimating = false;
    }, 1000);
    
    setTimeout(() => {
      const terminal = document.querySelector('.terminal-output');
      if (terminal) {
        terminal.scrollTop = terminal.scrollHeight;
      }
    }, 600);
  }
  
  function resetGame(): void {
    currentHint = null;
    clickCount = 0;
    terminalLines = [...terminalStartup.map(line => `> ${line}`)];
    isAnimating = false;
  }
</script>

<main class="min-h-screen bg-black text-green-400 font-mono relative overflow-hidden">
  <div class="absolute inset-0 opacity-10">
    <div class="matrix-bg"></div>
  </div>
  
  <div class="relative z-10 border-b border-green-400/30 p-4">
    <div class="flex items-center space-x-2">
      <div class="w-3 h-3 rounded-full bg-red-500"></div>
      <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
      <div class="w-3 h-3 rounded-full bg-green-500"></div>
      <span class="ml-4 text-green-300">Kumbai@hint-system:~$</span>
    </div>
  </div>
  
  <!-- Main content -->
  <div class="relative z-10 container mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-2 gap-8 min-h-[calc(100vh-80px)]">
    
    <div class="flex flex-col items-center justify-center space-y-8">
      <div class="text-center space-y-4">
        <h1 class="text-4xl md:text-6xl font-bold text-green-400 glitch-text">
          This is a Kumbai(Hint) System Naaa
        </h1>
        <p class="text-green-300/80 text-lg">
          Click for hint... 😏
        </p>
        <div class="text-sm text-green-400/60">
          Clicks: <span class="text-yellow-400 font-bold">{clickCount}</span>
        </div>
      </div>
      
      <button
        on:click={handleClick}
        class="group relative px-8 py-4 bg-gradient-to-r from-red-600 to-red-800 
               hover:from-red-500 hover:to-red-700 text-white font-bold text-xl
               border-2 border-red-400 rounded-lg shadow-lg hover:shadow-red-500/50
               transform transition-all duration-300 hover:scale-105
               {isAnimating ? 'animate-pulse scale-110' : ''}"
        disabled={isAnimating}
      >
        <span class="relative z-10">
          {isAnimating ? 'PROCESSING...' : 'CLICK ME! 🚫'}
        </span>
        <div class="absolute inset-0 bg-red-500/20 rounded-lg blur-lg group-hover:blur-xl transition-all"></div>
      </button>
      
      {#if currentHint}
        <div class="bg-gray-900/80 border border-green-400/50 rounded-lg p-6 max-w-md mx-auto
                    backdrop-blur-sm animate-fade-in">
          <div class="flex items-center space-x-2 mb-2">
            <span class="text-yellow-400">⚠️</span>
            <span class="text-green-300 font-semibold">HINT DETECTED</span>
          </div>
          <p class="{currentHint.color} text-lg font-medium mb-4">
            {#if clickCount === 1}
              {#each currentHint.text.split(' ') as word, index}
                {word}{#if index < currentHint.text.split(' ').length - 1}<br/>{/if}
              {/each}
            {:else}
              {currentHint.text}
            {/if}
          </p>
          {#if currentHint.image}
            <div class="mt-4 text-center">
              <img 
                src={currentHint.image} 
                alt="Hint illustration" 
                class="max-w-full h-auto rounded-lg border border-green-400/30 shadow-lg
                       hover:border-green-400/60 transition-all duration-300"
                loading="lazy"
              />
            </div>
          {/if}
        </div>
      {/if}
      
      {#if clickCount > 0}
        <button
          on:click={resetGame}
          class="px-4 py-2 bg-gray-800 hover:bg-gray-700 text-green-400 
                 border border-green-400/50 rounded transition-all duration-300
                 hover:border-green-400 hover:shadow-green-400/25 hover:shadow-lg"
        >
          Reset System / รีเซ็ต
        </button>
      {/if}
    </div>
    
    <div class="bg-black/80 border border-green-400/30 rounded-lg p-4 backdrop-blur-sm">
      <div class="flex items-center justify-between mb-4">
        <h2 class="text-green-400 font-semibold">System Log</h2>
        <div class="flex space-x-1">
          <div class="w-2 h-2 bg-green-400 rounded-full animate-ping"></div>
          <div class="w-2 h-2 bg-green-400 rounded-full animate-ping" style="animation-delay: 0.5s"></div>
          <div class="w-2 h-2 bg-green-400 rounded-full animate-ping" style="animation-delay: 1s"></div>
        </div>
      </div>
      
      <div class="terminal-output h-96 overflow-y-auto scrollbar-thin scrollbar-thumb-green-400/50 scrollbar-track-gray-800">
        {#each terminalLines as line, index}
          <div class="text-green-300 text-sm mb-1 typewriter">
            {line}
            {#if line.includes('IMAGE_LOADED') && currentHint?.image}
              <div class="mt-2 mb-2">
                <img 
                  src={currentHint.image} 
                  alt="Terminal hint illustration" 
                  class="max-w-32 h-auto rounded border border-green-400/30 shadow-md
                         hover:border-green-400/60 transition-all duration-300"
                  loading="lazy"
                />
              </div>
            {/if}
          </div>
        {/each}
        <div class="flex items-center text-green-400 mt-2">
          <span>$</span>
          <div class="ml-2 w-2 h-4 bg-green-400 animate-pulse"></div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
  @keyframes glitch {
    0%, 100% { transform: translate(0); }
    10% { transform: translate(-2px, -2px); }
    20% { transform: translate(2px, 2px); }
    30% { transform: translate(-2px, 2px); }
    40% { transform: translate(2px, -2px); }
    50% { transform: translate(-2px, -2px); }
    60% { transform: translate(2px, 2px); }
    70% { transform: translate(-2px, 2px); }
    80% { transform: translate(2px, -2px); }
    90% { transform: translate(-2px, -2px); }
  }
  
  .glitch-text:hover {
    animation: glitch 0.3s ease-in-out;
  }
  
  .matrix-bg {
    background-image: 
      radial-gradient(circle at 25% 25%, #00ff41 1px, transparent 1px),
      radial-gradient(circle at 75% 75%, #00ff41 1px, transparent 1px);
    background-size: 50px 50px;
    width: 100%;
    height: 100%;
    animation: matrix-fall 20s linear infinite;
  }
  
  @keyframes matrix-fall {
    0% { transform: translateY(-100%); }
    100% { transform: translateY(100%); }
  }
  
  @keyframes fade-in {
    from { 
      opacity: 0; 
      transform: translateY(20px) scale(0.9);
    }
    to { 
      opacity: 1; 
      transform: translateY(0) scale(1);
    }
  }
  
  .animate-fade-in {
    animation: fade-in 0.5s ease-out;
  }
  
  .typewriter {
    border-right: 2px solid transparent;
    animation: typing 0.5s steps(40, end), blink-caret 1s step-end infinite;
  }
  
  @keyframes typing {
    from { width: 0; }
    to { width: 100%; }
  }
  
  @keyframes blink-caret {
    from, to { border-color: transparent; }
    50% { border-color: #00ff41; }
  }
  
  .scrollbar-thin {
    scrollbar-width: thin;
  }
  
  .scrollbar-thumb-green-400\/50::-webkit-scrollbar-thumb {
    background-color: rgba(74, 222, 128, 0.5);
  }
  
  .scrollbar-track-gray-800::-webkit-scrollbar-track {
    background-color: #1f2937;
  }
  
  ::-webkit-scrollbar {
    width: 6px;
  }
  
  :global(html) {
    scroll-behavior: smooth;
  }
  
  main {
    overflow-y: auto;
  }
</style>