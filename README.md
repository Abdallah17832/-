<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>تاج فون | تسويق إلكتروني</title>
  <style>
    body {
      font-family: 'Cairo', sans-serif;
      background-color: #f0f4f8;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      font-size: 32px;
      color: #0077b6;
      margin-bottom: 0;
    }
    h2.subtitle {
      text-align: center;
      font-size: 20px;
      color: #555;
      margin-top: 5px;
      margin-bottom: 30px;
    }
    .container {
      max-width: 600px;
      margin: auto;
      background-color: white;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .product {
      background-color: #f8f9fa;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 15px;
      display: flex;
      align-items: center;
      gap: 15px;
    }
    .product img {
      max-width: 100px;
      max-height: 100px;
      border-radius: 10px;
      object-fit: contain;
      border: 1px solid #ccc;
    }
    .product-info {
      flex: 1;
    }
    .order-btn {
      background-color: #25d366;
      color: white;
      border: none;
      padding: 10px 16px;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
      white-space: nowrap;
      margin-bottom: 5px;
      width: 100%;
    }
    .order-btn:hover {
      background-color: #128c4a;
    }
    .delete-btn {
      background-color: #e63946;
      color: white;
      border: none;
      padding: 8px 12px;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
      white-space: nowrap;
      width: 100%;
    }
    .delete-btn:hover {
      background-color: #b22230;
    }
    .settings-btn {
      width: 100%;
      padding: 12px;
      background-color: #0077b6;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 18px;
      margin-top: 30px;
      cursor: pointer;
      font-weight: bold;
    }
    .settings-btn:hover {
      background-color: #005f8a;
    }
    .hidden {
      display: none;
    }
    input, select, textarea, button {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    .section-title {
      color: #0077b6;
    }
    footer {
      text-align: center;
      margin-top: 40px;
      color: #333;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <h1>📱 تاج فون</h1>
  <h2 class="subtitle">أسطورة</h2>

  <!-- قسم عرض المنتجات -->
  <div class="container" id="productList">
    <h2 class="section-title">🛍️ المنتجات المتاحة</h2>
    <p>لا توجد منتجات حالياً.</p>
  </div>

  <!-- زر الإعدادات -->
  <div class="container">
    <button class="settings-btn" onclick="toggleSettings()">⚙️ الإعدادات</button>
  </div>

  <!-- قسم الإعدادات -->
  <div class="container hidden" id="settingsPanel">
    <h2 class="section-title">⚙️ الإعدادات</h2>

    <label>🌐 اللغة:</label>
    <select>
      <option>العربية</option>
      <option>English</option>
    </select>

    <button onclick="alert('تطبيق تاج فون | إصدار 1.0')">ℹ️ حول التطبيق</button>

    <button onclick="document.getElementById('passwordBox').classList.remove('hidden')">➕ إضافة منتج</button>

    <div id="passwordBox" class="hidden">
      <input type="password" id="adminPass" placeholder="أدخل كلمة السر" />
      <button onclick="checkPassword()">تأكيد</button>
    </div>

    <div id="addForm" class="hidden">
      <input type="text" id="productName" placeholder="اسم الموبايل" />
      <input type="text" id="productPrice" placeholder="السعر" />
      <input type="number" id="productQuantity" placeholder="عدد المنتج" min="1" />
      <textarea id="productDesc" placeholder="الوصف"></textarea>
      <label>📷 صورة المنتج:</label>
      <input type="file" id="productImage" accept="image/*" />
      <button onclick="addProduct()">إضافة المنتج</button>
    </div>
  </div>

  <footer>
    📞 للتواصل: 01016124740
  </footer>

  <script>
    const password = "123456789";

    function toggleSettings() {
      const panel = document.getElementById("settingsPanel");
      panel.classList.toggle("hidden");
    }

    function checkPassword() {
      const pass = document.getElementById("adminPass").value;
      if (pass === password) {
        document.getElementById("addForm").classList.remove("hidden");
        document.getElementById("passwordBox").classList.add("hidden");
      } else {
        alert("كلمة السر غير صحيحة 😅");
      }
    }

    function addProduct() {
      const name = document.getElementById("productName").value.trim();
      const price = document.getElementById("productPrice").value.trim();
      const quantityStr = document.getElementById("productQuantity").value.trim();
      const desc = document.getElementById("productDesc").value.trim();
      const imageInput = document.getElementById("productImage");

      if (!name || !price || !quantityStr || !desc) {
        alert("من فضلك املأ كل الحقول!");
        return;
      }

      const quantity = parseInt(quantityStr);
      if (isNaN(quantity) || quantity < 1) {
        alert("عدد المنتج يجب أن يكون رقم صحيح أكبر من صفر");
        return;
      }

      if (imageInput.files.length === 0) {
        alert("من فضلك اختر صورة للمنتج");
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        const imageData = e.target.result;
        saveProduct({ name, price, quantity, desc, image: imageData });
      };
      reader.readAsDataURL(imageInput.files[0]);
    }

    function saveProduct(product) {
      let products = JSON.parse(localStorage.getItem("products")) || [];
      products.push(product);
      localStorage.setItem("products", JSON.stringify(products));
      alert("✅ تم إضافة المنتج!");
      displayProducts();

      // إعادة تعيين الحقول
      document.getElementById("productName").value = "";
      document.getElementById("productPrice").value = "";
      document.getElementById("productQuantity").value = "";
      document.getElementById("productDesc").value = "";
      document.getElementById("productImage").value = "";
      document.getElementById("addForm").classList.add("hidden");
    }

    function displayProducts() {
      let products = JSON.parse(localStorage.getItem("products")) || [];
      const list = document.getElementById("productList");
      list.innerHTML = "<h2 class='section-title'>🛍️ المنتجات المتاحة</h2>";
      if (products.length === 0) {
        list.innerHTML += "<p>لا توجد منتجات حالياً.</p>";
      }
      products.forEach((product, index) => {
        list.innerHTML += `
          <div class="product">
            <img src="${product.image}" alt="${product.name}" />
            <div class="product-info">
              <h3>${product.name}</h3>
              <p><strong>السعر:</strong> ${product.price} جنيه</p>
              <p><strong>الكمية المتاحة:</strong> ${product.quantity}</p>
              <p>${product.desc}</p>
              <button class="order-btn" onclick="orderWhatsApp(${index})">🛒 اطلب الآن</button>
              <button class="delete-btn" onclick="deleteProduct(${index})">❌ حذف المنتج</button>
            </div>
          </div>
        `;
      });
    }

    function orderWhatsApp(index) {
      let products = JSON.parse(localStorage.getItem("products")) || [];
      if (!products[index]) return;
      const product = products[index];
      if (product.quantity <= 0) {
        alert("عذراً، المنتج غير متوفر حالياً");
        return;
      }

      const phone = "201016124740"; // رقم مصر +20 1016124740 بدون صفر البداية
      const text = encodeURIComponent(
        `السلام عليكم، أريد أطلب المنتج:\n` +
        `اسم المنتج: ${product.name}\n` +
        `السعر: ${product.price} جنيه\n` +
        `الكمية: 1\n` +
        `الوصف: ${product.desc}`
      );
      const url = `https://wa.me/${phone}?text=${text}`;
      window.open(url, "_blank");

      // نقص الكمية واحد
      products[index].quantity -= 1;
      if (products[index].quantity <= 0) {
        // امسح المنتج لو خلص
        products.splice(index, 1);
      }
      localStorage.setItem("products", JSON.stringify(products));
      displayProducts();
    }

    function deleteProduct(index) {
      if (confirm("هل أنت متأكد أنك تريد حذف هذا المنتج؟")) {
        let products = JSON.parse(localStorage.getItem("products")) || [];
        products.splice(index, 1);
        localStorage.setItem("products", JSON.stringify(products));
        displayProducts();
      }
    }

    window.onload = displayProducts;
  </script>

</body>
</html>
