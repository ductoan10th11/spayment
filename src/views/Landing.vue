
<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const isMenuOpen = ref(false);
const isScrolled = ref(false);
const activeTab = ref('json'); 

onMounted(() => {
    const handleScroll = () => {
        isScrolled.value = window.scrollY > 20;
    };
    window.addEventListener('scroll', handleScroll);
    
    // Initial check
    handleScroll();
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});
import SolarSystem from '../components/SolarSystem.vue';
import { listbank } from '../utils/listbank';
import { Button } from '@/components/ui/button';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { 
  Card, 
  CardContent, 
  CardDescription, 
  CardHeader, 
  CardTitle 
} from '@/components/ui/card';
import { 
  ChevronDown, 
  BarChart3, 
  RefreshCw, 
  ShieldCheck, 
  Zap, 
  CheckCircle2,
  LayoutGrid,
  FileText,
  Home,
  Headset,
  User,
  Menu,
  X
} from 'lucide-vue-next';

onMounted(() => {
    // Scroll reveal
    gsap.utils.toArray('.reveal').forEach((elem) => {
        gsap.from(elem, {
            scrollTrigger: {
                trigger: elem,
                start: 'top 85%',
                toggleActions: 'play none none none'
            },
            y: 30,
            opacity: 0,
            duration: 1,
            ease: 'power2.out'
        });
    });
});
</script>

<template>
  <div class="landing-page min-h-screen bg-white font-sans selection:bg-accent/20 selection:text-accent overflow-x-hidden">
    <!-- Top Header (Logo & Toggle) -->
    <header 
        class="sticky md:fixed top-0 z-[100] w-full transition-all duration-500 font-heading"
        :class="[
            isScrolled || isMenuOpen 
                ? 'bg-white/95 border-b border-border shadow-sm py-0' 
                : 'bg-transparent border-b border-transparent py-4'
        ]"
    >
      <div class="container mx-auto flex items-center justify-between transition-all duration-500 px-4 md:px-0" :class="isScrolled ? 'h-16 md:h-20' : 'h-20 md:h-28'">
        <div class="flex items-center gap-3">
          <div class="flex h-10 w-10 items-center justify-center rounded-lg bg-accent text-xl font-bold text-white transition-transform duration-500" :class="isScrolled ? 'scale-90' : 'scale-100'">S</div>
          <span class="text-2xl font-extrabold tracking-tight transition-all" :class="isScrolled ? 'text-xl md:text-2xl' : 'text-2xl md:text-3xl'">Spayment</span>
        </div>
        
        <!-- Desktop Nav Items -->
        <div class="hidden items-center gap-10 md:flex">
          <a href="#" class="flex items-center gap-1 text-sm font-medium hover:text-accent transition-colors">Sản phẩm <ChevronDown class="h-4 w-4" /></a>
          <a href="#" class="flex items-center gap-1 text-sm font-medium hover:text-accent transition-colors">Giải pháp <ChevronDown class="h-4 w-4" /></a>
          <a href="#" class="flex items-center gap-1 text-sm font-medium hover:text-accent transition-colors">Tin tức <ChevronDown class="h-4 w-4" /></a>
          <a href="#" class="text-sm font-medium hover:text-accent transition-colors">Khuyến mại</a>
          <Button class="bg-accent hover:bg-[#237b7b] text-white rounded-xl px-6 transition-all" :class="isScrolled ? 'h-10' : 'h-11'">Đăng nhập</Button>
        </div>

        <!-- Mobile More Menu Toggle -->
        <button @click="isMenuOpen = !isMenuOpen" class="md:hidden p-2 hover:bg-slate-100 rounded-full transition-colors">
            <Menu v-if="!isMenuOpen" class="h-6 w-6 text-foreground" />
            <X v-else class="h-6 w-6 text-foreground" />
        </button>
      </div>

      <!-- Mobile Full Menu Overlay (Triggered by Hamburger) -->
      <div v-if="isMenuOpen" class="fixed inset-0 top-20 z-[90] bg-white md:hidden">
        <div class="flex flex-col p-6 gap-6 overflow-y-auto h-full pb-32">
            <div class="space-y-1">
                <p class="text-xs font-bold uppercase tracking-widest text-slate-400 px-4 mb-2">Tài khoản</p>
                <Button class="bg-accent hover:bg-[#237b7b] text-white w-full h-14 text-lg rounded-2xl">Đăng nhập ngay</Button>
            </div>
            
            <div class="space-y-1">
                <p class="text-xs font-bold uppercase tracking-widest text-slate-400 px-4 mb-2">Thông tin</p>
                <a href="#" class="flex items-center gap-4 text-lg font-semibold px-4 py-4 hover:bg-slate-50 rounded-2xl transition-colors border">
                    <div class="h-10 w-10 flex items-center justify-center bg-blue-50 text-blue-500 rounded-xl">📄</div>
                    Tài liệu API
                </a>
                <a href="#" class="flex items-center gap-4 text-lg font-semibold px-4 py-4 hover:bg-slate-50 rounded-2xl transition-colors border mt-3">
                    <div class="h-10 w-10 flex items-center justify-center bg-purple-50 text-purple-500 rounded-xl">💬</div>
                    Hỗ trợ kỹ thuật
                </a>
            </div>
            
            <div class="mt-auto pb-10">
                <p class="text-center text-slate-400 text-sm italic">Hệ thống Banking API chuẩn quốc tế</p>
            </div>
        </div>
      </div>
    </header>

    <!-- Bottom Navigation Bar (Mobile Only) - Professional App Style -->
    <nav class="md:hidden fixed bottom-0 left-0 z-[100] w-full border-t border-border bg-white/95 backdrop-blur-md px-2 pb-2 font-heading shadow-[0_-5px_20px_rgba(0,0,0,0.05)]">
      <div class="flex h-16 items-center justify-between relative">
        <!-- Danh mục -->
        <a href="#" class="flex flex-1 flex-col items-center justify-center gap-1.5 transition-all text-slate-500 hover:text-accent group">
          <LayoutGrid class="h-5 w-5 group-hover:scale-110 transition-transform" />
          <span class="text-[10px] font-bold">Danh mục</span>
        </a>

        <!-- Tài liệu -->
        <a href="#" class="flex flex-1 flex-col items-center justify-center gap-1.5 transition-all text-slate-500 hover:text-accent group">
          <FileText class="h-5 w-5 group-hover:scale-110 transition-transform" />
          <span class="text-[10px] font-bold">Tài liệu</span>
        </a>

        <!-- Trang chủ (Center Circle) -->
        <div class="flex-1 flex justify-center -translate-y-4">
          <a href="#" class="flex h-16 w-16 flex-col items-center justify-center gap-1 rounded-full bg-accent text-white shadow-xl shadow-accent/30 border-4 border-white transition-all hover:scale-110 active:scale-95 group hover:bg-[#237b7b]">
            <Home class="h-7 w-7" />
          </a>
        </div>

        <!-- Hỗ trợ -->
        <a href="#" class="flex flex-1 flex-col items-center justify-center gap-1.5 transition-all text-slate-500 hover:text-accent group">
          <Headset class="h-5 w-5 group-hover:scale-110 transition-transform" />
          <span class="text-[10px] font-bold">Hỗ trợ</span>
        </a>

        <!-- Tài khoản -->
        <a href="#" class="flex flex-1 flex-col items-center justify-center gap-1.5 transition-all text-slate-500 hover:text-accent group">
          <User class="h-5 w-5 group-hover:scale-110 transition-transform" />
          <span class="text-[10px] font-bold">Tài khoản</span>
        </a>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="relative overflow-hidden mt-8 lg:mt-[46px] pt-0 lg:pt-64 pb-24 lg:pb-48 px-4 md:px-0 min-h-[16vh] flex items-center">
      <!-- Background glow or effect can go here if needed -->
      
      <div class="container mx-auto relative z-10 h-full">
        <div class="grid w-full lg:grid-cols-12 lg:items-center relative">
          <!-- Text Content -->
          <div class="lg:col-span-5 pt-16 lg:pt-0 relative z-20 lg:pointer-events-none">
            <h1 class="text-4xl font-extrabold leading-[1.1] text-accent lg:text-7xl reveal uppercase font-heading pointer-events-auto">
              SPAYMENT
            </h1>
            <p class="mt-6 max-w-[500px] text-base leading-relaxed text-slate-500 lg:text-xl reveal font-heading pointer-events-auto">
              Spayment cung cấp API ngân hàng với tốc độ đồng bộ giao dịch hầu như tức thì. 
              Điều này có được nhờ Spayment ký kết hợp tác trực tiếp với nhiều ngân hàng tại Việt Nam.
            </p>
            <div class="mt-10 flex flex-wrap gap-4 reveal pointer-events-auto">
                <Button size="lg" class="bg-accent hover:bg-[#237b7b] text-white text-lg px-8 h-14 rounded-2xl shadow-lg hover:shadow-accent/20 transition-all font-heading">Bắt đầu ngay</Button>
                <Button size="lg" variant="outline" class="text-lg px-8 h-14 rounded-2xl border-slate-200 hover:bg-slate-50 hover:text-accent font-heading">Xem tài liệu API</Button>
            </div>
          </div>
          
          <!-- Spacer for mobile layout to maintain flow -->
          <div class="lg:col-span-7 h-0 pointer-events-none"></div>

          <!-- Model Container - Absolute on Large Screens, Hidden on Mobile -->
          <div class="hidden lg:flex absolute right-[-5%] top-1/2 -translate-y-1/2 lg:w-[70%] lg:h-[900px] z-10 reveal items-center justify-center overflow-visible pointer-events-auto">
            <SolarSystem />
          </div>
        </div>
      </div>
    </section>

    <!-- Bank List Section -->
    <section class="bg-slate-50 py-24 px-4 md:px-0">
      <div class="container mx-auto text-center">
        <div class="inline-block rounded-full bg-accent/10 px-4 py-1.5 text-sm font-semibold text-accent font-heading">
          Giải pháp cho doanh nghiệp
        </div>
        <h2 class="mt-6 text-4xl font-bold tracking-tight text-slate-900 md:text-5xl reveal font-heading">
          Danh sách ngân hàng
        </h2>
        
        <div class="mt-16 flex flex-wrap justify-center gap-8 reveal">
          <div v-for="bank in listbank" :key="bank.id" 
               class="group flex w-32 cursor-pointer flex-col items-center gap-4 rounded-2xl p-6 transition-all duration-300 hover:-translate-y-2 hover:scale-105">
            <div class="relative flex h-16 w-16 items-center justify-center">
                <img :src="bank.img" :alt="bank.name" width="64" 
                     class="transition-all duration-300 group-hover:drop-shadow-[0_0_15px_rgba(156,156,156,0.5)] drop-shadow-[0_0_10px_rgba(211,211,211,0.5)]" />
            </div>
            <p class="text-sm font-medium text-slate-600 font-heading">{{ bank.name }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Core Features Section -->
    <section id="features" class="py-24 px-4 md:px-0">
      <div class="container mx-auto">
        <div class="grid gap-16 lg:grid-cols-2">
          <div class="reveal">
            <div class="inline-block rounded-full bg-accent/10 px-4 py-1.5 text-sm font-semibold text-accent font-heading">
              Tính năng cốt lõi
            </div>
            <h2 class="mt-6 text-4xl font-bold tracking-tight text-slate-900 font-heading">
              Nền tảng dữ liệu ngân hàng chuẩn xác
            </h2>
            <p class="mt-4 text-lg text-slate-500 max-w-lg font-heading">
              Chúng tôi cung cấp các API chuyên biệt để bạn xây dựng ứng dụng tài chính an toàn và nhanh chóng.
            </p>
          </div>
          
          <div class="grid gap-8 sm:grid-cols-2 reveal font-heading">
            <div class="flex flex-col gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-slate-50 text-2xl">📊</div>
              <h3 class="text-xl font-bold">Account Information API</h3>
              <p class="text-sm text-slate-500">Lấy thông tin chủ tài khoản, loại tài khoản và trạng thái thực tế.</p>
            </div>
            <div class="flex flex-col gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-slate-50 text-2xl">🔄</div>
              <h3 class="text-xl font-bold">Balance Change API</h3>
              <p class="text-sm text-slate-500">Theo dõi mọi biến động số dư thời gian thực với độ chính xác tuyệt đối.</p>
            </div>
            <div class="flex flex-col gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-slate-50 text-2xl">🛡️</div>
              <h3 class="text-xl font-bold">Secure Read-only Access</h3>
              <p class="text-sm text-slate-500">Truy cập chỉ đọc, tuyệt đối không cho phép thực hiện giao dịch chuyển tiền.</p>
            </div>
            <div class="flex flex-col gap-4">
              <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-slate-50 text-2xl">⚡</div>
              <h3 class="text-xl font-bold">Webhook Integration</h3>
              <p class="text-sm text-slate-500">Nhận thông báo ngay lập tức khi có giao dịch mới phát sinh.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- How it works Section -->
    <section class="bg-slate-50 py-24 px-4 md:px-0">
      <div class="container mx-auto text-center">
        <div class="inline-block rounded-full bg-accent/10 px-4 py-1.5 text-sm font-semibold text-accent font-heading">
          Quy trình
        </div>
        <h2 class="mt-6 text-4xl font-bold tracking-tight text-slate-900 reveal font-heading">
          Kết nối dữ liệu trong 3 bước
        </h2>
        
        <div class="mt-16 flex flex-col items-center justify-between gap-12 md:flex-row reveal">
          <div class="flex flex-1 flex-col items-center gap-6">
            <div class="flex h-12 w-12 items-center justify-center rounded-full bg-accent text-xl font-bold text-white shadow-lg shadow-accent/20 font-heading">1</div>
            <h3 class="text-xl font-bold font-heading">Ủy quyền tài khoản</h3>
            <p class="text-slate-500 font-heading">Kết nối ngân hàng an toàn qua OAuth hoặc API Gateway.</p>
          </div>
          <div class="hidden text-3xl text-slate-300 md:block">→</div>
          <div class="flex flex-1 flex-col items-center gap-6">
            <div class="flex h-12 w-12 items-center justify-center rounded-full bg-accent text-xl font-bold text-white shadow-lg shadow-accent/20 font-heading">2</div>
            <h3 class="text-xl font-bold font-heading">Đồng bộ dữ liệu</h3>
            <p class="text-slate-500 font-heading">Spayment truy xuất biến động số dư và thông tin giao dịch.</p>
          </div>
          <div class="hidden text-3xl text-slate-300 md:block">→</div>
          <div class="flex flex-1 flex-col items-center gap-6">
            <div class="flex h-12 w-12 items-center justify-center rounded-full bg-accent text-xl font-bold text-white shadow-lg shadow-accent/20 font-heading">3</div>
            <h3 class="text-xl font-bold font-heading">Tích hợp hệ thống</h3>
            <p class="text-slate-500 font-heading">Ứng dụng của bạn nhận dữ liệu qua API hoặc Webhook tự động.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Security Section -->
    <section class="py-24 px-4 md:px-0">
      <div class="container mx-auto">
        <div class="grid gap-16 lg:grid-cols-2 lg:items-center">
          <div class="flex justify-center reveal">
            <div class="relative flex h-72 w-72 items-center justify-center rounded-full border border-dashed border-accent bg-accent/5">
              <div class="flex flex-col items-center gap-4">
                <span class="text-7xl">🛡️</span>
                <span class="font-bold text-accent tracking-widest text-lg font-heading">Encryption AES-256</span>
              </div>
            </div>
          </div>
          
          <div class="reveal">
            <div class="inline-block rounded-full bg-accent/10 px-4 py-1.5 text-sm font-semibold text-accent font-heading">
              An toàn & Bảo mật
            </div>
            <h2 class="mt-6 text-4xl font-bold tracking-tight text-slate-900 font-heading">
              Tiêu chuẩn bảo mật Fintech hàng đầu
            </h2>
            <ul class="mt-10 space-y-6 font-heading">
              <li class="flex gap-4">
                <CheckCircle2 class="mt-1 h-6 w-6 shrink-0 text-emerald-500" />
                <div>
                  <h4 class="font-bold text-slate-900">Dữ liệu chỉ đọc</h4>
                  <p class="text-sm text-slate-500">Chúng tôi không bao giờ yêu cầu quyền chuyển tiền hay thay đổi tài khoản.</p>
                </div>
              </li>
              <li class="flex gap-4">
                <CheckCircle2 class="mt-1 h-6 w-6 shrink-0 text-emerald-500" />
                <div>
                  <h4 class="font-bold text-slate-900">Xác thực đa lớp</h4>
                  <p class="text-sm text-slate-500">API Key kết hợp với IP Whitelisting và OAuth 2.0.</p>
                </div>
              </li>
              <li class="flex gap-4">
                <CheckCircle2 class="mt-1 h-6 w-6 shrink-0 text-emerald-500" />
                <div>
                  <h4 class="font-bold text-slate-900">Tuân thủ chặt chẽ</h4>
                  <p class="text-sm text-slate-500">Hệ thống được thiết kế theo các tiêu chuẩn quốc tế về an toàn dữ liệu tài chính.</p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- Developers Section -->
    <section id="developers" class="bg-[#0f172a] py-24 text-white px-4 md:px-0">
      <div class="container mx-auto">
        <div class="grid gap-16 lg:grid-cols-2">
          <div class="reveal">
            <div class="inline-block rounded-full bg-white/10 px-4 py-1.5 text-sm font-semibold text-white font-heading">
              For Developers
            </div>
            <h2 class="mt-6 text-4xl font-bold tracking-tight font-heading">API mạnh mẽ, tích hợp nhanh chóng</h2>
            <p class="mt-6 text-lg text-slate-400 font-heading">
              Chúng tôi tối ưu hóa trải nghiệm nhà phát triển với JSON chuẩn hóa, tài liệu chi tiết và môi trường Sandbox.
            </p>
            <div class="mt-12 flex flex-wrap gap-4 font-heading">
              <Button size="lg" class="bg-accent hover:bg-[#237b7b] text-white rounded-xl px-8 shadow-lg shadow-accent/20">Xem Full API Docs</Button>
              <Button size="lg" variant="outline" class="border-white/20 text-white hover:bg-white/10 rounded-xl px-8">Tham gia Discord</Button>
            </div>
          </div>
          
          <div class="reveal">
            <Tabs default-value="json" class="w-full overflow-hidden rounded-2xl border border-white/10 bg-[#020617] font-heading">
              <TabsList class="h-12 w-full justify-start rounded-none border-b border-white/10 bg-[#1e293b]/50 p-0 text-slate-400">
                <TabsTrigger value="json" class="h-full rounded-none border-b-2 border-transparent px-6 py-3 data-[state=active]:border-accent data-[state=active]:bg-slate-900 data-[state=active]:text-white">Response JSON</TabsTrigger>
                <TabsTrigger value="curl" class="h-full rounded-none border-b-2 border-transparent px-6 py-3 data-[state=active]:border-accent data-[state=active]:bg-slate-900 data-[state=active]:text-white">cURL</TabsTrigger>
              </TabsList>
              <TabsContent value="json" class="m-0 p-8">
                <pre class="overflow-auto text-sm font-mono text-emerald-400"><code>{
  "status": "success",
  "data": {
    "account": "123456789",
    "balance_changes": [
      { "amount": 1000000, "type": "CR" },
      { "amount": -250000, "type": "DR" }
    ]
  }
}</code></pre>
              </TabsContent>
              <TabsContent value="curl" class="m-0 p-8">
                <pre class="overflow-auto text-sm font-mono text-emerald-400"><code>curl -X GET "https://api.spayment.vn/v1/balances" \
-H "Authorization: Bearer YOUR_API_KEY" \
-H "Accept: application/json"</code></pre>
              </TabsContent>
            </Tabs>
          </div>
        </div>
      </div>
    </section>

    <!-- Final CTA -->
    <section class="py-32 text-center px-4 md:px-0">
      <div class="container mx-auto reveal">
        <h2 class="text-4xl font-bold tracking-tight text-slate-900 md:text-5xl font-heading">
          Bắt đầu tích hợp dữ liệu ngân hàng ngay hôm nay
        </h2>
        <p class="mt-6 text-xl text-slate-500 font-heading">Sẵn sàng nâng cấp hệ thống quản lý tài chính của bạn?</p>
        <div class="mt-12 flex flex-wrap justify-center gap-6 font-heading">
          <Button size="lg" class="bg-accent hover:bg-[#237b7b] text-white h-14 rounded-2xl px-10 text-lg shadow-xl shadow-accent/20">Đăng ký nhận API Key</Button>
          <Button size="lg" variant="outline" class="h-14 rounded-2xl px-10 text-lg border-slate-200">Liên hệ tư vấn kỹ thuật</Button>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="border-t border-slate-200 py-16 px-4 md:px-0 font-heading">
      <div class="container mx-auto">
        <div class="grid gap-12 lg:grid-cols-12 lg:items-start">
          <div class="lg:col-span-4">
            <div class="flex items-center gap-3">
              <div class="flex h-10 w-10 items-center justify-center rounded-lg bg-accent text-xl font-bold text-white">S</div>
              <span class="text-2xl font-bold tracking-tight">Spayment</span>
            </div>
            <p class="mt-6 text-slate-500 max-w-sm">Nền tảng Banking API chuyên nghiệp cho doanh nghiệp hiện đại.</p>
          </div>
          
          <div class="grid grid-cols-2 gap-8 sm:grid-cols-3 lg:col-span-8">
            <div class="flex flex-col gap-4">
                <h5 class="text-xs font-bold uppercase tracking-widest text-slate-400">Sản phẩm</h5>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">API Documentation</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Pricing</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Sandbox</a>
            </div>
            <div class="flex flex-col gap-4">
                <h5 class="text-xs font-bold uppercase tracking-widest text-slate-400">Công ty</h5>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Về chúng tôi</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Blog</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Tuyển dụng</a>
            </div>
            <div class="flex flex-col gap-4">
                <h5 class="text-xs font-bold uppercase tracking-widest text-slate-400">Pháp lý</h5>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Điều khoản sử dụng</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Chính sách bảo mật</a>
                <a href="#" class="text-slate-600 hover:text-accent transition-colors">Audit Report</a>
            </div>
          </div>
        </div>
        
        <div class="mt-20 border-t border-slate-100 pt-8 text-sm text-slate-400">
          <p>&copy; 2026 Spayment. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* Ensure SolarSystem fills its container */
:deep(.solar-system-container) {
  height: 100%;
  width: 100%;
}
</style>
