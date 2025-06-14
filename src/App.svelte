<script lang="ts">
  import { onMount } from 'svelte';
  
  interface Hint {
    text: string;
    image?: string;
    color: string;
  }
  
  let currentHint: Hint | null = null;
  let clickCount: number = 0;
  let popCatCount: number = 0;
  let isAnimating: boolean = false;
  let terminalLines: string[] = [];
  let showEndModal: boolean = false;
  let showNoModal: boolean = false;
  let showPopCatModal: boolean = false;
  let popCatModalContent: { title: string; message: string } = { title: "", message: "" };
  
  const hints: Hint[] = [
    { text: "มัธยมศิลป์แห่งคณิตศาสตร์บัณฑิต นักปราชญ์สรรค์สร้างขั้นตอนวิธี สมองกลจักรกลประมวลผลเทิดทูน ชาติกาลเวลาแปรเปลี่ยนไปตามกัน ยุคแห่งข้อมูลใหญ่ไหลเวียนวน อัลกอริทึมแทรกซ้อนซับซ้อนงาม องค์ความรู้ปัญญาประดิษฐ์คิดคำนวณ มิได้หยุดหย่อนแห่งความก้าวหน้าคำ", color: "text-red-400" },
    // { text: "พี่ทำงานกับคนนี้นะ https://www.facebook.com/momotheoxy",color: "text-yellow-400", image: "https://shorturl.asia/NQJeT"},
    // { text: "พี่เป็น PM(Project manager)มั้ง, Full stack Developer(Claude, ChatGPT :) )", color: "text-blue-400", image: "https://shorturl.at/H3BpY" },
    { text: "ออกซิเจน,อะเมริเซียม", color: "text-green-400" },
    { text: "https://www.youtube.com/watch?v=41O_MydqxTU&ab_channel=WaterTowerMusic", color: "text-purple-400" },
  ];
  
  const terminalStartup = [
    "CS Comedy Science v9.9.9 - Kumbai System Initialized",
    "Loading Kumbai database...",
    "WARNING: Unauthorized clicking detected!",
    "Sawasdee Nong Rahus Kong Chun!",
    "Get ready for Kumbai..."
  ];
  
  onMount(() => {
    // Load saved popCatCount from localStorage
    const savedPopCatCount = localStorage.getItem('popCatCount');
    if (savedPopCatCount) {
      popCatCount = parseInt(savedPopCatCount, 10) || 0;
    }
    
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
      // Show full hint text in terminal (including URLs that are not images)
      terminalLines = [...terminalLines, `> HINT_${clickCount}: ${currentHint?.text}`];
      if (currentHint?.image) {
        terminalLines = [...terminalLines, `> IMAGE_LOADED:${currentHint.image}`];
      }
    }, 500);
    
    // Check if all hints are exhausted
    if (clickCount >= hints.length) {
      setTimeout(() => {
        showEndModal = true;
      }, 2000);
    }
    
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
  
  function handlePopCatClick(): void {
    popCatCount++;
    
    // Save to localStorage
    localStorage.setItem('popCatCount', popCatCount.toString());
    
    terminalLines = [...terminalLines, `> POP_CAT_${popCatCount} clicked at ${new Date().toLocaleTimeString()} 🐱`];
    
    if (popCatCount === 400) {
      popCatModalContent = {
        title: "นามปากกา",
        message: "พี่ชื่อ เกาเหลา"
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);
    } else if (popCatCount === 500) {
      popCatModalContent = {
        title: "หมดแล้วน้องเลิกกดได้แล้ว",
        message: ""
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);
    }
      else if (popCatCount === 600) {
      popCatModalContent = {
        title: "เชื่อพี่เถอะว่ามันหมดแล้ว",
        message: ""
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);
    }
      else if (popCatCount === 700) {
      popCatModalContent = {
        title: "หมดแล้วจริงงงๆๆๆๆๆๆ",
        message: ""
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);
      }
      else if (popCatCount === 800) {
      popCatModalContent = {
        title: "พี่บอกแล้วว่ามันหมดแล้วววว",
        message: ""
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);

      }
      else if (popCatCount === 900) {
      popCatModalContent = {
        title: "เลิกกดก่อนนนนนนนนนนน",
        message: ""
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 0);

    
    } else if (popCatCount === 500000) {
      popCatModalContent = {
        title: "โอเคถ้าน้องกดขนาดนี้",
        message: "พี่ชื่อ ออม นะงับ"
      };
      setTimeout(() => {
        showPopCatModal = true;
      }, 100);
    }
    
    
    setTimeout(() => {
      const terminal = document.querySelector('.terminal-output');
      if (terminal) {
        terminal.scrollTop = terminal.scrollHeight;
      }
    }, 100);
  }
  
  function resetGame(): void {
    currentHint = null;
    clickCount = 0;
    popCatCount = 0;
    
    // Clear popCatCount from localStorage when resetting
    localStorage.removeItem('popCatCount');
    
    terminalLines = [...terminalStartup.map(line => `> ${line}`)];
    isAnimating = false;
    showEndModal = false;
  }
  
  function closeModal(): void {
    showEndModal = false;
  }
  
  function handleNoClick(): void {
    showNoModal = true;
  }
  
  function closeNoModal(): void {
    showNoModal = false;
  }
  
  function closePopCatModal(): void {
    showPopCatModal = false;
  }
  
  function parseTextWithLinks(text: string): string {
    const urlRegex = /(https?:\/\/[^\s]+)/g;
    return text.replace(urlRegex, '<a href="$1" target="_blank" rel="noopener noreferrer" class="text-cyan-400 hover:text-cyan-300 underline break-all">$1</a>');
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
  <div class="relative z-10 container mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-2 gap-8 h-[calc(100vh-80px)]">
    
    <div class="flex flex-col items-center justify-center space-y-8">
      <div class="text-center space-y-4">
        <h1 class="text-4xl md:text-6xl font-bold text-green-400 glitch-text">
          This is a Kumbai(Hint) System Naaa
        </h1>
        <p class="text-green-300/80 text-lg">
          Click for hint... 😏
        </p>
        <div class="text-sm text-green-400/60 space-y-1">
          <div>Hint: <span class="text-yellow-400 font-bold">{clickCount}</span></div>
          <div>Pop Cat Clicks: <span class="text-orange-400 font-bold">{popCatCount}</span></div>
        </div>
      </div>
      
      <div class="flex gap-4 items-center flex-wrap justify-center">
        {#if clickCount < hints.length}
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
              {isAnimating ? 'PROCESSING...' : 'CLICK ME!'}
            </span>
            <div class="absolute inset-0 bg-red-500/20 rounded-lg blur-lg group-hover:blur-xl transition-all"></div>
          </button>
        {/if}
        
        <button
          on:click={handlePopCatClick}
          class="group relative px-6 py-4 bg-gradient-to-r from-orange-600 to-orange-800 
                 hover:from-orange-500 hover:to-orange-700 text-white font-bold text-xl
                 border-2 border-orange-400 rounded-lg shadow-lg hover:shadow-orange-500/50
                 transform transition-all duration-300 hover:scale-105"
        >
          <span class="relative z-10">Pop Cat 🐱 (เผื่อน้องเบื่อ) ({popCatCount})</span>
          <div class="absolute inset-0 bg-orange-500/20 rounded-lg blur-lg group-hover:blur-xl transition-all"></div>
        </button>
        
        {#if clickCount < hints.length}
          <button
            on:click={handleNoClick}
            class="group relative px-6 py-4 bg-gradient-to-r from-gray-600 to-gray-800 
                   hover:from-gray-500 hover:to-gray-700 text-white font-bold text-xl
                   border-2 border-gray-400 rounded-lg shadow-lg hover:shadow-gray-500/50
                   transform transition-all duration-300 hover:scale-105"
          >
            <span class="relative z-10">No! 😤</span>
            <div class="absolute inset-0 bg-gray-500/20 rounded-lg blur-lg group-hover:blur-xl transition-all"></div>
          </button>
        {/if}
      </div>
      
      {#if currentHint}
        <div class="bg-gray-900/80 border border-green-400/50 rounded-lg p-6 max-w-md mx-auto
                    backdrop-blur-sm animate-fade-in">
          <div class="flex items-center space-x-2 mb-2">
            <span class="text-yellow-400">⚠️</span>
            <span class="text-green-300 font-semibold">HINT DETECTED</span>
          </div>
          <div class="{currentHint.color} text-lg font-medium mb-4">
            {#if clickCount === 1}
              {#each currentHint.text.split(' ') as word, index}
                {word}{#if index < currentHint.text.split(' ').length - 1}<br/>{/if}
              {/each}
            {:else}
              {@html parseTextWithLinks(currentHint.text)}
            {/if}
          </div>
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
            {#if line.includes('IMAGE_LOADED:')}
              {@const imageUrl = line.split('IMAGE_LOADED:')[1]}
              <div class="text-green-300">{'> IMAGE_LOADED:'}</div>
              <div class="mt-2 mb-2">
                <img 
                  src={imageUrl} 
                  alt="Terminal hint illustration" 
                  class="max-w-32 h-auto rounded border border-green-400/30 shadow-md
                         hover:border-green-400/60 transition-all duration-300"
                  loading="lazy"
                />
              </div>
            {:else if line.includes('https://') && !line.includes('IMAGE_LOADED:')}
              {@html parseTextWithLinks(line)}
            {:else}
              {line}
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
  
  <!-- No Modal -->
  {#if showNoModal}
    <div class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-gray-900 border-2 border-gray-400 rounded-lg p-8 max-w-md mx-auto text-center
                  shadow-2xl shadow-gray-500/50 animate-fade-in">
        <div class="text-6xl mb-4">
          <img src="https://shorturl.asia/Kc89B" alt="No reaction" class="mx-auto max-w-full h-auto rounded-lg" />
        </div>
        <button
          on:click={closeNoModal}
          class="px-6 py-3 bg-blue-600 hover:bg-blue-500 text-white font-bold rounded-lg
                 border-2 border-blue-400 transition-all duration-300
                 hover:shadow-blue-400/50 hover:shadow-lg transform hover:scale-105"
        >
          OK
        </button>
      </div>
    </div>
  {/if}
  
  {#if showPopCatModal}
    <div class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-gray-900 border-2 border-orange-400 rounded-lg p-8 max-w-md mx-auto text-center
                  shadow-2xl shadow-orange-500/50 animate-fade-in">
        <div class="text-6xl mb-4">🐱</div>
        <h2 class="text-2xl font-bold text-orange-400 mb-4">
          {popCatModalContent.title}
        </h2>
        {#if popCatModalContent.message}
          <p class="text-yellow-400 text-lg mb-6">
            {popCatModalContent.message}
          </p>
        {/if}
        <button
          on:click={closePopCatModal}
          class="px-6 py-3 bg-orange-600 hover:bg-orange-500 text-white font-bold rounded-lg
                 border-2 border-orange-400 transition-all duration-300
                 hover:shadow-orange-400/50 hover:shadow-lg transform hover:scale-105"
        >
          OK
        </button>
      </div>
    </div>
  {/if}
  
  <!-- End Modal -->
  {#if showEndModal}
    <div class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-gray-900 border-2 border-red-400 rounded-lg p-8 max-w-md mx-auto text-center
                  shadow-2xl shadow-red-500/50 animate-fade-in">
        <div class="text-6xl mb-4"><img src="https://shorturl.asia/2Sy1r" alt=""></div>
        <h2 class="text-2xl font-bold text-red-400 mb-4">
          คำใบ้หมดแล้วนะงับ
        </h2>
        <p class="text-yellow-400 text-lg mb-6">
          ขอให้ทายถูกน้าาาาาาา
        </p>
        <button
          on:click={closeModal}
          class="px-6 py-3 bg-green-600 hover:bg-green-500 text-white font-bold rounded-lg
                 border-2 border-green-400 transition-all duration-300
                 hover:shadow-green-400/50 hover:shadow-lg transform hover:scale-105"
        >
          OK
        </button>
      </div>
    </div>
  {/if}
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