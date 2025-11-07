<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>محرك بحث أنمي، مانغا، منهاوا، كيبوب، كيدراما</title>
<style>
  body {
    font-family: Arial, sans-serif;
    direction: rtl;
    padding: 20px;
    background: #121212;
    color: #eee;
  }
  h1 {
    text-align: center;
    margin-bottom: 25px;
  }
  #searchInput {
    width: 100%;
    max-width: 450px;
    padding: 12px;
    font-size: 1.2rem;
    border-radius: 8px;
    border: none;
    outline: none;
    margin: 0 auto 15px auto;
    display: block;
  }
  #searchBtn {
    display: block;
    margin: 0 auto 25px auto;
    padding: 12px 30px;
    font-size: 1.2rem;
    border-radius: 40px;
    border: none;
    background: #0077cc;
    color: white;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  #searchBtn:hover {
    background: #005fa3;
  }
  .types-filter {
    text-align: center;
    margin-bottom: 20px;
  }
  .types-filter label {
    margin: 0 10px;
    cursor: pointer;
  }
  .types-filter input[type="checkbox"] {
    margin-left: 5px;
    width: 18px;
    height: 18px;
    cursor: pointer;
  }
  #results {
    max-width: 700px;
    margin: 0 auto;
  }
  .media-item {
    background: #222;
    margin-bottom: 20px;
    border-radius: 12px;
    padding: 15px;
    box-shadow: 0 0 8px #0077cc;
    display: flex;
    gap: 15px;
    align-items: flex-start;
  }
  .media-image {
    width: 120px;
    border-radius: 10px;
    flex-shrink: 0;
    box-shadow: 0 0 8px #005f99;
  }
  .media-content {
    flex: 1;
  }
  .media-title {
    font-size: 1.4rem;
    font-weight: 700;
    margin-bottom: 8px;
  }
  .media-desc {
    margin-bottom: 10px;
    max-height: 120px;
    overflow: auto;
    line-height: 1.3rem;
  }
  .media-info {
    font-size: 0.95rem;
    margin-bottom: 6px;
  }
  .loading {
    text-align: center;
    font-size: 1.3rem;
  }
  a {
    color: #00aaff;
    text-decoration: none;
  }
  a:hover {
    text-decoration: underline;
  }
  /* Scrollbar for description */
  .media-desc::-webkit-scrollbar {
    width: 6px;
  }
  .media-desc::-webkit-scrollbar-thumb {
    background: #005f99;
    border-radius: 3px;
  }
  .media-desc::-webkit-scrollbar-track {
    background: #222;
  }
</style>
</head>
<body>

<h1>محرك بحث أنمي، مانغا، منهاوا، كيبوب، كيدراما</h1>

<input type="text" id="searchInput" placeholder="اكتب كلمة البحث هنا" />

<div class="types-filter">
  <label><input type="checkbox" value="ANIME" checked /> أنمي</label>
  <label><input type="checkbox" value="MANGA" checked /> مانغا</label>
  <label><input type="checkbox" value="MANHWA" checked /> منهاوا</label>
  <label><input type="checkbox" value="KPOP" checked /> كيبوب</label>
  <label><input type="checkbox" value="KDRAMA" checked /> كيدراما</label>
</div>

<button id="searchBtn">ابحث</button>

<div id="results"></div>

<script>
  const searchInput = document.getElementById('searchInput');
  const searchBtn = document.getElementById('searchBtn');
  const resultsDiv = document.getElementById('results');
  const checkboxes = document.querySelectorAll('.types-filter input[type="checkbox"]');

  // بيانات محلية إضافية (كيبوب و كيدراما)
  const localData = [
    // أنمي ومانغا و منهاوا (أمثلة بسيطة)
    {
      id: 1,
      title: { english: "Elite Classroom", romaji: "فصل النخبة", native: "نخبة الفصل" },
      description: "أنمي عن طلبة ذوي قدرات عالية وتحديات نفسية.",
      episodes: 12,
      chapters: null,
      volumes: null,
      coverImage: { medium: "https://i.imgur.com/I9m6IrP.jpg" },
      siteUrl: "#",
      type: "ANIME",
      status: "مكتمل"
    },
    {
      id: 2,
      title: { english: "Attack on Titan", romaji: "هجوم العمالقة", native: "進撃の巨人" },
      description: "مانغا تدور حول البشر يقاتلون العمالقة.",
      episodes: null,
      chapters: 139,
      volumes: 34,
      coverImage: { medium: "https://i.imgur.com/E9C0vIY.jpg" },
      siteUrl: "#",
      type: "MANGA",
      status: "مكتمل"
    },
    // كيبوب
    {
      id: 1001,
      title: { english: "BTS", romaji: "بي تي إس", native: "방탄소년단" },
      description: "فرقة كيبوب مشهورة عالمياً من كوريا الجنوبية.",
      episodes: null,
      chapters: null,
      volumes: null,
      coverImage: { medium: "https://i.imgur.com/VlAZUP7.jpg" },
      siteUrl: "https://en.wikipedia.org/wiki/BTS_(band)",
      type: "KPOP",
      status: "نشط"
    },
    {
      id: 1002,
      title: { english: "BLACKPINK", romaji: "بلاكبينك", native: "블랙핑크" },
      description: "فرقة فتيات كيبوب ناجحة وشعبية كبيرة.",
      episodes: null,
      chapters: null,
      volumes: null,
      coverImage: { medium: "https://i.imgur.com/1qWyvpN.jpg" },
      siteUrl: "https://en.wikipedia.org/wiki/Blackpink",
      type: "KPOP",
      status: "نشط"
    },
    // كيدراما
    {
      id: 2001,
      title: { english: "Crash Landing on You", romaji: "هبوط اضطراري عليك", native: "사랑의 불시착" },
      description: "دراما كورية رومانسية مشهورة ومحبوبة.",
      episodes: 16,
      chapters: null,
      volumes: null,
      coverImage: { medium: "https://i.imgur.com/rAqXTXC.jpg" },
      siteUrl: "https://en.wikipedia.org/wiki/Crash_Landing_on_You",
      type: "KDRAMA",
      status: "منتهي"
    },
    {
      id: 2002,
      title: { english: "Goblin", romaji: "جوابلين", native: "도깨비" },
      description: "دراما كورية تجمع بين الرومانسية والفانتازيا.",
      episodes: 16,
      chapters: null,
      volumes: null,
      coverImage: { medium: "https://i.imgur.com/uXq1QkB.jpg" },
      siteUrl: "https://en.wikipedia.org/wiki/Guardian:_The_Lonely_and_Great_God",
      type: "KDRAMA",
      status: "منتهي"
    }
  ];

  // جلب بيانات AniList (أنمي، مانغا، منهاوا فقط)
  async function fetchMedia(query, types) {
    // AniList لا يدعم KPOP أو KDRAMA لذا نستثنيها
    const aniListTypes = types.filter(t => ["ANIME", "MANGA", "MANHWA"].includes(t));
    if (aniListTypes.length === 0) return [];

    const promises = aniListTypes.map(type => fetchSingleType(query, type));
    const resultsArr = await Promise.all(promises);
    return resultsArr.flat();
  }

  async function fetchSingleType(query, type) {
    const graphqlQuery = {
      query: `
        query ($search: String, $type: MediaType) {
          Page(perPage: 10) {
            media(search: $search, type: $type) {
              id
              title {
                romaji
                english
                native
              }
              description(asHtml: false)
              episodes
              chapters
              volumes
              coverImage {
                medium
              }
              siteUrl
              type
              status
            }
          }
        }
      `,
      variables: {
        search: query,
        type: type
      }
    };

    const response = await fetch('https://graphql.anilist.co', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
      body: JSON.stringify(graphqlQuery)
    });

    if (!response.ok) {
      throw new Error('خطأ في جلب البيانات');
    }
    const data = await response.json();
    return data.data.Page.media || [];
  }

  // فلترة البيانات المحلية للكيبوب والكيدراما (وأيضاً الأنمي، مانغا، منهاوا)
  function filterLocalData(query, types) {
    query = query.toLowerCase();
    return localData.filter(item => {
      const matchesType = types.includes(item.type);
      const matchesQuery =
        (item.title.english && item.title.english.toLowerCase().includes(query)) ||
        (item.title.romaji && item.title.romaji.toLowerCase().includes(query)) ||
        (item.title.native && item.title.native.toLowerCase().includes(query)) ||
        (item.description && item.description.toLowerCase().includes(query));
      return matchesType && matchesQuery;
    });
  }

  function createMediaHTML(item) {
    const title = item.title.english || item.title.romaji || item.title.native || "لا يوجد عنوان";
    const desc = item.description ? item.description.replace(/<[^>]*>?/gm, '') : "لا يوجد وصف";
    let info = '';

    switch(item.type){
      case 'ANIME':
        info = `الحلقات: ${item.episodes || 'غير معروف'} - الحالة: ${item.status || 'غير معروف'}`;
        break;
      case 'MANGA':
      case 'MANHWA':
        info = `الفصول: ${item.chapters || 'غير معروف'} - المجلدات: ${item.volumes || 'غير معروف'} - الحالة: ${item.status || 'غير معروف'}`;
        break;
      case 'KPOP':
      case 'KDRAMA':
        info = `الحالة: ${item.status || 'غير معروف'}`;
        if(item.episodes) info = `الحلقات: ${item.episodes} - ${info}`;
        break;
      default:
        info = '';
    }

    return `
      <div class="media-item">
        <img src="${item.coverImage.medium}" alt="${title}" class="media-image" />
        <div class="media-content">
          <div class="media-title">${title} (${item.type})</div>
          <div class="media-desc">${desc}</div>
          <div class="media-info">${info}</div>
          <div><a href="${item.siteUrl}" target="_blank" rel="noopener noreferrer">رابط رسمي</a></div>
        </div>
      </div>
    `;
  }

  searchBtn.addEventListener('click', async () => {
    const query = searchInput.value.trim();
    resultsDiv.innerHTML = '';
    if (!query) {
      resultsDiv.innerHTML = '<p>يرجى كتابة نص للبحث.</p>';
      return;
    }

    const selectedTypes = Array.from(checkboxes)
      .filter(chk => chk.checked)
      .map(chk => chk.value);

    if(selectedTypes.length === 0){
      resultsDiv.innerHTML = '<p>اختر نوع واحد على الأقل.</p>';
      return;
    }

    resultsDiv.innerHTML = '<p class="loading">جاري البحث...</p>';

    try {
      const aniListResults = await fetchMedia(query, selectedTypes);
      const localResults = filterLocalData(query, selectedTypes);
      const combinedResults = [...localResults, ...aniListResults];

      if(combinedResults.length === 0){
        resultsDiv.innerHTML = '<p>لم يتم العثور على نتائج مطابقة.</p>';
        return;
      }
      resultsDiv.innerHTML = combinedResults.map(createMediaHTML).join('');
    } catch (error) {
      resultsDiv.innerHTML = `<p>حدث خطأ أثناء البحث: ${error.message}</p>`;
    }
  });
</script>

</body>
</html>
