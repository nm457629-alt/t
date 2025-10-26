<!DOCTYPE html>
<html lang="vi" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>World Museum - Khám Phá & Trải Nghiệm (Nâng Cấp)</title>

  <!-- Google Fonts (Giữ nguyên) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  
  <!-- Tailwind CSS (Giữ nguyên) -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Lucide Icons (Giữ nguyên) -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <style>
    /* Tùy chỉnh Tailwind (Giữ nguyên) */
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            'custom-gold': '#f0c674',
            'custom-dark': '#0e0e0e',
            'custom-card': '#1a1a1a',
          },
          fontFamily: {
            'sans': ['Poppins', 'sans-serif'],
            'serif': ['Playfair Display', 'serif'],
          }
        }
      }
    }

    /* --- CẢI TIẾN: CUSTOM CURSOR --- */
    /* Ẩn con trỏ mặc định */
    body, a, button {
      cursor: none;
    }
    
    #cursor-dot {
      position: fixed;
      width: 8px;
      height: 8px;
      background-color: #f0c674;
      border-radius: 50%;
      left: 0;
      top: 0;
      pointer-events: none;
      transform: translate(-50%, -50%);
      transition: width 0.3s, height 0.3s;
      z-index: 9999;
    }

    #cursor-outline {
      position: fixed;
      width: 40px;
      height: 40px;
      border: 2px solid #f0c674;
      border-radius: 50%;
      left: 0;
      top: 0;
      pointer-events: none;
      transform: translate(-50%, -50%);
      transition: width 0.4s, height 0.4s, opacity 0.4s;
      z-index: 9998;
    }

    /* Hiệu ứng khi hover vào link hoặc card */
    body:has(a:hover, button:hover, .card:hover) #cursor-dot {
      width: 12px;
      height: 12px;
    }
    body:has(a:hover, button:hover, .card:hover) #cursor-outline {
      width: 60px;
      height: 60px;
      opacity: 0.5;
    }
    /* --- HẾT CUSTOM CURSOR --- */


    body {
      background-color: #0e0e0e;
      color: #fff;
      overflow-x: hidden;
    }

    /* Giữ nguyên nền 3D canvas */
    canvas#bg {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
    }

    /* --- CẢI TIẾN: THANH ĐIỀU HƯỚNG DÍNH (STICKY NAVBAR) --- */
    #navbar {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      z-index: 1000;
      transition: background-color 0.4s ease, backdrop-filter 0.4s ease, box-shadow 0.4s ease;
    }
    
    /* Trạng thái khi cuộn */
    #navbar.scrolled {
      background-color: rgba(14, 14, 14, 0.7); /* Màu custom-dark với độ mờ */
      backdrop-filter: blur(10px);
      box-shadow: 0 4px 30px rgba(0, 0, 0, 0.2);
      border-bottom: 1px solid rgba(240, 198, 116, 0.1);
    }
    /* --- HẾT NAVBAR --- */


    /* Cải tiến thẻ Card với hiệu ứng 3D và viền vàng (Giữ nguyên) */
    .card {
      position: relative;
      background: #1a1a1a;
      border-radius: 15px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.6);
      transition: transform 0.4s ease, box-shadow 0.4s ease;
      transform-style: preserve-3d;
      will-change: transform;
      overflow: hidden; /* Cần cho hiệu ứng shine */

      /* CẢI TIẾN: Thiết lập trạng thái ban đầu cho GSAP */
      opacity: 0;
      transform: translateY(50px) scale(0.95);
    }

    .card::before {
      content: "";
      position: absolute;
      inset: 0;
      border: 2px solid transparent;
      border-radius: 15px;
      background: linear-gradient(120deg, transparent, #f0c674, transparent) border-box;
      -webkit-mask:
        linear-gradient(#fff 0 0) padding-box,
        linear-gradient(#fff 0 0);
      -webkit-mask-composite: destination-out;
      mask-composite: exclude;
      opacity: 0;
      transition: opacity 0.4s ease;
      z-index: 1;
    }

    /* --- CẢI TIẾN: HIỆU ỨNG SHINE KHI HOVER --- */
    .card::after {
      content: '';
      position: absolute;
      top: 0;
      left: -150%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
      transition: left 0.6s ease;
      transform: skewX(-25deg);
      z-index: 2;
    }

    .card:hover::after {
      left: 150%;
    }
    /* --- HẾT HIỆU ỨNG SHINE --- */

    .card:hover {
      box-shadow: 0 0 35px rgba(240,198,116,0.3);
      transform: scale(1.03) perspective(1000px) rotateX(2deg) rotateY(1deg);
    }
    
    .card:hover::before {
      opacity: 1;
    }

    .card img {
      width: 100%;
      height: 250px;
      object-fit: cover;
      filter: brightness(85%);
      transition: filter 0.4s;
      border-radius: 15px 15px 0 0;
      position: relative;
      z-index: 0;
    }

    .card:hover img {
      filter: brightness(100%);
    }

    .card h3 {
      font-family: 'Playfair Display', serif;
      color: #f0c674;
    }
    
    .card .card-content {
      position: relative;
      z-index: 2;
    }
    
    /* --- CẢI TIẾN: NÚT SCROLL TO TOP --- */
    #scrollToTopBtn {
      position: fixed;
      bottom: 30px;
      right: 30px;
      width: 50px;
      height: 50px;
      background-color: #f0c674;
      color: #0e0e0e;
      border: none;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 15px rgba(240, 198, 116, 0.4);
      z-index: 999;
      opacity: 0;
      visibility: hidden;
      transform: translateY(20px);
      transition: opacity 0.3s, visibility 0.3s, transform 0.3s;
    }
    
    #scrollToTopBtn.visible {
      opacity: 1;
      visibility: visible;
      transform: translateY(0);
    }
    
    #scrollToTopBtn:hover {
      transform: scale(1.1);
      box-shadow: 0 6px 20px rgba(240, 198, 116, 0.6);
    }
    /* --- HẾT SCROLL TO TOP --- */

  </style>
</head>
<body class="font-sans">
  <!-- Cải tiến: Custom Cursor -->
  <div id="cursor-dot"></div>
  <div id="cursor-outline"></div>

  <!-- Cải tiến: Thanh điều hướng -->
  <nav id="navbar" class="py-4 px-6 md:px-12">
    <div class="max-w-7xl mx-auto flex justify-between items-center">
      <a href="#" class="font-serif text-2xl font-bold text-custom-gold tracking-wider uppercase">
        World Museum
      </a>
      <div class="hidden md:flex gap-8">
        <a href="#home" class="text-gray-300 hover:text-custom-gold transition-colors duration-300">Trang chủ</a>
        <a href="#gallery" class="text-gray-300 hover:text-custom-gold transition-colors duration-300">Bộ sưu tập</a>
        <a href="#explore" class="text-gray-300 hover:text-custom-gold transition-colors duration-300">Khám phá</a>
      </div>
    </div>
  </nav>

  <!-- Nền 3D (Giữ nguyên) -->
  <canvas id="bg"></canvas>

  <!-- Header Section - (Giữ nguyên nội dung, thêm id) -->
  <header id="home" class="text-center pt-32 pb-24 px-6 bg-gradient-to-b from-[#151515] to-custom-dark">
    <h1 class="font-serif text-5xl md:text-6xl font-bold text-custom-gold tracking-wider uppercase">
      World Museum
    </h1>
    <p class="text-lg md:text-xl text-gray-300 mt-4 max-w-2xl mx-auto">
      Khám phá những bảo tàng nổi tiếng và tuyệt đẹp nhất thế giới
    </p>
  </header>

  <!-- Gallery Section - (Giữ nguyên nội dung, bỏ data-aos) -->
  <section id="gallery" class="gallery max-w-7xl mx-auto p-8 md:p-12 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
    
    <!-- 20 bảo tàng gốc - Bỏ data-aos để thay bằng GSAP -->
    <div class="card">
      <img src="https://th.bing.com/th?id=OIF.XFgdgxs2UXjyiTw5JTEs%2fQ&rs=1&pid=ImgDetMain&o=7&rm=3" alt="Louvre Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Louvre – Pháp</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nổi tiếng với kiệt tác Mona Lisa, bảo tàng Louvre là biểu tượng của nghệ thuật và kiến trúc Pháp, thu hút hàng triệu du khách mỗi năm.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://cdn.sanity.io/images/cctd4ker/production/7c82dc04b9b7cd6fdc7050baac63f9a0fa1d0b58-3200x1800.jpg?w=750&q=75&fit=clip&auto=format" alt="The Met">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng The Met – Hoa Kỳ</h3>
        <p class="text-gray-300 text-justify leading-relaxed">The Met tại New York là một trong những bảo tàng lớn nhất thế giới, lưu giữ hơn hai triệu tác phẩm từ cổ đại đến hiện đại.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://mia.vn/media/uploads/blog-du-lich/bao-tang-vatican-1-1708686666.jpeg" alt="Vatican Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Vatican – Vatican</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Bảo tàng Vatican lưu giữ hàng nghìn tác phẩm quý giá, trong đó có trần Sistine của Michelangelo – biểu tượng của nghệ thuật Phục Hưng.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://travelcultura.com/wp-content/uploads/The-main-complex-of-The-Hermitage-Museum-in-St-Petersburg.jpg" alt="Hermitage Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Hermitage – Nga</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nằm tại St. Petersburg, Hermitage là một trong những bảo tàng nghệ thuật cổ điển lớn nhất và sang trọng nhất thế giới.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://dulichokela.com/wp-content/uploads/2023/11/kham-pha-bao-tang-anh.jpg" alt="British Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Anh – Vương Quốc Anh</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Bảo tàng Anh lưu giữ hơn 8 triệu hiện vật lịch sử, từ xác ướp Ai Cập đến các di tích Hy Lạp cổ đại.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://static.wixstatic.com/media/15b084_48479e3212f44d0dbd1095d387ef2093~mv2.jpg/v1/fill/w_1000,h_667,al_c,q_90,usm_0.66_1.00_0.01/15b084_48479e3212f44d0dbd1095d387ef2093~mv2.jpg" alt="Prado Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Prado – Tây Ban Nha</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nơi trưng bày những tác phẩm vĩ đại của Velázquez, Goya và El Greco – một trong những bảo tàng nghệ thuật cổ điển hàng đầu châu Âu.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://www.historyhit.com/app/uploads/bis-images/5158533/Uffizi-788x537.jpg" alt="Uffizi Gallery">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Phòng trưng bày Uffizi – Ý</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Tọa lạc ở Florence, Uffizi là nơi lưu giữ những kiệt tác của thời kỳ Phục Hưng, bao gồm Botticelli, Leonardo da Vinci và Michelangelo.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://kimlientravel.com.vn/upload/image/image-20240117164725-1.jpeg" alt="National Museum of China">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Quốc gia Trung Quốc</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Bảo tàng lịch sử – văn hóa lớn nhất châu Á, trưng bày di sản Trung Hoa từ thời kỳ đồ đá đến hiện đại.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://media2.gody.vn/public/v7/place/nhat-ban/tokyo/tokyo-national-museum_660253cd5a35f.jpg" alt="Tokyo National Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Quốc gia Tokyo</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nơi lưu giữ những báu vật nghệ thuật Nhật Bản, từ kiếm Samurai, Kimono cổ đến tranh khắc gỗ Ukiyo-e.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://tse3.mm.bing.net/th/id/OIP.NGcb-05lRU9Kej0V5wP7rQHaFw?rs=1&pid=ImgDetMain&o=7&rm=3" alt="Guggenheim Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Guggenheim – TBN</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Biểu tượng của kiến trúc đương đại tại Bilbao, Guggenheim là sự kết hợp tuyệt vời giữa nghệ thuật, ánh sáng và kim loại titan.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://dulichphuonghoang.vn/upload/images/4(87).jpg" alt="Egyptian Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Ai Cập – Ai Cập</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nằm tại Cairo, đây là nơi lưu giữ bộ sưu tập cổ vật Pharaon lớn nhất thế giới, bao gồm kho báu từ lăng mộ Tutankhamun.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://tse1.mm.bing.net/th/id/OIP.j4U3PfaI2Rx8ynFnmni4qQHaEM?rs=1&pid=ImgDetMain&o=7&rm=3" alt="Rijksmuseum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Rijksmuseum – Hà Lan</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Trái tim của nghệ thuật Hà Lan tại Amsterdam, nổi tiếng với "The Night Watch" của Rembrandt và các kiệt tác Thời kỳ Vàng son.</p>
      </div>
    </div>
    
    <div class="card">
      <img src="https://eurotravel.com.vn/wp-content/uploads/2024/08/Musee-dOrsay.webp" alt="Musée d'Orsay">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Orsay – Pháp</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nằm trong một nhà ga xe lửa cũ, Orsay sở hữu bộ sưu tập nghệ thuật Ấn tượng và Hậu ấn tượng lớn nhất thế giới, với các tác phẩm của Monet, Van Gogh.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://th.bing.com/th/id/R.0ac0d7578fd7fdaa0ca32dc957abd838?rik=eRFORZBW2mJAuA&riu=http%3a%2f%2fmyathenstour.com%2fwp-content%2fuploads%2f2016%2f02%2fAcropolis_museum.jpg&ehk=GGCkBIKmFCf2tgM59a9ppVzxIiGcv5JmJ6Lv1a29TnM%3d&risl=&pid=ImgRaw&r=0" alt="Acropolis Museum">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Acropolis – Hy Lạp</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Tại Athens, bảo tàng hiện đại này trưng bày các hiện vật vô giá từ đền Parthenon và thành cổ Acropolis, nhìn thẳng ra di tích lịch sử.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://www.mepcinc.com/wp-content/uploads/2015/11/00-AIC_01.jpg" alt="Art Institute of Chicago">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Viện Nghệ thuật Chicago – Hoa Kỳ</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Một trong những bảo tàng nghệ thuật lâu đời và lớn nhất Hoa Kỳ, nổi tiếng với các bộ sưu tập Ấn tượng và nghệ thuật Mỹ, như "American Gothic".</p>
      </div>
    </div>
    
    <div class="card">
      <img src="https://cdn.apollo.audio/one/media/6337/21f7/38f0/1b04/f737/f51a/KAHH5H.jpg?quality=80&format=jpg" alt="Tate Modern">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Tate Modern – Vương Quốc Anh</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Từ một nhà máy điện cũ tại London, Tate Modern trở thành một trong những bảo tàng nghệ thuật đương đại và hiện đại hàng đầu thế giới.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://th.bing.com/th/id/OSK.HEROL-cElSiZ70TG4m2XjAffLH28UJ8tcEqUCW1hewLhYjc?o=7rm=3&rs=1&pid=ImgDetMain&o=7&rm=3" alt="National Gallery of Art">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Nghệ thuật Quốc gia – Hoa Kỳ</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Tại Washington D.C., bảo tàng này lưu giữ một bộ sưu tập nghệ thuật Tây phương đồ sộ, từ thời Trung Cổ đến hiện đại, và miễn phí vé vào cửa.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://baotanglichsu.vn/DataFiles/asset/upload/thang_10_nam_2017/bao_tang_nhan_chung_hoc_quoc_gia_mexico/21.jpg" alt="Museo Nacional de Antropología">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Nhân chủng học – Mexico</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Bảo tàng quan trọng nhất Mexico City, trưng bày di sản khảo cổ học và nhân chủng học của các nền văn minh Mesoamerica, nổi bật là Đá Mặt Trời (Lịch Aztec).</p>
      </div>
    </div>
    
    <div class="card">
      <img src="https://cdn3.ivivu.com/2024/02/bao-tang-Tretyakov-ivivu-10-1.jpg" alt="State Tretyakov Gallery">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Tretyakov – Nga</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Nằm tại Moscow, đây là kho tàng nghệ thuật quốc gia của Nga, lưu giữ bộ sưu tập nghệ thuật Nga lớn nhất và quan trọng nhất trên thế giới.</p>
      </div>
    </div>

    <div class="card">
      <img src="https://bazantravel.com/cdn/medias/uploads/30/30271-bao-tang-dan-gian-quoc-gia-han-quoc.jpg" alt="National Museum of Korea">
      <div class="p-6 card-content">
        <h3 class="text-2xl font-bold mb-3 text-center">Bảo tàng Quốc gia Hàn Quốc</h3>
        <p class="text-gray-300 text-justify leading-relaxed">Tại Seoul, đây là bảo tàng hàng đầu về lịch sử và nghệ thuật Hàn Quốc, trưng bày từ cổ vật thời tiền sử đến nghệ thuật đương đại.</p>
      </div>
    </div>
    
  </section>

  <!-- Explore Section (Giữ nguyên nội dung, bỏ data-aos) -->
  <section id="explore" class="explore text-center py-24 px-6 bg-gradient-to-t from-[#111] to-custom-dark">
    <h2 class="font-serif text-4xl md:text-5xl text-custom-gold mb-6">Khám phá thêm</h2>
    <p class="max-w-3xl mx-auto text-gray-300 text-lg md:text-xl mb-12">
      Thế giới còn vô vàn bảo tàng kỳ vĩ khác — nơi lưu giữ tinh hoa nghệ thuật, lịch sử và văn hóa nhân loại. Hãy cùng khám phá thêm những tuyệt tác ở Tokyo, London, Madrid và nhiều nơi khác.
    </p>
    <a href="https://artsandculture.google.com/partner?hl=vi" target="_blank"
       class="inline-flex items-center gap-3 bg-custom-gold text-black font-bold py-4 px-8 rounded-full text-lg
              transition-all duration-300 ease-in-out
              hover:shadow-lg hover:shadow-yellow-500/30 hover:-translate-y-1 transform">
      <i data-lucide="palmtree"></i>
      Khám phá trên Google Arts & Culture
    </a>
  </section>

  <!-- Footer (Giữ nguyên) -->
  <footer class="text-center py-12 px-6 bg-[#111] border-t border-gray-800">
    <div class="flex justify-center gap-6 mb-4">
      <a href="#" class="text-gray-500 hover:text-custom-gold transition-colors"><i data-lucide="facebook" class="w-6 h-6"></i></a>
      <a href="#" class="text-gray-500 hover:text-custom-gold transition-colors"><i data-lucide="instagram" class="w-6 h-6"></i></a>
      <a href="#" class="text-gray-500 hover:text-custom-gold transition-colors"><i data-lucide="twitter" class="w-6 h-6"></i></a>
      <a href="#" class="text-gray-500 hover:text-custom-gold transition-colors"><i data-lucide="youtube" class="w-6 h-6"></i></a>
    </div>
    <p class="text-gray-500 text-sm">© 2025 World Museum | Thiết kế bởi AI & bạn ✨</p>
  </footer>

  <!-- Cải tiến: Nút Scroll to Top -->
  <button id="scrollToTopBtn" title="Lên đầu trang">
    <i data-lucide="arrow-up"></i>
  </button>

  <!-- Script (Cải tiến) -->
  <!-- Bỏ AOS, giữ Three.js và GSAP -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

  <script>
    // Khởi tạo Lucide Icons (Giữ nguyên)
    lucide.createIcons();
    gsap.registerPlugin(ScrollTrigger);

    // === NỀN 3D (Giữ nguyên) ===
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ 
      canvas: document.querySelector('#bg'), 
      alpha: true 
    });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

    const geometry = new THREE.IcosahedronGeometry(100, 2);
    const material = new THREE.MeshStandardMaterial({
      color: 0xf0c674,
      wireframe: true,
      emissive: 0x111111,
      emissiveIntensity: 0.4
    });
    const mesh = new THREE.Mesh(geometry, material);
    scene.add(mesh);

    const pointLight = new THREE.PointLight(0xf0c674, 2);
    pointLight.position.set(40, 40, 40);
    scene.add(pointLight);
    const ambient = new THREE.AmbientLight(0x404040, 2);
    scene.add(ambient);

    camera.position.z = 250;

    gsap.to(pointLight.position, {
      x: -40,
      duration: 6,
      yoyo: true,
      repeat: -1,
      ease: "sine.inOut"
    });

    const mouse = new THREE.Vector2();
    document.addEventListener('mousemove', (event) => {
      mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
      mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
    });

    // Tương tác cuộn (Giữ nguyên)
    gsap.to(mesh.rotation, {
      x: "+=1.5",
      y: "+=1",
      scrollTrigger: {
        trigger: "body",
        start: "top top",
        end: "bottom bottom",
        scrub: 1.5,
      }
    });

    // Vòng lặp Animation (Giữ nguyên)
    const clock = new THREE.Clock();
    function animate() {
      const elapsedTime = clock.getElapsedTime();
      
      mesh.rotation.x += 0.0005;
      mesh.rotation.y += 0.0008;

      camera.position.x += (mouse.x * 20 - camera.position.x) * 0.05;
      camera.position.y += (mouse.y * 20 - camera.position.y) * 0.05;
      camera.lookAt(scene.position);
      
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }
    animate();

    // Xử lý resize (Giữ nguyên)
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    });

    // === HIỆU ỨNG 3D TILT CHO CARD (Giữ nguyên) ===
    const cards = document.querySelectorAll('.card');
    cards.forEach(card => {
      card.addEventListener('mousemove', (e) => {
        const rect = card.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        
        const { width, height } = rect;
        const rotateX = (y / height - 0.5) * -20;
        const rotateY = (x / width - 0.5) * 20;
        
        gsap.to(card, {
          transform: `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale(1.08)`,
          duration: 0.5,
          ease: "power2.out"
        });
      });
      
      card.addEventListener('mouseleave', () => {
        gsap.to(card, {
          transform: 'perspective(1000px) rotateX(0deg) rotateY(0deg) scale(1.03)',
          duration: 0.7,
          ease: "elastic.out(1, 0.3)"
        });
      });
    });

    // === CẢI TIẾN: LOGIC CHO CÁC TÍNH NĂNG MỚI ===
    document.addEventListener('DOMContentLoaded', () => {

      // --- 1. Custom Cursor ---
      const cursorDot = document.querySelector('#cursor-dot');
      const cursorOutline = document.querySelector('#cursor-outline');
      
      window.addEventListener('mousemove', (e) => {
        const { clientX, clientY } = e;
        
        gsap.to(cursorDot, {
          x: clientX,
          y: clientY,
          duration: 0.2,
          ease: 'power2.out'
        });
        
        gsap.to(cursorOutline, {
          x: clientX,
          y: clientY,
          duration: 0.5,
          ease: 'power2.out'
        });
      });

      // --- 2. Sticky Navbar ---
      const navbar = document.querySelector('#navbar');
      ScrollTrigger.create({
        start: 'top -80px', // Khi cuộn qua 80px
        onEnter: () => navbar.classList.add('scrolled'),
        onLeaveBack: () => navbar.classList.remove('scrolled'),
      });

      // --- 3. Scroll to Top Button ---
      const scrollToTopBtn = document.querySelector('#scrollToTopBtn');
      
      // Hiển thị/ẩn nút
      ScrollTrigger.create({
        start: 'top -400px', // Khi cuộn qua 400px
        onEnter: () => scrollToTopBtn.classList.add('visible'),
        onLeaveBack: () => scrollToTopBtn.classList.remove('visible'),
      });
      
      // Sự kiện click
      scrollToTopBtn.addEventListener('click', () => {
        window.scrollTo({ top: 0, behavior: 'smooth' });
      });

      // --- 4. CẢI TIẾN: HOẠT ẢNH CARD BẰNG GSAP (Thay thế AOS) ---
      gsap.utils.toArray('.card').forEach(card => {
        gsap.to(card, {
          opacity: 1,
          y: 0,
          scale: 1,
          duration: 0.8,
          ease: 'power2.out',
          scrollTrigger: {
            trigger: card,
            start: 'top 85%', // Bắt đầu khi 85% card lọt vào view
            toggleActions: 'play none none none',
            // markers: true, // Bỏ comment để debug
          }
        });
      });

      // --- 5. Hoạt ảnh cho phần Explore ---
      gsap.from('#explore h2, #explore p, #explore a', {
        opacity: 0,
        y: 50,
        duration: 1,
        stagger: 0.2, // Các phần tử xuất hiện lần lượt
        ease: 'power2.out',
        scrollTrigger: {
          trigger: '#explore',
          start: 'top 70%',
        }
      });
      
      // --- 6. Hoạt ảnh cho Header (cho "sịn" hơn) ---
      gsap.from('header h1, header p', {
        opacity: 0,
        y: 40,
        duration: 1.2,
        stagger: 0.2,
        ease: 'power3.out',
        delay: 0.3 // Chờ một chút để trang tải
      });
    });

  </script>
</body>
</html>
