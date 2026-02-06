
<script setup>
import { onMounted, ref } from 'vue';
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const isMenuOpen = ref(false);
const activeTab = ref('json'); 

import SolarSystem from '../components/SolarSystem.vue';
import '../assets/css/landing.css';
import { listbank } from '../utils/listbank';
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
    <div class="landing-page">
        <!-- Navbar -->
        <nav class="navbar">
            <div class="container nav-content">
                <div class="logo">
                    <div class="logo-symbol">S</div>
                    <span class="logo-text">Spayment</span>
                </div>
                <div class="nav-links" :class="{ 'nav-active': isMenuOpen }">
                    <a href="#">Sản phẩm ▾</a>
                    <a href="#">Giải pháp ▾</a>
                    <a href="#">Tin tức ▾</a>
                    <a href="#">Khuyến mại</a>
                    <a href="#" class="btn btn-primary">Đăng nhập</a>
                </div>
            </div>
        </nav>

        <!-- New Bank API Discovery Section -->
        <section class="bank-api-hero">
            <div class="hero-bg-diagonal"></div>
            <div class="container hero-split">
                <div class="hero-text-side">
                    <h1 class="main-title">SPAYMENT</h1>
                    <p class="main-desc">
                        Spayment cung cấp API ngân hàng với tốc độ đồng bộ giao dịch hầu như tức thì. 
                        Điều này có được nhờ Spayment ký kết hợp tác trực tiếp với nhiều ngân hàng tại Việt Nam.
                    </p>
                </div>
                <div class="hero-logos-side">
                        <SolarSystem />
                </div>
            </div>
        </section>

        <section class="problems bg-light">
            <div class="container text-center">
                <div class="section-badge">Giải pháp cho doanh nghiệp</div>
                <h2 class="section-title centered">Danh sách ngân hàng</h2> 
                <div class="listbankapi">
                    <div class="bank-item" v-for="bank in listbank" :key="bank.id">
                        <img :src="bank.img" width="64" alt="">
                        <p>{{ bank.name }}</p>
                    </div>
                </div> 
            </div>
        </section>

        <!-- Section 3: Core Features -->
        <section id="features" class="features">
            <div class="container grid-2">
                <div class="features-header reveal">
                    <div class="section-badge">Tính năng cốt lõi</div>
                    <h2 class="section-title">Nền tảng dữ liệu ngân hàng chuẩn xác</h2>
                    <p class="section-desc">Chúng tôi cung cấp các API chuyên biệt để bạn xây dựng ứng dụng tài chính an toàn và nhanh chóng.</p>
                </div>
                <div class="features-list grid-2-tight reveal">
                    <div class="f-item">
                        <div class="f-icon">📊</div>
                        <h4>Account Information API</h4>
                        <p>Lấy thông tin chủ tài khoản, loại tài khoản và trạng thái thực tế.</p>
                    </div>
                    <div class="f-item">
                        <div class="f-icon">🔄</div>
                        <h4>Balance Change API</h4>
                        <p>Theo dõi mọi biến động số dư thời gian thực với độ chính xác tuyệt đối.</p>
                    </div>
                    <div class="f-item">
                        <div class="f-icon">🛡️</div>
                        <h4>Secure Read-only Access</h4>
                        <p>Truy cập chỉ đọc, tuyệt đối không cho phép thực hiện giao dịch chuyển tiền.</p>
                    </div>
                    <div class="f-item">
                        <div class="f-icon">⚡</div>
                        <h4>Webhook Integration</h4>
                        <p>Nhận thông báo ngay lập tức khi có giao dịch mới phát sinh.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 4: How it Works -->
        <section id="how-it-works" class="how-it-works bg-light">
            <div class="container text-center">
                <div class="section-badge">Quy trình</div>
                <h2 class="section-title centered">Kết nối dữ liệu trong 3 bước</h2>
                <div class="flow reveal">
                    <div class="flow-step">
                        <div class="step-line">1</div>
                        <h4>Ủy quyền tài khoản</h4>
                        <p>Kết nối ngân hàng an toàn qua OAuth hoặc API Gateway.</p>
                    </div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-step">
                        <div class="step-line">2</div>
                        <h4>Đồng bộ dữ liệu</h4>
                        <p>Spayment truy xuất biến động số dư và thông tin giao dịch.</p>
                    </div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-step">
                        <div class="step-line">3</div>
                        <h4>Tích hợp hệ thống</h4>
                        <p>Ứng dụng của bạn nhận dữ liệu qua API hoặc Webhook tự động.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 5: Security -->
        <section class="security">
            <div class="container grid-2 items-center">
                <div class="security-visual reveal">
                    <div class="shield-badge">
                        <div class="shield-icon">🛡️</div>
                        <div class="shield-text">Encryption AES-256</div>
                    </div>
                </div>
                <div class="security-content reveal">
                    <div class="section-badge">An toàn & Bảo mật</div>
                    <h2 class="section-title">Tiêu chuẩn bảo mật Fintech hàng đầu</h2>
                    <ul class="security-list">
                        <li><strong>Dữ liệu chỉ đọc:</strong> Chúng tôi không bao giờ yêu cầu quyền chuyển tiền hay thay đổi tài khoản.</li>
                        <li><strong>Xác thực đa lớp:</strong> API Key kết hợp với IP Whitelisting và OAuth 2.0.</li>
                        <li><strong>Tuân thủ chặt chẽ:</strong> Hệ thống được thiết kế theo các tiêu chuẩn quốc tế về an toàn dữ liệu tài chính.</li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- Section 7: Developers -->
        <section id="developers" class="dev-section bg-primary text-white">
            <div class="container">
                <div class="grid-2">
                    <div class="dev-info reveal">
                        <div class="section-badge badge-white">For Developers</div>
                        <h2 class="text-white">API mạnh mẽ, tích hợp nhanh chóng</h2>
                        <p class="text-light-blue">Chúng tôi tối ưu hóa trải nghiệm nhà phát triển với JSON chuẩn hóa, tài liệu chi tiết và môi trường Sandbox.</p>
                        <div class="dev-ctas">
                            <a href="#" class="btn btn-primary">Xem Full API Docs</a>
                            <a href="#" class="btn btn-outline-white">Tham gia Discord</a>
                        </div>
                    </div>
                    <div class="dev-code reveal">
                        <div class="code-editor">
                            <div class="editor-tabs">
                                <button :class="{ active: activeTab === 'json' }" @click="activeTab = 'json'">Response JSON</button>
                                <button :class="{ active: activeTab === 'curl' }" @click="activeTab = 'curl'">cURL</button>
                            </div>
                            <div class="editor-content">
                                <pre v-if="activeTab === 'json'"><code>{
  "status": "success",
  "data": {
    "account": "123456789",
    "balance_changes": [
      { "amount": 1000000, "type": "CR" },
      { "amount": -250000, "type": "DR" }
    ]
  }
}</code></pre>
                                <pre v-else><code>curl -X GET "https://api.spayment.vn/v1/balances" \
-H "Authorization: Bearer YOUR_API_KEY" \
-H "Accept: application/json"</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 8: CTA Final -->
        <section class="cta-final text-center">
            <div class="container reveal">
                <h2 class="section-title centered">Bắt đầu tích hợp dữ liệu ngân hàng ngay hôm nay</h2>
                <p class="section-desc centered">Sẵn sàng nâng cấp hệ thống quản lý tài chính của bạn?</p>
                <div class="cta-btns">
                    <button class="btn btn-primary btn-lg">Đăng ký nhận API Key</button>
                    <button class="btn btn-outline btn-lg">Liên hệ tư vấn kỹ thuật</button>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer class="footer">
            <div class="container footer-grid">
                <div class="footer-brand">
                    <div class="logo">
                        <div class="logo-symbol">S</div>
                        <span class="logo-text">Spayment</span>
                    </div>
                    <p>Nền tảng Banking API chuyên nghiệp cho doanh nghiệp hiện đại.</p>
                </div>
                <div class="footer-links-group">
                    <div class="f-links">
                        <h5>Sản phẩm</h5>
                        <a href="#">API Documentation</a>
                        <a href="#">Pricing</a>
                        <a href="#">Sandbox</a>
                    </div>
                    <div class="f-links">
                        <h5>Công ty</h5>
                        <a href="#">Về chúng tôi</a>
                        <a href="#">Blog</a>
                        <a href="#">Tuyển dụng</a>
                    </div>
                    <div class="f-links">
                        <h5>Pháp lý</h5>
                        <a href="#">Điều khoản sử dụng</a>
                        <a href="#">Chính sách bảo mật</a>
                        <a href="#">Audit Report</a>
                    </div>
                </div>
            </div>
            <div class="footer-bottom container">
                <p>&copy; 2026 Spayment. All rights reserved.</p>
            </div>
        </footer>
    </div>
</template>


