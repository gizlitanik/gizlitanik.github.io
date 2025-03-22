<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>BOYKOT</title>
  <link href="https://fonts.googleapis.com/css?family=Roboto:400,700&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Roboto', sans-serif;
      background: #fff;
      color: #333;
    }
    /* Header with branding */
    .header {
      background: #ED1D24;
      padding: 20px;
      text-align: center;
    }
    .header h1 {
      margin: 0;
      font-size: 2em;
      color: #fff;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 40px 20px;
    }
    /* Search box styling */
    #searchBox {
      display: block;
      width: 100%;
      max-width: 400px;
      margin: 20px auto;
      padding: 12px 16px;
      border: 2px solid #ED1D24;
      border-radius: 4px;
      font-size: 16px;
      outline: none;
    }
    #searchBox:focus {
      box-shadow: 0 0 5px rgba(237,29,36,0.5);
    }
    /* Responsive grid for companies */
    #partners {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    .partner {
      background: #fff;
      border: 1px solid #eee;
      border-radius: 4px;
      padding: 20px;
      text-align: center;
      transition: transform 0.2s, box-shadow 0.2s;
      cursor: pointer;
    }
    .partner:hover {
      transform: scale(1.03);
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    .partner img {
      max-width: 100%;
      height: auto;
      margin-bottom: 10px;
    }
    .partner-name {
      font-size: 16px;
      font-weight: 700;
    }
    /* Modal Styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.5);
    }
    .modal-content {
      background-color: #fff;
      margin: 10% auto;
      padding: 20px;
      border-radius: 8px;
      max-width: 500px;
      text-align: center;
      position: relative;
    }
    .close-button {
      position: absolute;
      top: 10px;
      right: 20px;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }
    .modal img {
      max-width: 80px;
      margin-bottom: 20px;
    }
    .modal h2 {
      margin: 10px 0;
    }
    .modal p {
      margin: 5px 0;
      line-height: 1.5;
    }
  </style>
</head>
<body>
  <!-- Header -->
  <header class="header">
    <h1>BOYKOT</h1>
  </header>
  
  <!-- Main container -->
  <div class="container">
    <input type="text" id="searchBox" placeholder="Ara...">
    <div id="partners">
      <!-- Company cards will be rendered here -->
    </div>
  </div>

  <!-- Modal for displaying company details -->
  <div id="companyModal" class="modal">
    <div class="modal-content">
      <span class="close-button" id="closeModal">&times;</span>
      <img id="modalLogo" src="" alt="Company Logo">
      <h2 id="modalName"></h2>
      <p id="modalActivity"></p>
      <p id="modalHolding"></p>
    </div>
  </div>

  <script>
    // All companies from the file with additional information.
    const companies = [
      // Otomotiv Sektörü
      { name: "Volkswagen (VW)", logo: "images/volkswagen-vw.png", activity: "Binek ve ticari otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Audi", logo: "images/audi.png", activity: "Binek otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Bentley", logo: "images/bentley.png", activity: "Lüks otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Bugatti", logo: "images/bugatti.png", activity: "Spor otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Porsche", logo: "images/porsche.png", activity: "Lüks spor otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "SEAT", logo: "images/seat.png", activity: "Otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "CUPRA", logo: "images/cupra.png", activity: "Otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Škoda", logo: "images/skoda.png", activity: "Otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Lamborghini", logo: "images/lamborghini.png", activity: "Lüks spor otomobil markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "Scania", logo: "images/scania.png", activity: "Ağır vasıta ve kamyon markası (Türkiye distribütörü Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "DOD", logo: "images/dod.png", activity: "İkinci el araç alım-satım markası (Doğuş Otomotiv)", holding: "Doğuş Holding" },
      { name: "TÜVTÜRK", logo: "images/tuvturk.png", activity: "Araç muayene istasyonları zinciri (Doğuş, TÜV SÜD ve Akfen ortaklığı)", holding: "Doğuş Holding" },
      { name: "TÜV SÜD D-Expert", logo: "images/tuv-sud-d-expert.png", activity: "İkinci el araç ekspertiz hizmeti (Doğuş – TÜV SÜD ortak hizmeti)", holding: "Doğuş Holding" },
      { name: "Sigortaladim.com", logo: "images/sigortaladim.png", activity: "Online sigorta satış platformu (araç sigortaları vb.)", holding: "Doğuş Holding" },
      { name: "vdf (Volkswagen Doğuş Finans)", logo: "images/vdf.png", activity: "Araç finansmanı ve sigorta hizmetleri (VW grup araç kredileri)", holding: "Doğuş Holding" },
      { name: "Doğuş Oto", logo: "images/dogus-oto.png", activity: "Yetkili otomotiv bayii ağı (VW, Audi, Seat vb. markalar için perakende satış & servis)", holding: "Doğuş Holding" },
      { name: "TÜMOSAN", logo: "images/tumosan.png", activity: "Traktör ve dizel motor markası (TÜMOSAN Motor ve Traktör A.Ş.)", holding: "Albayrak Holding" },
      { name: "Kilim Mobilya", logo: "images/kilim-mobilya.png", activity: "Mobilya ve ev eşyası perakende markası (mağaza zinciri)", holding: "Demirören Holding" },
      { name: "Uyum Market", logo: "images/uyum-market.png", activity: "Süpermarket zinciri (İstanbul merkezli perakende gıda mağazaları)", holding: "Demirören Holding" },
      { name: "Makro Market", logo: "images/makro-market.png", activity: "Süpermarket zinciri (Ankara merkezli perakende gıda mağazaları)", holding: "Demirören Holding" },

      // Gıda & İçecek Sektörü
      { name: "Sukkar", logo: "images/sukkar.png", activity: "Toz şeker markası (Erzurum ve Erzincan şeker fabrikalarının ürün markası)", holding: "Albayrak Holding" },
      { name: "Şifa Yemek", logo: "images/sifa-yemek.png", activity: "Toplu yemek üretimi ve catering hizmeti (kurumsal yemek servisi)", holding: "İhlas Holding" },
      { name: "Elif Tur (İhlas Kuzuluk)", logo: "images/elif-tur.png", activity: "Kaplıca tesisi içinde akaryakıt istasyonu ve market hizmeti (Kuzuluk tesisleri)", holding: "İhlas Holding" },
      { name: "Marmara Yağ (Ham Yağ)", logo: "images/marmara-yag.png", activity: "Bitkisel ham yağ üretimi (endüstriyel gıda girdisi, Marmara Gıda A.Ş.)", holding: "Demirören Holding" },

      // Perakende & E-Ticaret Sektörü
      { name: "n11.com", logo: "images/n11.png", activity: "Online alışveriş ve pazar yeri platformu (e-ticaret sitesi)", holding: "Doğuş Holding" },
      { name: "Ihlas Pazarlama", logo: "images/ihlas-pazarlama.png", activity: "Doğrudan satış pazarlama ağı (ev aletleri, küçük ev cihazları satışı)", holding: "İhlas Holding" },
      { name: "Demirören İstiklal AVM", logo: "images/demiroren-istiklal-avm.png", activity: "Alışveriş merkezi (İstiklal Caddesi’nde Demirören AVM)", holding: "Demirören Holding" },
      { name: "Ketebe Yayınları", logo: "images/ketebe-yayinlari.png", activity: "Kültür, edebiyat ve çocuk kitapları yayıncılığı (Ketebe, Ketebe Genç, Ketebe Çocuk)", holding: "Albayrak Holding" },
      { name: "Antur Turizm", logo: "images/antur-turizm.png", activity: "Seyahat acentesi ve tur organizasyon hizmetleri (turizm & seyahat şirketi)", holding: "Doğuş Holding" },
      { name: "Yeşil Adamlar", logo: "images/yesil-adamlar.png", activity: "Atık toplama ve geri dönüşüm hizmetleri (çevre temizlik markası)", holding: "Albayrak Holding" },

      // Dijital Servisler
      { name: "Zubizu", logo: "images/zubizu.png", activity: "Mobil yaşam stili ve sadakat uygulaması (restoran, alışveriş fırsatları)", holding: "Doğuş Holding" },
      { name: "Zingat", logo: "images/zingat.png", activity: "Online emlak ve gayrimenkul ilan platformu (web sitesi ve uygulama)", holding: "Doğuş Holding" },
      { name: "Mobilet", logo: "images/mobilet.png", activity: "Etkinlik ve konser bileti satış platformu (mobil uygulama)", holding: "Doğuş Holding" },
      { name: "Misli.com", logo: "images/misli.png", activity: "Online iddaa ve bahis platformu (spor müsabakaları ve at yarışı bahis)", holding: "Demirören Holding" },
      { name: "Misli.az", logo: "images/misli-az.png", activity: "Online şans oyunları platformu (Azerbaycan - dijital bahis)", holding: "Demirören Holding" },
      { name: "Sisal Şans", logo: "images/sisal-sans.png", activity: "Milli Piyango ve sayısal loto işletme markası (Milli Piyango online platform ortağı)", holding: "Demirören Holding" },
      { name: "Azerlotereya", logo: "images/azerlotereya.png", activity: "Azerbaycan Milli Piyango işletme markası (Demirören iştiraki)", holding: "Demirören Holding" },
      { name: "Hipodrom.com", logo: "images/hipodrom.png", activity: "Online at yarışı ve ganyan bahis platformu", holding: "Demirören Holding" },
      { name: "Ihlas Net", logo: "images/ihlas-net.png", activity: "İnternet servis sağlayıcı ve web hizmetleri (IhlasNet)", holding: "İhlas Holding" },
      { name: "Asist", logo: "images/asist.png", activity: "Kamu araç filo yönetimi dijital çözüm platformu (belediye araçları bakım-yönetim)", holding: "Albayrak Holding" },

      // Otelcilik & Turizm Sektörü
      { name: "d.ream", logo: "images/d-ream.png", activity: "Dünya çapında restoran ve eğlence markaları işletmecisi (20+ ülkede, 46 marka)", holding: "Doğuş Holding (d.ream)" },
      { name: "1920 Istanbul Restoran & Bar", logo: "images/1920-istanbul.png", activity: "Restoran & bar", holding: "Doğuş Holding (d.ream)" },
      { name: "29 (Ulus 29)", logo: "images/29-ulus29.png", activity: "Restoran & gece kulübü (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Aarde Restoran", logo: "images/aarde.png", activity: "Restoran (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Adile Sultan Sarayı", logo: "images/adile-sultan-sarayi.png", activity: "Tarihi mekan/restoran (İstanbul, etkinlik ve restoran)", holding: "Doğuş Holding (d.ream)" },
      { name: "Amazonico Restoran", logo: "images/amazonico.png", activity: "Latin Amerika mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Aurea Restoran", logo: "images/aurea.png", activity: "Akdeniz mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Bar des Pres", logo: "images/bar-des-pres.png", activity: "Restoran & lounge (Fransız mutfağı; İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Biz İstanbul Restoran", logo: "images/biz-istanbul.png", activity: "Restoran (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Borsa Restoran", logo: "images/borsa.png", activity: "Türk mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Cafe di Dolce", logo: "images/cafe-di-dolce.png", activity: "Kafe & pastane (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Cova Kafe & Pastane", logo: "images/cova-kafe.png", activity: "Milano menşeli (İstanbul şubesi)", holding: "Doğuş Holding (d.ream)" },
      { name: "Coya Restoran", logo: "images/coya.png", activity: "Peru mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Çubuklu 29 Restoran & Etkinlik Mekanı", logo: "images/cubuklu-29.png", activity: "Etkinlik mekanı (Boğaz, İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Da Mario Restoran", logo: "images/da-mario.png", activity: "İtalyan mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "d.ream Catering (DD Scoop)", logo: "images/d-ream-catering.png", activity: "Etkinlik catering hizmetleri (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "El Paraguas Restoran", logo: "images/el-paraguas.png", activity: "İspanyol mutfağı (yurtdışı menşeli marka)", holding: "Doğuş Holding (d.ream)" },
      { name: "Etaru Restoran", logo: "images/etaru.png", activity: "Japon mutfağı, izakaya konsepti", holding: "Doğuş Holding (d.ream)" },
      { name: "Fenix Restoran & Bar", logo: "images/fenix.png", activity: "Asya füzyon mutfak (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Frankie Restoran & Müzik Kulübü", logo: "images/frankie.png", activity: "Restoran & müzik kulübü (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Gattopardo Restoran", logo: "images/gattopardo.png", activity: "İtalyan mutfağı konsepti", holding: "Doğuş Holding (d.ream)" },
      { name: "Gina Restoran", logo: "images/gina.png", activity: "İtalyan mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Günaydın Restoran", logo: "images/gunaydin.png", activity: "Steakhouse & kebap (Türkiye genelinde)", holding: "Doğuş Holding (d.ream)" },
      { name: "Inko Nito Restoran", logo: "images/inko-nito.png", activity: "Japon füzyon mutfağı", holding: "Doğuş Holding (d.ream)" },
      { name: "Kebapçı Etiler Restoran", logo: "images/kebapci-etiler.png", activity: "Kebap mekanı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Kiva Restoran", logo: "images/kiva.png", activity: "Anadolu mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Kojaki Restoran", logo: "images/kojaki.png", activity: "Japon-Kore mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Lacivert Restoran", logo: "images/lacivert.png", activity: "Boğaz manzaralı fine-dining (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Liman İstanbul Restoran", logo: "images/liman-istanbul.png", activity: "Deniz ürünleri (Karaköy, İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Masa Restoran", logo: "images/masa.png", activity: "Dünya mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Mezzaluna Restoran", logo: "images/mezzaluna.png", activity: "İtalyan mutfağı (İstanbul ve diğer şehirler)", holding: "Doğuş Holding (d.ream)" },
      { name: "Monochrome Kafe & Brasserie", logo: "images/monochrome.png", activity: "Kafe & Brasserie (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Numa Pompilio Restoran", logo: "images/numa-pompilio.png", activity: "İtalyan mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Nusr-Et Restoran", logo: "images/nusr-et.png", activity: "Steakhouse (Nusret Gökçe ile ünlü et lokantası)", holding: "Doğuş Holding (d.ream)" },
      { name: "Oblix Restoran", logo: "images/oblix.png", activity: "Modern Avrupa mutfağı (Londra menşeli, İstanbul’da pop-up etkinlikler)", holding: "Doğuş Holding (d.ream)" },
      { name: "Papaioannou Restoran", logo: "images/papaioannou.png", activity: "Deniz ürünleri (Yunan menşeli, İstanbul şubesi)", holding: "Doğuş Holding (d.ream)" },
      { name: "Parlé Restoran", logo: "images/parle.png", activity: "Fransız brasserie (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Roka Restoran", logo: "images/roka.png", activity: "Japon füzyon mutfağı (Londra menşeli, İstanbul şubesi)", holding: "Doğuş Holding (d.ream)" },
      { name: "Rüya Restoran", logo: "images/ruya.png", activity: "Çağdaş Anadolu mutfağı (Dubai & Londra menşeli, İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Sait Restoran", logo: "images/sait.png", activity: "Balık restoranı (Bodrum)", holding: "Doğuş Holding (d.ream)" },
      { name: "Saltbae Burger Restoran", logo: "images/saltbae-burger.png", activity: "Gurme burger konsepti (Nusret markası)", holding: "Doğuş Holding (d.ream)" },
      { name: "Ten Con Ten Restoran", logo: "images/ten-con-ten.png", activity: "İspanyol mutfağı (Madrid menşeli, İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "The Library Restoran & Bar", logo: "images/the-library.png", activity: "Londra menşeli konsept (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "The Populist Pub & Restoran", logo: "images/the-populist.png", activity: "Craft bira ve gastropub (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Ultramarinos Quintín Restoran", logo: "images/ultramarinos.png", activity: "İspanyol mutfağı (Madrid menşeli, İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Vogue Restaurant Restoran & Bar", logo: "images/vogue.png", activity: "Dünya mutfağı (İstanbul)", holding: "Doğuş Holding (d.ream)" },
      { name: "Zuma Restoran", logo: "images/zuma.png", activity: "Japon mutfağı (Londra menşeli, İstanbul şubesi)", holding: "Doğuş Holding (d.ream)" },

      // Otelcilik (D-Hotels)
      { name: "D Maris Bay Lüks Resort Otel", logo: "images/d-maris-bay.png", activity: "Lüks resort otel (Datça Yarımadası, Muğla)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "D-Resort Göcek Tatil Resort Oteli", logo: "images/d-resort-gocek.png", activity: "Tatil resort oteli (Göcek, Muğla)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "D-Resort Ayvalık Tatil Resort Oteli", logo: "images/d-resort-ayvalik.png", activity: "Tatil resort oteli (Ayvalık, Balıkesir)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "Argos in Cappadocia Butik Otel", logo: "images/argos-cappadocia.png", activity: "Butik otel (Ürgüp, Kapadokya)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "Maça Kızı Bodrum Lüks Butik Otel", logo: "images/maca-kizi-bodrum.png", activity: "Lüks butik otel (Türkbükü, Bodrum)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "Villa Maça Kızı Lüks Villa & Residence", logo: "images/villa-maca-kizi.png", activity: "Lüks villa & residence konaklama (Bodrum)", holding: "Doğuş Holding (D-Hotels)" },
      { name: "Kümbet Dağ Evi", logo: "images/kumbet-dag-evi.png", activity: "Dağ evi & doğal tatil tesisi (Kümbet Yaylası, Giresun)", holding: "Albayrak Holding" },
      { name: "Kemer Country Club", logo: "images/kemer-country-club.png", activity: "Golf, binicilik ve spor kulübü + konaklama (Göktürk, İstanbul)", holding: "Demirören Holding" },
      { name: "Mama Shelter İstanbul Butik Otel ve Restoran", logo: "images/mama-shelter.png", activity: "Butik otel ve restoran (Taksim, İstanbul)", holding: "Demirören Holding" },
      { name: "Türkiye Tatil Köyü", logo: "images/turkiye-tatil-koyu.png", activity: "Tatil köyü / devremülk tesis (Armutlu, Yalova – deniz kenarı tatil sitesi)", holding: "İhlas Holding" },
      { name: "Kuzuluk Kaplıca Evleri", logo: "images/kuzuluk-kaplica.png", activity: "Termal tatil köyü / devremülk tesis (Kuzuluk, Akyazı – kaplıca turizmi)", holding: "İhlas Holding" },
      { name: "Galataport İstanbul", logo: "images/galataport.png", activity: "Kruvaziyer limanı ve turistik yaşam merkezi (Karaköy, İstanbul)", holding: "Doğuş Holding" },

      // Ulaşım Sektörü
      { name: "İGA İstanbul Havalimanı", logo: "images/iga-istanbul.png", activity: "İstanbul Havalimanı işletme markası (dünyanın en büyük hub’larından)", holding: "Kalyon Holding (ortak girişim)" },
      { name: "Alport Banjul", logo: "images/alport-banjul.png", activity: "Banjul Limanı işletmesi (Gambiya, uluslararası denizyolu yolcu/yük limanı)", holding: "Albayrak Holding" },
      { name: "Alport Bakü", logo: "images/alport-baku.png", activity: "Bakü Limanı terminal işletmesi (Azerbaycan, deniz limanı)", holding: "Albayrak Holding" },
      { name: "Alport Conakry", logo: "images/alport-conakry.png", activity: "Conakry Limanı işletmesi (Gine, yük limanı)", holding: "Albayrak Holding" },
      { name: "Alport Mogadishu", logo: "images/alport-mogadishu.png", activity: "Mogadişu Limanı işletmesi (Somali, uluslararası liman)", holding: "Albayrak Holding" },
      { name: "Alport Trabzon", logo: "images/alport-trabzon.png", activity: "Trabzon Limanı işletmesi (Türkiye, Karadeniz yük/yolcu limanı)", holding: "Albayrak Holding" },
      { name: "Platform Turizm", logo: "images/platform-turizm.png", activity: "Personel ve yolcu taşımacılığı hizmetleri (servis kiralama, ulaşım organizasyonu)", holding: "Albayrak Holding" },
      { name: "Kuzey Marmara Otoyolu", logo: "images/kuzey-marmara-otoyolu.png", activity: "Kuzey Marmara Otoyolu işletmesi (İstanbul ve çevre iller paralı otoyol)", holding: "Kalyon Holding (ortak girişim)" },
      { name: "IDO", logo: "images/ido.png", activity: "İstanbul Deniz Otobüsleri (feribot ve deniz otobüsü seferleri; İstanbul)", holding: "Albayrak Holding (işletme ortaklığı)" },
      { name: "Zeyport Limanı", logo: "images/zeyport-limani.png", activity: "Zeytinburnu rıhtım ve marina işletmesi (yat ve gemi limanı)", holding: "Demirören Holding" },

      // Enerji Sektörü
      { name: "Milangaz", logo: "images/milangaz.png", activity: "Tüpgaz ve LPG dağıtım markası (Türkiye genelinde %9 pazar payı)", holding: "Demirören Holding" },
      { name: "Likidgaz", logo: "images/likidgaz.png", activity: "Tüpgaz ve LPG markası (Milangaz bünyesinde alt marka)", holding: "Demirören Holding" },
      { name: "Mutfakgaz", logo: "images/mutfakgaz.png", activity: "Tüpgaz markası (mutfak tüpü, evsel LPG)", holding: "Demirören Holding" },
      { name: "Güneşgaz", logo: "images/gunessgaz.png", activity: "Tüpgaz markası (LPG tüp dağıtımı)", holding: "Demirören Holding" },
      { name: "Türkpetrol (TP)", logo: "images/turkpetrol.png", activity: "Akaryakıt istasyon zinciri markası (86 yıllık yerli akaryakıt markası)", holding: "Demirören Holding" },
      { name: "Kalyon PV", logo: "images/kalyon-pv.png", activity: "Güneş paneli üretim markası (Konya Karapınar fabrikası, panel ve hücre satışı)", holding: "Kalyon Holding" },
      { name: "Ihlas Havacılık Yakıt", logo: "images/ihlas-havacilik-yakit.png", activity: "Yakıt Jet yakıtı ikmal hizmeti (İhlas’ın havacılık yakıt girişimi)", holding: "İhlas Holding" },

      // Sağlık Sektörü
      { name: "Türkiye Hastanesi", logo: "images/turkiye-hastanesi.png", activity: "Özel genel hastane (İstanbul-Şişli’de tam teşekküllü hastane)", holding: "İhlas Holding" },
      { name: "İhlas Hayat Hastanesi", logo: "images/ihlas-hayat-hastanesi.png", activity: "Özel tıp merkezi (İhlas’ın sağlık kuruluşu, poliklinik hizmetleri)", holding: "İhlas Holding" },
      { name: "Demirören Tıp Merkezi", logo: "images/demiroren-tip-merkezi.png", activity: "Özel poliklinik (Demirören Grubu bünyesinde sağlık merkezi)", holding: "Demirören Holding" },

      // Beyaz Eşya & Elektronik Sektörü
      { name: "Aura Cleanmax", logo: "images/aura-cleanmax.png", activity: "Su filtreli elektrikli süpürge (temizlik robotu)", holding: "İhlas Holding" },
      { name: "Aura Roboclean", logo: "images/aura-roboclean.png", activity: "Su filtreli elektrikli süpürge (ileri seviye, sağlık sertifikalı)", holding: "İhlas Holding" },
      { name: "Aura Cebilon", logo: "images/aura-cebilon.png", activity: "Ev tipi su arıtma cihazı (Reverse osmosis sistemi)", holding: "İhlas Holding" },
      { name: "Aura Qvac", logo: "images/aura-qvac.png", activity: "Elektrikli süpürge (küçük boy, kuru vakum)", holding: "İhlas Holding" },
      { name: "Aura Livac", logo: "images/aura-livac.png", activity: "Elektrikli süpürge (ıslak/kuru vakum)", holding: "İhlas Holding" },
      { name: "Aura WDry", logo: "images/aura-wdry.png", activity: "Halı yıkama ve ıslak vakum makinesi", holding: "İhlas Holding" },
      { name: "Aura Maxivac", logo: "images/aura-maxivac.png", activity: "Endüstriyel tip güçlü elektrikli süpürge", holding: "İhlas Holding" },
      { name: "Aura Technovac", logo: "images/aura-technovac.png", activity: "Çok amaçlı temizlik sistemi (sulu filtrasyonlu)", holding: "İhlas Holding" },
      { name: "Aura Stillvac", logo: "images/aura-stillvac.png", activity: "Dikey tipi elektrikli süpürge (sessiz seri)", holding: "İhlas Holding" },
      { name: "Besta Panelvan ve Minibüs", logo: "images/besta-panelvan.png", activity: "Ihlas Motor’un Kia lisansı ile üretilen model", holding: "İhlas Holding" },
      { name: "Ceres Kamyonet", logo: "images/ceres-kamyonet.png", activity: "Hafif kamyonet aracı (Ihlas Motor üretimi)", holding: "İhlas Holding" },
      { name: "Cobra Kamyon", logo: "images/cobra-kamyon.png", activity: "Orta sınıf kamyon aracı (Ihlas Motor üretimi)", holding: "İhlas Holding" },

      // Eğitim Kurumları
      { name: "İhlas Koleji", logo: "images/ihlas-koleji.png", activity: "Özel okul (K-12 eğitim; İhlas Eğitim Kurumları – kolej ve lise)", holding: "İhlas Holding" },
      { name: "Hasan Kalyoncu Üniversitesi", logo: "images/hasan-kalyoncu-universitesi.png", activity: "Vakıf üniversitesi (Gaziantep’te vakıf üniversitesi)", holding: "Kalyon Holding" },
      { name: "Erdem Koleji", logo: "images/erdem-koleji.png", activity: "Özel okul (Gaziantep, Kalyon Vakfı bünyesinde K-12 okulları)", holding: "Kalyon Holding" },
      { name: "Ata Koleji (Ata Eğitim)", logo: "images/ata-koleji.png", activity: "Özel okul (İstanbul’da anaokulu-ilkokul-lise; Demirören ailesine ait)", holding: "Demirören Holding" },
      { name: "Sakine Kalyoncu Özel Eğitim Okulu", logo: "images/sakine-kalyoncu.png", activity: "Özel eğitim uygulama okulu (engelli çocuklar için; İstanbul)", holding: "Kalyon Holding" },
      { name: "NUN Okulları", logo: "images/nun-okullari.png", activity: "Özel uluslararası okul (İstanbul, Albayrak ailesinin eğitim vakfı)", holding: "Albayrak Holding" },
      { name: "Enver Ören Anadolu Lisesi", logo: "images/enver-oren.png", activity: "Özel Anadolu lisesi (İstanbul; İhlas Holding kurucusu adına)", holding: "İhlas Holding" }
    ];

    // Reference DOM elements
    const partnersDiv = document.getElementById('partners');
    const searchBox = document.getElementById('searchBox');
    const modal = document.getElementById('companyModal');
    const closeModalButton = document.getElementById('closeModal');
    const modalLogo = document.getElementById('modalLogo');
    const modalName = document.getElementById('modalName');
    const modalActivity = document.getElementById('modalActivity');
    const modalHolding = document.getElementById('modalHolding');

    // Render company cards based on search filter.
    function renderCompanies(filter = "") {
      partnersDiv.innerHTML = "";
      const filteredCompanies = companies.filter(company =>
        company.name.toLowerCase().includes(filter.toLowerCase())
      );
      filteredCompanies.forEach(company => {
        const card = document.createElement('div');
        card.className = "partner";
        card.innerHTML = `
          <img src="${company.logo}" alt="${company.name} Logo">
          <div class="partner-name">${company.name}</div>
        `;
        card.addEventListener('click', () => showModal(company));
        partnersDiv.appendChild(card);
      });
    }

    // Show modal with company details.
    function showModal(company) {
      modalLogo.src = company.logo;
      modalLogo.alt = company.name + " Logo";
      modalName.textContent = company.name;
      modalActivity.textContent = "Faaliyet Alanı: " + company.activity;
      modalHolding.textContent = "Holding / Şirket Grubu: " + company.holding;
      modal.style.display = "block";
    }

    // Close modal events.
    closeModalButton.addEventListener('click', () => { modal.style.display = "none"; });
    window.addEventListener('click', (event) => { if (event.target === modal) modal.style.display = "none"; });

    // Search functionality.
    searchBox.addEventListener('input', (event) => { renderCompanies(event.target.value); });
    renderCompanies();
  </script>
</body>
</html>
